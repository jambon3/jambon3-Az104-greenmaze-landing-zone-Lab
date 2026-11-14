# AZ-104: Deploy and Manage Azure Compute Resources

## Overview

Azure compute resources provide the processing power required to run applications, services, and workloads in the cloud.

In traditional IT environments, administrators manage physical servers, VMware virtual machines, and operating systems. In Azure, compute workloads can run using several services, including:

- Azure Virtual Machines
- Azure Virtual Machine Scale Sets
- Azure App Service
- Azure Container Instances
- Azure Resource Manager (ARM) templates and Bicep

The goal of this section is to understand how to deploy, configure, manage, and scale Azure compute resources.

---

## 1. Azure Virtual Machines

An Azure Virtual Machine (VM) is an Infrastructure as a Service (IaaS) resource that provides a virtualized computer running in Microsoft's Azure infrastructure.

A VM can run different operating systems, including:

- Windows Server
- Windows 11
- Ubuntu Linux
- Red Hat Enterprise Linux

### Important VM components

| Component | Description |
|---|---|
| VM Size | Determines CPU, RAM, and available performance |
| Image | Operating system template used to create the VM |
| OS Disk | Contains the operating system |
| Data Disk | Additional storage attached to the VM |
| NIC | Network interface connecting the VM to a virtual network |
| NSG | Controls inbound and outbound network traffic |
| Public IP | Provides internet-facing connectivity when configured |
| Private IP | Allows communication within private networks |
| Availability Zone | Separate physical datacentre location within an Azure region |
| Availability Set | Helps protect VMs against certain hardware failures and planned maintenance events |

### VM sizes

A VM size determines the computing resources available to the virtual machine.

Example sizes:

| VM Size | vCPU | RAM |
|---|---:|---:|
| B2s | 2 | 4 GB |
| D2s v5 | 2 | 8 GB |
| D4s v5 | 4 | 16 GB |
| D8s v5 | 8 | 32 GB |

Actual availability depends on the Azure region, subscription quotas, and current VM offerings.

### Important VM operations

For AZ-104, understand how to:

- Create and delete virtual machines.
- Start, stop, restart, and redeploy VMs.
- Resize a VM.
- Attach and detach data disks.
- Configure network interfaces.
- Configure boot diagnostics.
- Connect using RDP or SSH.
- Configure VM extensions.
- Configure disk encryption and storage settings.

### Stop vs. Deallocate

| Operation | Description |
|---|---|
| Stop inside the OS | Shuts down the operating system. Compute resources may remain allocated. |
| Stop (deallocate) | Releases the underlying compute allocation. |
| Restart | Reboots the virtual machine. |
| Redeploy | Moves the VM to another Azure host. |

**Important:** Deallocating a VM generally stops its compute charges, but attached storage and other associated resources may continue generating costs.

### Scaling a VM

- **Scale up:** Increase CPU and RAM by selecting a larger VM size.
- **Scale down:** Reduce the VM size to use fewer resources.

Resizing may require the VM to be stopped or deallocated, depending on the target size and availability.

---

## 2. Azure Virtual Machine Scale Sets (VMSS)

Virtual Machine Scale Sets allow administrators to deploy and manage groups of virtual machines as a single resource.

They are useful when applications require multiple VM instances to handle incoming traffic or changing workloads.

### Example architecture

```text
                Internet Users
                      |
                      v
                Load Balancer
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
        VM 1        VM 2        VM 3
          |           |           |
          +-----------+-----------+
                VM Scale Set
```

The scale set manages the VM instances according to its configuration.

### Scaling methods

**Horizontal scaling**

- Scale out: Add more VM instances.
- Scale in: Remove VM instances.

**Vertical scaling**

- Scale up: Increase the CPU and RAM of an individual VM.
- Scale down: Decrease the CPU and RAM of an individual VM.

### Autoscaling example

Imagine a company website normally requires two VM instances but experiences increased traffic during business hours.

An autoscaling configuration could be:

| Setting | Example |
|---|---:|
| Minimum instances | 2 |
| Default instances | 2 |
| Maximum instances | 5 |
| Scale-out condition | CPU above 75% |
| Scale-in condition | CPU below a defined threshold |

Autoscaling rules can use metrics such as CPU utilization and other supported metrics.

**AZ-104 exam tip:** If a question asks how to automatically add VM instances when CPU utilization increases, consider VM Scale Set autoscaling.

---

## 3. Azure App Service

Azure App Service is a Platform as a Service (PaaS) offering used to host web applications and APIs.

Unlike an Azure VM, App Service does not require the administrator to manage the underlying virtual machine or operating system.

### Example use cases

A company needs to host:

- A public website.
- An employee portal.
- A customer portal.
- A web API.

Azure App Service can host these applications without requiring the company to maintain the underlying server infrastructure.

### App Service Plan

An App Service Plan defines the compute resources used by App Service applications.

It determines:

- Region.
- Operating system.
- Pricing tier.
- Available compute capacity.
- Number of instances.
- Scaling capabilities.

Multiple Web Apps can share the same App Service Plan and its compute resources.

This means that applications sharing a plan can affect one another's performance if they compete for available resources.

### Important App Service concepts

| Concept | Description |
|---|---|
| App Service Plan | Defines the compute environment and pricing tier |
| Web App | The actual hosted application |
| Deployment Slots | Allows testing a new application version before production |
| Custom Domain | Allows the application to use a company domain name |
| TLS/SSL | Secures connections using HTTPS |
| Scaling | Increases resources or the number of instances |
| Backup | Backs up supported application content and configuration |
| Networking | Controls application connectivity and access |

### Scale up vs. Scale out

**Scale up (vertical scaling)**

Increase the compute resources available by selecting a higher App Service Plan tier.

**Scale out (horizontal scaling)**

Increase the number of instances running the application.

### Deployment slots

Deployment slots allow administrators to deploy and test new application versions before making them available to production users.

Example:

```text
             Before Swap

       Production       Staging
       Version 1        Version 2
           |                |
           +-------+--------+
                   |
                Swap
                   |
                   v

             After Swap

       Production       Staging
       Version 2        Version 1
```

A typical workflow:

1. Deploy the new application version to the staging slot.
2. Test the application.
3. Verify that the application works correctly.
4. Swap staging with production.
5. Monitor the production application.

Deployment slots are available only in supported App Service tiers.

---

## 4. Azure Container Instances (ACI)

Azure Container Instances allow administrators to run containerized applications without managing the underlying virtual machines.

### What is a container?

A container packages an application and its dependencies into a portable unit.

For example, a Python application might require:

- Python runtime.
- Specific libraries.
- Application files.
- Configuration files.

Instead of installing these dependencies manually on a VM, the developer packages them into a container image.

The image can then be used to create a running container.

### VM vs. Container

| Virtual Machine | Container |
|---|---|
| Runs its own guest operating system | Shares the host OS kernel |
| Requires more infrastructure management | Requires less infrastructure management |
| Can run different guest operating systems | Must be compatible with the host kernel |
| Suitable for full server workloads | Suitable for packaged application workloads |

### Container groups

In Azure Container Instances, a container group is the top-level resource that contains one or more containers.

Containers in the same group can share:

- Network resources.
- Storage volumes.
- Lifecycle.

**AZ-104 exam tip:** If a requirement asks you to run a containerized application without managing virtual machines, consider Azure Container Instances.

---

## 5. ARM Templates and Bicep

Infrastructure as Code (IaC) allows administrators to define and deploy infrastructure using code instead of manually creating every resource.

For example, an environment might require:

- One virtual network.
- Two subnets.
- Three virtual machines.
- One NSG.
- One storage account.

Instead of manually creating these resources repeatedly, you can define them in a template and deploy the environment consistently.

### Azure Resource Manager (ARM)

ARM is the deployment and management service for Azure resources.

ARM templates use JSON to describe the resources that should be deployed.

### Bicep

Bicep is a declarative language designed to simplify Azure infrastructure deployments.

Example Bicep template:

```bicep
param location string = 'canadaeast'

resource storage 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'greenmazestorage01'
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

This template defines an Azure Storage Account.

### What to understand for AZ-104

- What ARM templates are.
- What Bicep is.
- How to deploy a template.
- How to review deployment status.
- How to identify and troubleshoot deployment failures.

You should understand the purpose and deployment process rather than trying to become an advanced Bicep developer.

---

## 6. Comparing Azure Compute Services

| Requirement | Azure Service |
|---|---|
| Need complete control over Windows Server or Linux | Azure Virtual Machine |
| Need multiple identical VMs that scale automatically | Virtual Machine Scale Sets |
| Need to host a website without managing VMs | Azure App Service |
| Need to run containers without managing VMs | Azure Container Instances |
| Need to deploy infrastructure repeatedly using code | ARM Templates / Bicep |

---

## 7. Practical Azure Portal Exercises

Use a test subscription and monitor costs before deploying resources.

### Virtual Machines

- Explore VM creation without completing deployment.
- Understand VM sizes and operating system images.
- Review OS disks and data disks.
- Explore network interfaces and NSGs.
- Understand start, stop, and deallocate operations.

### Virtual Machine Scale Sets

- Explore VM Scale Set creation.
- Understand instance counts.
- Review autoscaling settings.
- Understand scale-out and scale-in rules.

### App Service

- Create a basic App Service.
- Understand the relationship between a Web App and an App Service Plan.
- Explore scaling settings.
- Review deployment slots.
- Explore application configuration and networking.

### Containers

- Explore Azure Container Instances.
- Understand container images and running containers.
- Review container group networking.

### Infrastructure as Code

- Review an ARM template.
- Review a Bicep template.
- Understand how deployments are submitted and monitored.

---

## 8. AZ-104 Practice Questions

### Question 1

You need to host a website without managing Windows Server or Linux.

What should you use?

A. Azure Virtual Machine  
B. Azure App Service  
C. Azure Virtual Network  

**Answer: B — Azure App Service**

App Service is PaaS. Microsoft manages the underlying infrastructure and operating system.

### Question 2

Your application needs more VM instances automatically when CPU utilization increases.

What should you configure?

A. VM Availability Set  
B. VM Disk Encryption  
C. VM Scale Set Autoscaling  

**Answer: C — VM Scale Set Autoscaling**

VM Scale Set autoscaling adjusts the number of VM instances based on configured metrics and rules.

### Question 3

You want to test a new website version before making it available to customers.

What should you use?

A. Deployment Slots  
B. Availability Zones  
C. Azure Bastion  

**Answer: A — Deployment Slots**

A staging deployment slot allows testing before swapping the new version into production.

### Question 4

You want to run a container without managing the underlying VM.

Which service should you use?

A. Azure Virtual Desktop  
B. Azure Container Instances  
C. Azure Files  

**Answer: B — Azure Container Instances**

ACI runs containers without requiring you to manage the host VM.

### Question 5

You want to deploy the same Azure infrastructure repeatedly using code.

What should you use?

A. RDP  
B. Azure Monitor  
C. ARM Templates or Bicep  

**Answer: C — ARM Templates or Bicep**

Both allow infrastructure to be defined declaratively and deployed consistently.

---

## 9. Recommended Learning Order

Do not try to master every compute service simultaneously.

Recommended order:

1. **Azure Virtual Machines:** Understand VM creation, images, sizes, disks, networking, and management.
2. **VM Scale Sets:** Understand instance management, scaling, and availability.
3. **Azure App Service:** Understand PaaS, plans, slots, networking, and configuration.
4. **Azure Container Instances:** Understand container images, containers, and container groups.
5. **ARM Templates and Bicep:** Understand infrastructure as code and repeatable deployments.

### Final Study Reminder

Azure compute is not limited to virtual machines.

The AZ-104 exam tests whether you understand which compute service is appropriate for a particular business requirement.

The important distinction is knowing when to use IaaS, PaaS, containers, and infrastructure as code.

**Study goal:** Be able to explain what each service does, when to use it, how to deploy it, and how to manage it through the Azure Portal.

# AZ-104 — Remaining Compute Topics to Study

## Overview

This document tracks the remaining topics in the AZ-104 **Deploy and Manage Azure Compute Resources** section.

The introductory concepts have already been covered. This guide focuses on the advanced configuration, management, deployment, and troubleshooting topics that still require study and hands-on practice.

---

# 1. Azure Virtual Machines — Advanced Management

Previously covered: VM creation, sizes, images, disks, NICs, IP addresses, availability, and basic lifecycle operations.

## 1.1 VM Extensions

- Understand what VM extensions are.
- Install and configure VM extensions.
- Understand how extensions run after VM deployment.
- Troubleshoot extension installation failures.

## 1.2 Custom Script Extension

- Run PowerShell scripts on Windows VMs.
- Run shell scripts on Linux VMs.
- Understand how scripts are downloaded and executed.
- Identify common troubleshooting scenarios.

## 1.3 VM Encryption

- Understand encryption at host.
- Understand the difference between encryption at host and Azure Disk Encryption.
- Know which VM resources and disks are protected.

## 1.4 Moving Virtual Machines

- Move VMs between resource groups.
- Move supported resources between subscriptions.
- Understand resource move limitations and dependencies.
- Understand moving VMs between Azure regions.

## 1.5 VM Images and Capture

- Understand how to capture a VM image.
- Understand managed images and Azure Compute Gallery.
- Understand how to create additional VMs from an image.
- Know the difference between a VM image and a snapshot.

## 1.6 Managed Disk Management

- Change disk size and performance settings.
- Understand disk types and performance tiers.
- Attach and detach managed disks.
- Understand disk snapshots and disk encryption.

---

# 2. VM Scale Sets — Advanced Configuration

Previously covered: The purpose of VM Scale Sets and the basics of autoscaling.

Remaining topics:

- Create and configure VM Scale Sets.
- Configure autoscaling rules.
- Configure minimum, maximum, and default instance counts.
- Understand scaling based on metrics.
- Manage individual VM instances.
- Understand VMSS upgrade policies.
- Understand how load balancing integrates with VM Scale Sets.

---

# 3. Azure App Service — Configuration and Management

Previously covered: App Service Plans, Web Apps, deployment slots, pricing tiers, and basic scaling.

Remaining topics:

- Configure custom DNS names.
- Configure TLS/SSL certificates.
- Configure App Service backups and restoration.
- Configure networking settings.
- Understand VNet integration.
- Understand access restrictions.
- Understand private endpoints for App Service.
- Configure application settings and connection strings.
- Understand deployment slot settings and slot swaps.
- Configure scaling rules and autoscaling.

---

# 4. Azure Containers — Additional Services

Previously covered: Container fundamentals and an introduction to Azure Container Instances (ACI).

The following services and concepts still require study.

## 4.1 Azure Container Registry (ACR)

- Understand what a container registry is.
- Create and configure an Azure Container Registry.
- Store and manage container images.
- Understand image repositories and tags.
- Understand authentication and access to ACR.
- Deploy containers using images stored in ACR.

## 4.2 Azure Kubernetes Service (AKS)

- Understand Kubernetes fundamentals.
- Understand clusters, nodes, and pods.
- Understand deployments and services.
- Understand how AKS manages containerized applications.
- Understand scaling in AKS.
- Understand the relationship between AKS and ACR.

## 4.3 Azure Container Apps

- Understand what Azure Container Apps is.
- Understand environments and container apps.
- Configure container sizing.
- Configure scaling rules.
- Understand revisions and replicas.
- Understand event-driven scaling.
- Compare Azure Container Apps with ACI and AKS.

## 4.4 Container Service Comparison

| Service | Main Purpose |
|---|---|
| Azure Container Instances (ACI) | Run containers without managing VMs |
| Azure Container Registry (ACR) | Store and manage container images |
| Azure Kubernetes Service (AKS) | Orchestrate containers using Kubernetes |
| Azure Container Apps | Run and scale containerized applications without managing Kubernetes infrastructure |

---

# 5. Azure Functions — Serverless Computing

**Status:** Not covered yet.

## 5.1 Fundamentals

- Understand serverless computing.
- Understand how Azure Functions works.
- Understand Function Apps.
- Understand triggers and bindings.

## 5.2 Function Triggers

- HTTP triggers.
- Timer triggers.
- Queue triggers.
- Blob Storage triggers.
- Event-driven execution.

## 5.3 Management

- Create and configure Function Apps.
- Understand hosting plans.
- Configure application settings.
- Understand scaling.
- Monitor and troubleshoot function execution.

### Example: Blob Storage Trigger

A user uploads a file to Azure Blob Storage.

An Azure Function automatically executes code when the upload event occurs. The function can then process the file without requiring an administrator to manually start a server or script.

```text
User uploads file
       |
       v
Azure Blob Storage
       |
       v
Blob Storage Trigger
       |
       v
Azure Function executes
       |
       v
File processing
```

---

# 6. Infrastructure as Code — ARM Templates and Bicep

Previously covered: The purpose of ARM templates and Bicep, including a basic Bicep example.

The following topics still require study.

## 6.1 ARM Templates

- Understand ARM template JSON structure.
- Understand parameters, variables, and resources.
- Understand outputs and template functions.
- Modify existing ARM templates.
- Deploy resources using ARM templates.
- Export existing deployments as ARM templates.
- Troubleshoot deployment failures.

## 6.2 Bicep

- Understand Bicep syntax.
- Define parameters and variables.
- Define Azure resources.
- Understand resource dependencies.
- Modify existing Bicep templates.
- Deploy resources using Bicep.
- Understand how Bicep compiles into ARM JSON.
- Convert ARM templates to Bicep.

## 6.3 Automation Tools

- Deploy resources using Azure PowerShell.
- Deploy resources using Azure CLI.
- Understand when to use the Azure Portal, PowerShell, CLI, or Infrastructure as Code.

---

# 7. Recommended Study Order

To build knowledge progressively, follow this order:

1. **VM Extensions and Custom Script Extension** — Learn how to configure and automate VM management.
2. **VM Encryption and Disk Management** — Understand security and storage configuration.
3. **Moving VMs and VM Images** — Learn resource migration and image deployment.
4. **VM Scale Sets** — Study advanced autoscaling and instance management.
5. **App Service Advanced Configuration** — Focus on DNS, TLS, networking, backups, and deployment slots.
6. **Azure Container Registry** — Understand image storage and management.
7. **Azure Kubernetes Service** — Learn Kubernetes fundamentals and orchestration.
8. **Azure Container Apps** — Understand managed container hosting and scaling.
9. **Azure Functions** — Study serverless computing and event-driven execution.
10. **ARM Templates and Bicep** — Learn infrastructure automation and deployment.
11. **Azure PowerShell and CLI** — Practice deploying and managing resources through commands.

---

# 8. Final Study Objective

By completing these topics, you should be able to:

- Configure and troubleshoot Azure virtual machines.
- Automate VM configuration using extensions and scripts.
- Manage VM storage, encryption, images, and migration.
- Configure and manage VM Scale Sets.
- Configure App Service networking, security, and deployment.
- Understand the differences between ACI, ACR, AKS, and Azure Container Apps.
- Deploy and manage serverless applications using Azure Functions.
- Automate Azure infrastructure deployments using ARM templates, Bicep, PowerShell, and Azure CLI.

**Important:** Understanding a concept is only the first step. For AZ-104 preparation, practice deploying resources, changing configurations, troubleshooting failures, and answering scenario-based questions through the Azure Portal and command-line tools.

This document is a personal study tracker. Always cross-check the current Microsoft AZ-104 skills measured document for the official exam objectives.
