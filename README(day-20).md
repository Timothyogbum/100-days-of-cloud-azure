🌐 Azure IaC: Automated VNet Provisioning & Debugging
Project: Nautilus Infrastructure Modernization (Day 32)
1. Objective
The mission was to refactor an existing ARM template to meet new organizational standards for the Nautilus project. This involved updating network addressing, resource naming, and implementing mandatory metadata tagging for cost-center tracking.

2. Technical Environment
Platform: Microsoft Azure

Interface: Azure CLI (via azure-client host)

Configuration Language: ARM (JSON)

Resource Group: kml_rg_main-0b0ff02c606245be

3. Implementation & Troubleshooting Log
Phase 1: Automated Refactoring (sed)
Initial attempts utilized sed (Stream Editor) to perform non-interactive updates to the JSON file. While successful for naming and tags, a broad regex inadvertently corrupted the addressSpace schema.

Success: Renamed resource to arm-vnet-nautilus.

Success: Injected Environment: KKE-nautilus tag.

Issue encountered: NoAddressPrefixOrPoolProvided error due to malformed JSON key replacement.

Phase 2: Schema Restoration (Heredoc & tee)
To resolve the structural issues in the JSON, a Heredoc was used to overwrite the template with a clean, validated schema.

Method: sudo tee /root/arm-templates/vnet-deployment-template.json << 'EOF' ...

Result: Restored the "addressPrefixes" key and applied the correct 192.168.0.0/16 CIDR.

Phase 3: Final Deployment
The deployment was executed in Incremental Mode, ensuring that Azure only updated the specific properties (IP and Tags) without destroying existing dependencies.

az deployment group create \
  --name nautilus-vnet-final-success \
  --resource-group kml_rg_main-0b0ff02c606245be \
  --template-file /root/arm-templates/vnet-deployment-template.json

4. Verified Configuration
Property,Value
VNet Name,arm-vnet-nautilus
Address Space,192.168.0.0/16
Tag: displayName,arm-vnet-nautilus
Tag: Environment,KKE-nautilus

5. Key Engineering Takeaways
Idempotency: Using a clean template ensures that the deployment can be run multiple times with the same result, a core principle of DevOps.

Validation: The failure of early deployments highlighted the importance of Azure's pre-flight validation; the NoAddressPrefixOrPoolProvided error was a vital guardrail against deploying a broken network.

Tool Selection: While sed is powerful for simple text, heredocs or jq are superior for maintaining the integrity of structured data like JSON.