# Configurations with Specific Routes<a name="peering-configurations-partial-access"></a>

You can configure a VPC peering connections to provide access to part of the CIDR block, a specific CIDR block \(if the VPC has multiple CIDR blocks\) or a specific instance within the peer VPC\. In these examples, a central VPC is peered to two or more VPCs that have overlapping CIDR blocks\. For examples of scenarios in which you might need a specific VPC peering connection configuration, see [VPC Peering Scenarios](peering-scenarios.md)\. For more information about creating and working with VPC peering connections, see [Working with VPC Peering Connections](working-with-vpc-peering.md)\. For more information about updating your route tables, see [Updating Your Route Tables for a VPC Peering Connection](vpc-peering-routing.md)\.

**Topics**
+ [Two VPCs Peered to Two Subnets in One VPC](#one-to-two-vpcs-simple-hub)
+ [Two VPCs Peered to a Specific CIDR Block in One VPC](#two-vpcs-peered-specific-cidr)
+ [One VPC Peered to Specific Subnets in Two VPCs](#one-to-two-vpcs-specific-subnets)
+ [Instances in One VPC Peered to Instances in Two VPCs](#one-to-two-vpcs-instances)
+ [One VPC Peered with Two VPCs Using Longest Prefix Match](#one-to-two-vpcs-lpm)
+ [Multiple VPC Configurations](#multiple-configurations)

## Two VPCs Peered to Two Subnets in One VPC<a name="one-to-two-vpcs-simple-hub"></a>

You have a central VPC \(VPC A\), and you have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC A has two subnets: one for each VPC peering connection\. 

![\[Two VPCs peered to two subnets\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-to-two-subnets-one-vpc-diagram.png)

You may want to use this kind of configuration when you have a central VPC with separate sets of resources in different subnets\. Other VPCs may require access to some of the resources, but not all of them\.

The route table for subnet X points to VPC peering connection `pcx-aaaabbbb` to access the entire CIDR block of VPC B\. VPC B's route table points to `pcx-aaaabbbb` to access the CIDR block of only subnet X in VPC A\. Similarly, the route table for subnet Y points to VPC peering connection `pcx-aaaacccc` to access the entire CIDR block of VPC C\. VPC C's route table points to `pcx-aaaacccc` to access the CIDR block of only subnet Y in VPC A\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

Similarly, the central VPC \(VPC A\) can have multiple CIDR blocks, and VPC B and VPC C can have a VPC peering connection to a subnet in each CIDR block\. 

![\[Two VPCs peered to one CIDR block\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-to-multiple-cidr-diagram.png)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

For more information, see [Adding IPv4 CIDR Blocks to a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-resize) in the *Amazon VPC User Guide*\.

### Two VPCs Peered to Two Subnets in One VPC for IPv6<a name="one-to-two-vpcs-simple-hub-ipv6"></a>

You have the same VPC peering configuration as above\. VPC A and VPC B are enabled for IPv6, therefore both VPCs have associated IPv6 CIDR blocks\. Subnet X in VPC A has an associated IPv6 CIDR block\. The VPCs are in the same Region—communication over IPv6 is not supported for inter\-region VPC peering connections\.

![\[Two VPCs peered to two subnets\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-to-two-subnets-one-vpc-ipv6-diagram.png)

You can enable VPC B to communicate with subnet X in VPC A over IPv6 using the VPC peering connection\. To do this, add a route to the route table for VPC A with a destination of the IPv6 CIDR block for VPC B, and a route to the route table for VPC B with a destination of the IPv6 CIDR of subnet X in VPC A\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

## Two VPCs Peered to a Specific CIDR Block in One VPC<a name="two-vpcs-peered-specific-cidr"></a>

You have a central VPC \(VPC A\), and you have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC A has two CIDR blocks; one for each VPC peering connection\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

For more information, see [Adding IPv4 CIDR Blocks to a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-resize) in the *Amazon VPC User Guide*\.

## One VPC Peered to Specific Subnets in Two VPCs<a name="one-to-two-vpcs-specific-subnets"></a>

You have a central VPC \(VPC A\) with one subnet, and you have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC B and VPC C each have two subnets, and only one in each is used for the peering connection with VPC A\. 

![\[One VPC peered with two subnets\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-specific-subnets-diagram.png)

You may want to use this kind of configuration when you have a central VPC that has a single set of resources, such as Active Directory services, that other VPCs need to access\. The central VPC does not require full access to the VPCs that it's peered with\. 

The route table for VPC A points to both VPC peering connections to access only specific subnets in VPC B and VPC C\. The route tables for the subnets in VPC B and VPC C point to their VPC peering connections to access the VPC A subnet\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

### Routing for Response Traffic<a name="peering-incorrect-response-routing"></a>

If you have a VPC peered with multiple VPCs that have overlapping or matching CIDR blocks, ensure that your route tables are configured to avoid sending response traffic from your VPC to the incorrect VPC\. AWS currently does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. 

For example, VPC A is peered with VPC B and VPC C\. VPC B and VPC C have matching CIDR blocks, and their subnets have matching CIDR blocks\. The route table for subnet B in VPC B points to the VPC peering connection `pcx-aaaabbbb` to access the VPC A subnet\. The VPC A route table is configured to send `10.0.0.0/16` traffic to peering connection `pcx-aaaaccccc`\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

![\[Incorrect response routing in peering\]](http://docs.aws.amazon.com/vpc/latest/peering/images/peering-incorrect-response-routing-diagram.png)

An instance in subnet B in VPC B with a private IP address of `10.0.1.66/32` sends traffic to the Active Directory server in VPC A using VPC peering connection `pcx-aaaabbbb`\. VPC A sends the response traffic to `10.0.1.66/32`\. However, the VPC A route table is configured to send all traffic within the `10.0.0.0/16` range of IP addresses to VPC peering connection `pcx-aaaacccc`\. If subnet B in VPC C has an instance with an IP address of `10.0.1.66/32`, it receives the response traffic from VPC A\. The instance in subnet B in VPC B does not receive a response to its request to VPC A\.

To prevent this, you can add a specific route to VPC A's route table with a destination of `10.0.1.0/24` and a target of `pcx-aaaabbbb`\. The route for `10.0.1.0/24` traffic is more specific, therefore traffic destined for the `10.0.1.0/24` IP address range goes via VPC peering connection `pcx-aaaabbbb` 

Alternatively, in the following example, VPC A's route table has a route for each subnet for each VPC peering connection\. VPC A can communicate with subnet B in VPC B and with subnet A in VPC C\. This scenario is useful if you need to add another VPC peering connection with another subnet that falls within the `10.0.0.0/16` IP address range —you can simply add another route for that specific subnet\.


| Destination | Target | 
| --- | --- | 
| 172\.16\.0\.0/16 | Local | 
| 10\.0\.1\.0/24 | pcx\-aaaabbbb | 
| 10\.0\.0\.0/24 | pcx\-aaaacccc | 

Alternatively, depending on your use case, you can create a route to a specific IP address in VPC B to ensure that traffic routed back to the correct server \(the route table uses longest prefix match to prioritize the routes\):


| Destination | Target | 
| --- | --- | 
| 172\.16\.0\.0/16 | Local | 
| 10\.0\.1\.66/32 | pcx\-aaaabbbb | 
| 10\.0\.0\.0/16 | pcx\-aaaacccc | 

## Instances in One VPC Peered to Instances in Two VPCs<a name="one-to-two-vpcs-instances"></a>

You have a central VPC \(VPC A\) with one subnet, and you have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC A has one subnet that has multiple instances; one for each of the VPCs that it's peered with\. You may want to use this kind of configuration to limit peering traffic to specific instances\.

![\[Instances in a VPC peered to instances in two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-instances-diagram.png)

Each VPC route table points to the relevant VPC peering connection to access a single IP address \(and therefore a specific instance\) in the peer VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

## One VPC Peered with Two VPCs Using Longest Prefix Match<a name="one-to-two-vpcs-lpm"></a>

You have a central VPC \(VPC A\) with one subnet, and you have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC B and VPC C have matching CIDR blocks\. You want to use VPC peering connection `pcx-aaaabbbb` to route traffic between VPC A and specific instance in VPC B\. All other traffic destined for the `10.0.0.0/16` IP address range is routed through `pcx-aaaacccc` between VPC A and VPC C\. 

![\[Peering using longest prefix match\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-lpm-diagram.png)

VPC route tables use longest prefix match to select the most specific route across the intended VPC peering connection\. All other traffic is routed through the next matching route, in this case, across the VPC peering connection `pcx-aaaacccc`\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

**Important**  
If an instance other than `10.0.0.77/32` in VPC B sends traffic to VPC A, the response traffic may be routed to VPC C instead of VPC B\. For more information, see [Routing for Response Traffic](#peering-incorrect-response-routing)\.

## Multiple VPC Configurations<a name="multiple-configurations"></a>

In this example, a central VPC \(VPC A\) is peered with multiple VPCs in a spoke configuration\. For more information about this type of configuration, see [One VPC Peered with Multiple VPCs](peering-configurations-full-access.md#one-to-many-vpcs-full-access)\. You also have three VPCs \(VPCs M, N, and P\) peered together in a full mesh configuration\. For more information about this type of configuration, see [Three VPCs Peered Together](peering-configurations-full-access.md#three-vpcs-full-access)\. 

VPC C also has a VPC peering connection with VPC M \(`pcx-ccccmmmm`\)\. VPC A and VPC M have overlapping CIDR blocks\. This means that peering traffic between VPC A and VPC C is limited to a specific subnet \(subnet A\) in VPC C\. This is to ensure that if VPC C receives a request from VPC A or VPC M, it sends the response traffic to the correct VPC\. AWS currently does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. For more information, see [Routing for Response Traffic](#peering-incorrect-response-routing)\.

Similarly, VPC C and VPC P have overlapping CIDR blocks\. Peering traffic between VPC M and VPC C is limited to subnet B in VPC C, and peering traffic between VPC M and VPC P is limited to subnet A in VPC P\. This is to ensure that if VPC M receives peering traffic from VPC C or VPC P, it sends the response traffic back to the correct VPC\. 

![\[Multiple peering configurations\]](http://docs.aws.amazon.com/vpc/latest/peering/images/multiple-configurations-diagram.png)

The route tables for VPCs B, D, E, F, and G point to the relevant peering connections to access the full CIDR block for VPC A, and the VPC A route table points to the relevant peering connections for VPCs B, D, E, F, and G to access their full CIDR blocks\. For peering connection `pcx-aaaacccc`, the VPC A route table routes traffic only to subnet A in VPC C \(`192.168.0.0/24`\) and the subnet A route table in VPC C points to the full CIDR block of VPC A\.

The VPC N route table points to the relevant peering connections to access the full CIDR blocks of VPC M and VPC P, and the VPC P route table points to the relevant peering connection to access the full CIDR block of VPC N\. The subnet A route table in VPC P points to the relevant peering connection to access the full CIDR block of VPC M\. The VPC M route table points to the relevant peering connection to access subnet B in VPC C, and subnet A in VPC P\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)