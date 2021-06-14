# Configurations with routes to an entire CIDR block<a name="peering-configurations-full-access"></a>

You can configure VPC peering connections so that your route tables have access to the entire CIDR block of the peer VPC\. For more information about scenarios in which you might need a specific VPC peering connection configuration, see [VPC peering scenarios](peering-scenarios.md)\. For more information about creating and working with VPC peering connections in the Amazon VPC console, see [Working with VPC peering connections](working-with-vpc-peering.md)\.

**Topics**
+ [Two VPCs peered together](#two-vpcs-full-access)
+ [One VPC peered with two VPCs](#one-to-two-vpcs-full-access)
+ [Three VPCs peered together](#three-vpcs-full-access)
+ [One VPC peered with multiple VPCs](#one-to-many-vpcs-full-access)
+ [Multiple VPCs peered together](#many-vpcs-full-access)

## Two VPCs peered together<a name="two-vpcs-full-access"></a>

You have a VPC peering connection \(`pcx-11112222`\) between VPC A and VPC B, which are in the same AWS account, and do not have overlapping CIDR blocks\.

![\[Two VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-peered-diagram.png)

You may want to use this kind of configuration when you have two VPCs that require access to each others' resources\. For example, you set up VPC A for your accounting records, and VPC B for your financial records, and now you want each VPC to be able to access each others' resources without restriction\.

The route tables for each VPC point to the relevant VPC peering connection to access the entire CIDR block of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

 For more information about updating your route tables, see [Updating your Route tables for a VPC peering connection](vpc-peering-routing.md)\.

### Two VPCs peered together for IPv6<a name="two-vpcs-full-access-ipv6"></a>

You have the same two VPCs in the VPC peering configuration as above\. In this example, VPC A and VPC B both have associated IPv6 CIDR blocks\. 

![\[Two VPCs with IPv6 blocks peered\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-peered-ipv6-diagram.png)

The route tables for each VPC point to the VPC peering connection to access the entire IPv6 CIDR block of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

For more information about IPv6 in your VPC, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

### Two VPCs with multiple CIDRs peered together<a name="two-vpcs-full-access-multiple-cidrs"></a>

You can add IPv4 CIDR blocks to your VPC\. In this example, VPC A and VPC B have multiple IPv4 CIDR blocks\.

![\[Two VPCs with multiple CIDR blocks peered\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-peered-multiple-cidrs-diagram.png)

The route tables for each VPC point to the VPC peering connection to access all the IPv4 CIDR blocks of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

For more information, see [Adding IPv4 CIDR Blocks to a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-resize) in the *Amazon VPC User Guide*\.

## One VPC peered with two VPCs<a name="one-to-two-vpcs-full-access"></a>

You have a central VPC \(VPC A\), and you have a VPC peering connection between VPC A and VPC B \(`pcx-12121212`\), and between VPC A and VPC C \(`pcx-23232323`\)\. The VPCs are in the same AWS account, and do not have overlapping CIDR blocks\. 

![\[One VPC peered with two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-diagram.png)

You may want to use this 'flying V' configuration when you have resources on a central VPC, such as a repository of services, that other VPCs need to access\. The other VPCs do not need access to each others' resources; they only need access to resources on the central VPC\.

**Note**  
VPC B and VPC C cannot send traffic directly to each other through VPC A\. VPC peering does not support transitive peering relationships, nor edge to edge routing\. You must create a VPC peering connection between VPC B and VPC C in order to route traffic directly between them\. For more information, see [Three VPCs peered together](#three-vpcs-full-access)\. For more information about unsupported peering scenarios, see [Unsupported VPC peering configurations](invalid-peering-configurations.md)\.

The route tables for each VPC point to the relevant VPC peering connection to access the entire CIDR block of the peer VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

 For more information about updating your route tables, see [Updating your Route tables for a VPC peering connection](vpc-peering-routing.md)\.

### One VPC peered with two VPCs for IPv6<a name="one-to-two-vpcs-full-access-ipv6"></a>

You have the same three VPCs in the VPC peering configuration as above\. In this example, all three VPCs have associated IPv6 CIDR blocks\.

![\[One VPC peered to two\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-ipv6-diagram.png)

The route tables for each VPC point to the VPC peering connection to access the entire IPv6 CIDR block of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

## Three VPCs peered together<a name="three-vpcs-full-access"></a>

You have peered three VPCs together in a full mesh configuration\. The VPCs are in the same AWS account and do not have overlapping CIDR blocks:
+ VPC A is peered to VPC B through VPC peering connection `pcx-aaaabbbb`
+ VPC A is peered to VPC C through VPC peering connection `pcx-aaaacccc`
+ VPC B is peered to VPC C through VPC peering connection `pcx-bbbbcccc`

![\[Three VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/three-vpcs-peered-diagram.png)

You may want to use this full mesh configuration when you have separate VPCs that need to share resources with each other without restriction; for example, as a file sharing system\.

The route tables for each VPC point to the relevant VPC peering connection to access the entire CIDR block of the peer VPCs\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

 For more information about updating your route tables, see [Updating your Route tables for a VPC peering connection](vpc-peering-routing.md)\.

### Three VPCs peered for IPv6<a name="three-vpcs-full-access-ipv6"></a>

You have the same three VPCs in the VPC peering configuration as above\. In this example, VPC A and VPC B both have associated IPv6 CIDR blocks\. VPC C does not have an associated IPv6 CIDR block\. 

![\[Three VPCs peered with IPv6\]](http://docs.aws.amazon.com/vpc/latest/peering/images/three-vpcs-peered-ipv6-diagram.png)

The route tables for VPC A and VPC B include routes that point to VPC peering connection `pcx-aaaabbbb` to access the entire IPv6 CIDR block of the peer VPC\. VPC A and VPC B can communicate using IPv6 over the VPC peering connection\. VPC C cannot communicate using IPv6 with either VPC A or VPC B\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

The owner of VPC C associates an IPv6 CIDR block with the VPC \(`2001:db8:1234:cc00::/56`\)\. VPC C can now communicate over IPv6 with both VPC A and VPC B using the existing VPC peering connection\. To enable this, the following routes must be added to the existing route tables:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

For more information about IPv6 in your VPC, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

## One VPC peered with multiple VPCs<a name="one-to-many-vpcs-full-access"></a>

You have a central VPC \(VPC A\) that's peered to the following VPCs:
+ VPC B through `pcx-aaaabbbb`
+ VPC C through `pcx-aaaacccc`
+ VPC D through `pcx-aaaadddd`
+ VPC E through `pcx-aaaaeeee`
+ VPC F through `pcx-aaaaffff`
+ VPC G through `pcx-aaaagggg`

VPC A is peered with all other VPCs, but the other VPCs are not peered to each other\. The VPCs are in the same AWS account and do not have overlapping CIDR blocks\. 

**Note**  
None of the other VPCs can send traffic directly to each other through VPC A\. VPC peering does not support transitive peering relationships, nor edge to edge routing\. You must create a VPC peering connection between the other VPCs in order to route traffic between them\. For more information, see [Multiple VPCs peered together](#many-vpcs-full-access)\. For more information about unsupported peering scenarios, see [Unsupported VPC peering configurations](invalid-peering-configurations.md)\.

![\[One VPC peered to many VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-many-vpcs-diagram.png)

You may want to use this spoke configuration when you have resources on a central VPC, such as a repository of services, that other VPCs need to access\. The other VPCs do not need access to each others' resources; they only need access to resources on the central VPC\.

The route tables for each VPC point to the relevant VPC peering connection to access the entire CIDR block of the peer VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

 For more information about updating your route tables, see [Updating your Route tables for a VPC peering connection](vpc-peering-routing.md)\.

### One VPC peered with multiple VPCs for IPv6<a name="one-to-many-vpcs-full-access-ipv6"></a>

You have the same VPCs in the VPC peering configuration as above\. All VPCs have associated IPv6 CIDR blocks\.

![\[One VPC peered to many\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-many-vpcs-ipv6-diagram.png)

The route tables for each VPC point to the relevant VPC peering connection to access the entire IPv6 CIDR block of the peer VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

## Multiple VPCs peered together<a name="many-vpcs-full-access"></a>

You have peered seven VPCs together in a full mesh configuration:


| VPCs | VPC peering connection | 
| --- | --- | 
| A and B | pcx\-aaaabbbb | 
| A and C | pcx\-aaaacccc | 
| A and D | pcx\-aaaadddd | 
| A and E | pcx\-aaaaeeee | 
| A and F | pcx\-aaaaffff | 
| A and G | pcx\-aaaagggg | 
| B and C | pcx\-bbbbcccc | 
| B and D | pcx\-bbbbdddd | 
| B and E | pcx\-bbbbeeee | 
| B and F | pcx\-bbbbffff | 
| B and G | pcx\-bbbbgggg | 
| C and D | pcx\-ccccdddd | 
| C and E | pcx\-cccceeee | 
| C and F | pcx\-ccccffff | 
| C and G | pcx\-ccccgggg | 
| D and E | pcx\-ddddeeee | 
| D and F | pcx\-ddddffff | 
| D and G | pcx\-ddddgggg | 
| E and F | pcx\-eeeeffff | 
| E and G | pcx\-eeeegggg | 
| F and G | pcx\-ffffgggg | 

The VPCs are in the same AWS account and do not have overlapping CIDR blocks\. 

![\[Many VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/many-vpcs-peered-diagram.png)

You may want to use this full mesh configuration when you have multiple VPCs that must be able to access each others' resources without restriction; for example, as a file sharing network\. 

The route tables for each VPC point to the relevant VPC peering connection to access the entire CIDR block of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)

 For more information about updating route tables, see [Updating your Route tables for a VPC peering connection](vpc-peering-routing.md)\.

### Multiple VPCs peered together for IPv6<a name="many-vpcs-full-access-ipv6"></a>

You have the same VPCs in the VPC peering configuration as above\. All VPCs have associated IPv6 CIDR blocks\.

![\[Many VPCs peered together\]](http://docs.aws.amazon.com/vpc/latest/peering/images/many-vpcs-peered-ipv6-diagram.png)

The route tables for each VPC point to the VPC peering connection to access the entire IPv6 CIDR block of the peer VPC\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html)