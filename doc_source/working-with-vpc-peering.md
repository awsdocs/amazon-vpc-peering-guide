# Working with VPC Peering Connections<a name="working-with-vpc-peering"></a>

You can use the Amazon VPC console to create and work with VPC peering connections\.

**Topics**
+ [Creating and Accepting a VPC Peering Connection](create-vpc-peering-connection.md)
+ [Rejecting a VPC Peering Connection](reject-vpc-peering-connection.md)
+ [Updating Your Route Tables for a VPC Peering Connection](vpc-peering-routing.md)
+ [Updating Your Security Groups to Reference Peer VPC Groups](vpc-peering-security-groups.md)
+ [Modifying VPC Peering Connection Options](modify-peering-connections.md)
+ [Deleting a VPC Peering Connection](delete-vpc-peering-connection.md)
+ [Controlling Access to VPC Peering Connections](#vpc-peering-iam)

## Controlling Access to VPC Peering Connections<a name="vpc-peering-iam"></a>

By default, IAM users cannot create or modify VPC peering connections\. You can create an IAM policy that grants users permission to work with VPC peering connections, and you can control which resources users have access to during those requests\. For example policies for working with VPC peering connections, see [Controlling Access to Amazon VPC Resources](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_IAM.html) in the *Amazon VPC User Guide*\. For more information about IAM policies for Amazon EC2, see [IAM Policies for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) in the *Amazon EC2 User Guide for Linux Instances*\. 