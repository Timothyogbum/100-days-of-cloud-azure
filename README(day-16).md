☁️ Azure Infrastructure: Secure Data Landing Zone Project: Stratos DC Migration 

(Day 16)ObjectiveTo establish a secure, private cloud storage environment for the migration of project-critical data from the Stratos Data Center to Azure. This phase focuses on data residency, security, and ensuring zero public exposure.

I utilized the Standard_LRS (Locally Redundant Storage) SKU to balance cost-efficiency with high availability within a single data center.
az storage account create \
  --name xfusionst12482 \
  --resource-group kml_rg_main-60c2fb8df0de4859 \
  --location westus \
  --sku Standard_LRS \
  --kind StorageV2

2. Private Container InitializationProvisioned the specific blob container while explicitly disabling public access. This ensures that the container follows the Principle of Least Privilege, making the data unreachable via the public internet without an authorized SAS token or Access Key.Bashaz storage container create \
  --name xfusion-blob-32369 \
  --account-name xfusionst12482 \
  --public-access off

💡 Engineering Reflection Security Posture: By enforcing --public-access off at the container level, we mitigate the risk of accidental data leaks—a critical skill for my upcoming IBM X-Force Incident Response internship application.Automation Readiness: Using the Azure CLI for this deployment ensures that the storage setup is repeatable and can be integrated into a larger CI/CD pipeline or an Infrastructure as Code (IaC) script.Access Management: During deployment, the CLI handled credential querying via the managed identity on the azure-client, demonstrating a secure way to manage storage without hardcoding secrets.