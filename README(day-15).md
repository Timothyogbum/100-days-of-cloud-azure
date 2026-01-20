Azure Project Documentation: Day 15
Task: Network Security Group (NSG) Implementation
Objective: To provision a virtual firewall (nautilus-nsg) and configure specific inbound traffic rules to support the incremental migration of the Nautilus infrastructure to the Azure cloud.

Technical Specifications:
Resource Group: kml_rg_main-dccf5005e4ab44d4
NSG Name: nautilus-nsg
Region: westus

Inbound Rules:

Allow-HTTP: Port 80, Priority 100, Source 0.0.0.0/0

Allow-SSH: Port 22, Priority 110, Source 0.0.0.0/0

🛠️ The Implementation Workflow
Scope Discovery: Identified a scope mismatch during initial execution. Used az group list to locate the active, task-specific Resource Group.

Firewall Provisioning: Created the NSG container to act as a stateless firewall for the migration subnet.

Rule Orchestration: Defined granular security rules for web traffic and remote administration, ensuring that only specified ports are open to the public internet.

💡 Engineering Reflection: Adaptive Troubleshooting
Handling Authorization Failures: Encountering a 403 Forbidden error (AuthorizationFailed) highlighted the importance of dynamic scope auditing. In a managed lab environment, resources are often segmented into different containers to prevent accidental interference between tasks.

Security Posture: By explicitly defining Allow-HTTP and Allow-SSH, we adhere to the principle of "Implicit Deny". Any traffic not matching these rules is automatically blocked by Azure’s default security rules.


# Azure Day 15 - Network Security Configuration
Resource Group: kml_rg_main-dccf5005e4ab44d4
Action: Provisioned nautilus-nsg with HTTP (80) and SSH (22) rules.
Notes: Successfully navigated RBAC scope mismatch by auditing active groups.