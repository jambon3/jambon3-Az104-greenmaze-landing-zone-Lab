# Azure Monitor is the central monitoring service in Azure.  

The four important components
``` text
**Component	    Purpose**
Metrics	        Numerical measurements
Logs	        Detailed records and events
Alerts	        Notify you when something happens
Dashboards	    Visualize monitoring information
```
Azure Monitor Metrics
Metrics are numerical values collected at regular intervals.  
Azure Portal → Monitor → Metrics  

You can also access them through an individual resource:  
Virtual Machine → Monitoring → Metrics.  

Log Analytics workspace is where you collect and analyze log data. Think of it as a centralized database for monitoring information. Instead of checking 70 different virtual machines individually, you can send their logs to one workspace.  

### Important tables
**Table	What it contains**

``` text
Heartbeat	    Records showing that an agent is communicating
Syslog	        Linux system logs
Event	        Windows event logs
SecurityEvent	Windows security events
Perf	        Performance counters
AzureActivity	Azure subscription management activity
```

Note: Some tables require the appropriate diagnostic settings, agent, data collection rule or connector configuration before data appears.  

Azure Monitor Logs / Log Analytics can cost money, but the cost depends on how much data you collect and how long you retain it.  

What is KQL? Kusto Query Language is used to query log data.  

Azure Monitor Agent (AMA) sends heartbeat data to a Log Analytics workspace through a Data Collection Rule (DCR).  
<img src="image/Screenshot 2026-11-12 152249.png" alt="Screenshot 2026-11-12 152249" width="600">  

examples:  
Someone deletes a VM → Activity Log.  
Someone accesses a storage account → Resource Logs, if the relevant logging is enabled.  
VM CPU reaches 90% → Metric.  

Azure Monitor Alerts  

<img src="image/a.png" alt="a" width="600">  
Alerts notify administrators when a specific condition is met.


### Metric alerts
Monitor numerical values.  

Examples:  
- CPU exceeds 85%.  
- Disk queue length becomes too high.  
- Network traffic exceeds a threshold.  

### Log search alerts
Can un a KQL query periodically and trigger an alert when the query meets a specified condition.You want to be notified when more than 10 failed logins occur within 5 minutes.
You can query the relevant security logs and configure a log search alert.
Best for: Events and patterns that require querying logs.

### Activity Log alerts
Monitor Azure subscription-level activities.  
example:  
Someone deletes a virtual machine.  
Someone deletes a network security group.  
A resource is created.  
An administrative operation fails.  

### Action Groups 
An Action Group defines what happens when an alert fires.  
Possible actions include:Send email. Send SMS. Make a phone call. Trigger a webhook. Trigger an Azure Function. Trigger an Automation Runbook. Send notifications to supported ITSM integrations.

### Alert Processing Rules
These are different from Action Groups. Alert Processing Rules allow you to modify how alerts are handled.  
For example:  
- Suppress notifications during maintenance.  
- Suppress alerts for specific resources.  
- Add or remove action groups.  
- Apply rules to specific resource groups or subscriptions.  

**Remember:**  
- Alert rule = Detect the problem.  
- Action group = Perform the action.  
- Alert processing rule = Control how alerts are processed.  

VM Insights: Monitors virtual machines, including performance, dependencies and health information. Useful for identifying CPU, memory, disk and performance problems.  

Storage Insights: Provides monitoring information for storage accounts, including capacity, transactions and availability.  

Network Insights: Helps monitor network resources, connectivity and network health.  

VM Insights and Dependency Mapping: VM Insights can help you understand dependencies between machines and their network connections.

# Azure Network Watcher

<img src="image/Neetworkwatcher.png" alt="Neetworkwatcher" width="600">  
Azure Network Watcher is particularly important.
It helps diagnose network connectivity issues in Azure.  
<img src="image/features-networkwatch.png" alt="features-networkwatch" width="600">
### IP Flow Verify
IP Flow Verify tells you whether the traffic is allowed or denied by the applicable NSG rules and identifies the rule responsible.

### Next Hop
You want to understand where Azure routes the traffic.  
It can identify:- Internet- Virtual network- Virtual network gateway- Virtual appliance  

IP Flow Verify = Is the traffic allowed?  
Next Hop = Where will the packet go?  
Connection Troubleshoot = Why can't these endpoints communicate?  
Connection Monitor = Is connectivity consistently healthy over time?  
Packet Capture = What packets are actually being transmitted?  



# Backup & Recovery
Azure Backup is Microsoft's backup service for protecting Azure and supported on-premises workloads.

What can Azure Backup protect? Azure Virtual Machines. Azure Files. SQL Server running in Azure VMs. On-premises Windows machines through the MARS agent. Workloads protected through DPM or Microsoft Azure Backup Server. Other supported workloads using the appropriate vault and protection configuration.

### Recovery Services Vault vs Backup Vault
Azure has two important vault types.

<img src="image/Recovservvsbackupvault.png" alt="Recovservvsbackupvault" width="600">

<img src="image/2.png" alt="2" width="600">
Recovery Services vaults support Azure Site Recovery. Backup vaults do not replace Recovery Services vaults for Site Recovery.

Creating a Recovery Services Vault
Typical process:
1. Open Azure Portal.
2. Search for Recovery Services vaults.
3. Select Create.
4. Choose subscription.
5. Choose resource group.
6. Choose vault name.
7. Choose region.
8. Review and create.
<img src="image/createrevoceryservicevault.png" alt="createrevoceryservicevault" width="600">

**Important considerations**
A vault is a separate Azure resource.  
Its region is important for data protection and recovery planning.  
Backup redundancy options affect resilience.  
Vault access and security settings must be managed.   
A vault is not itself a backup policy.  

### Storage redundancy
Know these options:  
LRS: Copies data within one physical location in a region.  
ZRS: Replicates data across availability zones in a region.  
GRS: Replicates data to a secondary region.  


### Azure Backup Policies
A backup policy determines when backups occur and how long recovery points are retained.  

**Important policy components**
```text
**Setting**	            **Purpose**
Backup frequency	How often backups occur
Backup schedule	    When backups run
Retention	        How long recovery points are kept
Instant restore	    Provides short-term snapshot-based recovery, depending on workload
```