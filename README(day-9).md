# Day 9: Azure Networking - Secondary NIC Attachment

## 🎯 Task Overview
The goal was to attach a pre-existing network interface (`devops-nic`) to an active virtual machine (`devops-vm`) within the `KML_RG_MAIN-CB3F16AF86824DBE` resource group.

## 🛠️ Technical Implementation & Workflow
1. **Authentication:** - Initial `az login` attempts failed due to special characters in the password. 
   - Resolved by switching to `az login --use-device-code` for interactive authentication.
2. **Resource Discovery:** - Queried the environment using `az vm list` to identify the correct Resource Group (RG).
3. **State Management:** - Identified that Azure prevents NIC attachment while a VM is in the `Running` state.
   - Executed `az vm deallocate` to release hardware resources.
4. **Hardware Configuration:** - Successfully linked the NIC using `az vm nic add`.
5. **Initialization:** - Re-initialized the VM using `az vm start` and monitored the provisioning state.

## ✅ Final Validation Results
- **NIC Count:** 2 Interfaces detected.
- **Attachment Status:** `devops-nic` verified as successfully attached (Secondary role).
- **VM Power State:** Confirmed as `VM running`.

## 💡 Key Takeaway
Managing Azure VM hardware requires a clear understanding of the "Deallocated" vs "Stopped" state. Only deallocation ensures the underlying fabric is ready for networking changes.