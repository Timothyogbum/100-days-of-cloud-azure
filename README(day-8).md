## Day 8: Azure Compute - Managed Disk Attachment

### 📋 Objective
Extend the storage capabilities of the `xfusion-vm` by attaching a pre-existing 128GB managed disk (`xfusion-disk`) as a persistent data volume.

### ⚙️ Technical Details
- **Resource Group:** `kml_rg_main-da0b2744039c46ee`
- **VM Name:** `xfusion-vm`
- **Disk Name:** `xfusion-disk`
- **Logical Unit Number (LUN):** 0
- **Size:** 128 GB
- **Caching:** None (Standard for data disks to ensure data integrity)

### ✅ Verification Audit
The attachment was verified via the Azure CLI, confirming the disk is in the `Attach` state and not marked for detachment.

```text
$ az vm show -g kml_rg_main-da0b2744039c46ee -n xfusion-vm --query "storageProfile.dataDisks" -o table

Lun    Name          Caching    CreateOption    DiskSizeGb
-----  ------------  ---------  --------------  ------------
0      xfusion-disk  None       Attach          128

🏆 Achievement
Successfully integrated block storage into the compute layer. This enables the Nautilus application to store stateful data outside of the OS disk, allowing for easier VM redeployments and data persistence.