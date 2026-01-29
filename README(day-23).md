🌐 Azure Infrastructure: xfusion-vm Web Server
Project: Nautilus Initial Infrastructure Setup
1. Executive Summary
Provisioned an Ubuntu 22.04 LTS instance named xfusion-vm in the East US region. The deployment was fully automated using Cloud-Init (User Data) to ensure Nginx was installed and the service was started immediately upon provisioning. The VM is now accessible via the internet on port 80.

2. Technical Specifications
Attribute,Value,Status
VM Name,xfusion-vm,✅ Active
Region,East US,✅ Verified
Public IP,172.172.180.241,✅ Reachable
Image,Ubuntu 22.04,✅ Verified
Web Server,Nginx/1.18.0,✅ 200 OK

3. Implementation Details
Automation Strategy: Utilized the --custom-data flag to pass a cloud-config script. This handled the apt package management and systemd service lifecycle without manual intervention.

Networking: Modified the Network Security Group (xfusion-vmNSG) to include an inbound security rule (Priority 1010) allowing all traffic on TCP Port 80.

4. Verification Log
Provisioning State: Succeeded.

Connectivity Test: curl -I http://172.172.180.241

Response: HTTP/1.1 200 OK