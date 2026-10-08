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
### Policy Definition

### Policy Assignment

### Policy Scope
- Management Group
- Subscription
- Resource Group
- Resource

### Policy Effects
- Deny
- Audit
- Modify
- Append
- DeployIfNotExists
- AuditIfNotExists

### Policy Initiative
- Policy Set Definition

### Compliance
- Compliant
- Non-compliant

### Policy Inheritance

### Built-in Policies

### Custom Policies

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