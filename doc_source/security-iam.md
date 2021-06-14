# Identity and access management for VPC peering<a name="security-iam"></a>

By default, IAM users cannot create or modify VPC peering connections\. To allow access to VPC peering resources, you create and attach an IAM policy either to:
+  The IAM user or 
+  The group to which the IAM user belongs\. 

The following are example IAM policies for working with VPC peering connections\.

For a list of Amazon VPC actions and supported resources and conditions keys for each action, see [Actions, Resources, and Condition Keys for Amazon EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonec2.html) in the *IAM User Guide*\.

**Topics**
+ [Creating a VPC peering connection](#vpc-peering-iam-create)
+ [Accepting a VPC peering connection](#vpc-peering-iam-accept)
+ [Deleting a VPC peering connection](#vpc-peering-iam-delete)
+ [Working within a specific account](#vpc-peering-iam-account)
+ [Managing VPC peering connections in the console](#peering-connection-console-iam)

## Creating a VPC peering connection<a name="vpc-peering-iam-create"></a>

The following policy allows users to create VPC peering connection requests using only VPCs that are tagged with `Purpose=Peering`\. The first statement applies a condition key \(`ec2:ResourceTag`\) to the VPC resource\. Note that the VPC resource for the `CreateVpcPeeringConnection` action is always the requester VPC\. 

The second statement grants users permissions to create the VPC peering connection resource, and therefore uses the \* wildcard in place of a specific resource ID\. 

```
{
"Version": "2012-10-17",
"Statement":[{
 "Effect":"Allow",
 "Action": "ec2:CreateVpcPeeringConnection",
 "Resource": "arn:aws:ec2:region:account:vpc/*",
  "Condition": {
    "StringEquals": {
     "ec2:ResourceTag/Purpose": "Peering"
    }
   }
  },
  {
  "Effect": "Allow",
  "Action": "ec2:CreateVpcPeeringConnection",
  "Resource": "arn:aws:ec2:region:account:vpc-peering-connection/*"
  }
 ]
}
```

The following policy allows users in AWS account 333333333333 to create VPC peering connections using any VPC in the `us-east-1` region, but only if the VPC that will be accepting the peering connection is a specific VPC \(`vpc-11223344556677889`\) in a specific account \(777788889999\)\. 

```
{
"Version": "2012-10-17",
"Statement": [{
 "Effect":"Allow",
 "Action": "ec2:CreateVpcPeeringConnection",
 "Resource": "arn:aws:ec2:us-east-1:333333333333:vpc/*"
 },
 {
  "Effect": "Allow",
  "Action": "ec2:CreateVpcPeeringConnection",
  "Resource": "arn:aws:ec2:region:333333333333:vpc-peering-connection/*",
    "Condition": {
     "ArnEquals": {
       "ec2:AccepterVpc": "arn:aws:ec2:region:777788889999:vpc/vpc-11223344556677889"
     }
    }
   }
 ]
}
```

## Accepting a VPC peering connection<a name="vpc-peering-iam-accept"></a>

The following policy allows users to accept VPC peering connection requests from AWS account 444455556666 only\. This helps to prevent users from accepting VPC peering connection requests from unknown accounts\. The first statement uses the `ec2:RequesterVpc` condition key to enforce this\. 

The policy also grants users permissions to accept VPC peering requests only when your VPC has the tag `Purpose=Peering`\. 

```
{
"Version": "2012-10-17",
"Statement":[{
 "Effect":"Allow",
 "Action": "ec2:AcceptVpcPeeringConnection",
 "Resource": "arn:aws:ec2:region:account:vpc-peering-connection/*",
  "Condition": {
   "ArnEquals": {
    "ec2:RequesterVpc": "arn:aws:ec2:region:444455556666:vpc/*"
   }
  }
 },
 {
 "Effect": "Allow",
 "Action": "ec2:AcceptVpcPeeringConnection",
 "Resource": "arn:aws:ec2:region:account:vpc/*",
  "Condition": {
   "StringEquals": {
    "ec2:ResourceTag/Purpose": "Peering"
    }
   }
  }
 ]
}
```

## Deleting a VPC peering connection<a name="vpc-peering-iam-delete"></a>

The following policy allows users in account 444455556666 to delete any VPC peering connection, except those that use the specified VPC `vpc-11223344556677889`, which is in the same account\. The policy specifies both the `ec2:AccepterVpc` and `ec2:RequesterVpc` condition keys, as the VPC may have been the requester VPC or the peer VPC in the original VPC peering connection request\. 

```
{
"Version": "2012-10-17",
"Statement": [{
  "Effect":"Allow",
  "Action": "ec2:DeleteVpcPeeringConnection",
  "Resource": "arn:aws:ec2:region:444455556666:vpc-peering-connection/*",
   "Condition": {
    "ArnNotEquals": {
     "ec2:AccepterVpc": "arn:aws:ec2:region:444455556666:vpc/vpc-11223344556677889",
     "ec2:RequesterVpc": "arn:aws:ec2:region:444455556666:vpc/vpc-11223344556677889"
    }
   }
  }
 ]
}
```

## Working within a specific account<a name="vpc-peering-iam-account"></a>

The following policy allows users to work with VPC peering connections entirely within a specific account\. Users can view, create, accept, reject, and delete VPC peering connections, provided they are all within AWS account 333333333333\. 

The first statement allows users to view all VPC peering connections\. The `Resource` element requires a \* wildcard in this case, as this API action \(`DescribeVpcPeeringConnections`\) currently does not support resource\-level permissions\.

The second statement allows users to create VPC peering connections, and allows access to all VPCs in account 333333333333 in order to do so\. 

The third statement uses a \* wildcard as part of the `Action` element to allow all VPC peering connection actions\. The condition keys ensure that the actions can only be performed on VPC peering connections with VPCs that are part of account 333333333333\. For example, a user is not allowed to delete a VPC peering connection if either the accepter or requester VPC is in a different account\. A user cannot create a VPC peering connection with a VPC in a different account\.

```
{
"Version": "2012-10-17",
"Statement": [{
 "Effect": "Allow",
 "Action": "ec2:DescribeVpcPeeringConnections",
 "Resource": "*"
  },
  {
  "Effect": "Allow",
  "Action": ["ec2:CreateVpcPeeringConnection","ec2:AcceptVpcPeeringConnection"],
  "Resource": "arn:aws:ec2:*:333333333333:vpc/*"
  },
  {
  "Effect": "Allow",
  "Action": "ec2:*VpcPeeringConnection",
  "Resource": "arn:aws:ec2:*:333333333333:vpc-peering-connection/*",
  "Condition": {
   "ArnEquals": {
    "ec2:AccepterVpc": "arn:aws:ec2:*:333333333333:vpc/*",
    "ec2:RequesterVpc": "arn:aws:ec2:*:333333333333:vpc/*"
    }
   }
  }
 ]
}
```

## Managing VPC peering connections in the console<a name="peering-connection-console-iam"></a>

To view VPC peering connections in the Amazon VPC console, users must have permission to use the `ec2:DescribeVpcPeeringConnections` action\. To use the **Create Peering Connection** page, users must have permission to use the `ec2:DescribeVpcs` action\. This allows them to view and select a VPC\. You can apply resource\-level permissions to all the `ec2:*PeeringConnection` actions, except `ec2:DescribeVpcPeeringConnections`\. 

The following policy allows users to view VPC peering connections, and to use the **Create VPC Peering Connection** dialog box to create a VPC peering connection using a specific requester VPC \(`vpc-11223344556677889`\) only\. If users try to create a VPC peering connection with a different requester VPC, the request fails\.

```
{
  "Version": "2012-10-17",
  "Statement":[{
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
    "arn:aws:ec2:*:*:vpc/vpc-11223344556677889",
    "arn:aws:ec2:*:*:vpc-peering-connection/*"
   ]
  }
  ]
}
```