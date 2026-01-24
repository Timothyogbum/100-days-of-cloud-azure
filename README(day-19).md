☁️ Azure Cloud Engineering: Migration & Hardening
Project: Data Center Security Lifecycle (Day 19)
1. Executive Summary
The objective of this sprint was twofold: first, to successfully migrate local configuration artifacts to the Azure Cloud; and second, to remediate a high-priority security misconfiguration within the production storage environment by transitionining public containers to a restricted state.

2. Technical Environment
Resource Group: kml_rg_main-8d2ceccbcf154cad

Storage Account 1 (Migration): devopsst27537

Storage Account 2 (Hardening): datacenterst20959

Region: centralus

3. Milestone 1: Data Migration
Requirement: Transfer /tmp/devops.txt to the cloud for use by the Nautilus development team.

Tooling: Azure CLI (az storage blob upload)

Authentication: Shared Access Key

Destination: devops-blob-29010/devops.txt

Verification: Executed az storage blob list to confirm data integrity and availability in the Hot storage tier.

4. Milestone 2: Security Hardening (Public to Private)
Requirement: Remediate anonymous access on datacenter-container-30223 to prevent unauthorized data exfiltration.

Action Plan
Discovery: Audited container properties to confirm publicAccess was active.

Remediation: Executed the following command to disable anonymous entry:


az storage container set-permission \
  --name datacenter-container-30223 \
  --account-name datacenterst20959 \
  --public-access off
Validation: Verified via the show command that publicAccess returned null.

5. Security Posture Reflection
By converting the container to Private, we have successfully:

Enforced Authentication: Every request now requires a valid Storage Key, SAS Token, or Azure AD credential.

Prevented Scanning: Anonymous bots and search engine crawlers can no longer index or view the objects within this container.

Aligned with Zero Trust: We have moved away from "Security by Obscurity" toward a defined Identity-Based Access Control model.