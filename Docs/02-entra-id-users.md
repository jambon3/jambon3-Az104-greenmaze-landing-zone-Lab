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
## Create new user individually

<img src="create-user.png" alt="alt text" width="600">
Display Name: The user's friendly, full name as it appears in the organization's directory .   

Principal Name (User Principal Name / UPN): The unique sign-in identifier and email-like address used by the user to authenticate and access the directory.
Department and location are IMPORTANT to add for RBAC

## Create new user with Group-Based Management (Recommended)

Step 1: Create a Template Security Group
Go to Identity > Groups > All groups.
```text
Microsoft Entra ID
        │
        ▼
      Groups
        │
        ├── Group type
        │      │
        │      └── Security
        │             → Used to control access to resources,
        │               applications, Azure roles, etc.
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
Select New group.
Configure the settings:
Group type: Select Security
Group name: Choose a descriptive name (e.g., Sales Team - Standard Access).
Membership type: Dynamic User
Click Create.
<img src="create-group-user.png" alt="alt text" width="600">


Pick Assigned if: You don't have Entra ID P1/P2 licenses, your team is small, or membership doesn't follow a logical rule based on user attributes.

Pick Dynamic if: Every new user in a specific department/title needs access automatically as soon as their profile is created.