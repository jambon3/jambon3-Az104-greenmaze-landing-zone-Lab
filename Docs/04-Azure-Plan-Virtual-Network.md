### Plan virtual network

<img src="image/planvnet.png" alt="planvnet" width="600">   



## Things to consider when using virtual networks
<img src="image/plan.png" alt="plan" width="600">  


These three scenarios explain why you would create an Azure Virtual Network (VNet) and how it can connect cloud resources to each other, the internet, or your company's on-premises network.

Think of an Azure VNet as a private network in the cloud, similar to the LAN in your office.  

## 1. Create a dedicated private cloud-only virtual network
Example: Imagine you're building a test environment for your GreenMaze AZ-104 lab.  
- You create a VNet called VNet-GreenMaze.  
- Address space: 10.0.0.0/16.  
- You create a subnet for your virtual machines.  
- Two VMs communicate over the VNet's private IP addresses.  
- You configure internet access only for resources that need it.  

<img src="image/vnet-greenmaze.png" alt="vnet-greenmaze" width="600">  

<img src="image/addspace.png" alt="addspace" width="600">  

<img src="image/subnetvm.png" alt="subnetvm" width="600">   

Why do we use 10.0.1.0/24?  
Your VNet is 10.0.0.0/16, which covers a large range of private IP addresses. The subnet 10.0.1.0/24 reserves a smaller block inside it.  
- 10.0.1.0 — subnet network address.  
- 10.0.1.1 through 10.0.1.254 — 254 addresses in the subnet; Azure reserves five addresses, leaving 251 for other allocations.  
- 10.0.1.255 — broadcast address in conventional IPv4 subnetting; Azure reserves this address too.  
AZ-104 exam tip: Azure reserves five IP addresses in every IPv4 subnet.  
10.0.1.0 — network address  
10.0.1.1 — Azure-reserved  
10.0.1.2 — Azure-reserved  
10.0.1.3 — Azure-reserved  
10.0.1.4 through 10.0.1.254 — 251 addresses available for allocation  
10.0.1.255 — Azure-reserved   

A /24 has 256 total addresses, so \(256-5=251\) are available for Azure resources.  
The VMs can communicate privately, subject to network security groups (NSGs), routes, firewalls, and other controls.  

Important: A VNet does not automatically give every VM internet access, and a private IP address alone does not make a resource secure. Internet connectivity depends on the network configuration.  

When would you use this? Development environments, application testing, cloud-hosted services, or workloads that don't need to communicate with an on-premises data center.  


Step 2 — Create your first VM
In the Azure portal:
1. Search for Virtual machines.
2. Click + Create → Azure virtual machine.
3. On the Basics tab, enter:  

Virtual machine name: VM-GreenMaze-01  
Region: Canada East  
Image: Ubuntu Server LTS  
Size: A small supported size suitable for a lab  
Authentication type: SSH public key  
Username: azureuser  

Choose your existing subscription and resource group.  
  Step 3 — Connect the VM to your subnet
On the Networking tab, configure:
Setting	Value
Virtual network	VNet-GreenMaze
Subnet	Subnet-VMs (10.0.1.0/24)
Public IP	None
NIC network security group	Basic, if offered
Public inbound ports	None

Keep the VM private. You can use Azure Bastion or another configured private management method later to access it.

Click Review + create, then Create.
Step 4 — Create your second VM
Repeat the process with these changes:
Setting	Value
VM name	VM-GreenMaze-02
Virtual network	VNet-GreenMaze
Subnet	Subnet-VMs
Public IP	None

Both VMs must be connected to the same VNet and subnet for this exercise.
Step 5 — Verify private connectivity
Once both VMs are deployed:
1. Open each VM in the Azure portal.
2. Go to its Networking page or network interface.
3. Record both private IP addresses.
For example:
- VM1: 10.0.1.4
- VM2: 10.0.1.5
These are examples; Azure may assign different addresses.

When you have a secure way to connect to VM1, you can test VM2's private address from VM1:
ping 10.0.1.5


Replace that example IP with VM2's actual address. Ping may fail if the operating system firewall blocks ICMP, even when the VNet is working correctly.
Step 6 — Configure internet access only if needed
You don't need to configure internet access just to let the two VMs communicate privately.

If your VMs need outbound internet access for updates or downloads, you can configure an appropriate method such as an Azure NAT Gateway or Azure Firewall. A NAT Gateway allows outbound connectivity without assigning public IP addresses to the VMs, but it has a cost.

## Things to consider when using subnets

When you plan for adding subnet segments within your virtual network, there are several factors to consider.

Consider service requirements. Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets. A service might require or create their own subnet. There must be enough unallocated space to meet the service requirements. Suppose you connect a virtual network to an on-premises network by using Azure VPN Gateway. The virtual network must have a dedicated subnet for the gateway.

Consider network virtual appliances. Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.

Consider network security groups. You can associate zero or one network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.

Consider private links. Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.

