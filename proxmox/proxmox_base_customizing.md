# Proxmox VE base customization

## Removing Enterprise repositories

By default, the Enterprise repositories are enabled. If you don't have an Enterprise subscription, it is safe to remove the Enterprise repositories fomr the `/etc/apt/sources.list`.

My `/etc/apt/sources.list` looks like this:

```bash
root@proxmox:~# cat /etc/apt/sources.list
deb http://ftp.de.debian.org/debian bookworm main contrib
deb http://ftp.de.debian.org/debian bookworm-updates main contrib

# security updates
deb http://security.debian.org bookworm-security main contrib
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```

### Disable unnecessary servcices

Because I only ran a single node, I decided to disable unnecessary services. Furthermore, disabling these service should help extend the lifetime of my NMVe drive.

```bash
systemctl stop pve-ha-lrm
systemctl disable pve-ha-lrm
systemctl stop pve-ha-crm
systemctl disable pve-ha-crm
systemctl stop corosync.service
systemctl disable corosync.service
Removed "/etc/systemd/system/multi-user.target.wants/pve-ha-lrm.service".
Removed "/etc/systemd/system/multi-user.target.wants/pve-ha-crm.service".
Removed "/etc/systemd/system/multi-user.target.wants/corosync.service".
```
