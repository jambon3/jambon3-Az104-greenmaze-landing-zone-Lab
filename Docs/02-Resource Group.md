## resource group 
```text
Microsoft Entra Tenant
│
└── Management Group
    │
    └── GreenMaze
        │
        ├── Platform
        │   └── Subscription
        │
        ├── Landing Zones
        │   └── Subscription
        │
        └── Sandbox
            └── Subscription
                │
                └── Resource Groups
                    │
                    ├── rg-network
                    ├── rg-compute
                    ├── rg-storage
                    └── rg-security
                        │
                        └── Azure Resources
                            ├── VM
                            ├── VNet
                            ├── Storage Account
                            └── etc.
```