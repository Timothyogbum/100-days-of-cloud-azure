# Azure Day 13: Secure Root Access Orchestration

## 🎯 Project Objective
Configure seamless, password-less root SSH access from the Azure Landing Host to the `datacenter-vm` to support high-level system administration and automated infrastructure management.

## 🛠️ Technical Implementation
- **Authentication:** RSA 2048-bit Key-Based Authentication.
- **Troubleshooting & Resolution:**
    - **Issue 1:** Missing client identity. *Action:* Generated a new RSA key pair via `ssh-keygen`.
    - **Issue 2:** Azure's forced-user login restriction. *Action:* Sanitized the remote `authorized_keys` file to remove the forced command prefix.
    - **Issue 3:** Restricted SSH Policy. *Action:* Updated `sshd_config` to `PermitRootLogin prohibit-password` and restarted the service.
- **Verification:** Confirmed successful login directly into the root shell from the client host.

## 💡 Engineering Insight: Automation Foundations
In modern DevOps, manual passwords are a security risk and a blocker for automation. Establishing "Key-Based Trust" is the prerequisite for deploying configuration management tools like Ansible, which requires root-level access to manage packages, services, and security configurations across the datacenter.



## ✅ Final Status
The administrative bridge is complete. The `datacenter-vm` is now ready for automated provisioning and orchestration.