# Home-Lab-LDAP-Integration
SSSD and LDAP integration on Ubuntu Server 24.04

# Home-Lab-LDAP-Integration

This project documents the successful integration of SSSD with an LDAP backend on Ubuntu Server 24.04. It covers troubleshooting connectivity, resolving GID conflicts, and implementing group-based sudo privileges.

## 🛠 Project Components
* **OS:** Ubuntu Server 24.04
* **Identity Service:** SSSD (System Security Services Daemon)
* **Backend:** LDAP (OpenLDAP/AD)
* **Tools:** Vim, Git, SSSD-Tools

## 🚀 Configuration & Troubleshooting

### 1. Resolving "Backend is Offline"
Initially, SSSD failed to connect to the LDAP server. The fix involved:
* Switching from hostname resolution to a static IP (`192.168.56.101`).
* Setting `ldap_tls_reqcert = allow` to bypass certificate handshake issues in the lab environment.
* Disabling TLS for initial validation using `ldap_auth_disable_tls_never_use_in_production = true`.

### 2. GID Conflict Resolution
A conflict was discovered where a local `devops` group (GID 1002) existed alongside the LDAP `devops` group (GID 10001).
* **Action:** Used **Vim** to remove the local entry from `/etc/group`.
* **Result:** SSSD successfully mapped `user1` to the LDAP GID 10001 as the primary group.

### 3. Sudo Authorization
Instead of managing individual users, administrative rights were granted to the entire LDAP group.
* **Command:** `sudo EDITOR=vim visudo`
* **Rule Added:** `%devops ALL=(ALL:ALL) ALL`

## 📋 Verification Commands
```bash
# Check user and group mapping
getent passwd user1
getent group devops
id user1

# Test authentication and sudo
su - user1
sudo ls /root

