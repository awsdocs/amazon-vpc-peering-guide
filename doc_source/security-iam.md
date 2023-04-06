# Identity and access management for VPC peering<a name="security-iam"></a>

By default, users cannot create or modify VPC peering connections\. To grant access to VPC peering resources, attach an IAM policy to an IAM identity, such as a role\.

**Topics**
+ [Create a VPC peering connection](#vpc-peering-iam-create)
+ [Accept a VPC peering connection](#vpc-peering-iam-accept)
+ [Delete a VPC peering connection](#vpc-peering-iam-delete)
+ [Work within a specific account](#vpc-peering-iam-account)
+ [Manage VPC peering connections in the console](#peering-connection-console-iam)

For a list of Amazon VPC actions, and the supported resources and conditions keys for each action, see [Actions, resources, and condition keys for Amazon EC2](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html) in the *Service Authorization Reference*\.

## Example: Create a VPC peering connection<a name="vpc-peering-iam-create"></a>

The following policy grants users permission to create VPC peering connection requests using VPCs that are tagged with `Purpose=Peering`\. The first statement applies a condition key \(`ec2:ResourceTag`\) to the VPC resource\. Note that the VPC resource for the `CreateVpcPeeringConnection` action is always the requester VPC\. 

The second statement grants users permission to create the VPC peering connection resources, and therefore uses the \* wildcard in place of a specific resource ID\.

```
{
  "Version": "2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action": "ec2:CreateVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id:vpc/*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Purpose": "Peering"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:CreateVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id:vpc-peering-connection/*"
    }
  ]
}
```

The following policy grants users in the specified AWS account permission to create VPC peering connections using any VPC in the specified Region, but only if the VPC that accepts the peering connection is a specific VPC in specific account\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect":"Allow",
      "Action": "ec2:CreateVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id-1:vpc/*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:CreateVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id-1:vpc-peering-connection/*",
      "Condition": {
        "ArnEquals": {
          "ec2:AccepterVpc": "arn:aws:ec2:region:account-id-2:vpc/vpc-id"
        }
      }
    }
  ]
}
```

## Example: Accept a VPC peering connection<a name="vpc-peering-iam-accept"></a>

The following policy grants users permission to accept VPC peering connection requests from a specific AWS account\. This helps to prevent users from accepting VPC peering connection requests from unknown accounts\. The statement uses the `ec2:RequesterVpc` condition key to enforce this\. 

```
{
  "Version": "2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action": "ec2:AcceptVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id-1:vpc-peering-connection/*",
      "Condition": {
        "ArnEquals": {
          "ec2:RequesterVpc": "arn:aws:ec2:region:account-id-2:vpc/*"
        }
      }
    }
  ]
}
```

The following policy grants users permission to accept VPC peering requests if the VPC has the tag `Purpose=Peering`\.

```
{
  "Version": "2012-10-17",
  "Statement":[
    {
      "Effect": "Allow",
      "Action": "ec2:AcceptVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id:vpc/*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Purpose": "Peering"
        }
      }
    }
  ]
}
```

## Example: Delete a VPC peering connection<a name="vpc-peering-iam-delete"></a>

The following policy grants users in the specified account permission to delete any VPC peering connection, except those that use the specified VPC, which is in the same account\. The policy specifies both the `ec2:AccepterVpc` and `ec2:RequesterVpc` condition keys, as the VPC might have been the requester VPC or the peer VPC in the original VPC peering connection request\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect":"Allow",
      "Action": "ec2:DeleteVpcPeeringConnection",
      "Resource": "arn:aws:ec2:region:account-id:vpc-peering-connection/*",
      "Condition": {
        "ArnNotEquals": {
          "ec2:AccepterVpc": "arn:aws:ec2:region:account-id:vpc/vpc-id",
          "ec2:RequesterVpc": "arn:aws:ec2:region:account-id:vpc/vpc-id"
        }
      }
    }
  ]
}
```

## Example: Work within a specific account<a name="vpc-peering-iam-account"></a>

The following policy grants users permission to work with VPC peering connections within a specific account\. Users can view, create, accept, reject, and delete VPC peering connections, provided they are all within the same AWS account\.

The first statement grants users permission to view all VPC peering connections\. The `Resource` element requires a \* wildcard in this case, as this API action \(`DescribeVpcPeeringConnections`\) currently does not support resource\-level permissions\.

The second statement grants users permission to create VPC peering connections, and access to all VPCs in the specified account in order to do so\.

The third statement uses a \* wildcard as part of the `Action` element to grant permission for all VPC peering connection actions\. The condition keys ensure that the actions can only be performed on VPC peering connections with VPCs that are part of the account\. For example, a user is cannot delete a VPC peering connection if either the accepter or requester VPC is in a different account\. A user cannot create a VPC peering connection with a VPC in a different account\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:DescribeVpcPeeringConnections",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["ec2:CreateVpcPeeringConnection","ec2:AcceptVpcPeeringConnection"],
      "Resource": "arn:aws:ec2:*:account-id:vpc/*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:*VpcPeeringConnection",
      "Resource": "arn:aws:ec2:*:account-id:vpc-peering-connection/*",
      "Condition": {
        "ArnEquals": {
          "ec2:AccepterVpc": "arn:aws:ec2:*:account-id:vpc/*",
          "ec2:RequesterVpc": "arn:aws:ec2:*:account-id:vpc/*"
        }
      }
    }
  ]
}
```

## Example: Manage VPC peering connections using the console<a name="peering-connection-console-iam"></a>

To view VPC peering connections in the Amazon VPC console, users must have permission to use the `ec2:DescribeVpcPeeringConnections` action\. To use the **Create Peering Connection** page, users must have permission to use the `ec2:DescribeVpcs` action\. This grants them permission to view and select a VPC\. You can apply resource\-level permissions to all the `ec2:*PeeringConnection` actions, except `ec2:DescribeVpcPeeringConnections`\. 

The following policy grants users permission to view VPC peering connections, and to use the **Create VPC Peering Connection** dialog box to create a VPC peering connection using a specific requester VPC only\. If users try to create a VPC peering connection with a different requester VPC, the request fails\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect":"Allow",
      "Action": [
        "ec2:DescribeVpcPeeringConnections", "ec2:DescribeVpcs"
      ],
      "Resource": "*"
    },
    {
      "Effect":"Allow",
      "Action": "ec2:CreateVpcPeeringConnection",
      "Resource": [
        "arn:aws:ec2:*:*:vpc/vpc-id",
        "arn:aws:ec2:*:*:vpc-peering-connection/*"
      ]
    }
  ]
}
```