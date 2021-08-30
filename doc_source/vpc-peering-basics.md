# VPC peering basics<a name="vpc-peering-basics"></a>

To establish a VPC peering connection, you do the following:

1. The owner of the *requester VPC* sends a request to the owner of the *accepter VPC* to create the VPC peering connection\. The accepter VPC can be owned by you, or another AWS account, and cannot have a CIDR block that overlaps with the requester VPC's CIDR block\.

1. The owner of the accepter VPC accepts the VPC peering connection request to activate the VPC peering connection\. 

1. To enable the flow of traffic between the VPCs using private IP addresses, the owner of each VPC in the VPC peering connection must manually add a route to one or more of their VPC route tables that points to the IP address range of the other VPC \(the peer VPC\)\. 

1. If required, update the security group rules that are associated with your instance to ensure that traffic to and from the peer VPC is not restricted\. If both VPCs are in the same region, you can reference a security group from the peer VPC as a source or destination for ingress or egress rules in your security group rules\. 

1. By default, if instances on either side of a VPC peering connection address each other using a public DNS hostname, the hostname resolves to the instance's public IP address\. To change this behavior, enable DNS hostname resolution for your VPC connection\. After enabling DNS hostname resolution, if instances on either side of the VPC peering connection address each other using a public DNS hostname, the hostname resolves to the private IP address of the instance\.

For more information, see [Work with VPC peering connections](working-with-vpc-peering.md)\.

## VPC peering connection lifecycle<a name="vpc-peering-lifecycle"></a>

A VPC peering connection goes through various stages starting from when the request is initiated\. At each stage, there may be actions that you can take, and at the end of its lifecycle, the VPC peering connection remains visible in the Amazon VPC console and API or command line output for a period of time\.

![\[VPC peering connection lifecycle\]](http://docs.aws.amazon.com/vpc/latest/peering/images/peering-lifecycle-diagram.png)
+ **Initiating\-request**: A request for a VPC peering connection has been initiated\. At this stage, the peering connection can fail, or can go to `pending-acceptance`\.
+ **Failed**: The request for the VPC peering connection has failed\. While in this state, it cannot be accepted, rejected, or deleted\. The failed VPC peering connection remains visible to the requester for 2 hours\.
+ **Pending\-acceptance**: The VPC peering connection request is awaiting acceptance from the owner of the accepter VPC\. During this state, the owner of the requester VPC can delete the request, and the owner of the accepter VPC can accept or reject the request\. If no action is taken on the request, it expires after 7 days\.
+ **Expired**: The VPC peering connection request has expired, and no action can be taken on it by either VPC owner\. The expired VPC peering connection remains visible to both VPC owners for 2 days\.
+ **Rejected**: The owner of the accepter VPC has rejected a `pending-acceptance` VPC peering connection request\. While in this state, the request cannot be accepted\. The rejected VPC peering connection remains visible to the owner of the requester VPC for 2 days, and visible to the owner of the accepter VPC for 2 hours\. If the request was created within the same AWS account, the rejected request remains visible for 2 hours\.
+ **Provisioning**: The VPC peering connection request has been accepted, and will soon be in the `active` state\. 
+ **Active**: The VPC peering connection is active, and traffic can flow between the VPCs \(provided that your security groups and route tables allow the flow of traffic\)\. While in this state, either of the VPC owners can delete the VPC peering connection, but cannot reject it\. 
**Note**  
If an event in a region in which a VPC resides prevents the flow of traffic, the status of the VPC peering connection remains `Active`\.
+ **Deleting**: Applies to an inter\-region VPC peering connection that is in the process of being deleted\. The owner of either VPC has submitted a request to delete an `active` VPC peering connection, or the owner of the requester VPC has submitted a request to delete a `pending-acceptance` VPC peering connection request\.
+ **Deleted**: An `active` VPC peering connection has been deleted by either of the VPC owners, or a `pending-acceptance` VPC peering connection request has been deleted by the owner of the requester VPC\. While in this state, the VPC peering connection cannot be accepted or rejected\. The VPC peering connection remains visible to the party that deleted it for 2 hours, and visible to the other party for 2 days\. If the VPC peering connection was created within the same AWS account, the deleted request remains visible for 2 hours\.

## Multiple VPC peering connections<a name="vpc-peering-basics-multiple"></a>

A VPC peering connection is a one to one relationship between two VPCs\. You can create multiple VPC peering connections for each VPC that you own, but transitive peering relationships are not supported\. You do not have any peering relationship with VPCs that your VPC is not directly peered with\. 

The following diagram is an example of one VPC peered to two different VPCs\. There are two VPC peering connections: VPC A is peered with both VPC B and VPC C\. VPC B and VPC C are not peered, and you cannot use VPC A as a transit point for peering between VPC B and VPC C\. If you want to enable routing of traffic between VPC B and VPC C, you must create a unique VPC peering connection between them\.

![\[One VPC peered with two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-flying-v.png)

## Pricing for a VPC peering connection<a name="vpc-peering-pricing"></a>

If the VPCs in the VPC peering connection are within the same region, the charges for transferring data within the VPC peering connection are the same as the charges for transferring data across Availability Zones\. If the VPCs are in different regions, inter\-region data transfer costs apply\.

For more information, see [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer)\.

## VPC peering limitations<a name="vpc-peering-limitations"></a>

To create a VPC peering connection with another VPC, be aware of the following limitations and rules:
+ You cannot create a VPC peering connection between VPCs that have matching or overlapping IPv4 or IPv6 CIDR blocks\. Amazon always assigns your VPC a unique IPv6 CIDR block\. If your IPv6 CIDR blocks are unique but your IPv4 blocks are not, you cannot create the peering connection\.
+ You have a quota on the number of active and pending VPC peering connections that you can have per VPC\. For more information, see [Amazon VPC Quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*
+ VPC peering does not support transitive peering relationships\. In a VPC peering connection, your VPC does not have access to any other VPCs with which the peer VPC may be peered\. This includes VPC peering connections that are established entirely within your own AWS account\. For more information about unsupported peering relationships, see [Unsupported VPC peering configurations](invalid-peering-configurations.md)\. For examples of supported peering relationships, see [VPC peering scenarios](peering-scenarios.md)\.
+ You cannot have more than one VPC peering connection between the same two VPCs at the same time\.
+ Unicast reverse path forwarding in VPC peering connections is not supported\. For more information, see [Routing for response traffic](peering-configurations-partial-access.md#peering-incorrect-response-routing)\.
+ You can enable resources on either side of a VPC peering connection to communicate with each other over IPv6; however, IPv6 communication is not automatic\. You must associate an IPv6 CIDR block with each VPC, enable the instances in the VPCs for IPv6 communication, and add routes to your route tables that route IPv6 traffic intended for the peer VPC to the VPC peering connection\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.
+ Any tags that you create for your VPC peering connection are only applied in the account or region in which you create them\.
+ If the IPv4 CIDR block of a VPC in a VPC peering connection falls outside of the private IPv4 address ranges specified by [RFC 1918](http://www.faqs.org/rfcs/rfc1918.html), private DNS hostnames for that VPC cannot be resolved to private IP addresses\. To resolve private DNS hostnames to private IP addresses, you can enable DNS resolution support for the VPC peering connection\. For more information, see [Enable DNS resolution for a VPC peering connection](modify-peering-connections.md#vpc-peering-dns)\.
+ You cannot connect to or query the Amazon DNS server in a peer VPC\.

An inter\-region VPC peering connection has additional limitations:
+ You cannot create a security group rule that references a peer VPC security group\.
+ You cannot enable support for an EC2\-Classic instance that's linked to a VPC via ClassicLink to communicate with the peer VPC\.
+ The Maximum Transmission Unit \(MTU\) across the VPC peering connection is 1500 bytes \(jumbo frames are not supported\)\.
+ You must enable DNS resolution support for the VPC peering connection to resolve private DNS hostnames of the peered VPC to private IP addresses, even if the IPv4 CIDR for the VPC falls into the private IPv4 address ranges specified by RFC 1918\.
+ Inter\-region peering in China is only allowed between the China \(Beijing\) Region, operated by SINNET and the China \(Ningxia\) Region, operated by NWCD\.