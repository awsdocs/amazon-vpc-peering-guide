# Rejecting a VPC peering connection<a name="reject-vpc-peering-connection"></a>

You can reject any VPC peering connection request that you've received that's in the `pending-acceptance` state\. You should only accept VPC peering connections from AWS accounts that you know and trust; you can reject any unwanted requests\. 

**To reject a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **Actions**, **Reject Request**\.

1. In the confirmation dialog box, choose **Yes, Reject**\.

**To reject a VPC peering connection using the command line or an API**
+ [reject\-vpc\-peering\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/reject-vpc-peering-connection.html) \(AWS CLI\)
+ [Deny\-EC2VpcPeeringConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/Deny-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)
+ [RejectVpcPeeringConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-RejectVpcPeeringConnection.html) \(Amazon EC2 Query API\)