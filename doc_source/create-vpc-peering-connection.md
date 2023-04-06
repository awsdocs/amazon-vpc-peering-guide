# Create a VPC peering connection<a name="create-vpc-peering-connection"></a>

To create a VPC peering connection, first create a request to peer with another VPC\. You can request a VPC peering connection with another VPC in your account, or with a VPC in a different AWS account\. For an inter\-Region VPC peering connection where the VPCs are in different Regions, the request must be made from the Region of the requester VPC\.

To activate the request, the owner of the accepter VPC must accept the request\. For an inter\-Region VPC peering connection, the request must be accepted in the Region of the accepter VPC\. For more information, see [Accept a VPC peering connection](accept-vpc-peering-connection.md)\. For more information about the `Pending acceptance` peering connection status, see [VPC peering connection lifecycle](vpc-peering-basics.md#vpc-peering-lifecycle)\.

**Topics**
+ [Prerequisites](#vpc-peering-connection-prerequisites)
+ [Create with VPCs in the same account and Region](#same-account-same-region)
+ [Create with VPCs in the same account and different Regions](#same-account-different-region)
+ [Create with VPCs in different accounts and the same Region](#different-account-same-region)
+ [Create with VPCs in different accounts and Regions](#different-account-different-region)
+ [Create a VPC peering connection using the command line](#create-vpc-peering-connection-command-line)

## Prerequisites<a name="vpc-peering-connection-prerequisites"></a>
+ Review the [limitations and rules](vpc-peering-basics.md#vpc-peering-limitations) for VPC peering connections\.
+ Ensure that your VPCs do not have overlapping IPv4 CIDR blocks\. If they overlap, the status of the VPC peering connection immediately goes to `failed`\. This limitation applies even if the VPCs have unique IPv6 CIDR blocks\.

## Create with VPCs in the same account and Region<a name="same-account-same-region"></a>

**To create a VPC peering connection with VPCs in the same account and Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering connections**\.

1. Choose **Create peering connection**\.

1. Configure the following information, and choose **Create peering connection** when you are done:
   + **Name**: You can optionally name your VPC peering connection\. 
   + **VPC ID \(Requester\)**: Select the VPC in your account with which you want to create the VPC peering connection\. 
   + For **Select another VPC to peer with**, choose **My account** and select another of your VPCs\.
   + \(Optional\) To add a tag, choose **Add new tag** and enter the tag key and value\.

1. Choose **Actions**, **Accept request**\.

1. When prompted for confirmation, choose **Accept request**\.

1. Choose **Modify my route tables now** to add a route to the VPC route table so that you can send and receive traffic across the peering connection\.

## Create with VPCs in the same account and different Regions<a name="same-account-different-region"></a>

**To create a VPC peering connection with VPCs in the same account and different Regions**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering connections**\.

1. Choose **Create peering connection**\.

1. Configure the following information, and choose **Create peering connection** when you are done:
   + **Name**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and the value that you specify\. 
   + **VPC ID \(Requester\)**: Select the requester VPC in your account with which to request the VPC peering connection\.
   + **Account**: Choose **My account**\.
   + **Region**: Choose **Another region** and select the Region ifor the accepter VPC\.
   + **VPC ID \(Accepter\)**: Select the accepter VPC\.

1. In the Region selector, select the Region of the accepter VPC\.

1. In the navigation pane, choose **Peering connections**\. Select the VPC peering connection that you created, and choose **Actions**, **Accept request**\.

1. When prompted for confirmation, choose **Accept request**\.

1. Choose **Modify my route tables now** to add a route to the VPC route table so that you can send and receive traffic across the peering connection\.

## Create with VPCs in different accounts and the same Region<a name="different-account-same-region"></a>

**To request a VPC peering connection with VPCs in different accounts and the same Region**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering connections**\.

1. Choose **Create peering connection**\.

1. Configure the information as follows, and choose **Create peering connection** when you are done:
   + **Name**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and a value that you specify\. This tag is only visible to you; the owner of the peer VPC can create their own tags for the VPC peering connection\.
   + **VPC ID \(Requester\)**: Select the VPC in your account with which to create the VPC peering connection\. 
   + **Account**: Choose **Another account**\.
   + **Account ID**: Enter the ID of the AWS account that owns the accepter VPC\.
   + **VPC ID \(Accepter\)**: Enter the ID of the VPC with which to create the VPC peering connection\.

## Create with VPCs in different accounts and Regions<a name="different-account-different-region"></a>

**To request a VPC peering connection with VPCs in different accounts and Regions**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering connections**\.

1. Choose **Create peering connection**\.

1. Configure the information as follows, and choose **Create peering connection** when you are done:
   + **Name**: You can optionally name your VPC peering connection\. Doing so creates a tag with a key of `Name` and a value that you specify\. This tag is only visible to you; the owner of the peer VPC can create their own tags for the VPC peering connection\.
   + **VPC ID \(Requester\)**: Select the VPC in your account with which to create the VPC peering connection\. 
   + **Account**: Choose **Another account**\.
   + **Account ID**: Enter the ID of the AWS account that owns the accepter VPC\.
   + **Region**: Choose **Another region** and select the Region in which the accepter VPC resides\.
   + **VPC ID \(Accepter\)**: Enter the ID of the VPC with which to create the VPC peering connection\.

## Create a VPC peering connection using the command line<a name="create-vpc-peering-connection-command-line"></a>

You can create a VPC peering connection using the following commands:
+ [create\-vpc\-peering\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpc-peering-connection.html) \(AWS CLI\)
+ [New\-EC2VpcPeeringConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2VpcPeeringConnection.html) \(AWS Tools for Windows PowerShell\)