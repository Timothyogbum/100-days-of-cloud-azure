Lab Report: Azure Container Registry & Image Lifecycle
## Objective
Create a secure, centralized repository for Docker images and automate the transition of a local Python application to an Azure-managed environment.

## Technical Implementation Details
ACR Provisioning:

Resource: nautilusacr22521.

SKU: Basic (cost-effective for development/lab use).

Region: East US.

Authentication:

Utilized az acr login to exchange Azure CLI credentials for a temporary Docker access token, allowing secure push operations.

Image Engineering:

Context: Built from /root/pyapp on the azure-client host.

Base Image: python:3.8-slim (optimized for size and security).

Tagging: Followed the strict ACR naming convention: nautilusacr22521.azurecr.io/nautilusacr22521:latest.

Cloud Sync:

Successfully pushed all image layers to the Azure Container Registry.

Verified residency via the az acr repository list command.

## Final Verification Summary
Metric,Result
Registry Name,nautilusacr22521
Login Server,nautilusacr22521.azurecr.io
Provisioning State,Succeeded
Docker Build Status,Successfully built 0d36dabc1804
Registry Repository,nautilusacr22521 exists

