📂 Azure Infrastructure: Storage Expansion & Mounting
Project: Nautilus Web Application Storage Scaling
1. Executive Summary
The storage capacity of xfusion-vm was doubled to accommodate increased application logging and database requirements. This operation involved a critical resize of the primary OS disk and the provisioning of a secondary high-capacity data disk.

2. Technical Specifications
Component,Resource Name,Initial Size,Final Size
OS Disk,Managed OS Disk,32 GiB,64 GiB
Data Disk,xfusion-disk,N/A,64 GiB
Disk SKU,Standard HDD,—,Standard_LRS

3. Implementation Workflow
Phase A: OS Disk Expansion
Deallocation: The VM was placed in a Deallocated state to permit modification of the OS VHD.

Azure Update: The disk size was updated to 64 GiB via the Azure CLI.

OS Recognition: Ubuntu 22.04 automatically recognized the larger block device (/dev/sda) and expanded the root partition (/dev/sda1) upon reboot.

Phase B: Data Disk Provisioning & Mounting
Attachment: xfusion-disk was created and attached to the VM via LUN (Logical Unit Number).

Filesystem Creation: Initialized the raw block device /dev/sdc with an ext4 filesystem.

Mounting: Established a permanent mount point at /mnt/xfusion-disk.

4. Verification & Persistence
Capacity Check: Verified using df -h, showing 63G total on the new mount point and 62G on the root partition.

Reboot Protection: The /etc/fstab file was updated with the nofail parameter to ensure that if the data disk is ever detached, the VM can still successfully complete its boot cycle.

