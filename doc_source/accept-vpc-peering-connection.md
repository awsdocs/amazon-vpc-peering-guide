# Accept a VPC peering connection<a name="accept-vpc-peering-connection"></a>

A VPC peering connection that's in the `pending-acceptance` state must be accepted by the owner of the accepter VPC to be activated\. You cannot accept a VPC peering connection request that you've sent to another AWS account\. If you are creating a VPC peering connection in the same AWS account, you must both create and accept the request yourself\. 

If the VPCs are in different Regions, the request must be accepted in the Region of the accepter VPC\.

**Important**  
Do not accept VPC peering connections from unknown AWS accounts\. A malicious user may have sent you a VPC peering connection request to gain unauthorized network access to your VPC\. This is known as peer phishing\. You can safely reject unwanted VPC peering connection requests without any risk of the requester gaining access to any information about your AWS account or your VPC\. For more information, see [Reject a VPC peering connection](reject-vpc-peering-connection.md)\. You can also ignore the request and let it expire; by default, requests expire after 7 days\.

After you accept the VPC peering connection, you must add an entry to your route tables to enable traffic between the peered VPCs\. For more information, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**To accept a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Use the Region selector to choose the Region of the accepter VPC\.

1. In the navigation pane, choose **Peering connections**\. 

1. Select the pending VPC peering connection \(the status is `pending-acceptance`\), and choose **Actions**, **Accept Request**\.
**Note**  
If you cannot see the pending VPC peering connection, check the Region\. An inter\-Region peering request must be accepted in the Region of the accepter VPC\.

1. In the confirmation dialog box, choose **Yes, Accept**\. A second confirmation dialog displays; choose **Modify my route tables now** to go directly to the route tables page, or choose **Close** to do this later\.

Now that your VPC peering connection is active, you must add an entry to your VPC route table to enable traffic to be directed to the peer VPC\. For more information, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**To accept a VPC peering connection using the command line or an API**
+ [accept\-vpc\-peering\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/accept-vpc-peering-connection.html) \(AWS CLI\)
+ [Approve\-EC2VpcPeeringConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/Approve-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)
+ [AcceptVpcPeeringConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-AcceptVpcPeeringConnection.html) \(Amazon EC2 Query API\)