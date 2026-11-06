## Virtual networks (VNet) and subnets  
An Azure Virtual Network (VNet) is a logically isolated network in Azure where you place resources such as virtual machines and private endpoints. Think of it like your company's internal network, but configured in Azure.  

The VNet address space is divided into smaller networks called subnets. Subnets help organize resources and apply network controls. Subnet is a smaller IP address range inside the VNet.  
CIDR prefixes  

<img src="image/Subnet.png" alt="Subnet" width="600">
CIDR   total Ip address  
/32  1  
/31  2  
/30  4  
/29  8  
/28  16  
/27  32  
/26  64  
/25  128  
/24  256  

Azure reserves five IP addresses in every subnet, so a /24 provides 251 usable addresses.  

Remember: a smaller prefix number means a larger network. For example, /16 contains more addresses than /24.  

The smallest supported IPv4 subnet is /29. It means Azure supports IPv4 subnet sizes from /29 up to larger ranges such as /28, /27, /24, and /16 (among other prefixes).  

10.0.1.0/24 covers addresses from 10.0.1.0 to 10.0.1.255. In a standard Azure subnet, 5 addresses are reserved by the platform. Azure-reserved addresses: first 4 and last 1  
<img src="image/subnet-reserve.png" alt="subnet-reserve" width="600">  
First 4 + last 1 = 5 reserved IPv4 addresses per subnet.

### Creating a VNet in the portal
1. Search for Virtual networks in the Azure portal.
2. Select Create.
3. Select your subscription and resource group.
4. Enter VNet-GreenMaze as the name.
5. Choose a region.
6. Configure an address space, such as 10.0.0.0/16.
7. Create subnets, such as 10.0.1.0/24 and 10.0.2.0/24.
8. Review and create.

<img src="image/subnet1.png" alt="subnet1" width="600">  

https://learn.microsoft.com/en-us/azure/virtual-network/quickstart-create-virtual-network?utm_source=chatgpt.com&tabs=portal  

<img src="image/scenariso.png" alt="scenariso" width="600">  
<img src="image/scenario2.png" alt="scenario2" width="600">  

### Subnet 
A virtual network can be segmented into one or more subnets.  
Subnets provide logical division within your network.  
Subnets can help improve security, increase performance, and make it easier to manage the network.  
Each subnet must have a unique address range - cannot overlap with other subnets in the vnet in the subscription. 

### Reserved addresses 

For each subnet, Azure reserves five IP addresses. The first four addresses and the last address are reserved.
Let's examine the reserved addresses in an IP address range of 192.168.1.0/24.

192.168.1.0	This value identifies the virtual network address.  
192.168.1.1	Azure configures this address as the default gateway.  
192.168.1.2 and 192.168.1.3	Azure maps these Azure DNS IP addresses to the virtual network space.  
192.168.1.255	This value supplies the virtual network broadcast address.  

## IP Address
Static IP addresses don't change and are best for certain situations, such as:  
DNS name resolution, where a change in the IP address requires updating host records.  
IP address-based security models that require apps or services to have a static IP address.  
TLS/SSL certificates linked to an IP address.  
Firewall rules that allow or deny traffic by using IP address ranges.  
Role-based virtual machines such as Domain Controllers and DNS servers.   

### Networking foundation: Core connectivity for your resources in Azure - Virtual Network

## Az private link
Azure Private Link allows you to access Azure services privately through a private IP address inside your Virtual Network (VNet). Instead of accessing an Azure service through its public endpoint, you access it through a private endpoint in your VNet. You no longer need to expose your service to the public internet. 
- Private Link = private IP to access an Azure service.  
- Service Endpoint = subnet-based access restriction, but the service retains its public endpoint.  

## Virtual network

Azure Virtual Network (VNet) is the fundamental building block for your private network in Azure. Use VNets to:  
Deploy virtual machines and several other types of Azure resources to a virtual network, such as Azure App Service Environments, Azure Kubernetes Service (AKS), and Azure Virtual Machine Scale Sets.  

Communicate between each other: Connect virtual networks to each other, enabling resources in either virtual network to communicate with each other, by using virtual network peering or Azure Virtual Network Manager. The virtual networks you connect can be in the same, or different, Azure regions. a VNet is like a separate private company network. By default, two separate VNets do not automatically communicate just because they belong to the same Azure subscription.  

Communicate to the internet: All resources in a virtual network can communicate outbound to the internet, by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer. You can also use Public IP addresses or a public Load Balancer to manage your outbound connections.  

Communicate with on-premises networks: Connect your on-premises computers and networks to a virtual network by using VPN Gateway or ExpressRoute.  

Encrypt traffic between resources: Use Virtual network encryption to encrypt traffic between resources in a virtual network.  

## Azure DNS

Azure DNS provides DNS hosting and resolution by using the Microsoft Azure infrastructure. Azure DNS consists of three services:  
DNS (Domain Name System) translates domain names into IP addresses.  

Azure Public DNS is a hosting service for DNS domains. By hosting your domains in Azure, you can manage your DNS records by using the same credentials, APIs, tools, and billing as your other Azure services.  

Azure Private DNS is a DNS service for your virtual networks. Azure Private DNS manages and resolves domain names in the virtual network without configuring a custom DNS solution. Think of it as: DNS for your private Azure network.  

Azure DNS Private Resolver allows Azure and on-premises networks to resolve private DNS names across network boundaries without deploying and maintaining your own DNS servers. Think of it as a bridge between your on-premises DNS and Azure Private DNS.  
Why do we need it?  
Imagine your GreenMaze environment:  
- Your Azure VNet is 10.0.0.0/16.  
- Your Azure VM-Web has private IP 10.0.1.4.  
- Your Azure VM-DB has private IP 10.0.1.5.  
- Your company office has its own DNS server.  

You create an Azure Private DNS zone: greenmaze.local  
With records:  
Hostname	Private IP  
web.greenmaze.local	10.0.1.4  
db.greenmaze.local	10.0.1.5  
 
Azure VMs linked to this private DNS zone can resolve those names.   
But what if a computer in your Montreal office needs to resolve db.greenmaze.local?    
Your on-premises DNS server doesn't automatically know about Azure Private DNS zones. This is where Azure DNS Private Resolver helps.  

<img src="image/dns-resolver.png" alt="dns-resolver" width="800">  

# Inbound endpoint  
- Allows DNS queries to come into Azure from on-premises networks.  
- Your on-premises DNS server forwards queries for Azure private DNS names to the inbound endpoint's private IP.  
- Azure resolves the name using the linked Private DNS zone.  
Example: Montreal PC → On-prem DNS → Azure DNS Private Resolver inbound endpoint → Azure Private DNS  

# Outbound endpoint  
- Allows DNS queries to go out of Azure to external DNS servers, such as your company's on-premises DNS.  
- Azure DNS forwarding rules tell Azure where to send queries for specific domains.  
Example: Azure VM → Azure DNS Private Resolver outbound endpoint → On-prem DNS server  

Remember the direction:  
Inbound On-premises asks Azure  

Outbound Azure asks on-premises  


## Az Route server  
Azure Route Server allows Azure and your network virtual appliance (NVA) to automatically exchange network routing information using BGP, instead of you manually configuring routes.
<img src="image/route-server.png" alt="route-server" width="400">  
NVA stands for Network Virtual Appliance. It's a virtual network device running inside Azure that performs networking or security functions.  
Examples:- Firewall - Router - VPN appliance - Network traffic inspection appliance   
Examples of vendors include Fortinet, Cisco and Palo Alto Networks. Think of an NVA as a virtual firewall or router inside your Azure network.  

Azure Route Server automates the exchange of routing information.  
stands for Border Gateway Protocol = BGP: Routing protocol that dynamically exchanges network routes. It's a routing protocol that allows network devices to exchange information about which networks they can reach. If the advertised routes change, BGP can update the routing information automatically.  

## NAT Gateway (Network Address Translation Gateway)
NAT Gateway simplifies outbound-only internet connectivity for virtual networks. When you configure it on a subnet, all outbound connectivity uses your specified static public IP addresses. Outbound connectivity is possible without a load balancer or public IP addresses directly attached to virtual machines. You associate the NAT Gateway with a subnet.

<img src="image/NAT.png" alt="NAT" width="400">  

When VM 1 and VM 2 access the internet:  
- Both use the NAT Gateway.  
- Their private IP addresses are translated to the NAT Gateway's public IP.  
- External websites see the NAT Gateway's public IP, not the VMs' private IPs.  

Remember these three points:  
1. NAT Gateway is primarily forS outbound connectivity.  
2. It can provide a static public IP shared by multiple VMs.  
3. It does not provide inbound internet access to your VMs.  

## Traffic Manager.
Azure Traffic Manager is a DNS-based global load balancer that distributes user traffic across multiple application endpoints located in different Azure regions, datacenters, or even external environments. It is used used for failover, performance routing, and geographic routing between Azure regions. It improves availability, performance, and disaster recovery by directing users to the most appropriate endpoint.  It is an Azure service you create to decide which location users should be sent to.  
In the Azure portal.
```text
Azure Portal
 └─ Network foundation
      └─ Traffic Manager Profile
           └─ Create
```  
It is a managed Azure networking service. You don't install it on a VM. Microsoft runs it as a platform service.
Traffic Manager can direct traffic to:  
Azure App Services  
Azure Virtual Machines  
Public Load Balancers  
AKS Clusters  
External websites  
On-premises servers


# Load balancing and content delivery

A load balancer is a service that distributes incoming network traffic across multiple servers or resources so that no single server becomes overloaded.  
Azure Load Balancer: Distributes traffic between servers in the same region/VNet.  
Azure Traffic Manager: Directs users to different regions using DNS.  
Azure Application Gateway: Layer 7 load balancer for web apps (HTTP/HTTPS) with features like WAF.  


Service commonly found under load balancing and content delivery  

```text 
Load balancing and content delivery
├─ Azure Load Balancer
├─ Azure Application Gateway
├─ Azure Front Door
├─ Azure Traffic Manager
├─ Azure CDN
└─ Web Application Firewall (WAF)
```
### What each does
Azure Load Balancer  
Layer 4 (TCP/UDP)  
Distributes traffic across VMs within a region  
Internal or public load balancing  

Azure Application Gateway  
Layer 7 (HTTP/HTTPS)  
URL-based routing  
SSL termination  
Web Application Firewall (WAF)  

Azure Front Door  
Global Layer 7 load balancing  
Accelerates web traffic worldwide  
WAF support  
Modern replacement for many global web routing scenarios  

Azure Traffic Manager    
DNS-based global traffic routing  
Regional failover  
Performance-based routing  
Geographic routing  
Does not proxy the traffic itself.  
Manage, distribute, and optimize your applications and workload.  

### Way to Remember
Traffic Manager = Which REGION should the user go to?  

Load Balancer = Which SERVER should the user go to?  

Application Gateway = Which WEB APP path should the user go to?  

Front Door = Global web acceleration and routing.  

CDN = Cache content near the user.  

### Each service has a different job:
Traffic Manager chooses the region.  
Application Gateway inspects HTTP/HTTPS requests.  
Load Balancer distributes traffic among servers.  
CDN caches static content.  
Front Door can provide global web routing and acceleration.  


## Application Gateway
Azure Application Gateway is a web traffic load balancer (Layer 7) that manages HTTP/HTTPS traffic for web applications.  
The Application Gateway can inspect the URL, host name, headers, and cookies before deciding where to send traffic. 

Azure Load Balancer (Layer 4) Looks only at: IP Address, TCP/UDP Port
Azure Application Gateway (Layer 7) Looks at: URL, Hostname, HTTP Headers, Cookies.  


## Route Server,
Azure Route Server is a service that lets your network virtual appliances (NVAs), such as firewalls or routers, automatically exchange routes with Azure using the BGP (Border Gateway Protocol).

Without Route Server: You have to manually update routes: Azure VNet -- Route Table (manual) -- Firewall / Router  
Every network change may require updating route tables manually.  

With Route Server: Routes are learned automatically:  
``` text
Azure Route Server
        |
   BGP Routing
   /         \
Firewall     Router
     |
Azure VNet
```
✅ Automatic route exchange, ✅ No manual route updates, ✅ Dynamic routing, ✅ Easier management of network appliances  

# Border Gateway Protocol (BGP) 
BGP is the routing protocol that routers use to tell each other: "I know how to reach this network." It is the routing protocol that allows routers, especially those belonging to different networks, to exchange information about which IP networks they can reach.

### Hybrid connectivity: Secure communication to and from your resources in Azure
Hybrid connectivity means connecting your company's on-premises infrastructure to Microsoft Azure so both environments can communicate securely.
If the question asks...	Think about...
Connect an entire office to Azure securely over the internet	Site-to-Site VPN
Connect an individual employee's laptop to Azure	Point-to-Site VPN
Private dedicated connectivity to Microsoft cloud	ExpressRoute
Configure Azure VPN Gateway	GatewaySubnet
Represent the on-premises network in Azure	Local Network Gateway
Resolve private DNS names across environments	Azure DNS Private Resolver

## VPN Gateway,  
An Azure VPN Gateway creates an encrypted connection between an Azure Virtual Network (VNet) and another network using the public internet. You want employees and servers in Montreal to communicate securely with Azure. A VPN Gateway creates an encrypted tunnel between the office network and Azure.  
Imagine your company has:  
- An office in Montreal with servers and computers.  
- An Azure VNet hosting virtual machines in Canada Central.  
You want employees and servers in Montreal to communicate securely with Azure.  

A VPN Gateway creates an encrypted tunnel between the office network and Azure.  

Common VPN connection types:  
Type	Purpose  
Site-to-Site (S2S)	Connects an entire office network to Azure  
Point-to-Site (P2S)	Connects an individual laptop to Azure  
VNet-to-VNet	Connects two Azure VNets  

## ExpressRoute  
ExpressRoute connects your on-premises infrastructure to Microsoft cloud services through a private connectivity provider. Unlike a VPN Gateway, ExpressRoute does not send your traffic through the public internet. ExpressRoute provides a private network connection between your infrastructure and Microsoft's network.  
Why use it?  
- More predictable network performance.  
- Private connectivity.  
- High bandwidth options.  
- Suitable for large enterprise environments.  
Example: A bank needs reliable connectivity between its datacenter and Azure to support critical applications.  

Remember: ExpressRoute = private connection to Microsoft's network.  

## Azure Virtual WAN, 
Azure Virtual WAN is a networking service that simplifies connecting multiple offices, remote users, and Azure VNets through a centralized architecture.  
Imagine your company has:  
- Montreal office.  
- Toronto office.  
- Vancouver office.  
- London office.  
- Several Azure VNets.  

Without Virtual WAN, you may need to configure and manage many individual connections.  
Virtual WAN provides centralized hubs that can connect:  
- Branch offices through VPN.  
- Datacenters through ExpressRoute.  
- Remote users.  
- Azure VNets.  

## and Peering Service.
Azure Peering Service improves internet connectivity between your users or enterprise network and Microsoft's  cloud services through participating internet service providers.  
It is not the same as VNet peering.  

For example:  

Your employees in Montreal use:  
- Microsoft 365  
- Azure  
- SharePoint Online  
- Microsoft Teams  
Their traffic travels through an ISP before reaching Microsoft's network.  

Peering Service works with participating providers to optimize the routing of that traffic toward Microsoft.  

Remember: Peering Service = optimized internet routing to Microsoft services.  

```text
Compare all four
Service	Main purpose	Connection
VPN Gateway	Securely connect networks	Encrypted internet tunnel
ExpressRoute	Private enterprise connectivity	Private provider connection
Virtual WAN	Connect many networks centrally	VPN, ExpressRoute and other connections
Peering Service	Improve connectivity to Microsoft	Optimized internet routing
```

### Network security: Protect your web app and IaaS seviceces from DDos attacks and malicous actors.
## Firewall manager, Firewall, Web application Firewal, and DDos protection

### Network management and monitoring: Manage and monitor your network resources.
## Network Watcher, Az Monitor, and Az virtual Network Manager.

### Container networking: Enhanced security and observability for containerized workloads on AZ Kubernetes Service (AKS)
## Container network security and Container network observability.




### Azure Bastion
Azure Bastion is a service that you can deploy in a virtual network to connect to a virtual machine by using your browser and the Azure portal. You can also connect by using the native SSH or RDP client already installed on your local computer. The Azure Bastion service is a fully platform-managed PaaS service that you deploy inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly from the Azure portal over TLS. When you connect through Azure Bastion, your virtual machines don't need a public IP address, agent, or special client software. Azure Bastion offers four SKU tiers: Developer, Basic, Standard, and Premium. The tier you select affects the available features. 
<img src="image/howwork.png" alt="howwork" width="600">  
Think of it as a secure remote-access gateway into your Azure network.  
## Why use Azure Bastion?  
- More secure: You don't need to expose your VM to the internet with a public IP.   
- Easy access: Connect to Windows or Linux VMs directly from your browser.  
- No client installation: Browser-based RDP/SSH works through the Azure portal.   
Deployments cost money for every hour they exist, even when you're not connected. The Bastion Developer option is free, but has limitations and is available only in supported regions.  
One important networking detail: dedicated Bastion deployments require a separate subnet named AzureBastionSubnet with a /26 or larger address range. You cannot use your ordinary application subnet for Bastion.  

## Azure Firewall
Azure Firewall is a managed, cloud-based network security service that controls traffic entering, leaving, and moving between Azure networks according to rules you configure.  

Think of it as a central security guard for your Azure network.  

You just learned about Azure Bastion. The key difference is:  
- Azure Bastion lets an administrator securely connect to a VM.  
- Azure Firewall filters network traffic to help protect your resources.  
1. How Azure Firewall works  
Imagine your GreenMaze VNet contains two subnets:  

<img src="image/firewall.png" alt="firewall" width="600">   

For example, you could configure a firewall rule to allow your VM to access approved websites while blocking connections to other destinations. To make traffic follow the firewall, you may also need to configure Azure routes.  

Azure Firewall is stateful: it tracks connections and automatically allows the return traffic for permitted connections.  

### Azure Firewall vs. NSG vs. Bastion 
These three services have different purposes.  

Network Security Group (NSG): Filter traffic to and from network interfaces or subnets using IPs, ports, and protocols. An NSG contains inbound and outbound security rules. Each rule can specify a source, destination, port, protocol, and whether to allow or deny the traffic. NSGs are stateful, meaning return traffic for an allowed connection is automatically permitted.  

Azure Firewall: Centralize traffic inspection and filtering across networks, with application-aware rules and additional threat protection.  

Azure Bastion: Securely connect administrators to VMs over RDP or SSH through the Azure portal.  

An NSG is usually sufficient for basic subnet and VM traffic control. Azure Firewall is useful when you need centralized rules across multiple subnets or VNets.  

An important distinction: creating a VNet and subnets does not automatically isolate the subnets from each other. You use NSGs and other network controls to enforce the security boundaries you need.





Azure DDoS Network Protection  

### Address spaces, CIDR, subnetting, and VNet design.
An Azure virtual network is a logical isolation of the Azure cloud resources.  

You can use virtual networks to provision and manage virtual private networks (VPNs) in Azure.  

You control the DNS server settings for virtual networks, and segmentation of the virtual network into subnets.  


Each virtual network has its own Classless Inter-Domain Routing (CIDR) block and can be linked to other virtual networks and on-premises networks.  

You can link virtual networks with an on-premises IT infrastructure to create hybrid or cross-premises solutions, when the CIDR blocks of the connecting networks don't overlap.






## Network connectivity

### VNet peering, VPN Gateway, ExpressRoute, and service endpoints.

## Vnet service endpoints
The key idea: You can restrict access to an Azure service so that only selected VNets can access it.  
A service endpoint allows a selected subnet in your VNet to access a supported Azure service, while allowing you to restrict that service's network access to authorized VNets/subnets.  

Important details:  
1. You enable the service endpoint on a subnet, not directly on an individual VM.  
2. You configure the Azure service's firewall to allow that subnet.  
3. Every VM in that authorized subnet can potentially access the service, subject to other security controls and permissions.  
4. You still need the correct Azure permissions (for example, Storage data-plane RBAC permissions).  

Service Endpoint: Restricts an Azure service's network access to selected VNets/subnets.  
Private Endpoint: Gives an Azure service a private IP address inside your VNet.  

## Network security

### Network Security Groups (NSGs), application security groups, and Azure Firewall basics.




## DNS and traffic routing

### Azure DNS, private DNS zones, route tables, and effective routes.




## Network troubleshooting

### Network Watcher, connection troubleshoot, IP flow verify, and packet capture.
