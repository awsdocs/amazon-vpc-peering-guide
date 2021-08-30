# Create and accept VPC peering connections<a name="create-vpc-peering-connection"></a>

To create a VPC peering connection, first create a request to peer with another VPC\. You can request a VPC peering connection with another VPC in your account, or with a VPC in a different AWS account\. For an inter\-Region VPC peering connection where the VPCs are in different Regions, the request must be made from the Region of the requester VPC\.

To activate the request, the owner of the accepter VPC must accept the request\. For an inter\-Region VPC peering connection, the request must be accepted in the Region of the accepter VPC\.

Before you begin, ensure that you are aware of the [limitations and rules](vpc-peering-basics.md#vpc-peering-limitations) for a VPC peering connection\.

**Topics**
+ [Create a VPC peering connection with another VPC in your account](#create-vpc-peering-connection-local)
+ [Create a VPC peering connection with a VPC in another AWS account](#create-vpc-peering-connection-remote)
+ [Accept a VPC peering connection](#accept-vpc-peering-connection)
+ [View your VPC peering connections](#describe-vpc-peering-connections)

## Create a VPC peering connection with another VPC in your account<a name="create-vpc-peering-connection-local"></a>

To request a VPC peering connection with a VPC in your account, ensure that you have the IDs of the VPCs with which you are creating the VPC peering connection\. You must both create and accept the VPC peering connection request yourself to activate it\.

You can create a VPC peering connection with a VPC in the same Region, or a different Region\.

**Important**  
Ensure that your VPCs do not have overlapping IPv4 CIDR blocks\. If they do, the status of the VPC peering connection immediately goes to `failed`\. This limitation applies even if the VPCs have unique IPv6 CIDR blocks\.

**To create a VPC peering connection with a VPC in the same Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**, **Create Peering Connection**\.

1. Configure the following information, and choose **Create Peering Connection** when you are done:
   + **Peering connection name tag**: You can optionally name your VPC peering connection\. 
   + **VPC \(Requester\)**: Select the VPC in your account with which you want to create the VPC peering connection\. 
   + Under **Select another VPC to peer with**: Ensure **My account** is selected, and select another of your VPCs\.
   + \(Optionally add or remove a tag\.

     \[Add a tag\] Choose **Add tag** and do the following:
     + For **Key**, enter the key name\.
     + For **Value**, enter the key value\.

     \[Remove a tag\] Choose the Delete button \("X"\) to the right of the tag’s Key and Value\.

1. In the confirmation dialog box, choose **OK**\.

1. Select the VPC peering connection that you've created, and choose **Actions**, **Accept Request**\.

1. In the confirmation dialog, choose **Yes, Accept**\. A second confirmation dialog displays; choose **Modify my route tables now** to go directly to the route tables page, or choose **Close** to do this later\.

**To create a VPC peering connection with a VPC in a different Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**, **Create Peering Connection**\.

1. Configure the following information, and choose **Create Peering Connection** when you are done:
   + **Peering connection name tag**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and a value that you specify\. 
   + **VPC \(Requester\)**: Select the requester VPC in your account with which to request the VPC peering connection\. 
   + **Account**: Ensure **My account** is selected\.
   + **Region**: Choose **Another region**, select the Region in which the accepter VPC resides\.
   +  **VPC \(Accepter\)**: Enter the ID of the accepter VPC\.

1. In the confirmation dialog box, choose **OK**\.

1. In the Region selector, select the Region of the accepter VPC\.

1. In the navigation pane, choose **Peering Connections**\. Select the VPC peering connection that you've created, and choose **Actions**, **Accept Request**\.

1. In the confirmation dialog, choose **Yes, Accept**\. A second confirmation dialog displays; choose **Modify my route tables now** to go directly to the route tables page, or choose **Close** to do this later\.

Now that your VPC peering connection is active, you must add an entry to your VPC route tables to enable traffic to be directed between the peered VPCs\. For more information, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

## Create a VPC peering connection with a VPC in another AWS account<a name="create-vpc-peering-connection-remote"></a>

You can request a VPC peering connection with a VPC that's in another AWS account\. Before you begin, ensure that you have the AWS account number and VPC ID of the VPC to peer with\. After you've created the request, the owner of the accepter VPC must accept the VPC peering connection to activate it\.

You can create a VPC peering connection with a VPC in the same Region, or a different Region\.

**Important**  
If the VPCs have overlapping IPv4 CIDR blocks, or if the account ID and VPC ID are incorrect or do not correspond with each other, the status of the VPC peering connection immediately goes to `failed`\. 

**To request a VPC peering connection with a VPC in another account in the same Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**, **Create Peering Connection**\.

1. Configure the information as follows, and choose **Create Peering Connection** when you are done:
   + **Peering connection name tag**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and a value that you specify\. This tag is only visible to you; the owner of the peer VPC can create their own tags for the VPC peering connection\.
   + **VPC \(Requester\)**: Select the VPC in your account with which to create the VPC peering connection\. 
   + **Account**: Choose **Another account**\.
   + **Account ID**: Enter the AWS account ID of the owner of the accepter VPC\.
   + **VPC \(Accepter\)**: Enter the ID of the VPC with which to create the VPC peering connection\.

1. In the confirmation dialog box, choose **OK**\.

**To request a VPC peering connection with a VPC in another account in a different Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**, **Create Peering Connection**\.

1. Configure the information as follows, and choose **Create Peering Connection** when you are done:
   + **Peering connection name tag**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and a value that you specify\. This tag is only visible to you; the owner of the peer VPC can create their own tags for the VPC peering connection\.
   + **VPC \(Requester\)**: Select the VPC in your account with which to create the VPC peering connection\. 
   + **Account**: Choose **Another account**\.
   + **Account ID**: Enter the AWS account ID of the owner of the accepter VPC\.
   + **Region**: Choose **Another region**, select the Region in which the accepter VPC resides\.
   + **VPC \(Accepter\)**: Enter the ID of the VPC with which to create the VPC peering connection\.

1. In the confirmation dialog box, choose **OK**\.

The VPC peering connection that you've created is not active\. To activate it, the owner of the accepter VPC must accept the VPC peering connection request\. To enable traffic to be directed to the peer VPC, update your VPC route table\. For more information, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**To create a VPC peering connection using the command line or an API**
+ [create\-vpc\-peering\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpc-peering-connection.html) \(AWS CLI\)
+ [New\-EC2VpcPeeringConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)
+ [CreateVpcPeeringConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpcPeeringConnection.html) \(Amazon EC2 Query API\)

## Accept a VPC peering connection<a name="accept-vpc-peering-connection"></a>

A VPC peering connection that's in the `pending-acceptance` state must be accepted by the owner of the accepter VPC to be activated\. You cannot accept a VPC peering connection request that you've sent to another AWS account\. If you are creating a VPC peering connection in the same AWS account, you must both create and accept the request yourself\. 

If the VPCs are in different Regions, the request must be accepted in the Region of the accepter VPC\.

**Important**  
Do not accept VPC peering connections from unknown AWS accounts\. A malicious user may have sent you a VPC peering connection request to gain unauthorized network access to your VPC\. This is known as peer phishing\. You can safely reject unwanted VPC peering connection requests without any risk of the requester gaining access to any information about your AWS account or your VPC\. For more information, see [Reject a VPC peering connection](reject-vpc-peering-connection.md)\. You can also ignore the request and let it expire; by default, requests expire after 7 days\.

**To accept a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Use the Region selector to choose the Region of the accepter VPC\.

1. In the navigation pane, choose **Peering Connections**\. 

1. Select the pending VPC peering connection \(the status is `pending-acceptance`\), and choose **Actions**, **Accept Request**\.
**Note**  
If you cannot see the pending VPC peering connection, check the Region\. An inter\-Region peering request must be accepted in the Region of the accepter VPC\.

1. In the confirmation dialog box, choose **Yes, Accept**\. A second confirmation dialog displays; choose **Modify my route tables now** to go directly to the route tables page, or choose **Close** to do this later\.

Now that your VPC peering connection is active, you must add an entry to your VPC route table to enable traffic to be directed to the peer VPC\. For more information, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**To accept a VPC peering connection using the command line or an API**
+ [accept\-vpc\-peering\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/accept-vpc-peering-connection.html) \(AWS CLI\)
+ [Approve\-EC2VpcPeeringConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/Approve-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)
+ [AcceptVpcPeeringConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-AcceptVpcPeeringConnection.html) \(Amazon EC2 Query API\)

## View your VPC peering connections<a name="describe-vpc-peering-connections"></a>

You can view all of your VPC peering connections in the Amazon VPC console\. By default, the console displays all VPC peering connections in different states, including those that may have been recently deleted or rejected\. For more information about the lifecycle of a VPC peering connection, see [VPC peering connection lifecycle](vpc-peering-basics.md#vpc-peering-lifecycle)\.

**To view your VPC peering connections**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**\.

1. All of your VPC peering connections are listed\. Use the filter search bar to narrow your results\.

**To describe a VPC peering connection using the command line or an API**
+ [describe\-vpc\-peering\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpc-peering-connections.html) \(AWS CLI\)
+ [Get\-EC2VpcPeeringConnections](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2VpcPeeringConnections.html) \(AWS Tools for Windows PowerShell\)
+ [DescribeVpcPeeringConnections](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DescribeVpcPeeringConnections.html) \(Amazon EC2 Query API\)