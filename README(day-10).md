# Day 10: Azure Networking - Public IP Linkage & CLI Debugging

## 🎯 Project Objective
The Nautilus DevOps team required an existing Public IP (`nautilus-pip`) to be associated with a Virtual Machine (`nautilus-vm-pip`) to enable external access.

## 🛠️ Technical Implementation Workflow

### 1. Resource Discovery
Used `az vm list` with JMESPath filtering to map the VM to its underlying Network Interface (NIC) and Resource Group.
- **Resource Group:** `KML_RG_MAIN-108A0EB465194DA2`
- **Target NIC:** `nautilus-vm-pipVMNic`
- **Target IP Config:** `ipconfignautilus-vm-pip`

### 2. The Failure Points (What Broke)
* **Standard Association Failure:** Initial attempts using `az network nic ip-config update` returned a `Succeeded` provisioning state, but the `publicIpAddress` property remained `null`.
* **CLI Abstraction Issue:** The standard high-level command failed to correctly map the resource name to the internal configuration object in this specific lab environment.
* **Query Caching:** Subsequent `az network nic show` commands using `--query` and `-o tsv` returned empty results despite the underlying JSON being updated.



### 3. Resolution: Direct Property Injection
To bypass the abstraction layer, I used the `--set` parameter to perform a direct JSON path update on the NIC resource:
```bash
az network nic update \
  --resource-group KML_RG_MAIN-108A0EB465194DA2 \
  --name nautilus-vm-pipVMNic \
  --set ipConfigurations[0].publicIpAddress.id="$PIP_ID"
This forced the Azure Resource Manager (ARM) to update the specific id field within the ipConfigurations array.

✅ Final Validation
Verified the link via full JSON output inspection, confirming the publicIPAddress.id correctly points to the nautilus-pip resource.

💡 Key Takeaway
When cloud CLI abstractions fail to update nested properties, using the --set flag to target specific JSON paths (JSON Merge Patch) is a more reliable method for ensuring desired state configuration.