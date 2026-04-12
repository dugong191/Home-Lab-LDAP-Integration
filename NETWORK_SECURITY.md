### Netplan Security Hardening
- **Issue:** Permissions for `/etc/netplan/01-netcfg.yaml` were too open.
- **Fix:** Applied `chmod 600` to ensure only root can access network configurations.
- **Command:** `sudo chmod 600 /etc/netplan/01-netcfg.yaml`
