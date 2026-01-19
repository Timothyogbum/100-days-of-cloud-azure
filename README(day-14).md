📝 Azure Project Documentation: Day 14
Task: Provisioning Scoped Managed Storage
Objective: To create a standalone Managed Disk (nautilus-disk) as part of the incremental Stratos DC cloud migration, ensuring all parameters align with the assigned administrative scope.

Technical Specifications:
Resource Group: kml_rg_main-f5ec344dd4cd4899
Disk Name: nautilus-disk
Storage Tier: Standard_LRS (HDD-based, Locally Redundant)
Capacity: 2 GiB
Region: westus

🛠️ The Implementation Workflow
Scope Audit: Initial authorization failed due to an incorrect Resource Group. Performed a scope audit using az group list to identify the active project container.

Regional Alignment: Verified the metadata of the Resource Group to ensure the disk was provisioned in the same geographic region (westus) to prevent latency and cross-region costs.

Resource Creation: Executed the az disk create command with precise SKU and sizing parameters.

💡 Engineering Reflection: Overcoming Authorization Hurdles
The AuthorizationFailed error was a critical learning moment in Least Privilege environments.

The Root Cause: Attempting to write a resource to a scope (Resource Group) where the user identity lacked Microsoft.Compute/disks/write permissions.

The Solution: By correctly identifying the assigned Resource Group through the CLI, I aligned the deployment with the existing RBAC policies without needing to request escalated permissions. This demonstrates an understanding of "Working within the guardrails" of a managed cloud environment.