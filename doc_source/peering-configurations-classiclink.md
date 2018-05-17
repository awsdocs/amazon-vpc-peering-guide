# Configurations with ClassicLink<a name="peering-configurations-classiclink"></a>

If you have a VPC peering connection between two VPCs, and there are one or more EC2\-Classic instances that are linked to one or both of the VPCs via ClassicLink, you can extend the VPC peering connection to enable communication between the EC2\-Classic instances and the instances in the VPC on the other side of the VPC peering connection\. This enables the EC2\-Classic instances and the instances in the VPC to communicate using private IP addresses\. To do this, you enable a local VPC to communicate with a linked EC2\-Classic instance in a peer VPC, or you enable a local linked EC2\-Classic instance to communicate with VPC instances in a peer VPC\.

Communication over ClassicLink only works if both VPCs in the VPC peering connection are in the same region\.

**Important**  
EC2\-Classic instances cannot be enabled for IPv6 communication\. You can enable VPC instances on either side of a VPC peering connection to communicate with each other over IPv6; however, an EC2\-Classic instance that's ClassicLinked with a VPC can communicate with VPC instances on either side of the VPC peering connection over IPv4 only\.

To enable your VPC peering connection for communication with linked EC2\-Classic instances, you must modify the requester VPC peering options if you are the requester of the VPC peering connection, and you must modify the accepter VPC peering options if you are the accepter of the VPC peering connection\. You can use the [describe\-vpc\-peering\-connections](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpc-peering-connections.html) command to verify which VPC is the accepter and the requester for a VPC peering connection\. 

You can modify the VPC peering connection options as follows:
+ **Enable a local linked EC2\-Classic instance to communicate with instances in a peer VPC** 

  In this case, you modify the VPC peering connection options to enable outbound communication from the local ClassicLink connection to the peer VPC on the other side of the VPC peering connection\. The owner of the peer VPC modifies the VPC peering connection options to enable outbound communication from their local VPC to the remote ClassicLink connection\. 
+ **Enable a local VPC to communicate with a linked EC2\-Classic instance in a peer VPC** 

  In this case, you modify the VPC peering connection options to enable outbound communication from your local VPC to the remote ClassicLink connection on the other side of the VPC peering connection\. The owner of the peer VPC with the linked EC2\-Classic instance modifies the VPC peering connection options to enable outbound communication from their local ClassicLink connection to the remote VPC\.

When you enable a local linked EC2\-Classic instance to communicate with instances in a peer VPC, you must manually add a route to the main route table of your local VPC with a destination of the peer VPC CIDR block, and a target of the VPC peering connection\. The linked EC2\-Classic instance is not associated with any subnet in the VPC; it relies on the main route table for communication with the peer VPC\.

**Important**  
The route for the VPC peering connection must be added to the main route table, regardless of any custom route tables with existing routes to the peering connection\. If not, the EC2\-Classic instance cannot communicate with the peer VPC\.

When you enable a local VPC for communication with a remote ClassicLink connection, a route is automatically added to all the local VPC route tables with a destination of `10.0.0.0/8` and a target of `Local`\. This enables communication with the remote linked EC2\-Classic instance\. If your route table has an existing static route in the `10.0.0.0/8` IP address range \(including VPC peering connection routes\), you cannot enable the local VPC for communication with the remote ClassicLink connection\.

**Note**  
You can modify the VPC peering connection options in the following regions:  
US East \(N\. Virginia\)
US West \(N\. California\)
US West \(Oregon\)
EU \(Ireland\)
Asia Pacific \(Tokyo\)
Asia Pacific \(Singapore\)
South America \(São Paulo\)
Asia Pacific \(Sydney\)

## Enabling Communication Between a ClassicLink Instance and a Peer VPC<a name="peering-configurations-local-classiclink-remote-vpc"></a>

In the following scenario, VPC A is enabled for ClassicLink, and instance A is linked to VPC A using ClassicLink\. VPC B is in a different AWS account, and is peered to VPC A using VPC peering connection `pcx-aaaabbbb`\. The VPC peering connection was requested by VPC A and accepted by VPC B\. You want instance A to communicate with instances in VPC B over private IP, and you want instances in VPC B to communicate with instance A over private IP\.

**Note**  
In this scenario, VPC B can either be a VPC in an account that supports EC2\-Classic, or an account that supports EC2\-VPC only\.

![\[VPC peering with local ClassicLink\]](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/images/peering-with-local-classiclink-diagram.png)

The route tables for VPC A contain routes for local VPC traffic, and routes to enable communication to the linked EC2\-Classic instance\. The custom route table for VPC A contains a route that enables instances in the subnet to communicate with the peer VPC over the VPC peering connection, and a route to route all Internet traffic to the Internet gateway\. The custom route table for VPC B contains a route to enable instances in the subnet to communicate with the peer VPC over the VPC peering connection\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/peering-configurations-classiclink.html)

The owner of VPC A must modify the VPC peering connection to enable instance A to communicate with VPC B, and update the main route table\. The owner of VPC B must modify the VPC peering connection to enable VPC B to communicate with instance A\.

**Topics**
+ [Modifying the VPC Peering Connection for VPC A](#peering-configurations-modify-vpc-a)
+ [Modifying the VPC Peering Connection for VPC B](#peering-configurations-modify-vpc-b)
+ [Viewing VPC Peering Connection Options](#w3ab1c11c11c23c20)

### Modifying the VPC Peering Connection for VPC A<a name="peering-configurations-modify-vpc-a"></a>

To enable communication from the EC2\-Classic instance to VPC B, the AWS account owner of VPC A must modify the VPC peering connection options to enable the local ClassicLink connection to send traffic to instances in the peer VPC\. You can modify the VPC peering connection using the Amazon VPC console or the AWS CLI\.

**To modify the VPC peering connection using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

   The owner of VPC A must sign in to the console\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **Actions**, **Edit ClassicLink Settings**\.

1. Choose the option to allow local linked EC2\-Classic instances to communicate with the peer VPC, and choose **Save**\.

**To modify the VPC peering connection using the AWS CLI**

You can use the [modify\-vpc\-peering\-connection\-options](http://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpc-peering-connection-options.html) command\. In this case, VPC A was the requester of the VPC peering connection; therefore, modify the requester options as follows:

```
aws ec2 modify-vpc-peering-connection-options --vpc-peering-connection-id pcx-aaaabbbb --requester-peering-connection-options AllowEgressFromLocalClassicLinkToRemoteVpc=true
```

**To update the main route table**

There are no changes to the route tables for VPC B or to the custom route table for VPC A\. The owner of VPC A must manually add a route to the main route table that enables the linked EC2\-Classic instance to communicate over the VPC peering connection\.


| Destination | Target | 
| --- | --- | 
| 172\.31\.0\.0/16 | Local | 
| 10\.0\.0\.0/8 | Local | 
| 192\.168\.0\.0/16 | pcx\-aaaabbbb | 

For more information about adding routes, see [Adding and Removing Routes from a Route Table](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Route_Tables.html#AddRemoveRoutes) in the *Amazon VPC User Guide*\.

### Modifying the VPC Peering Connection for VPC B<a name="peering-configurations-modify-vpc-b"></a>

Next, the AWS account owner of VPC B must modify the VPC peering connection options to enable VPC B to send traffic to EC2\-Classic instance A\. 

**To modify the VPC peering connection using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

   The owner of VPC B must sign in to the console\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **Actions**, **Edit ClassicLink Settings**\.

1. Choose the option to allow local VPC instances to communicate with EC2\-Classic instances in the peer VPC, and choose **Save**\.

**To modify the VPC peering connection using the AWS CLI**

 VPC B accepted the VPC peering connection; therefore, modify the accepter options as follows:

```
aws ec2 modify-vpc-peering-connection-options --vpc-peering-connection-id pcx-aaaabbbb --accepter-peering-connection-options AllowEgressFromLocalVpcToRemoteClassicLink=true
```

There are no changes to the route tables for VPC A\. A new route is automatically added to the route table for VPC B that allows instances in VPC B to communicate with the linked EC2\-Classic instance in VPC A\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/peering-configurations-classiclink.html)

### Viewing VPC Peering Connection Options<a name="w3ab1c11c11c23c20"></a>

You can view the VPC peering connection options for the accepter VPC and requester VPC using the Amazon VPC console or the AWS CLI\.

**To view VPC peering connection options using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Peering Connections**\.

1. Select the VPC peering connection, and choose **ClassicLink**\. Information about the enabled or disabled VPC peering connection options is displayed\.

**To view VPC peering connection options using the AWS CLI**

You can use the [describe\-vpc\-peering\-connections](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpc-peering-connections.html) command:

```
aws ec2 describe-vpc-peering-connections --vpc-peering-connection-id pcx-aaaabbbb
```

```
{
    "VpcPeeringConnections": [
        {
            "Status": {
                "Message": "Active", 
                "Code": "active"
            }, 
            "Tags": [
                {
                    "Value": "MyPeeringConnection", 
                    "Key": "Name"
                }
            ], 
            "AccepterVpcInfo": {
                "PeeringOptions": {
                    "AllowEgressFromLocalVpcToRemoteClassicLink": true, 
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowDnsResolutionFromRemoteVpc": false
                }, 
                "OwnerId": "123456789101", 
                "VpcId": "vpc-80cb52e4", 
                "CidrBlock": "172.31.0.0/16"
            }, 
            "VpcPeeringConnectionId": "pcx-aaaabbbb", 
            "RequesterVpcInfo": {
                "PeeringOptions": {
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false, 
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": true,
                    "AllowDnsResolutionFromRemoteVpc": false
                }, 
                "OwnerId": "111222333444", 
                "VpcId": "vpc-f527be91", 
                "CidrBlock": "192.168.0.0/16"
            }
        }
    ]
}
```