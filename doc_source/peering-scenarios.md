# VPC peering scenarios<a name="peering-scenarios"></a>

There are a number of reasons you might need to set up a VPC peering connection between your VPCs, or between a VPC that you own and a VPC in a different AWS account\. The following scenarios can help you determine which configuration is best suited to your networking requirements\.

**Topics**
+ [Peering two or more VPCs to provide full access to resources](#peering-scenarios-full)
+ [Peering to one VPC to access centralized resources](#peering-scenarios-partial)
+ [Peering with ClassicLink](#peering-scenarios-classiclink)

## Peering two or more VPCs to provide full access to resources<a name="peering-scenarios-full"></a>

In this scenario, you have two or more VPCs that you want to peer to enable full sharing of resources between all VPCs\. The following are some examples:
+ Your company has a VPC for the finance department, and another VPC for the accounting department\. The finance department requires access to all resources that are in the accounting department, and the accounting department requires access to all resources in the finance department\. 
+ Your company has multiple IT departments, each with their own VPC\. Some VPCs are located within the same AWS account, and others in a different AWS account\. You want to peer together all VPCs to enable the IT departments to have full access to each others' resources\.

For more information about how to set up the VPC peering connection configuration and route tables for this scenario, see the following documentation:
+ [Two VPCs peered together](peering-configurations-full-access.md#two-vpcs-full-access)
+ [Three VPCs peered together](peering-configurations-full-access.md#three-vpcs-full-access)
+ [Multiple VPCs peered together](peering-configurations-full-access.md#many-vpcs-full-access)

For more information about creating and working with VPC peering connections in the Amazon VPC console, see [Working with VPC peering connections](working-with-vpc-peering.md)\.

## Peering to one VPC to access centralized resources<a name="peering-scenarios-partial"></a>

In this scenario, you have a central VPC that contains resources that you want to share with other VPCs\. Your central VPC may require full or partial access to the peer VPCs, and similarly, the peer VPCs may require full or partial access to the central VPC\. The following are some examples:
+ Your company's IT department has a VPC for file sharing\. You want to peer other VPCs to that central VPC, however, you do not want the other VPCs to send traffic to each other\. 
+ Your company has a VPC that you want to share with your customers\. Each customer can create a VPC peering connection with your VPC, however, your customers cannot route traffic to other VPCs that are peered to yours, nor are they aware of the other customers' routes\. 
+ You have a central VPC that is used for Active Directory services\. Specific instances in peer VPCs send requests to the Active Directory servers and require full access to the central VPC\. The central VPC does not require full access to the peer VPCs; it only needs to route response traffic to the specific instances\. 

For more information about how to set up the VPC peering connection configuration and route tables for this scenario, see the following topics:
+ [One VPC peered with two VPCs](peering-configurations-full-access.md#one-to-two-vpcs-full-access)
+ [One VPC peered with multiple VPCs](peering-configurations-full-access.md#one-to-many-vpcs-full-access)
+ [Two VPCs peered to two subnets in one VPC](peering-configurations-partial-access.md#one-to-two-vpcs-simple-hub)
+ [One VPC peered to specific subnets in two VPCs](peering-configurations-partial-access.md#one-to-two-vpcs-specific-subnets)
+ [Instances in one VPC peered to instances in two VPCs](peering-configurations-partial-access.md#one-to-two-vpcs-instances)
+ [One VPC peered with two VPCs using longest prefix match](peering-configurations-partial-access.md#one-to-two-vpcs-lpm)

For more information about creating and working with VPC peering connections in the Amazon VPC console, see [Working with VPC peering connections](working-with-vpc-peering.md)\.

## Peering with ClassicLink<a name="peering-scenarios-classiclink"></a>

You can modify a VPC peering connection to enable one or more EC2\-Classic instances that are linked to your VPC via ClassicLink to communicate with instances in the peer VPC\. Similarly, you can modify a VPC peering connection to enable instances in your VPC to communicate with linked EC2\-Classic instances in the peer VPC\.

For more information about how to set up the VPC peering connection configuration and route tables for this scenario, see [Configurations with ClassicLink](peering-configurations-classiclink.md)\.