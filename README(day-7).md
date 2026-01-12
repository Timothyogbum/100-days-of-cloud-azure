## Day 7: Azure Networking - Public IP (PIP) Allocation

### 📋 Objective
Provision a static Public IP address (`devops-pip`) within the Azure `eastus` region to serve as a persistent external entry point for the Nautilus migration project.

### ⚙️ Resource Parameters
- **Resource Name:** `devops-pip`
- **Resource Group:** `kml_rg_main-8c24081a9a884b4b`
- **Location:** `eastus`
- **SKU:** `Standard`
- **Allocation Method:** `Static`
- **Assigned IP Address:** `52.226.18.29`

### ✅ Provisioning Results (Terminal Audit)
The following state was captured directly from the `azure-client` host:

```text
$ az network public-ip show -g kml_rg_main-8c24081a9a884b4b -n devops-pip -o table

Name        IP             State
----------  -------------  ---------
devops-pip  52.226.18.29   Succeeded
🧠 Architectural Insights
SKU Choice: Choosing Standard over Basic allows for the IP to be used with Azure Standard Load Balancers and provides inherent security (Standard PIPs are closed to inbound traffic by default).

Persistence: By choosing Static allocation, the IP 52.226.18.29 is reserved for our use and will not change even if the associated resource is restarted, simplifying DNS management.

Migration Strategy: This resource acts as a bridge between the Stratos DC on-premise users and the newly provisioned Azure VNet resources. 