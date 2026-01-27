🌐 Azure Infrastructure: Nautilus Web Server
Project: Automated Nginx Provisioning (Phase 1)
1. Executive Summary
Successfully deployed a high-availability Ubuntu 22.04 LTS instance named nautilus-vm within the East US region. The deployment utilizes Cloud-Init automation to ensure the web server is operational immediately upon provisioning, minimizing manual configuration time.

2. Technical Specifications
Attribute,Value,Status
VM Name,nautilus-vm,✅ Active
Region,East US,✅ Verified
Public IP,20.102.67.27,✅ Reachable
Storage,Standard_LRS,✅ Compliant
Web Engine,Nginx/1.18.0,✅ Running

3. Implementation Details
Custom Scripting (User Data): Injected a #cloud-config script during the az vm create process. This automation handles the package repository update, Nginx installation, and service enablement without requiring an initial SSH login.

Network Security: Configured the Network Security Group (nautilus-vmNSG) with an inbound security rule (Priority 1010) to allow traffic on Port 80 (HTTP).

4. Verification Proof
The deployment was validated using a remote HTTP header request from the jump host:

Response Code: HTTP/1.1 200 OK

Server Header: Server: nginx/1.18.0 (Ubuntu)

Provisioning State: Succeeded

