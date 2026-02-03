Lab Report: Azure Nginx Infrastructure Recovery
## Objective
Establish public internet accessibility for the devops-vm Nginx server by resolving multi-layer connectivity blocks within the devops-vnet architecture.

## Technical Resolution Path
The project required troubleshooting across four distinct Azure networking layers:

Identity Layer (NIC Config):

Found that the default ipconfig1 name was replaced with ipconfigdevops-vm.

This specific configuration name was required to successfully attach the devops-pip.

Routing Layer (The "Silent Killer"):

Identified a User Defined Route (UDR) in devops-rtb named Block-Internet that sent all traffic (0.0.0.0/0) to None.

Fix: Deleted the blocker and created a new route, Allow-Internet, targeting the Internet Gateway.

Access Layer (NSG Inbound):

The VM was shielded by default.

Fix: Created an Inbound Security Rule AllowHTTPInbound on devops-vmNSG to permit TCP Port 80.

Application Layer (Provisioning):

The server was initially inaccessible because Nginx was not installed on the base image.

Fix: Leveraged az vm run-command to remotely update apt and install the Nginx service once outbound routing was restored.

## Final Verification Summary
Resource,Status,Result
Public IP,172.184.105.67,Attached & Reachable
Route Table,devops-rtb,0.0.0.0/0 -> Internet
Security Rule,AllowHTTPInbound,Port 80 (Priority 100)
Nginx Service,Active,HTTP/1.1 200 OK