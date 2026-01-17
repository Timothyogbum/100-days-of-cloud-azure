Azure Day 12: Cloud Governance & Resource Metadata Tagging

🎯 Project Overview
As the Nautilus DevOps team migrates infrastructure to Azure, maintaining visibility over resource costs and environment boundaries is critical. This project involved implementing a standardized tagging schema for the devops-vm instance to ensure it is correctly categorized within the "Development" lifecycle for billing and automation purposes.

🛠️ Technical Implementation
1. Resource Discovery & Identification
Before applying metadata, I performed a targeted query using the Azure CLI to resolve the human-readable VM name into a unique Azure Resource ID. This ensures that the operation targets the exact instance across the subscription.

Command:
az vm list --query "[?name=='devops-vm'].{RG:resourceGroup, ID:id}"
Result:

Resource Group: KML_RG_MAIN-169804E02A014728

Location: westus (as inherited from the Resource Group)

2. Metadata Injection
I utilized the az resource tag command to apply the specific key-value pair. Using the Resource ID is the most resilient method for tagging, as it bypasses potential naming conflicts in different resource groups.

Command:
az resource tag \
  --tags Environment=dev \
  --id "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/KML_RG_MAIN-169804E02A014728/providers/Microsoft.Compute/virtualMachines/devops-vm"

3. Verification & Compliance Audit
To close the task, I verified that the tags were correctly registered in the Azure Resource Manager (ARM) layer.

Verification Command:
az vm show --name devops-vm -g KML_RG_MAIN-169804E02A014728 --query "tags"
Confirmed Metadata:


{
  "Environment": "dev"
}
💡 Engineering Reflection: The Importance of Tagging
In a Cloud & AI Engineering context, tagging is more than just documentation—it is Infrastructure as Code (IaC) logic.

Cost Attribution: Enables the use of Azure Cost Management to filter spending by the dev tag, preventing development overhead from bloating production budgets.

Automated Lifecycles: This tag allows for the implementation of Azure Automation runbooks that can automatically shut down all Environment=dev resources outside of business hours (9-5) to optimize cloud spend.

Security Scope: Network Security Groups (NSGs) and Azure Policies can be scoped to resources with this tag, ensuring that development workloads remain isolated from sensitive production data.

✅ Final Status
Status: Successfully Tagged

Compliance: Verified via CLI

Environment: Stratos DC Migration / Azure Subscription