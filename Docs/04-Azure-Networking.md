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

Azure Bastion is a managed Azure service that lets you securely connect to a virtual machine (VM) using RDP (Windows) or SSH (Linux) through the Azure portal, without exposing the VM's public IP address.  
<img src="image/howwork.png" alt="howwork" width="600">  
Think of it as a secure remote-access gateway into your Azure network.  

Why use Azure Bastion?  
- More secure: You don't need to expose your VM to the internet with a public IP.  
- Easy access: Connect to Windows or Linux VMs directly from your browser.  
- No client installation: Browser-based RDP/SSH works through the Azure portal.   

Deployments cost money for every hour they exist, even when you're not connected. The Bastion Developer option is free, but has limitations and is available only in supported regions.  
One important networking detail: dedicated Bastion deployments require a separate subnet named AzureBastionSubnet with a /26 or larger address range. You cannot use your ordinary application subnet for Bastion.  


Azure Firewall is a managed, cloud-based network security service that controls traffic entering, leaving, and moving between Azure networks according to rules you configure.  

Think of it as a central security guard for your Azure network.  

You just learned about Azure Bastion. The key difference is:  
- Azure Bastion lets an administrator securely connect to a VM.  
- Azure Firewall filters network traffic to help protect your resources.  
1. How Azure Firewall works  
Imagine your GreenMaze VNet contains two subnets:  

<img src="firewall.png" alt="firewall" width="600">   

For example, you could configure a firewall rule to allow your VM to access approved websites while blocking connections to other destinations. To make traffic follow the firewall, you may also need to configure Azure routes.  

Azure Firewall is stateful: it tracks connections and automatically allows the return traffic for permitted connections.  

### Azure Firewall vs. NSG vs. Bastion
These three services have different purposes.
Feature	Main purpose
Network Security Group (NSG)	Filter traffic to and from network interfaces or subnets using IPs, ports, and protocols.
Azure Firewall	Centralize traffic inspection and filtering across networks, with application-aware rules and additional threat protection.
Azure Bastion	Securely connect administrators to VMs over RDP or SSH through the Azure portal.
An NSG is usually sufficient for basic subnet and VM traffic control. Azure Firewall is useful when you need centralized rules across multiple subnets or VNets.
Azure DDoS Network Protection  

### Address spaces, CIDR, subnetting, and VNet design.




## Network connectivity

### VNet peering, VPN Gateway, ExpressRoute, and service endpoints.




## Network security

### Network Security Groups (NSGs), application security groups, and Azure Firewall basics.




## DNS and traffic routing

### Azure DNS, private DNS zones, route tables, and effective routes.




## Network troubleshooting
### Network Watcher, connection troubleshoot, IP flow verify, and packet capture.