What you need to know
For AZ-104, focus on these skills:
JSON skill	Required knowledge
Objects {}	Understand key-value pairs
Arrays []	Understand lists of resources or values
Strings	Recognize text values
Numbers and booleans	Recognize numeric values, true and false
Nested objects	Understand configurations inside configurations
Key-value pairs	Identify resource properties
ARM templates	Read and understand basic deployment templates

Imagine you want to define an Azure virtual machine.
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2024-03-01",
  "name": "MyAzureVM",
  "location": "Canada Central",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D2s_v5"
    }
  }
}


You should understand:
- type: What Azure resource you're deploying.
- apiVersion: Which resource API version you're using.
- name: Resource name.
- location: Azure region.
- properties: Configuration of the resource.
- vmSize: VM hardware size.

3. JSON vs Bicep

ARM template (JSON)
{
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2023-01-01",
  "name": "mystorageaccount"
}

Bicep
resource storage 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: 'mystorageaccount'
}

Both are used to deploy Azure infrastructure.

4. My recommendation for your AZ-104 preparation
Learn JSON fundamentals before studying ARM templates and Bicep.

You should spend approximately 1–2 hours learning basic JSON syntax, then practise reading Azure templates.

You do NOT need:
- Advanced JavaScript.
- JSON programming libraries.
- Complex application development.
- Writing hundreds of lines of JSON from scratch.
We can learn JSON as we work through ARM templates, so you don't need to stop your Azure studies for a separate programming course.


hese concepts help you answer scenario-based questions:
Concept	What to understand
$schema	Defines the template schema
contentVersion	Version of the template
parameters	Values supplied at deployment
variables	Reusable values
resources	Azure resources being deployed
outputs	Values returned after deployment
dependsOn	Controls deployment dependencies
apiVersion	Resource provider API version
Deployment scopes	Resource group, subscription, management group, tenant
What-If	Previews proposed deployment changes


1. Interpret ARM templates and Bicep files
- Understand template structure.
- Identify resources and resource types.
- Understand parameters and variables.
- Understand resource properties.
- Identify dependencies (dependsOn).
- Understand outputs.

2. Modify existing ARM templates
- Change resource names.
- Change locations.
- Modify VM sizes and storage configurations.
- Change parameter values.
- Modify resource properties.
- Understand API versions.

3. Modify existing Bicep files
- Understand Bicep syntax.
- Modify resource declarations.
- Change parameters and variables.
- Understand resource references.
- Understand dependencies.
- Recognize modules.

4. Deploy resources using ARM or Bicep
- Deploy through Azure Portal.
- Deploy through Azure CLI.
- Deploy through Azure PowerShell.
- Understand deployment scopes.
- Understand incremental and complete deployment modes.
- Preview changes using What-If.

5. Export and convert templates
- Export an existing deployment as an ARM template.
- Understand exported templates.
- Convert ARM JSON templates into Bicep.
- Understand the purpose of decompilation.