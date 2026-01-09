## Day 4: Azure Virtual Network (VNet) Deployment
**Scenario:** Establishing the foundational network for the Nautilus incremental migration.

**Technical Configuration:**
- **Resource:** `Microsoft.Network/virtualNetworks`
- **Name:** `xfusion-vnet`
- **Address Space:** `10.0.0.0/16`
- **Region:** `eastus`
- **ID:** `/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-52483f024afd4950/providers/Microsoft.Network/virtualNetworks/xfusion-vnet`

**Outcome:** Network successfully provisioned and ready for subnet partitioning.