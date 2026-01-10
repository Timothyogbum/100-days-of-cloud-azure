## Day 5: Azure Networking - Multi-VNet Architecture
**Objective:** Provision a secondary, isolated Virtual Network (VNet) for datacenter-specific workloads as part of a phased cloud migration.

**Technical Configuration:**
- **Resource Name:** `datacenter-vnet`
- **Region:** `East US`
- **CIDR Block:** `192.168.0.0/24` (IPv4)
- **Deployment Method:** Azure CLI (Idempotent execution)

**Architecture Decision Record (ADR):**
By utilizing a distinct `/24` address space for the datacenter tier, we ensure network isolation from the primary `xfusion-vnet`. This follows the principle of least privilege at the network layer, allowing for granular Network Security Group (NSG) rules and simplified routing between segments.