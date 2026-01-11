## Day 6: Azure Infrastructure - Scalable VNet Design

### 📋 Objective
Create a primary Virtual Network (`devops-vnet`) and a logically isolated subnet (`devops-subnet`) in the East US region.

### ⚙️ Provisioning Details
- **Resource Group:** `kml_rg_main-a9e66e0196e943d2`
- **Location:** `eastus`
- **Address Space:** `10.0.0.0/16`
- **Subnet CIDR:** `10.0.0.0/24`

### ✅ Provisioning Result (JSON CLI Output)
Running the `az network vnet create` command returned the following success state:

```json
{
  "newVNet": {
    "addressSpace": {
      "addressPrefixes": ["10.0.0.0/16"]
    },
    "location": "eastus",
    "name": "devops-vnet",
    "provisioningState": "Succeeded",
    "resourceGroup": "kml_rg_main-a9e66e0196e943d2",
    "subnets": [
      {
        "addressPrefix": "10.0.0.0/24",
        "name": "devops-subnet",
        "provisioningState": "Succeeded"
      }
    ]
  }
}

📊 Network Capacity AnalysisLayerCIDRTotal IP CapacityReserved IPsVNet10.0.0.0/1665,5360Subnet10.0.0.0/242565 (Azure Default)🏆 Outcome: The network is successfully partitioned and ready for Network Security Group (NSG) implementation.