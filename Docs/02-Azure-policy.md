## Azure Policy
Azure Policy is a governance service that evaluates Azure resources against organizational rules.  

Think of  
Azure Policy
     │
     ▼
"Are Azure resources following our rules?"  

Only allow resources in Canada Central  
Require an Environment tag  
Require a specific tag value  
Don't allow public IP addresses  
Only allow approved VM SKUs  

Policy can help enforce organizational standards and assess compliance.

### Policy vs RBAC
RBAC = WHO can do something?  
``` text
David
   │
   ▼
Contributor
   │
   ▼
RG-app
```  

Azure Policy = WHAT is allowed or required?
```text
Azure Policy
     │
     ▼
Only Canada Central
```
So  
```text
RBAC   → Who has permission?
Policy → What rules must resources follow?
```
### Policy Scope
The policy can be assigned at different scopes, including management groups, subscriptions, and resource groups.  
- Management group
- Subscription
- Resource Group
- Resource  

Management Group  
Management Groups are specifically designed to provide governance above subscriptions, with policy assignments cascading down the hierarchy. Use this when: you want a common rule across multiple subscriptions.

Subscription  
A policy assigned at the subscription applies to resources within that subscription and its child resource groups/resources. Example: Everything in Subscription 1 must have an Environment tag.  

Resource Group  
Assigning a policy to RG-App means the policy applies to resources in that resource group.  
Use this when: you only want the rule to affect one workload/resource group.  Ex: Resources in RG-App must be located in Canada Central.  

Individual Resource  
You can also target an individual resource.  This is useful for very specific situations, but it's generally less common for broad governance.  

### Policy Effects
- Deny - Prevent a non-compliant resource from being created or updated.
- Audit - Allow the resource but report it as non-compliant. This is useful when you're introducing governance without immediately breaking deployments.
- Modify - Modify resources or add properties during deployment/update.
- Append - Adds additional fields to a resource request.
- DeployIfNotExists - If something isn't present, Azure Policy can trigger a deployment to create/configure it.
- AuditIfNotExists - Checks whether something exists and reports non-compliance if it doesn't.

### Policy Initiative
An initiative is basically a collection of policies grouped together.

### Policy Compliance
Azure Policy gives you a compliance view.  
GreenMaze Subscription  
Policy Compliance  
────────────────────────  
Compliant       87%  
Non-compliant   13%  

### Policy Inheritance
This connects directly to your Management Groups at the Management Group level. The policy can then apply to the subscriptions underneath that management group. This is one of the reasons Management Groups are important for enterprise governance.  

Azure Policy supports exclusions through the assignment's notScopes, and policy exemptions can also be used for justified exceptions.  

### Built-in Policies
Built-in policies are policies Microsoft provides for you. Examples:  
Allowed locations
Required tags
Resource types
Security configurations
Storage
Compute
Networking
Monitoring 

### Custom Policies
A custom policy is one that your organization creates because the built-in policies don't meet your requirements.  
Custom policies are defined using JSON and contain things such as the policy rule and effect.
### GreenMaze Examples
- Allowed locations
- Required Environment tag
- Allowed VM SKUs

## Mental model
RBAC
  ↓
WHO can do it?

Azure Policy
  ↓
WHAT rules must resources follow?

Tags
  ↓
WHAT metadata describes the resource?

Resource Locks
  ↓
CAN it be accidentally deleted/changed?