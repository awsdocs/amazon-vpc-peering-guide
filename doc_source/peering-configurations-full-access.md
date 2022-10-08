# VPC peering configurations with routes to an entire VPC<a name="peering-configurations-full-access"></a>

You can configure VPC peering connections so that your route tables have access to the entire CIDR block of the peer VPC\. For more information about scenarios in which you might need a specific VPC peering connection configuration, see [VPC peering scenarios](peering-scenarios.md)\. For more information about creating and working with VPC peering connections, see [Work with VPC peering connections](working-with-vpc-peering.md)\.

 For more information about updating your route tables, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**Topics**
+ [Two VPCs peered together](#two-vpcs-full-access)
+ [One VPC peered with two VPCs](#one-to-two-vpcs-full-access)
+ [Three VPCs peered together](#three-vpcs-full-access)
+ [Multiple VPCs peered together](#many-vpcs-full-access)

## Two VPCs peered together<a name="two-vpcs-full-access"></a>

In this configuration, you have a VPC peering connection, pcx\-11112222, between VPC A and VPC B, which are in the same AWS account, and do not have overlapping CIDR blocks\.

![\[Two VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-peered.png)

You might use this configuration when you have two VPCs that require access to each others' resources\. For example, you set up VPC A for your accounting records and VPC B for your financial records, and these each VPC must be able to access resources from the other VPC without restriction\.

**Single VPC CIDR**  
Update the route table for each VPC with a route that sends traffic for the CIDR block of the peer VPC to the VPC peering connection\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

**Multiple IPv4 VPC CIDRs**  
If VPC A and VPC B have multiple associated IPv4 CIDR blocks, you can update the route table for each VPC with routes for some or all of the IPv4 CIDR blocks of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

**IPv4 and IPv6 VPC CIDRs**  
If VPC A and VPC B have associated IPv6 CIDR blocks, you can update the route table for each VPC with routes for both the IPv4 and IPv6 CIDR blocks of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

## One VPC peered with two VPCs<a name="one-to-two-vpcs-full-access"></a>

In this configuration, you have a central VPC \(VPC A\), a VPC peering connection between VPC A and VPC B \(`pcx-12121212`\), and a VPC peering connection between VPC A and VPC C \(`pcx-23232323`\)\. All three VPCs are in the same AWS account, and do not have overlapping CIDR blocks\.

![\[One VPC peered with two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-vpc-peered-to-two.png)

You might use this configuration when you have resources on a central VPC, such as a repository of services, that other VPCs need to access\. The other VPCs do not need access to each others' resources; they only need to access resources in the central VPC\.

**Note**  
Spoke VPCs can't send traffic directly to each other through a hub VPC, because VPC peering does not support transitive peering relationships\. You can create a VPC peering connection between VPC B and VPC C, as shown in [Three VPCs peered together](#three-vpcs-full-access)\. For more information about unsupported peering scenarios, see [VPC peering limitations](vpc-peering-basics.md#vpc-peering-limitations)\.

Update the route table for each VPC as follows to implement this configuration using one CIDR block per VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

You can extend this configuration to additional VPCs\. For example, VPC A is peered with VPC B through VPC G using both IPv4 and IPv6 CIDRs, but the other VPCs are not peered to each other\. In this diagram, the lines represent VPC peering connections\.

![\[One VPC peered with two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-many-vpcs.png)

Update the route table as follows\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

## Three VPCs peered together<a name="three-vpcs-full-access"></a>

In this configuration, you have peered three VPCs together in a full mesh configuration\. The VPCs are in the same AWS account and do not have overlapping CIDR blocks:
+ VPC A is peered to VPC B through VPC peering connection `pcx-aaaabbbb`
+ VPC A is peered to VPC C through VPC peering connection `pcx-aaaacccc`
+ VPC B is peered to VPC C through VPC peering connection `pcx-bbbbcccc`

![\[Three VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/three-vpcs-peered.png)

You might use this full mesh configuration when you have VPCs that need to share resources with each other without restriction\. For example, as a file sharing system\.

Update the route table for each VPC as follows to implement this configuration\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

If VPC A and VPC B have both IPv4 and IPv6 CIDR blocks, but VPC C does not have an IPv6 CIDR block, update the route tables as follows\. Resources in VPC A and VPC B can communicate using IPv6 over the VPC peering connection\. However, VPC C cannot communicate with either VPC A or VPC B using IPv6\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

## Multiple VPCs peered together<a name="many-vpcs-full-access"></a>

In this configuration, you have seven VPCs peered in a full mesh configuration\. The VPCs are in the same AWS account and do not have overlapping CIDR blocks\.


| VPC | VPC | VPC peering connection | 
| --- | --- | --- | 
| A | B | pcx\-aaaabbbb | 
| A | C | pcx\-aaaacccc | 
| A | D | pcx\-aaaadddd | 
| A | E | pcx\-aaaaeeee | 
| A | F | pcx\-aaaaffff | 
| A | G | pcx\-aaaagggg | 
| B | C | pcx\-bbbbcccc | 
| B | D | pcx\-bbbbdddd | 
| B | E | pcx\-bbbbeeee | 
| B | F | pcx\-bbbbffff | 
| B | G | pcx\-bbbbgggg | 
| C | D | pcx\-ccccdddd | 
| C | E | pcx\-cccceeee | 
| C | F | pcx\-ccccffff | 
| C | G | pcx\-ccccgggg | 
| D | E | pcx\-ddddeeee | 
| D | F | pcx\-ddddffff | 
| D | G | pcx\-ddddgggg | 
| E | F | pcx\-eeeeffff | 
| E | G | pcx\-eeeegggg | 
| F | G | pcx\-ffffgggg | 

You might use this full mesh configuration when you have multiple VPCs that must be able to access each others' resources without restriction\. For example, as a file sharing network\. In this diagram, the lines represent VPC peering connections\.

![\[Seven VPCs in a full mesh\]](http://docs.aws.amazon.com/vpc/latest/peering/images/full-mesh.png)

Update the route table for each VPC as follows to implement this configuration\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

If all VPCs have associated IPv6 CIDR blocks, update the route tables as follows\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)