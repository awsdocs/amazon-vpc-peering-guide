# What is VPC Peering?<a name="Welcome"></a>

[Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) \(Amazon VPC\) enables you to launch Amazon Web Services \(AWS\) resources into a virtual network that you've defined\. 

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses\. Instances in either VPC can communicate with each other as if they are within the same network\. You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account\. The VPCs can be in different regions \(also known as an *inter\-region* VPC peering connection\)\.

![\[A VPC peering connection\]](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/images/peering-intro-diagram.png)

AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection, and does not rely on a separate piece of physical hardware\. There is no single point of failure for communication or a bandwidth bottleneck\. 

A VPC peering connection helps you to facilitate the transfer of data\. For example, if you have more than one AWS account, you can peer the VPCs across those accounts to create a file sharing network\. You can also use a VPC peering connection to allow other VPCs to access resources you have in one of your VPCs\. 

For more information, see the following topics:

+ [VPC Peering Basics](vpc-peering-basics.md)

+ [Working with VPC Peering Connections](working-with-vpc-peering.md)

+ [VPC Peering Scenarios](peering-scenarios.md)

+ [Configurations with Routes to an Entire CIDR Block](peering-configurations-full-access.md)

+ [Configurations with Specific Routes](peering-configurations-partial-access.md)

+ [Invalid VPC Peering Connection Configurations](invalid-peering-configurations.md)