## Project: Nautilus DevOps Migration
This repository tracks my transition from On-Premise System Administration to Cloud Engineering.

---

### Day 1 (Cloud Day 1): Identity & Access Management
**Goal:** Create a secure RSA SSH Key Pair for the Azure migration.

**Hurdles Faced:**
* **Authentication Mismatch:** Attempted to run the username directly in the terminal. Resolved by using the `az login --use-device-code` flow to connect my local shell to the Azure cloud.
* **Resource Group Scoping:** Learned that Azure resources require a logical container. Used `az group list` to find the lab's pre-assigned Resource Group: `kml_rg_main-89a5458a2fd346f7`.
* **Standardization:** Successfully provisioned an RSA-type key named `datacenter-kp`.

**Technical Commands Used:**
- `az login`: Authenticated with Azure.
- `az group list`: Identified the Resource Group.
- `az sshkey create`: Generated the managed SSH key.
- `az sshkey list -o table`: Verified the resource was active.

---
