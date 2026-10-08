  A resource is an individual Azure service that you deploy inside a resource group. A resource cannot exist independently of a resource group.  

  when naming resources, Microsoft recommends including components such as resource type, workload/project, environment, region, and instance number, while remembering that each Azure resource type has its own naming restrictions.  
  
  ```text
| Virtual Machine | `vm-win-01` |
| Storage Account | `stgreenmazedev01` |
| Virtual Network | `vnet-greenmaze-dev` |
| Network Security Group | `nsg-greenmaze-dev` |
| Key Vault | `kv-greenmaze-dev` |
| App Service | `app-greenmaze-dev` |
| SQL Database | `sqldb-greenmaze-dev` |
```
  
## Resources have properties &  dependencies
A resource isn't just a name.  
For example, a VM has properties such as:  
- Name  
- Region  
- Size  
- Operating system  
- Disk  
- Network interface  
- Virtual network  
- Subnet  
- Tags  
- Identity  
- Access permissions  
So when you create a resource, you're configuring its desired state.  

## Azure services are exposed through resource providers. / Resource Types
```text
| Full resource type                  | Provider             | Resource type     |
| ----------------------------------- | -------------------- | ----------------- |
| `Microsoft.Compute/virtualMachines` | `Microsoft.Compute`  | `virtualMachines` |
| `Microsoft.Storage/storageAccounts` | `Microsoft.Storage`  | `storageAccounts` |
| `Microsoft.Network/virtualNetworks` | `Microsoft.Network`  | `virtualNetworks` |
| `Microsoft.KeyVault/vaults`         | `Microsoft.KeyVault` | `vaults`          |
| `Microsoft.Web/sites`               | `Microsoft.Web`      | `sites`           |
```

Azure provides many different types of resources. Each resource represents an Azure service or component that can be deployed, configured, and managed.

Common Azure resource types include:

| Category | Resource Examples |
|---|---|
| Compute | Virtual Machines, Virtual Machine Scale Sets, App Service |
| Networking | Virtual Network, Subnet, Network Security Group, Public IP, Load Balancer |
| Storage | Storage Account, Managed Disk, Blob Container |
| Databases | Azure SQL Database, Azure Database for PostgreSQL |
| Security | Key Vault, Managed Identity |
| Monitoring | Log Analytics Workspace, Application Insights |
| Containers | Azure Container Instances, Azure Kubernetes Service |
| Integration | Logic Apps, Service Bus, Event Grid |

For example, a virtual machine is a compute resource:

`Microsoft.Compute/virtualMachines`

A virtual network is a networking resource:

`Microsoft.Network/virtualNetworks`

A storage account is a storage resource:

`Microsoft.Storage/storageAccounts`

Different resource types have different properties, dependencies, configuration options, and management requirements.

Resources can also depend on other resources. For example, a virtual machine commonly uses a network interface, virtual network, subnet, network security group, public IP address, and managed disk.

Understanding resource types is important because Azure administrators need to know what resources are being deployed, how they interact with each other, and how they should be organized and managed within resource groups and subscriptions.

# Tags
Microsoft describes them as key-value metadata used to identify and organize Azure resources. For example: Environment = Production.


### Common Tags

Environment = Development
Workload = Perforce
Department = IT
Owner = IT
CostCenter = IT
Project = GreenMaze

### Where Can Tags Be Applied?
- Subscriptions
- Resource Groups
- Resources
- Not Management Groups

### Tag Inheritance
- Tags aren't automatically inherited
- Azure Policy can enforce/inherit tags
- Cost Management has separate tag inheritance

### Tags and Cost Management
- Group costs
- Identify ownership
- Track departments/projects/environments

### Tags and Azure Policy
- Require tags
- Require specific tag values
- Add/inherit tags

### Tag Limitations
- Not all resources support tags
- Tag limits
- Naming restrictions

### Security Consideration
- Tags are plain text
- Never store secrets