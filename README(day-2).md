## Day 2: Azure Virtual Machine Migration
**Objective:** Provision a standardized Linux instance to begin the infrastructure migration to Azure.

### 📋 Requirements Met:
* **Instance Name:** `datacenter-vm`
* **Region:** `West US`
* **Image:** Ubuntu 22.04 LTS
* **Size:** `Standard_B1s`
* **Storage:** 30 GB Standard HDD (`Standard_LRS`)
* **Security:** Attached NSG allowing Inbound SSH on Port 22.

### 🚀 Technical Verification:
* **CLI Deployment:** Used `az vm create` with specific overrides for disk sizing.
* **Connectivity:** Established successful SSH handshake using generated RSA keys.
* **Disk Audit:** Confirmed 29.8GB partition availability via `df -h`.

> **Hurdle:** Needed to reconcile SSH keys across different terminal sessions. 
> **Solution:** Used `--generate-ssh-keys` to ensure immediate access from the current host.
