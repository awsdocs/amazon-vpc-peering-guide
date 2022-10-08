# What is VPC peering?<a name="what-is-vpc-peering"></a>

A *virtual private cloud* \(VPC\) is a virtual network dedicated to your AWS account\. It is logically isolated from other virtual networks in the AWS Cloud\. You can launch AWS resources, such as Amazon EC2 instances, into your VPC\.

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses\. Instances in either VPC can communicate with each other as if they are within the same network\. You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account\. The VPCs can be in different Regions \(also known as an inter\-Region VPC peering connection\)\.

![\[A VPC peering connection\]](http://docs.aws.amazon.com/vpc/latest/peering/images/peering-intro-diagram.png)

AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection, and does not rely on a separate piece of physical hardware\. There is no single point of failure for communication or a bandwidth bottleneck\. 

A VPC peering connection helps you to facilitate the transfer of data\. For example, if you have more than one AWS account, you can peer the VPCs across those accounts to create a file sharing network\. You can also use a VPC peering connection to allow other VPCs to access resources you have in one of your VPCs\.

When you establish peering relationships between VPCs across different AWS Regions, resources in the VPCs \(for example, EC2 instances and Lambda functions\) in different AWS Regions can communicate with each other using private IP addresses, without using a gateway, VPN connection, or network appliance\. The traffic remains in the private IP space\. All inter\-Region traffic is encrypted with no single point of failure, or bandwidth bottleneck\. Traffic always stays on the global AWS backbone, and never traverses the public internet, which reduces threats, such as common exploits, and DDoS attacks\. Inter\-Region VPC peering provides a simple and cost\-effective way to share resources between regions or replicate data for geographic redundancy\. 

## Pricing for a VPC peering connection<a name="vpc-peering-pricing"></a>

There is no charge to create a VPC peering connection\. There is a charge for data transfer across peering connections\. For more information, see [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer)\.