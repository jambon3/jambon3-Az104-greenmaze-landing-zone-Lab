
# Management group

For your GreenMaze Azure foundation, you can eventually expand it to:
MS Entra tenant and Azure Tenant is the same Tenant.

Microsoft Entra Tenant
│
├── Users
├── Groups
└── Enterprise Applications
        │
        ▼
Azure Tenant
│
└── Management Groups
    │
    ├── Production
    │   └── Azure Subscription
    │       ├── Resource Groups
    │       │   ├── VMs
    │       │   ├── Storage
    │       │   ├── VNets
    │       │   └── NSGs
    │
    └── Development
        └── Azure Subscription
            ├── Resource Groups
            │   ├── VMs
            │   ├── Storage
            │   ├── VNets
            │   └── NSGs

Your Entra tenant is the identity directory associated with this Azure environment; it isn't another level in the Azure resource hierarchy.  

Management Groups are important because they sit above Azure subscriptions and let you manage many subscriptions together.

## The key hierarchy

Management Group → Subscription → Resource Group → Resource  

## Why do Management Groups exist?

Instead of configuring policies and permissions individually on every subscription, you can put them under a Management Group:

GreenMaze Root Management Group
│
├── Production
│   └── Production Subscription
│
├── Non-Production
│   ├── Development Subscription
│   └── Test Subscription
│
└── Security
    └── Security Subscription

Then you can apply Azure Policy or RBAC at the Management Group level. The settings can then be inherited by the subscriptions underneath. 

## Management Groups are useful for Policy

This is the most important reason to understand them.
