☁️ Azure Infrastructure: Public Data Distribution
Project: Stratos DC Migration (Day 17)
Objective
To establish a public-facing storage repository for the Nautilus project. This environment is designed for non-sensitive data (binaries, public documentation, and static assets) that requires anonymous read access without authentication tokens.

📋 Prerequisites
Azure CLI installed and authenticated.

Resource Group: kml_rg_main-3bda703646a649fd

🛠️ Step-by-Step Implementation
Step 1: Create the Storage Account
We initialize the account with the allow-blob-public-access flag set to true. This is a critical security override; without it, Azure will block all anonymous requests at the account level regardless of container settings.
az storage account create \
  --name devopsst31750 \
  --resource-group kml_rg_main-3bda703646a649fd \
  --location westus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true

Step 2: Initialize the Public Container
Once the account is ready, we create the specific container. We set --public-access to container, which allows anonymous users to both list the files and read the content.
az storage container create \
  --name devops-blob-1964 \
  --account-name devopsst31750 \
  --public-access container

Step 3: Verification of Access
Finally, we verify that the metadata correctly reflects the public access level.
az storage container show \
  --name devops-blob-1964 \
  --account-name devopsst31750 \
  --query "{Name:name, PublicAccess:publicAccess}" \
  -o table

💡 Engineering Reflection
Tiered Security: Unlike our previous Private storage setup (for database backups), this architecture is built for intentional accessibility.

Anonymous Access: By enabling "Container" level access, we eliminate the administrative overhead of managing SAS tokens for public users, while still keeping our write permissions secured behind Azure RBAC.

Infrastructure as Code (IaC) Readiness: This CLI-based approach allows these steps to be easily converted into a shell script or Terraform module for automated deployments in the Stratos DC migration.
