
# IPv6 on AWS


## Best practices for adopting and designing IPv6-based networks on AWS

- Every node connected to an Internet Protocol (IP) network must have an IP address for communication purposes. 

- As the internet continues to grow, so does the need for IP addresses. 

- IPv6 is a version of the Internet Protocol that uses a larger address space than its predecessor, IPv4. 

- This whitepaper focuses on best practices for adopting and designing IPv6-based networks on AWS Cloud. 

- It covers IPv6 dual-stack networks for both internet-facing and private IPv6 networks use cases.


## Intro

- An increasing number of organizations operate dual-stack IPv4/IPv6 networks. 
- Carrier networks and ISPs were the first groups to start deploying IPv6 on their networks, with mobile networks leading the charge. 
- Adoption of IPv6 has been delayed, partly due to NAT with IPv4, which takes private IP addresses and turns them into public IP addresses.

- An increasing number of organizations are adopting IPv6 in their environments, driven by the public IPv4 space exhaustion, private IPv4 scarcity, especially within large-scale networks, and the need to provide connectivity to IPv6-only clients. 
- There is no onesize-fits-all approach with IPv6; however, there are best practices AWS customers can follow to plan and implement IPv6 into their existing cloud networks. 

- This whitepaper explains key drivers for adoption and highlights best practices to guide you while leaving enough space for you to decide, based on your specific use case and implementation, how to approach IPv6 in your network.


### Tenets for IPv6 adoption

- Adhering to the following tenets for IPv6 adoption can help make the process and decision more manageable: 
   - Re-evaluate network controls — IPv6 offers opportunities to rethink your approach to perimeter security and make design decisions that further improve your security posture.
   - Design for scale — More usable IPv6 addresses doesn’t mean you can shortcut IP allocation and planning.
   - Phase your IPv6 adoption — Focus on your business needs to implement IPv6 where needed, and remember that IPv4 and IPv6 can be made to coexist as long as needed.
   

### Internet Protocol version 6

- IPv6 is the next generation IP address standard intended to supplement and eventually replace IPv4, the original and ubiquitous IP address scheme. 

- Every computer, mobile phone, home automation component, IoT sensor, and any other device connected to an IP-based network needs a numerical IP address to communicate with other devices.

- Public reachable IPv4 addresses are in short supply due to their widespread usage and constantly increasing demand stemming from the proliferation of connected devices globally. 

- The last available block of new IP version 4 (IPv4) addresses was allocated back in 2011, and from that time on, everyone has been reusing a finite set of available addresses. 

- IP version 6 (IPv6) is the replacement for IPv4, and it is designed to address the depletion of IP addresses, and change the way traffic is managed.


### IPv6 addressing

The IPv6 address space is organized by using format prefixes, that logically divide it in the form of a tree so that a route from one network to another can easily be found.


The main categories of IPv6 addresses are:
      
      - Aggregatable global unicast addresses (GUA) — 2000::/3 
      
      - Unique-local unicast addresses (ULA) — FC00::/7 
      
      - Link-local unicast addresses — FE80::/10 
      
      - Multicast addresses — FF00::/8 

### IPv6 adoption strategies and mechanisms

Working with customers, AWS observed the following two main drivers for IPv6 adoption.

   - Network Address Translation is no longer sufficient to work around exhaustion and poses significant challenges with overlapping IP addresses.
   - There are numerous organizational or regulatory mandates to adopt IPv6. 
   

Following is a summary of IPv6 adoption drivers:

- Mandated IPv6 endpoints — Either mandated by a government policy or an industry regulator, and not necessarily tied to a particular use case.


- Interoperability with IPv6 networks — The last years have seen a growing population of IPv6-only clients, and as a result so have the number of organizations wanting to cater to this user base. With the number of mobile IPv6-only users, many companies find they can’t afford to lose that section of their potential user base.

- Public IPv4 exhaustion — As public IPv4 addresses become more scarce, allocating contiguous IP address ranges for public routing becomes more difficult and costly.

- Private IPv4 exhaustion — As private IPv4 (RFC1918) addresses become exhausted and too fragmented within organizations’ private networks, IPv6-only networks offer opportunities to address additional nodes.

IPv6 adoption strategy depends on the driving force behind it. You may have an immediate goal such as addressing private IPv4 exhaustion, or the ability to provide IPv6 service endpoints as per government mandate, with the long-term goal of fully converting to an IPv6 only network. 

Following are the four main driving forces, and the corresponding adoption strategy:

- Private IPv4 exhaustion — The goal is to provision new nodes and allocate routable IP addresses without IP overlap, and without the challenge of sourcing contiguous and usable IP addresses.

- Adoption strategy — Configure IPv6-only routing between dual stack network segments, to facilitate communication using the IPv6 stack. Provide IPv6 to IPv4 interoperability layer, such as dual-stack load balancers. 


- Public IPv4 exhaustion — The goal is to support IPv6 only nodes connecting to your public endpoints. As an example, you may have an API endpoint for data upload from IoT devices which are connected to an IPv6-only network. The IoT devices have IPv6 addresses, and the network does not provide interoperability layer to IPv4. Other devices may operate in IPv4 networks.

- Adoption strategy — Create dual-stack VPCs and subnets. Configure AWS services such as load balancers and edge service in dual-stack mode with corresponding DNS record on the AWS Cloud. Optionally, provide separate endpoints for IPv4 and IPv6 in dedicated IPv4-only or IPv6-only deployments.


### Interoperability

Although operating in dual-stack mode solves a lot of the problems with IPv4 and IPv6 interoperability, it creates management overhead. For example, security becomes harder because you have to manage two sets of security rules, one for each network stack. Routing and troubleshooting become harder, and you have to maintain additional records to existing DNS names.

You may be able to avoid making the entire network dual-stack by focusing on implementing dual-stack at your border via load balancers. Existing segments of your network can continue to operate as IPv4 in most cases, and new segments are built with IPv6. Focus on implementing and operating interoperability layer where AWS services such as dual-stack VPC and load balancers, to help you solve interoperability challenges.



### Planning IPv6 adoption in the AWS Cloud network

Elastic network interfaces in an IP network could operate in three different modes:

- IPv4-only mode — Your resources can communicate over IPv4, and if communicating to IPv6 nodes, require an interoperability layer.

- IPv6-only mode — Your resources can communicate over IPv6, and if communicating to IPv4 nodes, require an interoperability layer.

- Dual-stack mode — Your resources can communicate over both IPv4 and IPv6.

A separate interoperability layer is not required

#### IPv6 addressing plan on AWS

Coming up with an IPv6 addressing plan is one of the most important initial tasks for any organization proceeding with IPv6 adoption. For most organizations, IPv6 is deployed in parallel with IPv4 in existing IPv4 AWS and hybrid networks. 

IPv4 addressing plans tend to grow over time, and consequently may be highly fragmented, not contiguous, or not big enough. Simply duplicating the IPv4 addressing scheme in
some fashion in IPv6 might initially prove advantageous. 

However, any temporary advantage gained by such a shortcut will ultimately be surpassed by the ease and efficiency of operation and design offered by a proper IPv6 addressing plan that incorporates the key benefits of the larger allocations possible with IPv6. 


#### AWS-assigned IPv6 VPC CIDR

By default, Amazon provides one fixed size (/56) IPv6 CIDR block to a VPC. This range is assigned by the service, and consequently, you can’t assign contiguous IPv6 CIDR blocks to VPCs in the same Region or based on other custom-defined criteria.

For customers that have a large VPC footprint in AWS and prefer to use IP route summarization to simplify their overall environment, bring your own IPv6 (BYOIPv6) described, in the next section, may be the preferred solution.


#### BYOIPv6 VPC CIDR

Alternatively, if you own an IPv6 address space, you can import it into AWS using the Bring Your Own IPv6 service. The smallest IPv6 address range that you can bring is /48 for CIDRs that are publicly advertised by AWS, and /56 for CIDRs that are not publicly advertised by AWS. 


You can also choose to bring a /48 and mark it as non-advertisable, keeping control of IP advertisements on your on-premises setup. After importing it, you can assign /56 ranges from the space to individual VPCs in the same account.

#### VPC subnet addressing

Although you can assign one /56 IPv6 CIDR block to a VPC, the VPC subnets are /64 fixed in length. This yields to the interface ID being /64 in length, in accordance with the general format of the IPv6 unicast addresses. Given the fixed size of the VPC CIDR and the subnet prefix, you have 8 bits for subnet allocation in the VPC, enabling you to create 256 subnets in the VPC.


### Designing an IPv6 AWS Cloud network

##### Amazon VPC design


Planning and implementing network connectivity in AWS is usually one of the foundational tasks you perform when deploying workloads in AWS. 

Following are some of the aspects typically considered:

   - Amount and nature of Amazon VPCs required
   - Amazon VPC CIDR range and IP address allocation including Bring Your Own IP (BYOIP) for public connectivity
   
   - Number and type of subnets
   - Number of availability zones to cover
   - Permitted traffic paths
   - Internet incoming and outgoing traffic options
   - Hybrid connectivity
   - Inter-VPC connectivity
   - Scalability and expansion


##### VPC IP address assignment

Your VPC can operate in dual-stack mode—your resources can communicate over IPv4, IPv6, or both. IPv4 and IPv6 communication are independent of each other. You cannot disable IPv4 support for your VPC and subnets; you are required to allocate at least one IPv4 CIDR range to your VPC. In addition, you may associate up to one IPv6 CIDR block range per VPC.


##### Subnet address assignment

After you have associated an IPv6 prefix to a VPC, you can begin to assign one /64 IPv6 prefix to each subnet. 

Note that these assignments are configured on a per-subnet basis, and it’s possible to have a mix of subnets with and without IPv6 within the same VPC. 

This is useful in scenarios where you merely require IPv6 capability for a subset of the network as described in the drivers for adoption section.


The address assignment of resource within a subnet occurs at two levels:

  - The Amazon VPC elastic network interface construct configuration
  - A resource’s networking stack configuration


##### IP addressing of the elastic network interface

- Network-addressable resources deployed within a VPC must have an elastic network interface. Examples of resources include: 

   - Amazon Elastic Compute Cloud (Amazon EC2) instances
   - Interface VPC endpoints
   - AWS Lambda functions (deployed in VPCs)
   - Amazon Relational Database Service (Amazon RDS) database instances


Elastic network interfaces are logical constructs in the VPC which represent a resource’s network adapter at runtime. 

Each elastic network interface may have one or more IPv4 addresses as well as one or more IPv6 addresses. 

This means you are not required to provision separate elastic network interfaces for IPv4 and IPv6, and there is no need to configure additional elastic network interfaces on your workloads to enable IPv6.


![1](https://user-images.githubusercontent.com/23625821/142264990-174070e1-4445-454d-abe0-1432f448d921.png)


#### IP addressing at the resource’s networking stack

In IPv4, the preferred method for assigning IPv4 addresses is to use Dynamic Host Configuration Protocol (DHCP). 

DHCP is based on IPv4’s broadcast mechanism that allows hosts to announce themselves to DHCP servers. 

These servers can then offer an IP address lease to the client. IPv6 has no concept of broadcast and initially did not feature DHCP capability. 

However, the community has become used to DHCP, and so RFC 8415 was developed to introduce DHCPv6. 

In the absence of broadcast capability, DHCPv6 makes use of the well-known multicast address for all DHCP servers/relays, ff02::1:2.

Amazon VPC has built in support for address assignment via DHCP for both IPv4 and IPv6. 

Address allocation works similar to static address reservation in traditional DHCP servers: 

   the IP address assigned to the elastic network interface construct determines the IP address the VPC DHCP infrastructure offers the resource requesting an address.

Amazon VPC also offers the ability to configure DHCP option sets which can be used to provide additional configuration information such as domain name or DNS servers to use. In a dual-stack design all IP addresses used in an option set need to be IPv4.


### Supporting Amazon VPC services

AWS exposes a set of supporting services within customer VPCs at wellknown/reserved addresses. 

These services are traditionally exposed from the IPv4 linklocal address range (169.254.0.0/16). 

For <a href="https://aws.amazon.com/ec2/nitro/"> AWS Nitro System instances </a> , AWS also provides these services using IPv6 ULAs.


### Instance Metadata Service (IMDS)

The instance metadata is information about your instance. 

Instances can introspect this at runtime by querying the IMDS available to it at 169.254.169.254. 

For Nitro-based instances, AWS also provides this service at the fd00:ec2::254 IPv6 endpoint.


### Route 53 DNS resolver

Amazon VPC features a built-in DNS resolver which resides at VPC_CIDR_BASE + 2 and 169.254.169.253. 

IPv6 enabled Nitro instances can access the service via fd00:ec2::253. 

Amazon Route 53 Resolver and DNS in general is discussed at greater length in the Designing DNS for IPv6 section of this document.

### Network Time Protocol server

Amazon VPC provides a Stratum-3 NTP server at 169.254.169.123. 

Nitro-based IPv6 enabled instances can reach this server via fd00:ec2::123.


### Amazon VPC connectivity options for IPv6

There are a growing number of ways in which Amazon VPCs can connect to each other. 

Many of these options are detailed in the VPC to VPC connectivity section of the Building a Scalable and Secure Multi-VPC AWS Network Infrastructure <a href="https://docs.aws.amazon.com/whitepapers/latest/building-scalable-secure-multi-vpc-network-infrastructure/welcome.html"> whitepaper </a> .

AWS recommends you read the following subsections alongside, and it follows the same structure while providing additional insight regarding IPv6 operation as both papers cover:

   - VPC peering
   - AWS Transit Gateway
   - VPC subnet sharing
   - AWS PrivateLink


#### VPC peering

VPC peering is the simplest method for VPC-to-VPC connectivity. 

It supports both intraand inter-Region connectivity. 

The peering itself is IP protocol agnostic. 

After you establish peering, you must configure one or more static routes defining which prefixes are reachable. 

Both IPv4 and IPv6 prefixes may be routed across the same peering.

The following diagram depicts a VPC peering between two VPCs supporting IPv4 and IPv6 simultaneously. 

The peering is agnostic, and the subnet route tables are the deciding factor for which prefixes are reachable.


![1](https://user-images.githubusercontent.com/23625821/142267059-f212f122-0d46-4070-b678-c84748a84e1a.png)


#### AWS Transit Gateway

It is a scalable highly available way to establish network connectivity between multiple VPCs. 

A Transit Gateway is a Regional construct, and attaches VPCs within the same Region. 

Transit Gateways located in different AWS Regions can establish a peering relationship, enabling global connectivity for your network.


#### IPv6 connectivity into Transit Gateway

You use a Transit Gateway attachment to connect a VPC to a Transit Gateway. 

An attachment deploys an elastic network interface into each subnet you select. 

Traffic is routed into Transit Gateways using static routes in VPC subnet routing tables with the attachment as the next-hop. 

The attachments themselves are IP protocol agnostic, and you can route IPv4 and IPv6 prefixes via the same attachment. 

To support IPv6, the elastic network interface s used by the attachments need to have IPv6 addresses assigned to them.


#### IPv6 traffic within and between Transit Gateways

A Transit Gateway attachment is both a source and a destination of packets. 

You can attach the following resources to your Transit Gateway:

   - VPCs
   - One or more VPN connections
   - One or more AWS Direct Connect gateways
   - One or more Transit Gateway Connect attachments
   - One or more Transit Gateway peering connections


A Transit Gateway has one or more routing tables. 

A routing table can receive its entries through a combination of static route configuration and dynamic propagations from other attachments (VPC, Direct Connect, Site-to-Site VPN, or Connect Peering). In either case, IPv6 routes are supported.


#### AWS Transit Gateway Connect Attachments for IPv6

You can create a Transit Gateway Connect attachment to establish a connection and dynamic routing between a transit gateway and third-party virtual appliances (such as SD-WAN appliances).

These attachments take the form of IP Generic Routing Encapsulation (GRE) protocol tunnels and enable dynamic exchange of routing information between an EC2 instances in a VPC and a TGW. 

Route exchange is facilitated by a Border Gateway Protocol (BGP) peering. TGW connect peers support IPv6 using Multi-Protocol BGP (MP-BGP) and a /125 CIDR block from the well-known fd00::/8 unique local address range.

Multiprotocol BGP (MP-BGP) is an extension to BGP that enables BGP to carry routing information for multiple network layers and address families. MP-BGP can carry the unicast routes used for multicast routing separately from the routes used for unicast IP forwarding.


![1](https://user-images.githubusercontent.com/23625821/142367647-1ee6aff8-99f3-41bc-9f2b-ee108ed2a965.png)


#### AWS PrivateLink

AWS PrivateLink provides private connectivity between VPCs, AWS services, and customer on-premises networks, without exposing traffic to the public internet. 

AWS PrivateLink makes it easy to connect services across different accounts and VPCs to significantly simplify your network architecture.

AWS PrivateLink does not currently support IPv6. However, PrivateLink has the useful property of abstracting the IP addressing used between source and destination. 

In the meantime, it’s possible to operate a dual-stack setup for the purpose of communicating via PrivateLink endpoints. 

It is possible for a workload to use IPv6 for most communication and use IPv4 purely for accessing the IPv4 address of the PrivateLink endpoint.


![1](https://user-images.githubusercontent.com/23625821/142368216-ed49991d-e692-44b6-8902-efe1f0727fb1.png)



#### VPC sharing

VPC sharing allows VPC owners to share a subnet across AWS accounts. 

You may share dual-stack subnets the same way as IPv4-only ones. 

IPv6 resources deployed into a shared subnet function identical to those deployed into non-shared subnets.


### Amazon VPC internet access

#### Internet reachable IPv6 resources

Elastic network interfaces retain their IPv6 addresses throughout their lifetime. 

For IPv4, elastic network interfaces can have zero or more Elastic IP addresses associated with them. 

An Elastic IP address defines a static 1:1 NAT relationship between an elastic network interface’s IPv4 address and a public internetroutable address.


In IPv6, VPC addressing is already globally unique, and therefore Elastic IP addresses are not required. 

Amazon assigned IPv6 addresses are automatically publicly advertised whereas for BYOIP ranges this is optional. 

In either case, resources deployed only have IPv6 internet reachability if their subnet’s routing table contains IPv6 destinations (such as ::/0) via either an internet gateway or outbound traffic-only internet gateway.


### Hybrid connectivity design

Hybrid connectivity scenarios are a reality for many customers. 

Two methods for addressing these: AWS Direct Connect and AWS managed Site-to-Site VPN.


AWS previously published the Hybrid Connectivity whitepaper, which focused on designs and considerations around these solutions, and most of that content remains relevant. 

However, that paper does not consider IPv6. This section assumes you are acquainted with the aforementioned document, and it focuses only on the best practices and differences compared to IPv4 implementations.


#### AWS Direct Connect


AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from customer premises to AWS.

Different aspects of the Direct Connect service deal with different layers of the OSI model. 

The choice of IPv6 only affects configuration related to Layer 3, so many aspects of Direct Connect configuration such as physical connections, link aggregation, VLANs, and jumbo frame are no different from IPv4 use cases.


Where IPv6 does differ is when it comes to addressing and configuration of BGP peerings on top of a virtual network interface (VIF). There are three types of VIFs: 

- Private
- Transit
- Public


Transit and Private VIF IPv6 peerings — Whereas in IPv4 you are free to choose your own addressing for the logical point-to-point, in IPv6, AWS automatically allocates a
125 CIDR for each VIF, and it’s not possible to specify custom IPv6 addresses.


Advertising IPv6 prefixes from AWS

When associating a Direct Connect Gateway directly with a VGW you can specify “Allowed Prefixes . 

Think of this like a traditional “prefix-list” filter, controlling the prefixes advertised to your customer gateway. With IPv4, specifying no filter equates to 0.0.0.0/0 — no filtering. 

With IPv6, not specifying a value here results in all advertisements being implicitly blocked.


### Amazon-managed VPN

AWS Site-to-Site VPN connectivity configuration comprises multiple parts:

   1. The customer gateway, which is the logical representation of the onpremises VPN end point.
   2. The VPN connection.
   3. The local device configuration on the VPN appliance, represented by the customer gateway. 


Any AWS S2S VPN connection consists of two tunnels. It is this connection that defines the IP addressing, ISAKMP, IPsec, and BGP peering parameters.

### Designing DNS for IPv6

The core concept of DNS is unchanged from IPv4. From a Layer 3 perspective, DNS is just another application, and therefore, by virtue of the OSI/ISO model provided abstraction, agnostic to the chosen network layer protocol.

Regardless of the IP version, there is a deep link between DNS and the IP layer. 

The DNS specification has adapted and introduced an additional type to accommodate IPv6 addresses. 

In IPv6 the equivalent of the IPv4 “A” records are AAAA records. 

This means that it is possible to use IPv4 as the network protocol to connect to a DNS server and resolve an IPv6 (AAAA) record.

#### PTR records

A pointer (PTR) record translates an IP address to its domain name. 

IPv6 addresses are reverse mapped under the domain IP6.ARPA. 

IPv6 reverse maps use a sequence of nibbles separated by dots with the suffix “.IP6.ARPA” as defined in RFC 3596. 

For example, the reverse lookup domain name corresponding to the address 2001:db8:1234:1a00:1:2:3:4 would be 4.0.0.0.3.0.0.0.2.0.0.0.1.0.0.0.0.0.a.1.4.3.2.1.8.b.d.0.1.0.0.2.ip6.arpa


#### Alias records

Amazon Route 53 supports alias records. 

Route 53 alias records are mapped internally to the DNS name of alias targets such as AWS resources. 

Route 53 monitors the IP address associated with an alias target's DNS name for scaling actions and software updates. 

The authoritative response from Route 53 name servers contains an A record (for IPv4 addresses) or AAAA record (for IPv6 addresses) with the IP address of the alias target.


#### DNS resolution within a host

External configuration aside it is up to a host’s networking stack at runtime to resolve DNS records. 

When configured as dual-stack, most modern operating systems default to preferring IPv6. 

In other words, when a query for a FQDN returns both an A and AAAA record the OS prefers to use the AAAA record and establishes IPv6 connectivity to the target.


### Amazon Route 53 DNS records

In AWS, Amazon Route 53 provides DNS capabilities. Route 53 provides features for two use cases:

   - Public DNS for externally hosted content
   - DNS capability within a VPC both from a resolver and authoritative name server standpoint



#### Public IPv6 DNS resolution

For externally queryable DNS, you can use Route 53 public hosted zones, with both A and AAAA records. 

Route 53 health checks support health checking IPv6 services. 

The name servers exist both for IPv4 and IPv6, meaning clients wanting to resolve a FQDN hosted on Route 53 public hosted zone can do so natively.

#### DNS resolution within a VPC

Amazon VPC comes with Route 53 Resolver, which provides a built-in capability for resolving DNS names. 

This resolver is reachable either on 169.254.169.253 or VPC_CIDR_NETWORK + 2 for IPv4 and fd00:ec2::253 for Nitro-based IPv6 hosts.

Requests sent to this resolver are resolved against the combination of private hosted zones associated with the VPC, and any (shared) resolver rules


### IPv6 security and monitoring considerations

#### Network-level access control

Amazon VPCs feature two network access control mechanisms, and these exist irrespectively of which version of the IP protocol is used (IPv4 or IPv6):

   - Security groups (SGs) at the elastic network interface level
   - Network access control lists (network ACLs) at the subnet level


#### VPC Flow Logs

VPC Flow Logs is a feature that enables you to capture information about the IPv6 traffic going to and from network interfaces in your VPC. 

VPC Flow Logs for IPv6 traffic works the same as IPv4 where you can create flow logs at the VPC level, the subnet level, or the network interface level. 

If you create VPC Flow Logs at a VPC or subnet level, every network interface in that VPC or subnet is monitored.

The flow log records can use the default format or the custom format. 

With a custom format, you specify which fields are included in the IPv6 flow log records and in which order. 


VPC Logs default format: 


![1](https://user-images.githubusercontent.com/23625821/142719605-08952018-6fdf-42c5-a379-5250fc750e6c.png)



#### VPC Traffic Mirroring

VPC Traffic Mirroring is a complementary feature to flow logs that copies entire packets, including their payload of network traffic from a specified elastic network interface of an Amazon EC2 instance. 

Traffic Mirroring copies inbound and outbound IPv4 and IPv6 traffic from the network interfaces that are attached to your Amazon EC2 instances. 

You can send the mirrored traffic to the network interface of another EC2 instance, or a Network Load Balancer that has a UDP listener (listening on UDP port 4789 - VXLAN).

The mirrored traffic is sent to the traffic mirror target by means of the source VPC IPv4 route table. 

Note that all mirrored traffic is encapsulated in an IPv4 packet. 

Traffic Mirroring mirrors both your IPv4 and IPv6 traffic. 

No special configuration is necessary to enable Traffic Mirroring for your IPv6 traffic, whether the traffic mirror source and the target are in the same VPC.  

Or in a different VPC connected via VPC peering or a Transit Gateway (as long as the traffic mirror source can route to the traffic mirror target by IPv4).


### AWS Web Application Firewall

AWS Web Application Firewall (AWS WAF) lets you monitor the HTTP(S) requests that are forwarded to an Amazon CloudFront distribution, an Amazon API Gateway REST
API, an Application Load Balancer, or an AWS AppSync GraphQL API. 

With AWS WAF, the services that are associated with the protected resources can respond either with the requested content or with HTTP 403 status code based on conditions that are specified, such as the IP addresses (either IPv4 or IPv6) that the request originate from.



#### Web ACL

You use the rules in a web ACL to block or allow web requests based on criteria which includes IP addresses or address ranges that requests originate from. 

The IP set match statement inspects the IP address of a web request against a set of IP addresses and address ranges. 

Use this to allow or block web requests based on the IP addresses (either IPv4 or IPv6) that the requests originate from. 

AWS WAF IP sets supports all IPv4 and IPv6 CIDR ranges except for 0.0.0.0/0 and ::/0.


### AWS Shield 

It provides protection against DDoS attacks. 

A DDoS attack can prevent legitimate users from accessing a service and can cause the system to crash due to the overwhelming traffic volume. 

All of the AWS Shield detection and mitigations work with IPv4 and IPv6 without any impact to performance, scalability, or availability of the service


#### AWS Network Firewall

AWS Network Firewall is a stateful, managed, network firewall and intrusion detection and prevention service for Amazon Virtual Private Cloud (Amazon VPC). 

AWS Network Firewall does not currently support IPv6. In the dual-stack mode, you can still use AWS Network Firewall to filter IPv4 traffic going to and coming from an internet gateway, NAT gateway, or over VPN or AWS Direct Connect.


#### AWS Systems Manager

Resources managed by AWS Systems Manager must have IPv4 connectivity to Systems Manager’s endpoints. 

For example, to connect to an EC2 instance using Systems Manager Session Manager, the instance must be running dual-stack and must have an IPv4 connectivity to the internet or AWS PrivateLink VPC endpoint. 

Similarly, on-premises resources must also be in dual-stack network mode



### Scaling the dual-stack network design in AWS

#### Elastic Load Balancing

ELB automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones. 

It supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers.

Both load balancer types support internet-facing and internal load balancer schemes. 

You create internet-facing Application Load Balancers and internet-facing Network Load 


![1](https://user-images.githubusercontent.com/23625821/142721677-274e3668-5f56-4bc5-a84f-ba34b2d1be47.png)


### Conclusion

There are multiple driving forces behind IPv6 adoption. This paper describes what they are and explains how you can respond to them. 

It also explains differences between IPv4 and IPv6 where applicable, and covers interoperability between both network stacks. 

As always, security remains paramount and so this paper covered how to evolve your perimeter design to take advantage of IPv6 protocol features.

Remember that IPv6 only makes a difference at the network layer of the networking stack. 

Many connectivity and security elements, especially in cloud native applications, are handled at higher layers and are therefore not affected.

AWS offers comprehensive IPv6 support in Amazon VPC and AWS services running at the edge of AWS Cloud. 

You can adopt IPv6 at your own pace and focus on use cases where you will benefit the most from the adoption.






#### Reference

<a href="https://d1.awsstatic.com/whitepapers/IPv6-on-AWS.pdf"> Original paper </a> 
