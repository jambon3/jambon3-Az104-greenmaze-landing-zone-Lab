# Microsoft Entra Admin Center  
GUI  
Entra > Users > New user > Create new user or Invite external user
```text
│
└── Users
    │
    └── New user
        │
        ├── Create new user
        │
        └── Invite external user
```
# Create new user individually
<img src="image/create-user.png" alt="Create User" width="600">
Display Name: The user's friendly, full name as it appears in the organization's directory.

Principal Name: (User Principal Name / UPN): The unique sign-in identifier and email-like address used by the user to authenticate and access the directory.

Department and location are IMPORTANT to add for RBAC

# Create new user with Group-Based Management (Recommended)

## Step 1: Create a Template Security Group

Go to Identity > Groups > All groups.
```text
Microsoft Entra ID
        │
        ▼
      Groups
        │
        ├── Group type
        │      │
        │      ├── Security
        │      │     → Used to control access to resources,
        │      │       applications, Azure roles, etc.
        │      │
        │      └── Microsoft 365
        │            → Used for collaboration (shared mailbox,
        │              Teams, SharePoint, calendar, etc.).
        │
        └── Membership type
               │
               ├── Assigned
               │     → Admin manually adds/removes members.
               │
               ├── Dynamic User
               │     → Users are automatically added/removed
               │       based on user attributes/rules.
               │
               └── Dynamic Device
                     → Devices are automatically added/removed
                       based on device attributes/rules.
```  
## Group type  
Security groups are commonly used for access management.  
Microsoft 365 are used mainly for collaboration. Provides shared resources such as Outlook, SharePoint, Planner, etc.

Select New group. 
Group type: Select Security  
Group name: Choose a descriptive name (e.g., Sales Team - Standard Access).  
Membership type: Membership type: Assigned User
Click Create.

<img src="image/create-group-user.png" alt="Create Group User" width="600">

Pick Assigned if: You don't have Entra ID P1/P2 licenses, your team is small, or membership doesn't follow a logical rule based on user attributes.

Pick Dynamic if: Every new user in a specific department/title needs access automatically as soon as their profile is created.  
****Dynamic membership in Microsoft Entra ID requires Microsoft Entra ID P1 or P2>Assigned User

The important distinction is:  
Assigned = you manage membership.  
Dynamic User = Entra manages users based on rules.  
Dynamic Device = Entra manages devices based on rules.  

## Step 2: Add the template group to your management structure

User → Security Group → Permissions/Access
The security group becomes the reusable template. Instead of assigning permissions individually to every user, you put the user in the appropriate group.

## Step 3: Create the user  
Microsoft Entra admin center → Identity → Users → All users → New user

Display name: Luc 
User principal name: Luc.Plante@company.com  
Job title: Software Engineering Manager  
Department: Software Engineering  

# Create Bulk user  
Microsoft Entra admin center → Entra ID → Users → Bulk operations → Bulk create  
1. To download Microsoft's CSV template > Bulk create → Download  
 → Entra ID → Users → Bulk operations → Bulk create → Upload → submit

***The important required fields are: User's display name, User principal name = login, password, Block sign in

My group membership type is Assigned because I dont have P1, P2 licence.

Add bulk user to group using Az Powershell    
az login  
Get-Module Microsoft.Graph -ListAvailable
 
Get your tenant ID  
$tenantId = (az account show --query tenantId -o tsv)  
$tenantId

Find the users  
$users = az rest --method GET --url "https://graph.microsoft.com/v1.0/users?`$filter=department eq 'Software Engineering'&`$select=id,displayName,userPrincipalName,department&`$top=999" | ConvertFrom-Json  
$users.value.  Count

See the users  
$users.value | Select-Object displayName,userPrincipalName

Find the Software Engineering group  
$groups = az rest --method GET --url "https://graph.microsoft.com/v1.0/groups?`$filter=displayName eq 'Software Engineering'&`$select=id,displayName"  
($groups | ConvertFrom-Json).value

Group Object ID #  
save it

$groupId = (($groups | ConvertFrom-Json).value | Select-Object -First 1).id  
$groupId

Your loop stays exactly like this  
foreach ($user in $users.value) {
    $body = @{
        "@odata.id" = "https://graph.microsoft.com/v1.0/directoryObjects/$($user.id)"
    } | ConvertTo-Json

    az rest `
        --method POST `
        --url "https://graph.microsoft.com/v1.0/groups/$groupId/members/`$ref" `
        --headers "Content-Type=application/json" `
        --body $body
}

# Add user to Security Group cybersecurity portal bulk import  
In the Entra admin center, go to Groups → New group.  
Group type: Security  
Name: Cybersecurity  
Membership type: Assigned  
Click Create. Skip this step if the group already exists.

Open the group, then Members → Bulk operations → Import members.  
Upload ImportGroupMembers_Cybersecurity.csv and click Submit.

# PowerShell in Azure Cloud Shell
Connect-MgGraph -Scopes "Group.ReadWrite.All","User.Read.All"

## Create the group (Assigned is the default) - skip if it already exists
$g = New-MgGroup -DisplayName "Cybersecurity" -MailEnabled:$false -SecurityEnabled -MailNickname "Cybersecurity"

## Add the 15 users
Get-Content ./ImportGroupMembers_Cybersecurity.csv | Select-Object -Skip 1 | ForEach-Object {
  $u = Get-MgUser -UserId $_.Trim()
  New-MgGroupMember -GroupId $g.Id -DirectoryObjectId $u.Id
}


# I did security groups so do i still need to do management group
Yes, you may still need management groups because they do a completely different job than security groups.
• Security groups (in Microsoft Entra ID) organize users and devices to control who has access to specific applications, data, or individual resources.
• Management groups (in Azure) organize multiple Azure subscriptions (billing and container boundaries) to control governance, compliance, and policies at scale.

• Security Groups (The "Who"): A security group (like a Microsoft Entra ID group) acts as a security principal. You assign an RBAC role to a security group so that all users inside that group inherit the permissions, which makes managing users much easier than assigning permissions individually.
• Management Groups (The "Where" / Scope): A management group is a scope level used to organize multiple subscriptions. When you assign a role to a security group at the management group level, those permissions automatically apply downward to all child management groups, subscriptions, and resource groups inside that hierarchy.


## Microsoft Entra Self-Service Password Reset (SSPR).
This is the feature that lets a user say “I forgot my password” and reset it themselves without calling IT.  
In the Azure / Microsoft Entra admin center:  
Entra ID → Password reset → Properties

Self service password reset enabled

None — nobody can use SSPR
Selected — only a specific group
All — everyone in the tenant

Microsoft recommends starting with a test group before enabling it broadly.
***Microsoft's current documentation says Microsoft Entra ID P1 is required for password reset in the Entra ID configuration shown in the tutorial.

GreenMaze were a hybrid environment with on-premises Active Directory, there's another concept you'll want to learn: password writeback. That allows a password reset performed through Entra SSPR to be written back to on-prem AD.