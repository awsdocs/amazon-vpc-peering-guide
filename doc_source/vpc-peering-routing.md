# Updating Your Route Tables for a VPC Peering Connection<a name="vpc-peering-routing"></a>

To send private IPv4 traffic from your instance to an instance in a peer VPC, you must add a route to the route table that's associated with your subnet in which your instance resides\. The route points to the CIDR block \(or portion of the CIDR block\) of the peer VPC in the VPC peering connection\.

Similarly, if the VPCs in the VPC peering connection have associated IPv6 CIDR blocks, you can add a route to your route table to enable communication with the peer VPC over IPv6\. 

If a subnet is not explicitly associated with a route table, it uses the main route table by default\.

You have a [limit](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Appendix_Limits.html) on the number of entries you can add per route table\. If the number of VPC peering connections in your VPC exceeds the route table entry limit for a single route table, consider using multiple subnets that are each associated with a custom route table\. 

The owner of the other VPC in the peering connection must also add a route to their subnet's route table to direct traffic back to your VPC\. For more information about supported route table configurations for VPC peering connections, see [VPC Peering Configurations](peering-configurations.md)\.

You can add a route for a VPC peering connection that's in the `pending-acceptance` state\. However, the route will have a state of `blackhole` and have no effect until the VPC peering connection is in the `active` state\. 

**Warning**  
If you have a VPC peered with multiple VPCs that have overlapping or matching IPv4 CIDR blocks, ensure that your route tables are configured to avoid sending response traffic from your VPC to the incorrect VPC\. AWS currently does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. For more information, see [Routing for Response Traffic](peering-configurations-partial-access.md#peering-incorrect-response-routing)\.

**To add an IPv4 route for a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Route Tables**\.

1. Select the route table that's associated with the subnet in which your instance resides\.
**Note**  
If you do not have a route table associated with that subnet, select the main route table for the VPC, as the subnet then uses this route table by default\. 

1. Choose **Routes**, **Edit**, **Add Route**\.

1. For **Destination**, enter the IPv4 address range to which the network traffic in the VPC peering connection must be directed\. You can specify the entire IPv4 CIDR block of the peer VPC, a specific range, or an individual IPv4 address, such as the IP address of the instance with which to communicate\. For example, if the CIDR block of the peer VPC is `10.0.0.0/16`, you can specify a portion `10.0.0.0/28`, or a specific IP address `10.0.0.7/32`\.

1. Select the VPC peering connection from **Target**, and then choose **Save**\.  
![\[Create VPC peering connection dialog\]](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/images/update-route-table-peering.png)

If both VPCs in the VPC peering connection are in the same region, have IPv6 CIDR blocks, and the resources in the VPC are enabled to use IPv6, you can also add a route for IPv6 communication\.

**To add an IPv6 route for a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Route Tables** and select the route table that's associated with your subnet\.

1. On the **Routes** tab, choose **Edit**, **Add another route**\.

1. For **Destination**, enter the IPv6 address range for the peer VPC\. You can specify the entire IPv6 CIDR block of the peer VPC, a specific range, or an individual IPv6 address\. For example, if the CIDR block of the peer VPC is `2001:db8:1234:1a00::/56`, you can specify a portion `2001:db8:1234:1a00::/64`, or a specific IP address `2001:db8:1234:1a00::123/128`\.

1. Select the VPC peering connection from **Target** and choose **Save**\.

For more information, see [Route Tables](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Route_Tables.html) in the *Amazon VPC User Guide*\.

**To add or replace a route using the command line or an API**
+ [create\-route](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-route.html) \(AWS CLI\)
+ [New\-EC2Route](http://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2Route.html) \(AWS Tools for Windows PowerShell\)
+ [CreateRoute](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateRoute.html) \(Amazon EC2 Query API\)
+ [replace\-route](http://docs.aws.amazon.com/cli/latest/reference/ec2/replace-route.html) \(AWS CLI\)
+ [Set\-EC2Route](http://docs.aws.amazon.com/powershell/latest/reference/items/Set-EC2Route.html) \(AWS Tools for Windows PowerShell\)
+ [ReplaceRoute](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-ReplaceRoute.html) \(Amazon EC2 Query API\)