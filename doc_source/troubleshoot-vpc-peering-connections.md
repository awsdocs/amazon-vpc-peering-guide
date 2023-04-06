# Troubleshoot a VPC peering connection<a name="troubleshoot-vpc-peering-connections"></a>

If you're having trouble connecting to a resource in a VPC from a resource in a peer VPC, do the following:
+ For each resource in each VPC, verify that the route table for its subnet contains a route that sends traffic destined for the peer VPC to the VPC peering connection\. For more information, see [Update route tables](vpc-peering-routing.md)\.
+ For EC2 instances, verify that the security groups for the EC2 instances allow traffic from the peer VPC\. For more information, see [Reference peer security groups](vpc-peering-security-groups.md)\.
+ For each resource in each VPC, verify that the network ACL for its subnet allows traffic from the peer VPC\.

You can also use Reachability Analyzer to identify the component with a configuration issue, such as a route table, security group, or network ACL\. For more information, see the [Reachability Analyzer Guide](https://docs.aws.amazon.com/vpc/latest/reachability/)\.