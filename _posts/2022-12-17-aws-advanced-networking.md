---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# VPC Endpoints
VPC endpoints enable traffic to AWS services to be routed directly using a dedicated endpoint within the VPC. There are two type: Gateway endpoints and Interface endpoints (cost, available for almost all services & many APN services). You can connect a single endpoint policies & 1 or more security groups to control access. 

Connectivity issues are generally because of DNS settings or route table problems - both of these elements are required to be present in the VPC. The VPC DNS setting must have _Enable DNS Hostnames_ and _Enable DNS Support_ to be true. 

## Gateway Endpoints
Only for DynamoDB and S3; free; one gateway per VPC and this gateway ONLY works in the VPC (VPN, DX, TGW, VPC peering don't work); can use the same public hostname in route table

## Interface Endpoints
Under the covers it provisions an ENI that will have a private interface hostname so SG work; can be accessed via DX or Site-to-Site VPN. Using VPCE services (Private Link) requires an NLB, an NLB fronting an ALB, or an API Gateway Troubleshooting a VPCE:
0. Service linked permission of caller
0. Check SG of caller
0. Route table
0. VPCE policy
0. Bucket policy 
0. DNS private hosted zone
0. VPC _Enable DNS Hostnames_ and _Enable DNS Support_ to be true

## Endpoint Policies
Control access to the endpoint NOT the underlying service on the other end of the Private Link. Use `aws:SourceVpce` condition to restrict traffic to a specific VPCE or `awsSourceVPC` for a specific VPC; `aws:SourceIP` doesn't apply because the request does not come from a public IP address. Access to specific buckets or parts of buckets is also possible.

# Site to Site VPN
Requires software/hardware appliance on-prem with Internet accessibility; the result is traffic routed over redundant, IPSec encrypted channels tunnel. There are four options to VPN into a VPC including hardware VPN, Direct Connect, VPN CloudHub, Software VPN. 

## Basic VPN Setup
0. Set up software/hardware appliance on-prem with Internet accessibility
1. Attach Virtual Private Gateway (VGW) to VPC
2. Create CGW ASN for BGP to enable dynamic routing; static route for non-BGP for static routing
3. Create 2 VPN Connection from VPC to on-prem (set VPG, CGW &amp; manual or dynamic Routing)

Routing could be:
*Manual routing* which requires you to configure on-prem route table to enable traffic to VGW
and configure VPC route table to enable traffic from private subnet can reach VGW.

*Dynamic routing* Using Border Gateway Protocol (BGP) where you would need to specify the ASN of the CGW and VGW.

### Site-to-Site VPN Scenarios
For scenarios where we want the traffic to flow from on-prem through the VPN to a VPC and out to the internet, NAT Gateways block traffic from VGW to IGW; NAT instances do not.

For scenarios where we want traffic to flow from the VPC over the VPN to on-prem and out to the Internet, which takes away the need to have an IGW on the VPC.

Site-to-Site VPN active-active configurations with multiple on-prem location can be easily enabled by have a VPN connection from both 

## VPN Sharing
CloudHub enables up to 10 CGW to connect to a VGW which enables a low cost primary or secondary network (failover) between locations. 

Instead of enabling direct access to on-prem resource from a VPC (thinking that the VPC might be compromised and give access to on-prem resources direct), a shared services VPC is a recommended architecture. The shared services vpc could contain resources replicated from on-prem or proxies that enable access to on-prem resources; then the shard services VPCs is peered by other VPC. Not really getting the need for an architecture like this...

# Client VPN
Client VPN is a connection from a computer to a VPC, and potentially other resources routable from that VPC, using the OpenVPN. Almost all connectivity options work in this configuration: 
0. Access to the IGW in the VPC
0. Access to peered VPC
0. Access to VPC connected via TGW

# Direct Connect
Direct connect dedicated connection comes in three sizes: 1Gbps, 10Gbps, and 100 Gbps; all three use a dedicated ethernet port dedicated to the customer. Hosted connections come in 50Mbps, 500 mbps, and 10Gbps while capacity can be added or removed on demand. Some partners have 1, 2, 5, 10 Gbps connections. Lead times are often longer than 1 month.

3 types of Virtual interfaces. 
- Private VIF to access resources in a VPC using the private IP address
- Public VIF to access AWS public stuff (using public IP - does this traverse the public Internet?)
- Transit Virtual Interface - connect directly to a TGW via a Direct Connect Gateway

Traffic is private but not encrypted; use a VPN to achieve encryption.

Not redundant; add failover DX or failover VPN as mitigation. _Link Aggregation Groups (LAG)_ can aggregate up to 4 connections in an active-active configuration. Links in an LAG must be dedicated, have the same bandwidth, and terminate at the same DX endpoint.

Use a _Direct Connect Gateway_ to connect to one or more VPC, inter or intra region using Private VIF. Direct Connect gateways can be associated with a transit gateway (if there are many VPC in the region) OR directly to each VPC using a VPG. Use the _Direct Connect SiteLink_ feature of the Direct Connect Gateway to route data via DX to multiple DX connected on-prem locations bypassing AWS regions - essentially using the AWS network as backhaul network.

Direct Connect uses BGP and an Autonomous System Number (ASN) and IP prefixes. 

## Direct connect vs VPN Capability
- VPN = quick to bring up and easy; slower and sucks Internet bandwidth
- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

# Transit Gateway (TGW)
Transit gateway is a hub-spoke model for transitive peering of lots of VPC. The downside is that it requires NON-overlapping IP address ranges. Transit gateway works across regions using inter-region TGW peering (using the TGW ID) and across accounts using Resource Access Manager (which requires an Organizations implementation). Route tables can be used to shape connectivity (along with Transit Gateway associations), it works cross-region and DOES support edge-to-edge routing. Transit gateway works with Direct Connect and VPN AND supports IP Multicast (a unique connectivity feature in the connecting-vpcs-together space).

Transit Gateway can be used to create a single Internet exit point enabling centralization of security at a single point using this [guidance](https://aws.amazon.com/blogs/networking-and-content-delivery/creating-a-single-internet-exit-point-from-multiple-vpcs-using-aws-transit-gateway/). 

## ENI vs EN vs EFA vs Elastic IP
- ENI = Elastic Network Interface - use these to create dual homed instances, possibly for IP restricted software licenses, low budget HA solution or an interface per subnet; by default ENI
- EN = Enhanced networking - uses SR-IO; no cost but requires specific instance types; uses either an ENA (100 Gps) or a VF (10 Gps) with an ENA being the default thing to do if possible
- EFA = Elastic Fabric Adapter - HPC or ML applications using OS-by-pass
- Elastic IP - public IPv4 address remappable to another instance or resource; you can specify the Elastic IP address in a DNS record for your domain, so that your domain points to your instance. 

# AWS Global Accelerator
_Global Accelerator_ is a networking service that helps improve the availability, performance, and security of your public applications by using the AWS network instead of the Internet for connectivity. The benefits include regional failover (HA), improve peformance, and consistent IP caching. Global Accelerator provides two global static Anycast public IPs that act as a fixed entry point to your application endpoints, such as Application Load Balancers, Network Load Balancers, Amazon Elastic Compute Cloud (EC2) instances, and elastic IPs. 

Global Accelerator supports healthchecks, only requires the two IP addresses to be whitelisted, and fully support Shield. Because GA supports TCP &amp; UDP it is great for games, IoT, and VoIP

## Workflow to create a Global Accelerator
0. Create the Accelerator - name it, selected IP address type, use AWS IP or provide 2 IP addresses
0. Add listeners - select port, protocol, and client affinity (for stateful applications)
0. Add End Point groups - select region, % of traffic, and health checks
0. Add Endpoints - can be ALB, NLB, EC2 instance, or EIP

# VPC Flow Logs
Flow logs can be enabled at the VPC, subnet, or ENI level; when enabling at the VPC/Subnet level it simply turns flow logs on for the ENIs in that VPC/Subnet. All data is forwarded to CloudWatch logs or S3 but does not work in real time. Cross VPC flow logs are possible within an account.

You can create flow logs for network interfaces that are created by other AWS services; for example, Elastic Load Balancing, Amazon RDS, Amazon ElastiCache, Amazon Redshift, and Amazon WorkSpaces using the EC2 Console or API. Some traffic is not recorded: Amazon DNS traffic, windows license activation traffic, DHCP traffic, instance metadata traffic, default VPC router

Can record all traffic, accepted traffic, or rejected traffic. Records source and destination IP, source and destination port, action (success/reject). Athena is a good analysis tool for S3 data; CloudWatch Log Insights is useful to find the top contributors to traffic.

Flow logs are commonly used to troubleshoot SG VS NACL issues. If traffic is rejected both inbound/outbound the issue could be either; if rejected either outbound or inbound, but accepted the opposite the issue is NACL. Another common analysis is to make sure the NAT Gateway is functioning using the public IP address of the NAT and see if that traffic is going elsewhere in the VPC.

Limits: 
- Cross-account flow logs aren't possible.
- ~~Can't tag a flow log~~ Nope, now you can tag flow logs!
- Can't modify a flow log once created (change role, etc.)

# AWS Network Firewall
AWS Network Firewall is a stateful, managed, network firewall and intrusion detection and prevention service for VPC. Works layer 3 to Layer 7 and inspects traffice VPC to VPC, outbound, inbound and to/from DX &amp; Site-to-Site VPN. Under the covers, Network Firewall uses the Gateway Load Balancer and an in-house developed inspection capability instead of some 3rd party inspection product.  Logs can be sent to S3, CloudWatch Logs, Kinesis Firehose.

Supports 1000s of rules; inspected traffic can be allowed, dropped, or trigger an alert. Rules can be managed using Firewall Manager.

# DB Subnet Group (RDS)
RDS instances in a VPC - There is a mythical sort of subnet, called a [DB Subnet Group](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html), that must span multiple availability zones and can house RDS instances.  

# Triage 
## HPC Use Cases

- Grid - distributed work loads that are loosely coupled and don't require tight communication between nodes; ASG has application here

- Cluster Computing - two or more instances connected together to support an application; high throughput low latency communication between nodes; weather computations; placement groups; Jumbo Frames - Bigger MTU (9000 byte) enables higher throughput; Risky outside of VPC; most useful in VPC with shared file systems like Lustre & NFS

