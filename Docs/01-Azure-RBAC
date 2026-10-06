# Azure RBAC (Role-Based Access Control)
Azure RBAC controls who can do what to Azure resources, and at what scope.
# Who + What + Where

Every Azure RBAC assignment has three fundamental parts:
```text
Security Principal
        +
     Role
        +
      Scope
```
## WHO?
Security Principal = Who receives the permission? 
Examples:  
User  
Group  
Service principal  
Managed identity  

For your GreenMaze lab:  
Software Engineering Group  
Cybersecurity Group  
IT & Cloud Infrastructure Group  

## what
Role = WHAT can they do?  
Examples:  
Owner  
Contributor  
Reader  
There are also many specialized roles:  
Virtual Machine Contributor  
Storage Blob Data Contributor  
Network Contributor  
Security Reader  
User Access Administrator  

The role determines what actions the principal can perform.

## Scope = WHERE?
A role can be assigned at:  
```text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```
Example:  
Contributor
    │
    └── Resource Group: rg-compute

Reader / Can view resources.  
Contributor / Can manage Azure resources. Contributor cannot grant other people Azure RBAC permissions.
Owner  / Manage resources & Manage access


## User Access Administrator
Its purpose is primarily to manage access to Azure resources.
```text
Contributor
      ↓
Can manage resources

User Access Administrator
      ↓
Can manage access

Owner
      ↓
Can manage resources + access  
```
Least privilege: This is a major security principle. Give users only the permissions they need.  

Microsoft Entra / identity services: Global Administrator, User Administrator, Groups Administrator  
Azure RBAC: Owner, Contributor, Reader, Virtual Machine Contributor, Network Contributor

## Azure RBAC vs Azure Policy
RBAC: Who can do something?  
Azure Policy: What is allowed or required?

## Azure RBAC vs Locks


RBAC  
↓
Who can perform actions?  

Policy  
↓ 
What configurations are allowed?  

Lock  
↓
Prevent accidental modification/deletion  

These three work together.  

## Group-based RBAC  
Don't assign permissions individually whenever possible.  
Instead:  
Users
│
├── David
├── Alice
├── Bob
└── Sarah
       │
       ▼
Software Engineering Group
       │
       ▼
Contributor
       │
       ▼
rg-application

This is much easier to manage.  
If Sarah leaves the department: Remove Sarah from group  

You don't have to hunt through Azure RBAC assignments and remove her individually everywhere.

Multiple RBAC assignments  
A user can receive permissions from multiple places.  
For example:  

David
│
├── Reader
│     └── Subscription
│
└── Contributor
      └── rg-compute

David therefore has:

Reader

throughout the subscription and:

Contributor

within rg-compute.  
The more specific scope gives additional permissions there.

Azure RBAC does NOT work like traditional NTFS permissions

This is another useful mental distinction.

Don't think:

Allow
Deny
Inheritance

in exactly the same way as Windows NTFS.

Azure RBAC is based on role assignments at scopes, which are inherited downward.

For AZ-104, focus on:

Principal
+
Role
+
Scope

```text
GreenMaze
│
├── Platform
│
│   └── Platform Subscription
│       │
│       ├── IT & Cloud Infrastructure
│       │       └── Contributor
│       │
│       └── Cybersecurity
│               └── Security Reader
│
├── Landing Zones
│
│   └── Workload Subscription
│       │
│       ├── Software Engineering
│       │       └── Contributor → rg-app
│       │
│       ├── Data & Analytics
│       │       └── Contributor → rg-data
│       │
│       └── Cybersecurity
│               └── Security Reader
│
└── Sandbox
    │
    └── Sandbox Subscription
        │
        └── AZ-104 Lab Users
                └── Contributor
```
This demonstrates RBAC + management groups + subscriptions + resource groups all working together.

Azure RBAC determines who can perform which actions on Azure resources at a specific scope.

And the formula:

WHO
 ↓
Security Principal
 +
WHAT
 ↓
Role
 +
WHERE
 ↓
Scope

That is the foundation of the entire RBAC topic.