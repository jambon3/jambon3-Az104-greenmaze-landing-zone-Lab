# jambon3-Az104-greenmaze-landing-zone-Lab

Landing zone lab for GreenMaze.

## 1. Project

# Azure Company Foundation

This project simulates the design and implementation of an Azure foundation for a fictional company, KrinCloud Technologies.

## 2. Objective

The objective of this lab is to establish a basic Azure governance and resource organization structure before deploying application workloads.

## 3. Architecture

                    GreenMaze Azure Tenant
                             │
                    ┌────────┴────────┐
                    │  Management      │
                    │     Groups       │
                    └────────┬────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
        Production                    Development
              │                             │
      ┌───────┴───────┐             ┌───────┴───────┐
      │ Resource       │             │ Resource       │
      │ Groups         │             │ Groups         │
      └───────┬───────┘             └───────┬───────┘
              │                             │
       Azure Resources                Azure Resources

The following diagram shows the proposed Azure organizational structure for GreenMaze.

## Phase 1 — Azure Foundation

This project simulates the design and implementation of an Azure foundation for a fictional company,  greenmaze.
The objective of this phase is to establish the foundational Azure governance, identity, access control, and organizational structure for GreenMaze.

### Tasks

1. Inspect Entra ID
2. Inspect subscription
3. Design company hierarchy
4. Create Management Group
5. Define naming convention
6. Define tagging strategy
7. Create Resource Groups
8. Configure RBAC
9. Configure Azure Policy

### Documentation

- [Entra ID](docs/01-entra-id.md)
- [Subscription](docs/02-subscription.md)
- [Company Hierarchy](docs/03-company-hierarchy.md)
- [Management Groups](docs/04-management-groups.md)
- [Naming Convention](docs/05-naming-convention.md)
- [Tagging Strategy](docs/06-tagging-strategy.md)
- [Resource Groups](docs/07-resource-groups.md)
- [RBAC](docs/08-rbac.md)
- [Azure Policy](docs/09-azure-policy.md)