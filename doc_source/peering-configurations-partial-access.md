# VPC peering configurations with specific routes<a name="peering-configurations-partial-access"></a>

You can configure route tables for a VPC peering connection to restrict access to a subnet CIDR block, a specific CIDR block \(if the VPC has multiple CIDR blocks\), or a specific resource in the peer VPC\. In these examples, a central VPC is peered to at least two VPCs that have overlapping CIDR blocks\.

For examples of scenarios in which you might need a specific VPC peering connection configuration, see [VPC peering scenarios](peering-scenarios.md)\. For more information about working with VPC peering connections, see [Work with VPC peering connections](working-with-vpc-peering.md)\. For more information about updating your route tables, see [Update your route tables for a VPC peering connection](vpc-peering-routing.md)\.

**Topics**
+ [Two VPCs that access specific subnets in one VPC](#one-to-two-vpcs-simple-hub)
+ [Two VPCs that access specific CIDR blocks in one VPC](#two-vpcs-peered-specific-cidr)
+ [One VPC that accesses specific subnets in two VPCs](#one-to-two-vpcs-specific-subnets)
+ [Instances in one VPC that access specific instances in two VPCs](#one-to-two-vpcs-instances)
+ [One VPC that accesses two VPCs using longest prefix matches](#one-to-two-vpcs-lpm)
+ [Multiple VPC configurations](#multiple-configurations)

## Two VPCs that access specific subnets in one VPC<a name="one-to-two-vpcs-simple-hub"></a>

In this configuration, there is a central VPC with two subnets \(VPC A\), a peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and a peering connection between VPC A and VPC C \(`pcx-aaaacccc`\)\. Each VPC requires access to the resources in only one of the subnets in VPC A\.

![\[Two VPCs peered to two subnets in one VPC\]](http://docs.aws.amazon.com/vpc/latest/peering/images/two-vpcs-to-two-subnets-one-vpc.png)

The route table for subnet 1 uses VPC peering connection `pcx-aaaabbbb` to access the entire CIDR block of VPC B\. The route table for VPC B uses `pcx-aaaabbbb` to access the CIDR block of subnet 1 in VPC A\. The route table for subnet 2 uses VPC peering connection `pcx-aaaacccc` to access the entire CIDR block of VPC C\. The route table for VPC C table uses `pcx-aaaacccc` to access the CIDR block of subnet 2 in VPC A\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

You can extend this configuration to multiple CIDR blocks\. Suppose that VPC A and VPC B have both IPv4 and IPv6 CIDR blocks, and that subnet 1 has an associated IPv6 CIDR block\. You can enable VPC B to communicate with subnet 1 in VPC A over IPv6 using the VPC peering connection\. To do this, add a route to the route table for VPC A with a destination of the IPv6 CIDR block for VPC B, and a route to the route table for VPC B with a destination of the IPv6 CIDR of subnet 1 in VPC A\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

## Two VPCs that access specific CIDR blocks in one VPC<a name="two-vpcs-peered-specific-cidr"></a>

In this configuration, there is a central VPC \(VPC A\), a peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and a peering connection between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC A has one CIDR block for each peering connection\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

## One VPC that accesses specific subnets in two VPCs<a name="one-to-two-vpcs-specific-subnets"></a>

In this configuration, there is a central VPC \(VPC A\) with one subnet, a peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and a peering connection between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC B and VPC C each have two subnets\. The peering connection between VPC A and VPC B uses only one of the subnets in VPC B\. The peering connection between VPC A and VPC C uses only one of the subnets in VPC C\.

![\[One VPC peered with two subnets\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-specific-subnets.png)

Use this configuration when you have a central VPC that has a single set of resources, such as Active Directory services, that other VPCs need to access\. The central VPC does not require full access to the VPCs that it's peered with\. 

The route table for VPC A uses the peering connections to access only specific subnets in the peered VPCs\. The route table for subnet 1 uses the peering connection with VPC A to access the subnet in VPC A\. The route table for subnet 2 uses the peering connection with VPC A to access the subnet in VPC A\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

### Routing for response traffic<a name="peering-incorrect-response-routing"></a>

If you have a VPC peered with multiple VPCs that have overlapping or matching CIDR blocks, ensure that your route tables are configured to avoid sending response traffic from your VPC to the incorrect VPC\. AWS does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. 

For example, VPC A is peered with VPC B and VPC C\. VPC B and VPC C have matching CIDR blocks, and their subnets have matching CIDR blocks\. The route table for subnet 2 in VPC B points to the VPC peering connection `pcx-aaaabbbb` to access the VPC A subnet\. The VPC A route table is configured to send traffic destined for the VPC CIDR to peering connection `pcx-aaaaccccc`\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

Suppose that an instance in subnet 2 in VPC B sends traffic to the Active Directory server in VPC A using VPC peering connection `pcx-aaaabbbb`\. VPC A sends the response traffic to Active Directory server\. However, the VPC A route table is configured to send all traffic within the VPC CIDR range to VPC peering connection `pcx-aaaacccc`\. If subnet 2 in VPC C has an instance with the same IP address as the instance in subnet two of VPC B, it receives the response traffic from VPC A\. The instance in subnet 2 in VPC B does not receive a response to its request to VPC A\.

To prevent this, you can add a specific route to the VPC A route table with the CIDR of subnet 2 in VPC B as the destination and a target of `pcx-aaaabbbb`\. The new route is more specific, therefore traffic destined for the subnet 2 CIDR is routed to the VPC peering connection `pcx-aaaabbbb` 

Alternatively, in the following example, the VPC A route table has a route for each subnet for each VPC peering connection\. VPC A can communicate with subnet B in VPC B and with subnet A in VPC C\. This scenario is useful if you need to add another VPC peering connection with another subnet that falls within the same address range as VPC B and VPC C —you can simply add another route for that specific subnet\.


| Destination | Target | 
| --- | --- | 
| VPC A CIDR | Local | 
| Subnet 2 CIDR | pcx\-aaaabbbb | 
| Subnet 1 CIDR | pcx\-aaaacccc | 

Alternatively, depending on your use case, you can create a route to a specific IP address in VPC B to ensure that traffic routed back to the correct server \(the route table uses longest prefix match to prioritize the routes\):


| Destination | Target | 
| --- | --- | 
| VPC A CIDR | Local | 
| Specific IP address in subnet 2 | pcx\-aaaabbbb | 
| VPC B CIDR | pcx\-aaaacccc | 

## Instances in one VPC that access specific instances in two VPCs<a name="one-to-two-vpcs-instances"></a>

In this configuration, there is a central VPC \(VPC A\) with one subnet, a peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and a peering connection between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC A has a subnet with one instance for each peering connection\. You can use this configuration to limit peering traffic to specific instances\.

![\[Instances in a VPC peered to instances in two VPCs\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-instances.png)

Each VPC route table points to the relevant VPC peering connection to access a single IP address \(and therefore a specific instance\) in the peer VPC\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

## One VPC that accesses two VPCs using longest prefix matches<a name="one-to-two-vpcs-lpm"></a>

In this configuration, there is a central VPC \(VPC A\) with one subnet, a peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and a peering connection between VPC A and VPC C \(`pcx-aaaacccc`\)\. VPC B and VPC C have matching CIDR blocks\. You use VPC peering connection `pcx-aaaabbbb` to route traffic between VPC A and a specific instance in VPC B\. All other traffic destined for the CIDR address range shared by VPC B and VPC C is routed to VPC C through `pcx-aaaacccc`\.

![\[Peering using the longest prefix match\]](http://docs.aws.amazon.com/vpc/latest/peering/images/one-to-two-vpcs-longest-prefix.png)

VPC route tables use longest prefix match to select the most specific route across the intended VPC peering connection\. All other traffic is routed through the next matching route, in this case, across the VPC peering connection `pcx-aaaacccc`\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)

**Important**  
If an instance other than instance X in VPC B sends traffic to VPC A, the response traffic might be routed to VPC C instead of VPC B\. For more information, see [Routing for response traffic](#peering-incorrect-response-routing)\.

## Multiple VPC configurations<a name="multiple-configurations"></a>

In this configuration, there is a central VPC \(VPC A\) is peered with multiple VPCs in a spoke configuration\. You also have three VPCs \(VPCs X, Y, and Z\) peered in a full mesh configuration\.

VPC D also has a VPC peering connection with VPC X \(`pcx-ddddxxxx`\)\. VPC A and VPC X have overlapping CIDR blocks\. This means that peering traffic between VPC A and VPC D is limited to a specific subnet \(subnet 2\) in VPC D\. This is to ensure that if VPC D receives a request from VPC A or VPC X, it sends the response traffic to the correct VPC\. AWS does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. For more information, see [Routing for response traffic](#peering-incorrect-response-routing)\.

Similarly, VPC D and VPC Z have overlapping CIDR blocks\. Peering traffic between VPC D and VPC X is limited to subnet 2 in VPC D, and peering traffic between VPC X and VPC Z is limited to subnet 1 in VPC Z\. This is to ensure that if VPC X receives peering traffic from VPC D or VPC Z, it sends the response traffic back to the correct VPC\. 

![\[Multiple peering configurations\]](http://docs.aws.amazon.com/vpc/latest/peering/images/multiple-configurations.png)

The route tables for VPCs B, C, E, F, and G point to the relevant peering connections to access the full CIDR block for VPC A, and the VPC A route table points to the relevant peering connections for VPCs B, C, E, F, and G to access their full CIDR blocks\. For peering connection `pcx-aaaadddd`, the VPC A route table routes traffic only to subnet 1 in VPC D and the subnet 1 route table in VPC D points to the full CIDR block of VPC A\.

The VPC Y route table points to the relevant peering connections to access the full CIDR blocks of VPC X and VPC Z, and the VPC Z route table points to the relevant peering connection to access the full CIDR block of VPC Y\. The subnet 1 route table in VPC Z points to the relevant peering connection to access the full CIDR block of VPC Y\. The VPC X route table points to the relevant peering connection to access subnet 2 in VPC D and subnet 1 in VPC Z\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-partial-access.html)