☁️ Azure Infrastructure: Data Ingestion & Migration
Project: Stratos DC Data Migration (Day 18)
Objective
To migrate critical on-premise configuration data to the Azure Cloud environment. This task involved transferring assets from local temporary storage to a secured Production Blob Container within the centralus region.

📋 Migration Metadata
Attribute,Value
Storage Account,devopsst27537
Blob Container,devops-blob-29010
Resource Group,kml_rg_main-ca594ebd45c64de2
Source File,/tmp/devops.txt
Destination Blob,devops.txt

🛠️ Implementation Workflow
1. Identity & Access Verification
Retrieved the Storage Account Access Key to authenticate the migration session. Using the Shared Key method ensures a secure, high-throughput connection for data transfer without requiring immediate RBAC configuration.

az storage account keys list \
  --account-name devopsst27537 \
  --resource-group kml_rg_main-ca594ebd45c64de2 \
  --query "[0].value" -o tsv

2. Data Ingestion (Upload)
Executed the upload of the local artifact to the cloud. This step transforms the local file into a Block Blob, optimized for streaming and general-purpose storage.
az storage blob upload \
  --account-name devopsst27537 \
  --account-key <REDACTED_KEY> \
  --container-name devops-blob-29010 \
  --name devops.txt \
  --file /tmp/devops.txt

3. Integrity Validation
Verified the "Data at Rest" status by listing container contents. This confirms that the file was not only uploaded but is correctly indexed and available in the Azure Global infrastructure.

💡 Engineering Reflection
Data Integrity: By performing a post-migration list check, we confirm the successful transfer of bits across the cloud boundary.

Security Best Practices: While the Access Key was used for this migration, a production-level recommendation would be to rotate this key periodically or migrate to Azure Managed Identities to further reduce the risk of credential leakage.

Storage Tiers: The data was placed in the Hot Access Tier by default, ensuring low-latency access for the application development team.
