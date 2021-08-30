# What is VPC peering?<a name="what-is-vpc-peering"></a>

[Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) enables you to launch AWS resources into a virtual network that you've defined\.

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses\. Instances in either VPC can communicate with each other as if they are within the same network\. You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account\. The VPCs can be in different regions \(also known as an inter\-region VPC peering connection\)\.

![\[A VPC peering connection\]](http://docs.aws.amazon.com/vpc/latest/peering/images/peering-intro-diagram.png)

AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection, and does not rely on a separate piece of physical hardware\. There is no single point of failure for communication or a bandwidth bottleneck\. 

A VPC peering connection helps you to facilitate the transfer of data\. For example, if you have more than one AWS account, you can peer the VPCs across those accounts to create a file sharing network\. You can also use a VPC peering connection to allow other VPCs to access resources you have in one of your VPCs\. 

You can establish peering relationships between VPCs across different AWS Regions \(also called Inter\-Region VPC Peering\)\. This allows VPC resources including EC2 instances, Amazon RDS databases and Lambda functions that run in different AWS Regions to communicate with each other using private IP addresses, without requiring gateways, VPN connections, or separate network appliances\. The traffic remains in the private IP space\. All inter\-region traffic is encrypted with no single point of failure, or bandwidth bottleneck\. Traffic always stays on the global AWS backbone, and never traverses the public internet, which reduces threats, such as common exploits, and DDoS attacks\. Inter\-Region VPC Peering provides a simple and cost\-effective way to share resources between regions or replicate data for geographic redundancy\. 

For more information, see the following:
+ [VPC peering basics](vpc-peering-basics.md)
+ [Work with VPC peering connections](working-with-vpc-peering.md)
+ [VPC peering scenarios](peering-scenarios.md)
+ [Configurations with routes to an entire CIDR block](peering-configurations-full-access.md)
+ [Configurations with specific routes](peering-configurations-partial-access.md)
+ [Unsupported VPC peering configurations](invalid-peering-configurations.md)