# Azure RG Migration between Subscriptions

Is it possible to move a resource group and all its resources from one azure subscription to another azure subscription?

Yes, it is possible to move an Azure resource group and its resources to a different subscription, provided both are 
within the same Microsoft Entra ID (Azure Active Directory) tenant. 

The move is performed via: 

- Azure Portal
- PowerShell
- CLI

often requiring dependent resources to move together, and can take up to four hours, though resources generally 
remain operational. 

> Key Requirements and Considerations:

- Same Tenant: Subscriptions must share the same Azure Active Directory tenant.
- Dependencies: Dependent resources (e.g., virtual machines with managed disks) must be moved together.
- Permissions: 
You must have `Microsoft.Resources/subscriptions/resourceGroups/moveResources/action` permission on both source and destination subscriptions.

- Downtime/Locks: The resource group is locked during the move, preventing changes to resources.
- Orphaned Roles: 
Role assignments are not moved and must be re-created!

- Limitations: Certain services cannot be moved, such as Azure Cache for Redis with VNet configurations. 

> Steps to Move in Azure Portal:

1. Navigate to the resource group in the source subscription.
2. Select the resources or the entire group, then click Move > Move to another subscription.
3. Choose the target subscription and resource group.
4. Validate and acknowledge that scripts/tools referencing resource IDs will need updating. 