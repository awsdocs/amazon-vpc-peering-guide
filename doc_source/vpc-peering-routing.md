# Update your route tables for a VPC peering connection<a name="vpc-peering-routing"></a>

To enable private IPv4 traffic between instances in peered VPCs, you must add a route to the route tables associated with the subnets for both instances\. The route destination is the CIDR block \(or portion of the CIDR block\) of the peer VPC and the target is the ID of the VPC peering connection\. For more information, see [Configure route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) in the *Amazon VPC User Guide*\.

The following is an example of the route tables that enables communication between instances in two peered VPCs, VPC A and VPC B\. Each table has a local route and a route that sends traffic for the peer VPC to the VPC peering connection\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-routing.html)

Similarly, if the VPCs in the VPC peering connection have associated IPv6 CIDR blocks, you can add routes that enable communication with the peer VPC over IPv6\.

For more information about supported route table configurations for VPC peering connections, see [VPC peering configurations](peering-configurations.md)\.

**Considerations**
+ If you have a VPC peered with multiple VPCs that have overlapping or matching IPv4 CIDR blocks, ensure that your route tables are configured to avoid sending response traffic from your VPC to the incorrect VPC\. AWS currently does not support unicast reverse path forwarding in VPC peering connections that checks the source IP of packets and routes reply packets back to the source\. For more information, see [Routing for response traffic](peering-configurations-partial-access.md#peering-incorrect-response-routing)\.
+ Your account has a [quota](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) on the number of entries you can add per route table\. If the number of VPC peering connections in your VPC exceeds the route table entry quota for a single route table, consider using multiple subnets that are each associated with a custom route table\.
+ You can add a route for a VPC peering connection that's in the `pending-acceptance` state\. However, the route has a state of `blackhole`, and has no effect until the VPC peering connection is in the `active` state\.

**To add an IPv4 route for a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Route tables**\.

1. Select the check box next to the route table that's associated with the subnet in which your instance resides\.

   If you do not have a route table explicitly associated with that subnet, the main route table for the VPC is implicitly associated with the subnet\.

1. Choose **Actions**, **Edit routes**\.

1. Choose **Add route**\.

1. For **Destination**, enter the IPv4 address range to which the network traffic in the VPC peering connection must be directed\. You can specify the entire IPv4 CIDR block of the peer VPC, a specific range, or an individual IPv4 address, such as the IP address of the instance with which to communicate\. For example, if the CIDR block of the peer VPC is `10.0.0.0/16`, you can specify a portion `10.0.0.0/24`, or a specific IP address `10.0.0.7/32`\.

1. For **Target**, select the VPC peering connection\.

1. Choose **Save changes**\.

The owner of the peer VPC must also complete these steps to add a route to direct traffic back to your VPC through the VPC peering connection\.

If you have resources in different AWS Regions that use IPv6 addresses, you can create an inter\-Region peering connection\. You can then add an IPv6 route for communication between the resources\.

**To add an IPv6 route for a VPC peering connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Route tables**\.

1. Select the check box next to the route table that's associated with the subnet in which your instance resides\.
**Note**  
If you do not have a route table associated with that subnet, select the main route table for the VPC, as the subnet then uses this route table by default\. 

1. Choose **Actions**, **Edit routes**\.

1. Choose **Add route**\.

1. For **Destination**, enter the IPv6 address range for the peer VPC\. You can specify the entire IPv6 CIDR block of the peer VPC, a specific range, or an individual IPv6 address\. For example, if the CIDR block of the peer VPC is `2001:db8:1234:1a00::/56`, you can specify a portion `2001:db8:1234:1a00::/64`, or a specific IP address `2001:db8:1234:1a00::123/128`\.

1. For **Target**, select the VPC peering connection\.

1. Choose **Save changes**\.

For more information, see [Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) in the *Amazon VPC User Guide*\.

**To add or replace a route using the command line or an API**
+ [create\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-route.html) \(AWS CLI\)
+ [New\-EC2Route](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2Route.html) \(AWS Tools for Windows PowerShell\)
+ [CreateRoute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateRoute.html) \(Amazon EC2 Query API\)
+ [replace\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/replace-route.html) \(AWS CLI\)
+ [Set\-EC2Route](https://docs.aws.amazon.com/powershell/latest/reference/items/Set-EC2Route.html) \(AWS Tools for Windows PowerShell\)
+ [ReplaceRoute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-ReplaceRoute.html) \(Amazon EC2 Query API\)