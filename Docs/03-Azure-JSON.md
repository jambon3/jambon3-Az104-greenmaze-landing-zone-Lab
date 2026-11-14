# AZ-104: ARM Templates and Bicep

## 1. Introduction to Infrastructure as Code (IaC)

Infrastructure as Code (IaC) means defining and deploying infrastructure using code instead of manually creating resources through the Azure Portal.

For example, instead of manually creating a Storage Account, Virtual Network, or Virtual Machine, you describe the resources in a template and deploy them through Azure Resource Manager.

### Why use Infrastructure as Code?

- **Consistency:** Deploy the same configuration repeatedly.
- **Automation:** Reduce manual configuration.
- **Repeatability:** Recreate infrastructure in different environments.
- **Version control:** Track changes using Git and GitHub.
- **Documentation:** The template describes the intended infrastructure.
- **Efficiency:** Deploy multiple resources together.

### Manual deployment vs. IaC

| Manual deployment | Infrastructure as Code |
|---|---|
| Create resources through the Portal | Define resources in a template |
| Repeat configuration manually | Reuse the same template |
| More risk of human error | More consistent deployments |
| Difficult to track changes | Track changes using Git |
| Time-consuming for multiple environments | Automate deployments |

---

## 2. Understanding Azure Resource Manager (ARM)

Azure Resource Manager is the management and deployment service for Azure.

When you create, update, or delete an Azure resource, Azure Resource Manager processes the request.

ARM handles:

- Resource deployment
- Resource configuration
- Access control through Azure RBAC
- Resource dependencies
- Resource management
- Deployment scopes

There are two important template technologies:

| Technology | Description |
|---|---|
| ARM JSON | JSON-based language for defining Azure infrastructure |
| Bicep | More readable language that compiles into ARM JSON |

**Important:** Bicep does not replace Azure Resource Manager. Bicep is a language used to define infrastructure that Azure Resource Manager deploys.

### How they work together

```text
Bicep file (.bicep)
        |
        v
Bicep compiler
        |
        v
ARM JSON template (.json)
        |
        v
Azure Resource Manager
        |
        v
Azure resources deployed
```

You can also deploy ARM JSON templates directly without writing Bicep.

---

## 3. Understanding JSON Basics

ARM templates use JSON.

JSON stands for JavaScript Object Notation. It stores information using objects, arrays, properties, and values.

### JSON objects

An object uses curly brackets `{}`.

```json
{
  "name": "storageaccount01",
  "location": "canadacentral",
  "enabled": true
}
```

### JSON arrays

An array uses square brackets `[]`.

```json
{
  "locations": [
    "canadacentral",
    "canadaeast",
    "eastus"
  ]
}
```

### Common JSON data types

| Data type | Example |
|---|---|
| String | `"Canada Central"` |
| Integer | `100` |
| Boolean | `true` |
| Array | `["VM1", "VM2"]` |
| Object | `{"name": "VM1"}` |
| Null | `null` |

You should understand these structures because ARM templates use them to describe Azure resources.

---

## 4. ARM JSON Template Structure

An ARM template has several possible sections.

```json
{
  "$schema": "...",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "functions": [],
  "resources": [],
  "outputs": {}
}
```

| Section | Purpose |
|---|---|
| `$schema` | Defines the template schema |
| `contentVersion` | Identifies the template version |
| `parameters` | Accepts values during deployment |
| `variables` | Stores reusable values |
| `functions` | Defines reusable template functions |
| `resources` | Defines the Azure resources to deploy |
| `outputs` | Returns information after deployment |

The schema and resources sections are essential in a standard ARM deployment template. Other sections are optional.

### 4.1 Parameters

Parameters allow you to provide values when deploying a template.

For example, you might want to deploy the same Storage Account template in different Azure regions.

```json
"parameters": {
  "storageLocation": {
    "type": "string",
    "defaultValue": "canadacentral",
    "allowedValues": [
      "canadacentral",
      "canadaeast"
    ]
  }
}
```

Common parameter types:

| Type | Description |
|---|---|
| `string` | Text |
| `int` | Integer |
| `bool` | True or false |
| `array` | List of values |
| `object` | Collection of properties |
| `secureString` | Sensitive text |
| `secureObject` | Sensitive object |

**Exam tip:** Use `secureString` or `secureObject` for sensitive parameter values instead of ordinary strings or objects. Avoid putting passwords or secrets directly into templates.

### 4.2 Variables

Variables store reusable values inside a template.

```json
"variables": {
  "storagePrefix": "greenmaze",
  "environment": "production"
}
```

You can reference a variable using:

```json
"[variables('storagePrefix')]"
```

**Difference:**

- Parameters are supplied from outside the template during deployment.
- Variables are defined inside the template.

### 4.3 Resources

The `resources` section defines the Azure resources to deploy.

Example:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2023-05-01",
    "name": "greenmazestorage01",
    "location": "canadacentral",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "StorageV2",
    "properties": {
      "accessTier": "Hot"
    }
  }
]
```

Important properties:

| Property | Purpose |
|---|---|
| `type` | Azure resource provider and resource type |
| `apiVersion` | API version used to manage the resource |
| `name` | Resource name |
| `location` | Azure region |
| `sku` | Resource pricing or performance tier |
| `properties` | Resource-specific configuration |

### 4.4 Outputs

Outputs return information after deployment.

Example:

```json
"outputs": {
  "storageAccountName": {
    "type": "string",
    "value": "[variables('storagePrefix')]"
  }
}
```

Outputs can return:

- Resource names
- Resource IDs
- Endpoint information
- Other deployment results

**Security tip:** Do not expose passwords, keys, tokens, or other secrets through outputs.

---

## 5. Understanding Bicep

Bicep is a domain-specific language designed to make Azure infrastructure deployments easier to write and maintain.

It provides a cleaner syntax than ARM JSON.

### ARM JSON vs. Bicep

| ARM JSON | Bicep |
|---|---|
| Uses JSON syntax | Uses a simplified syntax |
| More brackets and quotation marks | Easier to read |
| Can be difficult to maintain | Easier to maintain |
| Native ARM template format | Compiles into ARM JSON |
| Uses JSON expressions | Uses Bicep expressions |

Microsoft recommends Bicep for new Azure infrastructure deployments because of its simpler authoring experience.

### Example: ARM JSON

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-05-01",
      "name": "greenmazestorage01",
      "location": "canadacentral",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2"
    }
  ]
}
```

### Equivalent Bicep

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'greenmazestorage01'
  location: 'canadacentral'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

Both templates define an Azure Storage Account.

---

## 6. Important Bicep Syntax

### 6.1 Parameters

Parameters allow you to customize a deployment.

```bicep
param location string = 'canadacentral'

param storageName string
```

You can also add descriptions and validation rules using decorators.

```bicep
@description('Azure region for the Storage Account')
param location string = 'canadacentral'

@description('Storage Account name')
@minLength(3)
@maxLength(24)
param storageName string
```

Common Bicep decorators:

| Decorator | Purpose |
|---|---|
| `@description()` | Describes a parameter |
| `@allowed()` | Restricts accepted values |
| `@minValue()` | Sets minimum numeric value |
| `@maxValue()` | Sets maximum numeric value |
| `@minLength()` | Sets minimum string or array length |
| `@maxLength()` | Sets maximum string or array length |
| `@secure()` | Marks a parameter as sensitive |

Example:

```bicep
@allowed([
  'Standard_LRS'
  'Standard_GRS'
])
param storageSku string = 'Standard_LRS'
```

### 6.2 Variables

Variables store reusable values.

```bicep
var environment = 'production'
var storagePrefix = 'greenmaze'
```

Reference a variable by using its name:

```bicep
name: storagePrefix
```

### 6.3 Resources

Resources define the Azure infrastructure.

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'greenmazestorage01'
  location: 'canadacentral'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

The resource declaration contains:

1. A symbolic name (`storageAccount`).
2. The Azure resource type.
3. The API version.
4. The resource configuration.

The symbolic name is used inside the Bicep file to reference the resource. It is not necessarily the Azure resource's actual name.

### 6.4 Outputs

Outputs return values after deployment.

```bicep
output storageName string = storageAccount.name

output storageResourceId string = storageAccount.id
```

You can return resource properties such as names and IDs.

---

## 7. Resource Dependencies

Some Azure resources depend on other resources.

For example:

- A Virtual Machine needs a Network Interface.
- A Network Interface needs a Subnet.
- A Virtual Machine may need a Managed Disk.
- A Web App may need an App Service Plan.

Bicep can automatically identify dependencies when one resource references another.

### Example: Implicit dependency

```bicep
resource vnet 'Microsoft.Network/virtualNetworks@2023-09-01' = {
  name: 'vnet-greenmaze'
  location: 'canadacentral'
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2023-09-01' = {
  parent: vnet
  name: 'subnet-app'
  properties: {
    addressPrefix: '10.0.1.0/24'
  }
}
```

The subnet references the VNet through `parent`, so Bicep understands the dependency.

### Explicit dependency: dependsOn

You can also define dependencies manually.

```bicep
resource anotherResource 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'greenmazestorage02'
  location: 'canadacentral'
  dependsOn: [
    storageAccount
  ]
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

**Exam tip:**

- Implicit dependencies are created through resource references.
- Explicit dependencies use `dependsOn`.
- Prefer implicit dependencies when possible.

---

## 8. Conditions and Loops in Bicep

### 8.1 Conditional deployment

You can deploy a resource only when a condition is true.

```bicep
param deployStorage bool = true

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = if (deployStorage) {
  name: 'greenmazestorage01'
  location: 'canadacentral'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

If `deployStorage` is false, the Storage Account is not deployed.

### 8.2 Loops

Loops allow you to deploy multiple resources using one declaration.

```bicep
param storageNames array = [
  'greenmazestorage01'
  'greenmazestorage02'
  'greenmazestorage03'
]

resource storageAccounts 'Microsoft.Storage/storageAccounts@2023-05-01' = [for storageName in storageNames: {
  name: storageName
  location: 'canadacentral'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}]
```

This creates three Storage Accounts.

**Exam tip:** Understand that loops can create multiple instances of a resource without repeating the entire resource definition.

---

## 9. Bicep Modules

Modules allow you to separate infrastructure into reusable files.

For example, you could have:

```text
main.bicep
modules/
    storage.bicep
    network.bicep
    virtualmachine.bicep
```

The main file can call other Bicep files.

Example:

```bicep
module storageModule './modules/storage.bicep' = {
  name: 'storageDeployment'
  params: {
    location: 'canadacentral'
    storageName: 'greenmazestorage01'
  }
}
```

Benefits of modules:

- Reusable infrastructure definitions.
- Easier maintenance.
- Better organization.
- Separation of responsibilities.
- Easier management of complex deployments.

**Exam tip:** A module is a Bicep file deployed from another Bicep file.

---

## 10. Deployment Scopes

ARM and Bicep support different deployment scopes.

| Scope | Purpose |
|---|---|
| Tenant | Deploy resources or policies at the tenant level |
| Management group | Deploy or manage resources at a management group |
| Subscription | Deploy resources at subscription scope |
| Resource group | Deploy resources into a resource group |

Bicep uses `targetScope` to specify the deployment scope.

Example:

```bicep
targetScope = 'resourceGroup'
```

Other possible scopes include:

```bicep
targetScope = 'subscription'
```

```bicep
targetScope = 'managementGroup'
```

```bicep
targetScope = 'tenant'
```

**AZ-104 focus:** Understand the difference between subscription-level and resource-group-level deployments.

A resource-group deployment creates or updates resources within a specified resource group. A subscription-level deployment can perform subscription-scope operations, such as creating resource groups.

---

## 11. Parameter Files

You can separate deployment configuration from the main Bicep template.

For example:

```text
main.bicep
dev.bicepparam
test.bicepparam
prod.bicepparam
```

This allows you to deploy the same infrastructure with different settings.

### Example Bicep template

```bicep
param location string
param storageName string

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

### Example parameter file

```bicep
using './main.bicep'

param location = 'canadacentral'
param storageName = 'greenmazedevstorage01'
```

You can create separate parameter files for development, testing, and production.

ARM JSON templates can also use separate JSON parameter files.

**Security tip:** Do not store passwords, access keys, or other secrets in plain-text parameter files committed to GitHub.

---

## 12. Deploying ARM Templates and Bicep

You can deploy templates using:

- Azure Portal
- Azure CLI
- Azure PowerShell

### 12.1 Deploy using Azure CLI

First, sign in:

```bash
az login
```

Select the subscription:

```bash
az account set --subscription "YOUR-SUBSCRIPTION-ID"
```

Create a resource group:

```bash
az group create \
  --name RG-GreenMaze \
  --location canadacentral
```

Deploy a Bicep template:

```bash
az deployment group create \
  --resource-group RG-GreenMaze \
  --template-file main.bicep
```

Deploy an ARM JSON template:

```bash
az deployment group create \
  --resource-group RG-GreenMaze \
  --template-file main.json
```

Deploy with parameters:

```bash
az deployment group create \
  --resource-group RG-GreenMaze \
  --template-file main.bicep \
  --parameters location=canadacentral storageName=greenmazestorage01
```

### 12.2 Deploy using Azure PowerShell

Sign in:

```powershell
Connect-AzAccount
```

Select the subscription:

```powershell
Set-AzContext -Subscription "YOUR-SUBSCRIPTION-ID"
```

Create a resource group:

```powershell
New-AzResourceGroup `
  -Name "RG-GreenMaze" `
  -Location "Canada Central"
```

Deploy a Bicep template:

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName "RG-GreenMaze" `
  -TemplateFile "./main.bicep"
```

### 12.3 Deploy using the Azure Portal

A typical Portal workflow:

1. Open the Azure Portal.
2. Search for **Deploy a custom template**.
3. Select **Build your own template in the editor** or upload a template.
4. Provide the ARM JSON template.
5. Enter the required parameters.
6. Select the subscription and resource group.
7. Review the configuration.
8. Select **Review + create** and deploy.

The exact Portal workflow can vary as the interface changes.

---

## 13. Bicep CLI Commands

The Bicep CLI helps you validate, compile, and work with templates.

### Check the Bicep version

```bash
az bicep version
```

### Compile Bicep into ARM JSON

```bash
az bicep build --file main.bicep
```

This generates an ARM JSON template.

### Decompile ARM JSON into Bicep

```bash
az bicep decompile --file main.json
```

Decompilation attempts to convert an ARM JSON template into Bicep. The result may need manual review and cleanup.

### Important distinction

| Command | Purpose |
|---|---|
| `az bicep build` | Bicep to ARM JSON |
| `az bicep decompile` | ARM JSON to Bicep |
| `az bicep version` | Displays Bicep CLI version |

---

## 14. Validate and Preview Deployments

Before deploying infrastructure, you should validate the template and understand what changes it will make.

### What-if deployment

What-if previews the changes that a deployment would make without actually applying them.

Example using Azure CLI:

```bash
az deployment group what-if \
  --resource-group RG-GreenMaze \
  --template-file main.bicep
```

Example using PowerShell:

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName "RG-GreenMaze" `
  -TemplateFile "./main.bicep" `
  -WhatIf
```

What-if can show:

- Resources that will be created.
- Resources that will be modified.
- Resources that will be deleted.
- Changes to resource properties.

**Important:** What-if is a preview, not a deployment.

### Validation vs. What-if

| Feature | Purpose |
|---|---|
| Template validation | Checks whether the template is valid for deployment |
| What-if | Previews expected resource changes |
| Deployment | Applies the changes |

---

## 15. Exporting Existing Azure Resources

You can export an existing Azure deployment or resource group as an ARM template.

### Azure Portal

1. Open the Resource Group.
2. Select **Export template**.
3. Review the generated ARM template.
4. Download or edit the template.

### Azure CLI

```bash
az group export --name RG-GreenMaze
```

This exports an ARM template representing supported resources in the resource group.

**Important:** Exported templates may need modifications before they can be reused. Not every resource configuration is necessarily captured exactly as required.

---

## 16. Important ARM Template Functions

ARM templates support functions that help build dynamic values.

| Function | Purpose |
|---|---|
| `parameters()` | Retrieves a parameter value |
| `variables()` | Retrieves a variable value |
| `concat()` | Combines strings or arrays |
| `uniqueString()` | Generates a deterministic unique string |
| `resourceId()` | Returns a resource ID |
| `reference()` | Retrieves resource properties |
| `resourceGroup()` | Returns resource group information |
| `subscription()` | Returns subscription information |
| `if()` | Evaluates a condition |
| `length()` | Returns the length of a string or array |
| `format()` | Formats a string |

Examples:

### parameters()

```json
"[parameters('location')]"
```

### variables()

```json
"[variables('storagePrefix')]"
```

### concat()

```json
"[concat('greenmaze', 'storage')]"
```

Result:

```text
greenmazestorage
```

### uniqueString()

```json
"[uniqueString(resourceGroup().id)]"
```

Generates a deterministic string based on the supplied value.

### resourceId()

```json
"[resourceId('Microsoft.Storage/storageAccounts', 'greenmazestorage01')]"
```

Returns the resource ID.

**Exam tip:** Be familiar with the purpose of these functions and recognize their syntax in ARM templates.

---

## 17. Deployment Modes: Incremental vs. Complete

ARM resource-group deployments historically support two deployment modes.

### Incremental mode

Incremental mode is the default for ARM resource-group deployments.

- Creates resources defined in the template.
- Updates resources defined in the template.
- Does not delete unrelated resources simply because they are absent from the template.

### Complete mode

Complete mode can delete resources in the resource group that are not included in the template.

| Incremental | Complete |
|---|---|
| Creates resources | Creates resources |
| Updates resources | Updates resources |
| Keeps unrelated resources | Can delete resources absent from the template |
| Safer for many routine deployments | Requires careful review |

**Important:** Complete mode is potentially destructive. Understand its behavior before using it.

Deployment stacks are another Azure capability for managing the lifecycle of resources deployed through templates.

---

## 18. Common Deployment Errors

| Error | Possible cause |
|---|---|
| Invalid template | Incorrect JSON or Bicep syntax |
| Invalid API version | Unsupported or incorrect API version |
| Authorization failed | Insufficient Azure RBAC permissions |
| Resource name unavailable | Name already exists or violates naming rules |
| Quota exceeded | Subscription quota reached |
| Location unavailable | Resource type or SKU not supported in the region |
| Dependency error | Resource deployed before its dependency |
| Invalid parameter | Parameter value does not meet requirements |

### Where to troubleshoot

In the Azure Portal:

1. Open the Resource Group.
2. Select **Deployments**.
3. Open the failed deployment.
4. Review the error message.
5. Inspect the operation details.

Using Azure CLI:

```bash
az deployment group list \
  --resource-group RG-GreenMaze
```

Show a specific deployment:

```bash
az deployment group show \
  --resource-group RG-GreenMaze \
  --name DEPLOYMENT-NAME
```

List deployment operations:

```bash
az deployment operation group list \
  --resource-group RG-GreenMaze \
  --name DEPLOYMENT-NAME
```

---

## 19. Practical AZ-104 Lab: Deploy a Storage Account Using Bicep

This lab uses a simple Storage Account deployment.

### Step 1: Create a file

Create a file named:

```text
storage.bicep
```

### Step 2: Add the Bicep template

```bicep
@description('Azure region')
param location string = 'canadacentral'

@description('Storage Account name')
@minLength(3)
@maxLength(24)
param storageName string

@description('Storage Account SKU')
@allowed([
  'Standard_LRS'
  'Standard_GRS'
])
param storageSku string = 'Standard_LRS'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageName
  location: location
  sku: {
    name: storageSku
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
    supportsHttpsTrafficOnly: true
    minimumTlsVersion: 'TLS1_2'
  }
}

output storageAccountName string = storageAccount.name

output storageAccountId string = storageAccount.id
```

### Step 3: Build the template

```bash
az bicep build --file storage.bicep
```

This checks the Bicep compilation and generates ARM JSON.

### Step 4: Preview the deployment

```bash
az deployment group what-if \
  --resource-group RG-management \
  --template-file storage.bicep \
  --parameters storageName=greenmazelabstorage01
```

### Step 5: Deploy the Storage Account

```bash
az deployment group create \
  --resource-group RG-management \
  --template-file storage.bicep \
  --parameters storageName=greenmazelabstorage01
```

### Step 6: Verify the deployment

Open the Azure Portal:

1. Navigate to `RG-management`.
2. Open **Deployments**.
3. Review the successful deployment.
4. Open the Storage Account.
5. Review its configuration.
6. Check the outputs from the deployment.

### Step 7: Experiment

Try changing:

- The Storage Account SKU.
- The Azure region.
- The Storage Account name.
- The default parameter values.

Run what-if again before deploying your changes.

**Lab objective:** Understand how Bicep parameters, resources, outputs, compilation, what-if, and deployment work together.

---

## 20. AZ-104 Exam Knowledge Checklist

Make sure you understand the following topics.

### ARM templates

- [ ] Understand Infrastructure as Code.
- [ ] Understand Azure Resource Manager.
- [ ] Understand ARM JSON template structure.
- [ ] Identify parameters, variables, resources, and outputs.
- [ ] Understand API versions.
- [ ] Understand ARM template functions.
- [ ] Understand deployment scopes.
- [ ] Understand incremental and complete deployment modes.
- [ ] Know how to deploy ARM templates.

### Bicep

- [ ] Understand what Bicep is.
- [ ] Understand how Bicep relates to ARM JSON.
- [ ] Understand parameters and variables.
- [ ] Understand resources and API versions.
- [ ] Understand outputs.
- [ ] Understand resource dependencies.
- [ ] Understand `dependsOn`.
- [ ] Understand conditional deployments.
- [ ] Understand loops.
- [ ] Understand modules.
- [ ] Understand parameter files.
- [ ] Know how to compile Bicep into ARM JSON.
- [ ] Know how to decompile ARM JSON into Bicep.

### Deployment and troubleshooting

- [ ] Deploy templates using Azure Portal.
- [ ] Deploy templates using Azure CLI.
- [ ] Deploy templates using Azure PowerShell.
- [ ] Understand template validation.
- [ ] Understand what-if deployments.
- [ ] Understand how to export templates.
- [ ] Locate failed deployments.
- [ ] Troubleshoot common deployment errors.

---

## 21. Important Exam Comparisons

| Question | Answer |
|---|---|
| What is ARM? | Azure's resource management and deployment service |
| What is an ARM template? | A JSON file defining Azure infrastructure |
| What is Bicep? | A language for defining Azure infrastructure |
| Does Bicep replace ARM? | No. Bicep compiles into ARM JSON |
| What defines the resources to deploy? | `resources` |
| What accepts deployment values? | `parameters` |
| What stores reusable internal values? | `variables` |
| What returns deployment information? | `outputs` |
| What specifies the resource API version? | `apiVersion` |
| What defines deployment dependencies? | Resource references or `dependsOn` |
| What deploys resources conditionally? | `if` |
| What deploys multiple resources using one declaration? | Loops |
| What allows reusable Bicep files? | Modules |
| What previews deployment changes? | What-if |
| What compiles Bicep into JSON? | `az bicep build` |
| What converts JSON into Bicep? | `az bicep decompile` |
| Which mode can delete resources absent from the template? | Complete mode |

---

## 22. Practice Questions

Test your understanding before moving to the next AZ-104 topic.

### Question 1

What is the main purpose of Infrastructure as Code?

A. To manually configure Azure resources faster.

B. To define and deploy infrastructure through code.

C. To replace Azure RBAC.

D. To manage Microsoft 365 licenses.

**Answer: B**

### Question 2

What is Bicep?

A. A replacement for Azure Resource Manager.

B. A programming language for Windows applications.

C. A language that compiles into ARM JSON templates.

D. A monitoring service.

**Answer: C**

### Question 3

Which ARM template section defines Azure resources?

A. Parameters.

B. Variables.

C. Outputs.

D. Resources.

**Answer: D**

### Question 4

What is the purpose of parameters?

A. To accept values when deploying a template.

B. To delete resources.

C. To monitor virtual machines.

D. To assign Azure RBAC roles.

**Answer: A**

### Question 5

What is the difference between parameters and variables?

A. They are exactly the same.

B. Parameters are supplied during deployment, while variables are defined inside the template.

C. Variables are always supplied through the Azure Portal.

D. Parameters can only contain numbers.

**Answer: B**

### Question 6

Which command compiles a Bicep file into ARM JSON?

A. `az bicep decompile`

B. `az deployment group create`

C. `az bicep build`

D. `az group export`

**Answer: C**

### Question 7

What does the What-if operation do?

A. Deploys all resources immediately.

B. Deletes unused resources.

C. Previews the changes a deployment would make.

D. Converts JSON into Bicep.

**Answer: C**

### Question 8

Which deployment mode can delete resources that are not included in the template?

A. Incremental.

B. Complete.

C. Preview.

D. Validation.

**Answer: B**

### Question 9

What is the purpose of `dependsOn`?

A. To define a resource dependency.

B. To create a parameter.

C. To return an output.

D. To change an API version.

**Answer: A**

### Question 10

What is the purpose of a Bicep module?

A. To store Azure passwords.

B. To create Azure subscriptions.

C. To reuse and organize infrastructure definitions.

D. To monitor deployment costs.

**Answer: C**

---

## 23. Microsoft Learn Resources

Use the official Microsoft documentation to reinforce these topics:

- [ARM template structure and syntax](https://learn.microsoft.com/azure/azure-resource-manager/templates/syntax)
- [Compare ARM templates and Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/compare-template-syntax)
- [ARM template parameters](https://learn.microsoft.com/azure/azure-resource-manager/templates/parameters)
- [Bicep parameters](https://learn.microsoft.com/azure/azure-resource-manager/bicep/parameters)
- [Bicep modules](https://learn.microsoft.com/azure/azure-resource-manager/bicep/modules)
- [Bicep best practices](https://learn.microsoft.com/azure/azure-resource-manager/bicep/best-practices)
- [What-if deployment operation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-what-if)
- [Bicep parameter files](https://learn.microsoft.com/azure/azure-resource-manager/bicep/parameter-files)

---

## Final Takeaway

For the AZ-104 exam, understand the relationship between ARM, ARM JSON, and Bicep.

- **ARM:** Azure's resource management and deployment service.
- **ARM JSON:** A template format used to define Azure infrastructure.
- **Bicep:** A simpler language that compiles into ARM JSON.
- **Parameters:** Values supplied during deployment.
- **Variables:** Reusable values defined inside the template.
- **Resources:** Azure resources to deploy.
- **Outputs:** Information returned after deployment.
- **Modules:** Reusable Bicep files.
- **What-if:** Previews deployment changes.
- **Incremental vs. Complete:** Understand how each deployment mode handles existing resources.

The most important practical skill is being able to read a template, understand what it will deploy, identify its parameters and dependencies, preview its changes, and understand how to deploy it safely.
