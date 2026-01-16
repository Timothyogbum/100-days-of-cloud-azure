# Azure Day 11: VM Size Optimization (Vertical Scaling)

## 🎯 Project Objective
The Nautilus DevOps team identified `xfusion-vm` as underutilized/underpowered. The goal was to perform vertical scaling to increase compute and memory capacity while ensuring service availability post-migration.

## 🛠️ Technical Implementation

### 1. Environment Discovery
Identified the target resource group and VM location within the Azure global infrastructure.
- **Resource Group:** `KML_RG_MAIN-402C1EA4AC4045DB`
- **Location:** `westus`

### 2. Vertical Scaling (Resizing)
Used the Azure CLI to trigger a redeployment of the instance to a new hardware SKU.
- **Command:** `az vm update --resource-group KML_RG_MAIN-402C1EA4AC4045DB --name xfusion-vm --size Standard_B2s`
- **Upgrade Path:** `Standard_B1s` (1 vCPU, 1GiB RAM) ➔ `Standard_B2s` (2 vCPU, 4GiB RAM)

### 3. Lifecycle Management
Post-resize, I ensured the VM was properly initialized on the new host.
- **Command:** `az vm start --resource-group KML_RG_MAIN-402C1EA4AC4045DB --name xfusion-vm`

## ✅ Final Verification
I performed a targeted query using the `--query` flag to confirm the hardware profile and power state.

**Validation Output:**
```json
{
  "Current_Size": "Standard_B2s",
  "Power_State": "VM running",
  "VM_Name": "xfusion-vm"
}
💡 Engineering Reflection
Resizing is a disruptive operation. In a production environment, this would typically be performed during a maintenance window because the VM must be stopped and restarted on a new physical host. This task demonstrates the ability to manage cloud costs and performance through SKU management.