# Updating Your Security Groups to Reference Peer VPC Groups<a name="vpc-peering-security-groups"></a>

You can update the inbound or outbound rules for your VPC security groups to reference security groups in the peered VPC\. Doing so allows traffic to flow to and from instances that are associated with the referenced security group in the peered VPC\.

To reference a security group in a peer VPC, the VPC peering connection must be in the `active` state\. 

You cannot reference the security group of a peer VPC that's in a different region\.

**To update your security group rules**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Select the security group, and choose **Inbound Rules**\. If you're modifying the outbound rules, choose **Outbound Rules**\.

1. Choose **Edit**, **Add another rule**\.

1. Specify the type, protocol, and port range as required\. For **Source** \(or **Destination** for an outbound rule\), enter the ID of the security group in the peer VPC\.
**Note**  
The peer VPC can be a VPC in your account, or a VPC in another AWS account\. 

1. Choose **Save**\.

A security group that references a security group in another AWS account includes the account number in **Source** or **Destination** field; for example, `123456789012/sg-1a2b3c4d`\.

Alternatively, you can use the following commands\.


| Action | Commands | 
| --- | --- | 
| Authorize inbound rules |  [authorize\-security\-group\-ingress](http://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-security-group-ingress.html) \(AWS CLI\) [Grant\-EC2SecurityGroupIngress](http://docs.aws.amazon.com/powershell/latest/reference/items/Grant-EC2SecurityGroupIngress.html) \(AWS Tools for Windows PowerShell\)  | 
| Authorize outbound rules |  [authorize\-security\-group\-egress](http://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-security-group-egress.html) \(AWS CLI\) [Grant\-EC2SecurityGroupEgress](http://docs.aws.amazon.com/powershell/latest/reference/items/Grant-EC2SecurityGroupEgress.html) \(AWS Tools for Windows PowerShell\)  | 
| Revoke inbound rules |  [revoke\-security\-group\-ingress](http://docs.aws.amazon.com/cli/latest/reference/ec2/revoke-security-group-ingress.html) \(AWS CLI\) [Revoke\-EC2SecurityGroupIngress](http://docs.aws.amazon.com/powershell/latest/reference/items/Revoke-EC2SecurityGroupIngress.html) \(AWS Tools for Windows PowerShell\)  | 
| Revoke outbound rules |  [revoke\-security\-group\-egress](http://docs.aws.amazon.com/cli/latest/reference/ec2/revoke-security-group-egress.html) \(AWS CLI\) [Revoke\-EC2SecurityGroupEgress](http://docs.aws.amazon.com/powershell/latest/reference/items/Revoke-EC2SecurityGroupEgress.html) \(AWS Tools for Windows PowerShell\)  | 

For example, to update your security group `sg-aaaa1111` to allow inbound access over HTTP from `sg-bbbb2222` that's in a peer VPC, you can use the following AWS CLI command:

```
aws ec2 authorize-security-group-ingress --group-id sg-aaaa1111 --protocol tcp --port 80 --source-group sg-bbbb2222 
```

The peer VPC can be a VPC in your account, or a VPC in another AWS account\.

After you've updated the security group rules, use the [describe\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command to view the referenced security group in your security group rules\. 

## Identifying Your Referenced Security Groups<a name="vpc-peering-referenced-groups"></a>

To determine if your security group is being referenced in the rules of a security group in a peer VPC, use one of the following commands for one or more security groups in your account\.

+ [describe\-security\-group\-references](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-group-references.html) \(AWS CLI\)

+ [Get\-EC2SecurityGroupReference](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2SecurityGroupReference.html) \(AWS Tools for Windows PowerShell\)

+ [DescribeSecurityGroupReferences](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DescribeSecurityGroupReferences.html) \(Amazon EC2 Query API\)

In the following example, the response indicates that security group `sg-bbbb2222` is being referenced by a security group in VPC `vpc-aaaaaaaa`:

```
aws ec2 describe-security-group-references --group-id sg-bbbb2222
```

```
{    
  "SecurityGroupsReferenceSet": [
    {
      "ReferencingVpcId": "vpc-aaaaaaaa ",
      "GroupId": "sg-bbbb2222",
      "VpcPeeringConnectionId": "pcx-b04deed9"       
    }   
  ]
}
```

**Note**  
Currently, you cannot identify security group references using the Amazon VPC or Amazon EC2 consoles\.

If the VPC peering connection is deleted, or if the owner of the peer VPC deletes the referenced security group, the security group rule becomes stale\. 

## Working with Stale Security Group Rules<a name="vpc-peering-stale-groups"></a>

A stale security group rule is a rule that references a security group in a peer VPC where the VPC peering connection has been deleted or the security group in the peer VPC has been deleted\. When a security group rule becomes stale, it's not automatically removed from your security group—you must manually remove it\. If a security group rule was stale because the VPC peering connection was deleted and you then create a new VPC peering connection with the same VPCs, it will no longer be marked as stale\.

You can view and delete the stale security group rules for a VPC using the Amazon VPC console\. 

**To view and delete stale security group rules**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\. 

1. Choose **View your stale rules** in the notification icon on the right \(this icon only displays if you have stale security group rules\)\.

1. To delete a stale rule, choose **Edit**, and then delete the rule\. Choose **Save Rules**\. You can check for stale rules in another VPC by entering the VPC ID in the **VPC** field\.

1. When you are done, choose **Close**\.

**To describe your stale security group rules using the command line or an API**

+ [describe\-stale\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-stale-security-groups.html) \(AWS CLI\)

+ [Get\-EC2StaleSecurityGroup](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2StaleSecurityGroup.html) \(AWS Tools for Windows PowerShell\)

+ [DescribeStaleSecurityGroups](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DescribeStaleSecurityGroups.html) \(Amazon EC2 Query API\)

In the following example, VPC A `(vpc-aaaaaaaa`\) and VPC B were peered, and the VPC peering connection was deleted\. Your security group `sg-aaaa1111` in VPC A references `sg-bbbb2222` in VPC B\. When you run the `describe-stale-security-groups` command for your VPC, the response indicates that security group `sg-aaaa1111` has a stale SSH rule that references `sg-bbbb2222`\.

```
aws ec2 describe-stale-security-groups --vpc-id vpc-aaaaaaaa
```

```
{
    "StaleSecurityGroupSet": [
        {
            "VpcId": "vpc-aaaaaaaa", 
            "StaleIpPermissionsEgress": [], 
            "GroupName": "Access1", 
            "StaleIpPermissions": [
                {
                    "ToPort": 22, 
                    "FromPort": 22, 
                    "UserIdGroupPairs": [
                        {
                            "VpcId": "vpc-bbbbbbbb", 
                            "PeeringStatus": "deleted", 
                            "UserId": "123456789101", 
                            "GroupName": "Prod1", 
                            "VpcPeeringConnectionId": "pcx-b04deed9", 
                            "GroupId": "sg-bbbb2222"
                        }
                    ], 
                    "IpProtocol": "tcp"
                }
            ], 
            "GroupId": "sg-aaaa1111", 
            "Description": "Reference remote SG"
        }
    ]
}
```

After you've identified the stale security group rules, you can delete them using the [revoke\-security\-group\-ingress](http://docs.aws.amazon.com/cli/latest/reference/ec2/revoke-security-group-ingress.html) or [revoke\-security\-group\-egress](http://docs.aws.amazon.com/cli/latest/reference/ec2/revoke-security-group-egress.html) commands\.