# Unsupported VPC peering configurations<a name="invalid-peering-configurations"></a>

The following VPC peering connection configurations are not supported\. In some cases you can use a transit gateway attachment in place of the peering connection\. For more information, see [Examples](https://docs.aws.amazon.com/vpc/latest/tgw/TGW_Scenarios.html) in *Amazon VPC Transit Gateways*\.

For more information about VPC peering limitations, see [VPC peering limitations](vpc-peering-basics.md#vpc-peering-limitations)\.

**Topics**
+ [Overlapping CIDR blocks](#overlapping-cidr)
+ [Transitive peering](#transitive-peering)
+ [Edge to edge routing through a gateway or private connection](#edge-to-edge-vgw)

## Overlapping CIDR blocks<a name="overlapping-cidr"></a>

You cannot create a VPC peering connection between VPCs with matching or overlapping IPv4 CIDR blocks\.

![\[VPCs with matching IPv4 CIDR blocks\]](http://docs.aws.amazon.com/vpc/latest/peering/images/overlapping-cidrs-diagram.png)

If the VPCs have multiple IPv4 CIDR blocks, you cannot create a VPC peering connection if any of the CIDR blocks overlap \(regardless of whether you intend to use the VPC peering connection for communication between the non\-overlapping CIDR blocks only\)\.

![\[VPCs with overlapping IPv4 CIDR blocks\]](http://docs.aws.amazon.com/vpc/latest/peering/images/overlapping-multiple-cidrs-diagram.png)

This limitation also applies to VPCs that have non\-overlapping IPv6 CIDR blocks\. Even if you intend to use the VPC peering connection for IPv6 communication only, you cannot create a VPC peering connection if the VPCs have matching or overlapping IPv4 CIDR blocks\.

![\[VPCs with matching IPv4 CIDR blocks\]](http://docs.aws.amazon.com/vpc/latest/peering/images/overlapping-cidrs-ipv6-diagram.png)

## Transitive peering<a name="transitive-peering"></a>

Instead of using VPC peering, you can use an AWS Transit Gateway that acts as a network transit hub, to interconnect your VPCs and on\-premises networks\. For more information about transit gateways, see [What is a Transit Gateway](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html) in *Amazon VPC Transit Gateways*\.

You have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\), and between VPC A and VPC C \(`pcx-aaaacccc`\)\. There is no VPC peering connection between VPC B and VPC C\. You cannot route packets directly from VPC B to VPC C through VPC A\. 

![\[Transitive peering\]](http://docs.aws.amazon.com/vpc/latest/peering/images/transitive-peering-diagram.png)

To route packets directly between VPC B and VPC C, you can create a separate VPC peering connection between them \(provided they do not have overlapping CIDR blocks\)\. For more information, see [Three VPCs peered together](peering-configurations-full-access.md#three-vpcs-full-access)\.

## Edge to edge routing through a gateway or private connection<a name="edge-to-edge-vgw"></a>

If either VPC in a peering relationship has one of the following connections, you cannot extend the peering relationship to that connection:
+ A VPN connection or an AWS Direct Connect connection to a corporate network
+ An internet connection through an internet gateway
+ An internet connection in a private subnet through a NAT device
+ A gateway VPC endpoint to an AWS service; for example, an endpoint to Amazon S3\.
+ \(IPv6\) A ClassicLink connection\. You can enable IPv4 communication between a linked EC2\-Classic instance and instances in a VPC on the other side of a VPC peering connection\. However, IPv6 is not supported in EC2\-Classic, so you cannot extend this connection for IPv6 communication\.

For example, if VPC A and VPC B are peered, and VPC A has any of these connections, then instances in VPC B cannot use the connection to access resources on the other side of the connection\. Similarly, resources on the other side of a connection cannot use the connection to access VPC B\.

**Example: Edge to edge routing through a VPN connection or an AWS Direct Connect connection**

You have a VPC peering connection between VPC A and VPC B\. VPC A also has a Site\-to\-Site VPN connection or an AWS Direct Connect connection to a corporate network\. Edge to edge routing is not supported; you cannot use VPC A to extend the peering relationship to exist between VPC B and the corporate network\. For example, traffic from the corporate network can’t directly access VPC B by using the VPN connection or the AWS Direct Connect connection to VPC A\.

![\[Edge to edge routing through a Site-to-Site VPN connection\]](http://docs.aws.amazon.com/vpc/latest/peering/images/edge-to-edge-vpn-diagram.png)

**Example: Edge to edge routing through an internet gateway**

You have a VPC peering connection between VPC A and VPC B \(`pcx-abababab`\)\. VPC A has an internet gateway; VPC B does not\. Edge to edge routing is not supported; you cannot use VPC A to extend the peering relationship to exist between VPC B and the internet\. For example, traffic from the internet can’t directly access VPC B by using the internet gateway connection to VPC A\.

![\[Edge to edge routing through an internet gateway\]](http://docs.aws.amazon.com/vpc/latest/peering/images/edge-to-edge-igw-diagram.png)

Similarly, if VPC A has a NAT device that provides internet access to instances in private subnets in VPC A, instances in VPC B cannot use the NAT device to access the internet\.

**Example: Edge to edge routing through a VPC gateway endpoint**

You have a VPC peering connection between VPC A and VPC B \(`pcx-aaaabbbb`\)\. VPC A has a VPC gateway endpoint that connects it to Amazon S3\. Edge to edge routing is not supported; you cannot use VPC A to extend the peering relationship to exist between VPC B and Amazon S3\. For example, VPC B can't directly access Amazon S3 using the VPC gateway endpoint connection to VPC A\.

![\[Edge to edge routing through a VPC endpoint\]](http://docs.aws.amazon.com/vpc/latest/peering/images/edge-to-edge-s3-diagram.png)