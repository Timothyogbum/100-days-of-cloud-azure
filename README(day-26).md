⚓ Project Documentation: Datacenter Public VNet
Department: Networking Team

Status: Completed & Verified ✅

1. Infrastructure Overview
The public-facing environment was provisioned in the westus region with specific focus on governance compliance.
Resource,Configuration Detail
VNet Name,datacenter-pub-vnet
Subnet,datacenter-pub-subnet (10.0.1.0/24)
VM Name,datacenter-pub-vm
Public IP,20.237.165.73 (Standard SKU)

2. Security Configuration
A Network Security Group (NSG) was implemented to secure the instance while allowing required administrative access.

NSG Name: datacenter-pub-nsg

Rule AllowSSH: Port 22 (TCP) permitted from all source addresses.

3. Compliance & Governance Adjustments
Due to active Azure Policies and regional capacity restrictions, the following specialized configurations were enforced:

VM Size: Switched to Standard_B1s to resolve westus capacity limits for the D-series.

Storage SKU: Enforced Standard_LRS to satisfy the "Non-Premium" governance policy.

Disk Size: Hard-coded to 30 GB to stay within the 128 GB administrative limit.

📑 Post-Deployment Incident Report: datacenter-pub-vm
1. Regional Capacity Exhaustion (westus)
The Error: SkuNotAvailable / Capacity Restrictions.

What Broke: The default Azure VM size (Standard_DS1_v2) was unavailable in the westus region at the time of deployment.

The Fix: Manually downgraded the instance type to Standard_B1s to find available hardware slots.

2. Governance Policy Violation (Storage SKU)
The Error: RequestDisallowedByPolicy.

What Broke: The environment has an active Azure Policy that forbids Premium_LRS (SSD) storage and any disk larger than 128 GB.

The Conflict: Most modern Ubuntu AMIs default to Premium storage. This caused the deployment to be killed by the Azure Resource Manager (ARM) mid-provisioning.

The Fix: Explicitly passed the --storage-sku Standard_LRS and --os-disk-size-gb 30 flags to force compliance with the lab's security baseline.

3. Terminal Provisioning State (The "Zombie" Resource)
The Error: PropertyChangeNotAllowed / osDisk.managedDisk.storageAccountType.

What Broke: Because the first attempt failed during creation, it left behind a "Managed Disk" resource record. When we tried to fix the command, Azure CLI attempted to update the existing failed disk. However, Azure does not allow changing a disk's SKU (from Premium to Standard) once the resource record is initialized.

The Fix: A "Nuclear Cleanup" was required—deleting the failed VM, the Network Interface (NIC), and the specific Disk ID manually before the second attempt could succeed.

4. CLI Command Compatibility
The Error: unrecognized arguments: --map-public-ip-on-launch true.

What Broke: The specific Azure CLI version in the lab environment did not support the subnet-level auto-assign flag.

The Fix: Shifted the public IP logic from the Subnet level to the VM level by using --public-ip-address datacenter-pub-ip during the az vm create call.

