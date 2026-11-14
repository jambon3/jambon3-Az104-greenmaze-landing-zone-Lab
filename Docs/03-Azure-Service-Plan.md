Azure App Service Plans  
## 1. What is an App Service Plan?

An **Azure App Service Plan** defines the compute resources used to run web applications hosted in Azure App Service.

Think of it as renting computing capacity from Microsoft to host websites, web applications, and APIs without managing the underlying virtual machines.

**Key distinction:**

- **App Service:** The web application itself.
- **App Service Plan:** The compute infrastructure that runs the application.

### Example

A company has three web applications:

- Company website
- Customer portal
- Internal employee application

All three applications can run inside the same App Service Plan.

They share the plan's underlying compute resources, including CPU and memory.

---

## 2. App Service Plan Architecture

```text
Azure Subscription
│
└── Resource Group
    │
    └── App Service Plan
        │
        ├── Compute Instance 1
        ├── Compute Instance 2
        │
        ├── Web App 1 - Company Website
        ├── Web App 2 - Customer Portal
        └── Web App 3 - Internal Application
```

**Important:** Multiple Web Apps can share the same App Service Plan and its compute resources.

---

## 3. App Service Plan Pricing Tiers

The pricing tier determines the available computing resources, performance, and features.

| Tier | Purpose | Main Characteristics |
|---|---|---|
| Free (F1) | Learning and testing | Limited resources |
| Shared (D1) | Basic hosting | Shared infrastructure |
| Basic (B1-B3) | Small production workloads | Dedicated compute |
| Standard (S1-S3) | Production workloads | Autoscaling and deployment slots |
| Premium (P*v3, etc.) | High-performance workloads | More resources and advanced features |
| Isolated (I*v2) | Enterprise workloads | Isolated hosting environment |

*Exact features and available SKUs depend on the operating system, region, and current Azure offerings.*

### AZ-104 Exam Notes

- Free and Shared tiers have significant limitations.
- Basic and higher tiers provide dedicated compute resources.
- Standard and higher tiers support additional production features.
- Premium tiers provide more powerful compute options.
- Isolated tiers are designed for workloads requiring dedicated and isolated environments.

---

## 4. Scaling: Scale Up vs. Scale Out

Understanding the difference between vertical and horizontal scaling is important for AZ-104.

### Scale Up (Vertical Scaling)

Increase the computing power of the existing instances.

For example:

```text
Before:
2 CPU cores
4 GB RAM

After:
4 CPU cores
8 GB RAM
```

You upgrade the App Service Plan to a more powerful pricing tier or instance size.

**Use case:** Your application needs more CPU or memory.

### Scale Out (Horizontal Scaling)

Increase the number of instances running your application.

```text
Before:

[Web App - Instance 1]


After:

[Web App - Instance 1]
[Web App - Instance 2]
[Web App - Instance 3]
```

Azure distributes application traffic across the available instances.

**Use case:** Your application receives increased traffic and needs more instances to handle requests.

### Autoscaling

Azure can automatically increase or decrease the number of instances based on predefined conditions.

Examples:
- CPU utilization exceeds 70%.
- Application traffic increases.
- CPU utilization decreases and fewer instances are required.

Autoscaling helps maintain performance while controlling costs.

---

## 5. Important App Service Plan Concepts

| Concept | Explanation |
|---|---|
| Region | Geographic location where the plan's compute resources run |
| Operating System | Windows or Linux |
| Instance | Compute instance running the application |
| Autoscale | Automatically adjusts the number of instances |
| Deployment Slots | Separate environments for testing and deploying application updates |
| Pricing Tier | Determines available resources and features |

### Important Rules

1. Multiple Web Apps can share one App Service Plan.
2. Applications sharing a plan share its compute resources.
3. An App Service Plan is associated with a specific Azure region.
4. Windows and Linux Web Apps cannot share the same App Service Plan.
5. Scaling the plan can affect all applications sharing it.
6. Deleting an App Service Plan can affect all applications hosted within it.

---

## 6. App Service vs. Azure Virtual Machines

App Service is a Platform as a Service (PaaS) solution.

A Virtual Machine is an Infrastructure as a Service (IaaS) solution.

| Azure Virtual Machine (IaaS) | Azure App Service (PaaS) |
|---|---|
| Manage the operating system | Microsoft manages the underlying platform |
| Configure and maintain the OS | Less operating system administration |
| Install and configure applications | Deploy web applications |
| Manage OS patching | Microsoft handles much of the platform maintenance |
| Greater infrastructure control | Less infrastructure control |

### Practical Example

With a virtual machine, you might:

1. Deploy Windows Server.
2. Install IIS.
3. Configure the web server.
4. Configure firewall rules.
5. Install and deploy your application.
6. Maintain the operating system.

With Azure App Service, you deploy your web application directly to the managed platform without having to administer the underlying server.

**Remember:** PaaS reduces infrastructure administration, but you remain responsible for your application, its code, configuration, and data.

---

## 7. Hands-on Azure Portal Lab

Create an App Service Plan and Web App.

### Step 1 — Create a Web App

1. Open the Azure Portal.
2. Search for `App Services`.
3. Select **Create**.
4. Choose your subscription.
5. Select your resource group.
6. Enter a unique Web App name.
7. Select Windows or Linux.
8. Choose a runtime stack.

### Step 2 — Create an App Service Plan

1. Select Create new under App Service Plan.
2. Enter a plan name.
3. Select the Azure region.
4. Choose a pricing tier.
5. Review the configuration.
6. Create the Web App.

### Step 3 — Explore Scaling

Open your App Service Plan.

Explore:

- Scale up: Upgrade the pricing tier.
- Scale out: Increase the number of instances.
- Autoscale: Configure scaling conditions.

### Step 4 — Explore Web App Features

Open your Web App and investigate:

- Deployment slots
- Configuration
- Environment variables
- Networking
- Diagnose and solve problems
- Monitoring

---

## 8. AZ-104 Exam Practice

### Question 1

What determines the CPU and memory resources available to an Azure Web App?

- A. Resource Group
- B. App Service Plan
- C. DNS Zone

**Answer: B — App Service Plan**

The App Service Plan defines the compute resources available to its hosted applications.

### Question 2

You need more CPU power for your application. What should you do?

- A. Scale out
- B. Scale up
- C. Create a DNS record

**Answer: B — Scale up**

Scale up means upgrading to a more powerful pricing tier or instance size.

### Question 3

Your website receives increased traffic. You want more instances handling requests. What should you do?

- A. Scale up
- B. Scale out
- C. Change the region

**Answer: B — Scale out**

Scale out increases the number of application instances.

### Question 4

Can Windows and Linux Web Apps share the same App Service Plan?

- A. Yes
- B. No
- C. Only on Free tier

**Answer: B — No**

An App Service Plan supports one operating system: Windows or Linux.

### Question 5

Three Web Apps share one App Service Plan. What happens when you scale out the plan?

- A. Only the first application receives additional instances.
- B. The plan's applications can use the additional compute instances.
- C. Each application automatically receives a separate App Service Plan.

**Answer: B**

Applications hosted in the same plan share its compute resources.

---

## 9. Key Points to Memorize

| Topic | Remember |
|---|---|
| App Service | Hosts the web application |
| App Service Plan | Provides compute resources |
| Scale Up | Increase CPU, memory, or instance size |
| Scale Out | Increase the number of instances |
| Autoscale | Automatically adjust instance count |
| PaaS | Microsoft manages the underlying platform |
| Deployment Slots | Separate deployment environments |
| Shared Plan | Multiple applications share compute resources |

### Final AZ-104 Memory Trick

- **App Service = Application**
- **App Service Plan = Computing Resources**
- **Scale Up = Bigger**
- **Scale Out = More**
- **Autoscale = Automatic**

