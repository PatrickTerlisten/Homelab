# Create a cloud-init Ubuntu Template

## Download Ubuntu Cloud Image

wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

## Create a new template

In this example the ID of the VM is 9999.

```bash
qm create 9999 --ostype=l26 --agent=1 --memory 1024 --net0 virtio,bridge=vmbr0 --boot c --bootdisk scsi0 --serial0 socket --vga serial0
qm importdisk 9999 jammy-server-cloudimg-amd64-disk-kvm.img local
qm set 9999 --scsihw virtio-scsi-pci --scsi0 local:9999/vm-9999-disk-0.raw,discard=on,ssd=1
qm set 9999 --ide2 local:cloudinit
qm template 9999
```

## Deploy a new VM from the template

```bash
qm clone 9999 1000 --name ubuntu-ci
qm set 1000 --sshkeys ~/.ssh/id_homelab.pub
qm set 1000 --ipconfig0 ip=192.168.20.101/24,gw=192.168.20.254
```

## Add additional cloud-init customizations

To add additional customizations, create a cloud-init yaml file unter /var/lib/vz/snippets on your Proxmox node.

```bash
touch /var/lib/vz/snippets/1000-cloud-init.yml
```

Add the following content to the file.

```yaml
#cloud-config

hostname: ubuntu-ci
manage_etc_hosts: true
users:
- name: ansible
  gecos: Ansible Service Account
  groups: users,admin,wheel
  sudo: ALL=(ALL) NOPASSWD:ALL
  shell: /bin/bash
  lock_passwd: true
  ssh_authorized_keys:
    - ssh-ed25519 xxxxxxxxxxxxxxxxxxxxxxxxxx/gR1Oxoq3UtY8uuISBJOVgdloZh/xxxxxxxxxxxxxx homelab@blazilla.de
- name: jdoe
  gecos: John Doe
  groups: users,admin,wheel
  sudo: ALL=(ALL) NOPASSWD:ALL
  shell: /bin/bash
  lock_passwd: true
  ssh_authorized_keys:
    - ssh-ed25519 xxxxxxxxxxxxxxxxxxxxxxxxxx/gR1Oxoq3UtY8uuISBJOVgdloZh/xxxxxxxxxxxxxx homelab@blazilla.de
package_upgrade: true
package_reboot_if_required: true
locale: en_GB.UTF-8
timezone: Europe/Berlin
runcmd:
  - sed -i 's/[#]*PermitRootLogin yes/PermitRootLogin prohibit-password/g' /etc/ssh/sshd_config
  - sed -i 's/[#]*PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
  - sed -i 's/#HostKey \/etc\/ssh\/ssh_host_ed25519_key/HostKey \/etc\/ssh\/ssh_host_ed25519_key/g' /etc/ssh/sshd_config
  - systemctl reload ssh
  - systemctl enable qemu-guest-agent
  - systemctl start qemu-guest-agent
packages:
  - qemu-guest-agent
```

Add the cloud-init file to the newly created VM and power-on the VM.

```bash
qm set 1000 --cicustom "user=local:snippets/1000-cloud-init.yml"
```

After a couple of minutes you should be able to login into your shiny new Ubuntu VM
