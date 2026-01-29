Deployment ID: Nautilus-Azure-WestUS-2026
1. Objective
To establish a secure, password-less management channel between the landing host (azure-client) and a newly provisioned instance (xfusion-vm) in the West US region. This setup eliminates the risk of brute-force password attacks and streamlines automated administrative tasks.

2. Architecture Overview
The deployment consists of a management-to-host trust relationship using Asymmetric RSA Cryptography.

Source Host: azure-client (Landing Zone)

Target VM: xfusion-vm

Location: westus (Azure)

Identity: azureuser

3. Technical Implementation Details
Key Management
Type: RSA 4096-bit

Source Path: /root/.ssh/id_rsa

Encryption: Enabled (Public key injected via Azure Metadata Service)

Virtual Machine Specifications
Name: xfusion-vm

Size: Standard_B1s (General Purpose)

Image: Ubuntu 22.04 LTS

Networking: Public IP assigned via Azure Dynamic Allocation.

4. Configuration & Validation Steps
Step,Command / Action,Result
Key Generation,ssh-keygen -t rsa -b 4096,Public/Private pair created
Provisioning,az vm create ... --ssh-key-values,VM launched with key pre-installed
Connectivity,ssh azureuser@52.225.93.191,200 OK (Password-less access)

5. Security Verification Log
The following handshake was performed to confirm the trust relationship:
# Handshake Log:
Warning: Permanently added '52.225.93.191' (ECDSA) to the list of known hosts.
xfusion-vm
TRUST RELATIONSHIP: ESTABLISHED

