## Networking Concepts

### Client-Server and Serverless Computing

Serverless Computing - means that the server is abstracted.

Network Addressing:
	Each device is assigned a network address.
	Subnets further segregate parts of the address space into virtual networks.

Domain Name System (DNS) - maps numeric IP addresses to hostnames.

Azure subnets are private subnets.
Azure networks meet the Internet using public network addresses.

Public Addresses are owned and managed by Microsoft.
Private addresses are assigned and managed by the user.

### Virtual Networks

Azure Virtual Network (VNet) - service enables virtual machines and other Azure services to communicate among themselves.
	Adds availability and scalability to your network resources in Azure.

When you create VNet you must specify the private IP address space that the VNet will use.

You can use VNet to communicate with your resources globally.
### Load Balancers

Load balancing - refers to distributing network traffic across multiple resources to improve responsiveness, reliability, and availability.

Azure Load Balancer service functions at Layer 4, transport layer.

Azure Load Balancing services:
	Azure Front Door - designed for global or multi-region routing and site acceleration of Internet-facing web traffic. 
	Azure Traffic Manager - application layer DNS-based traffic load balancer that balances traffic at the domain level. It can balance traffic across global Azure regions. Traffic Manager offers several options for routing traffic and detecting endpoint health.
	Azure Application Gateway - Application Delivery Controller (ADC) as a service at the domain level. Can balance traffic across Azure regions. 
	Azure Load Balancer - transport layer service designed for high performance and low latency. Zone redundant to provide high availability across availability zones. Applicable for non-HTTP(S) traffic.

Traffic Manager - routes traffic to the data centre that is geographically closest to the user.

Azure Application Gateway - is designed to support regional load balancing for HTTP(S) traffic and support for path-based routing such as /videos.

Azure Front Door - supports URL path-based routing. Intended for globally distributed web applications where speed, user location, fast fail-over, caching, and high availability are critical.
### VPN Gateway

Virtual Private Network (VPN) - establishes an encrypted tunnel between two private networks across a public network.

Azure VPN Gateway - enables you to create VPN connections between Azure virtual networks and between Azure and your on-premises network.

Types of VPN configurations:
	Site-to-site - establishes a VPN tunnel between two sites, such as between your on premises data centre and Azure.
	Multi-site - a variation of site-to-site, a multi-site VPN establishes VPN tunnels between Azure and multiple on-premises sites.
	Point-to-site - establishes a VPN tunnel from a single device (point) to a site.
	VNet-to-VNet - establishes a VPN tunnel between two Azure VNets.

ExpressRoute - enables you to extend your on-premises networks into Azure over a private connection managed by a third-party connectivity provider.
### Content Delivery Networks

Content Deliver Network (CDN) - places web  content across a distributed network of servers to make that content readily available to users based on their location.

