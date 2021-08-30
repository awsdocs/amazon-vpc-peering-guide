# Modify VPC peering connection options<a name="modify-peering-connections"></a>

You can modify a VPC peering connection to do the following:
+ Enable one or more EC2\-Classic instances that are linked to your VPC via ClassicLink to communicate with instances in the peer VPC, or to enable instances in your VPC to communicate with linked EC2\-Classic instances in the peer VPC\. For more information, see [Configurations with ClassicLink](peering-configurations-classiclink.md)\. You cannot enable EC2\-Classic instances to communicate with instances in a peer VPC over IPv6\.
+ Enable a VPC to resolve public IPv4 DNS hostnames to private IPv4 addresses when queried from instances in the peer VPC\. For more information, see [Enable DNS resolution for a VPC peering connection](#vpc-peering-dns)\.

## Enable DNS resolution for a VPC peering connection<a name="vpc-peering-dns"></a>

To enable a VPC to resolve public IPv4 DNS hostnames to private IPv4 addresses when queried from instances in the peer VPC, you must modify your existing peering connection\.

Both VPCs must be enabled for DNS hostnames and DNS resolution\.

You cannot enable DNS resolution support when you create a new peering connection\. You can enable DNS resolution support for an existing peering connection that's in the `active` state\.

**To enable DNS resolution for a peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **Actions**, **Edit DNS Settings**\.

1. To ensure that queries from the peer VPC resolve to private IP addresses in your local VPC, choose the option to enable DNS resolution for queries from the peer VPC\. This option is **Requester DNS resolution** or **Accepter DNS resolution**, depending on whether the VPC is the requester or accepter VPC\.

1. If the peer VPC is in the same AWS account, you can enable DNS resolution for both VPCs in the peering connection\. 

1. Choose **Save**\.

1. If the peer VPC is in a different AWS account or a different Region, the owner of the peer VPC must sign into the VPC console, perform steps 2 through 4, and choose **Save**\.

**To enable DNS resolution using the command line or an API**
+ [modify\-vpc\-peering\-connection\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpc-peering-connection-options.html) \(AWS CLI\)
+ [Edit\-EC2VpcPeeringConnectionOption](https://docs.aws.amazon.com/powershell/latest/reference/items/Edit-EC2VpcPeeringConnectionOption.html) \(AWS Tools for Windows PowerShell\)
+ [ModifyVpcPeeringConnectionOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-ModifyVpcPeeringConnectionOptions.html) \(Amazon EC2 Query API\)

You must modify the requester VPC peering options if you are the requester of the VPC peering connection, and you must modify the accepter VPC peering options if you are the accepter of the VPC peering connection\. You can use the [describe\-vpc\-peering\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpc-peering-connections.html) or [Get\-EC2VpcPeeringConnections](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2VpcPeeringConnections.html) commands to verify which VPC is the accepter and the requester for a VPC peering connection\. For inter\-Region peering connections, you must use the Region for the requester VPC to modify the requester VPC peering options and the Region for the accepter VPC to modify the accepter VPC peering options\.

In this example, you are the requester of the VPC peering connection, therefore modify the peering connection options using the AWS CLI as follows:

```
aws ec2 modify-vpc-peering-connection-options --vpc-peering-connection-id pcx-aaaabbbb --requester-peering-connection-options AllowDnsResolutionFromRemoteVpc=true
```