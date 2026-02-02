That 2-day marathon was a masterclass in why "technically correct" isn't always "validator correct." You fought through every possible Azure roadblock—from silent backend field mismatches to rigid naming requirements and storage policy traps.

Here is a detailed README that documents the solution and honestly catalogs the struggles you faced.

Lab Report: Private Azure VNet & VM Deployment
## Objective
To deploy a secure, isolated infrastructure in Central US consisting of a private Virtual Network, a subnet-attached Network Security Group, and a private Virtual Machine accessible only via internal SSH.

## The Struggle (Lessons Learned the Hard Way)
This lab presented three specific challenges that required iterative troubleshooting:

The "Singular" Field Trap: Azure's CLI often defaults to a plural array (sourceAddressPrefixes). However, the lab validator was looking for a string match in the singular sourceAddressPrefix field. If the value lived in the array, the validator returned a "Fail" even if the rule was active.

Resource Naming Strictness: Initially, resources were named using the nautilus- prefix. The validator was hard-coded to look for devops- specific names, causing it to report that the resources didn't exist.

The "Double CIDR" Requirement: Standard networking logic usually dictates a destination of * (Any) or a specific VM IP. This validator specifically required the VNet CIDR (10.0.0.0/16) to be present in both the Source and Destination fields of the NSG rule to pass the "Does Not Allow Access" check.

Storage Policy Constraints: Default VM creation attempts failed initially because the subscription had a policy enforcing Standard_LRS. Any attempt to create a VM with standard SSD/Premium disks resulted in an immediate DisallowedByPolicy error.

## Final Architecture
Location: centralus

VNet: devops-priv-vnet (Address Space: 10.0.0.0/16)

Subnet: devops-priv-subnet (10.0.1.0/24)

NSG: devops-priv-nsg (Attached to Subnet)

VM: devops-priv-vm (Private IP only, Standard_LRS disk)

## Implementation Steps
### 1. Networking Setup
az network vnet create \
    --resource-group $RG \
    --name devops-priv-vnet \
    --location centralus \
    --address-prefix 10.0.0.0/16 \
    --subnet-name devops-priv-subnet \
    --subnet-prefix 10.0.1.0/24

### 2. Security Configuration (The Winning Logic)
The key was creating the rule with the CIDR in both fields and applying the NSG to the subnet.

# Create NSG
az network nsg create -g $RG -n devops-priv-nsg

# Create the specific 'Validator-Friendly' Rule
az network nsg rule create \
    --resource-group $RG \
    --nsg-name devops-priv-nsg \
    --name AllowInternalSSH \
    --priority 100 \
    --direction Inbound \
    --access Allow \
    --protocol Tcp \
    --source-address-prefix '10.0.0.0/16' \
    --destination-address-prefix '10.0.0.0/16' \
    --destination-port-range 22

# Attach to Subnet
az network vnet subnet update \
    -g $RG --vnet-name devops-priv-vnet \
    -n devops-priv-subnet \
    --network-security-group devops-priv-nsg

### 3. VM Provisioning
az vm create \
    --resource-group $RG \
    --name devops-priv-vm \
    --image Ubuntu2204 \
    --storage-sku Standard_LRS \
    --vnet-name devops-priv-vnet \
    --subnet devops-priv-subnet \
    --nsg "" \
    --public-ip-address "" \
    --admin-username azureuser \
    --generate-ssh-keys

## Verification Results
Field,Expected,Actual
NSG Source,10.0.0.0/16,✅ 10.0.0.0/16
NSG Destination,10.0.0.0/16,✅ 10.0.0.0/16
Public IP,None,✅ None
VM Name,devops-priv-vm,✅ devops-priv-vm