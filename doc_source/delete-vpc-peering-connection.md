# Deleting a VPC Peering Connection<a name="delete-vpc-peering-connection"></a>

Either owner of a VPC in a peering connection can delete the VPC peering connection at any time\. You can also delete a VPC peering connection that you've requested that is still in the `pending-acceptance` state\.

Deleting a VPC in the Amazon VPC console that's part of an active VPC peering connection also deletes the VPC peering connection\. If you have requested a VPC peering connection with a VPC in another account, and you delete your VPC before the other party has accepted the request, the VPC peering connection is also deleted\. You cannot delete a VPC for which you have a `pending-acceptance` request from a VPC in another account\. You must first reject the VPC peering connection request\.

**To delete a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **Actions**, **Delete VPC Peering Connection**\.

1. In the confirmation dialog box, choose **Yes, Delete**\.

**To delete a VPC peering connection using the command line or an API**
+ [delete\-vpc\-peering\-connection](http://docs.aws.amazon.com/cli/latest/reference/ec2/delete-vpc-peering-connection.html) \(AWS CLI\)
+ [Remove\-EC2VpcPeeringConnection](http://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)
+ [DeleteVpcPeeringConnection](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DeleteVpcPeeringConnection.html) \(Amazon EC2 Query API\)