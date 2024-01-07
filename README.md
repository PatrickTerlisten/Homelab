# Patricks Homelab

[![tag](https://img.shields.io/github/v/tag/patrickterlisten/homelab?style=flat-square&logo=semver&logoColor=white)](https://github.com/patrickterlisten/homelab/tags)
![GitHub License](https://img.shields.io/github/license/patrickterlisten/homelab?style=flat-square&logo=semver&color=white)
![GitHub Repo stars](https://img.shields.io/github/stars/patrickterlisten/homelab?style=flat-sqaure&logo=semver&color=white)


After years of relying on hardware at work or cloud infrastructure, I started to setup a lab at home again.

After being more than two decades in IT, while primarly working on the full stack (server, storage, networking, virtualization), the stacks starts to shift again. While using virtualization with VMware and Hyper-V for over 15y, Container, Cloud and IaC seems to be the next big stack evolution. This lab is mainly used to get my hands dirty with

- Linux
- Docker/ Podman
- Proxmox, and
- Infrastructure-as-Code with Ansible, Terraform and Packer

## Overview

Project status: **ALPHA**

This project is still in a very early stage.

### Hardware

Instead of using four HPE ProLiant DL360 G7 and a Synology NAS as in my first lab, I decided to use something smaller, quiter and less power hungry. The hardware of choise was a chinese barebone [HUNSN RJ42](https://amzn.eu/d/3kxxmGl).

- Intel N100 (4C/4T Alder Lake-N 12th Gen)
- DDR5-4800 MHz
- 2x M.2 NVMe 2280 Slot
- 1x TF-Card Slot
- 2x SATA 6 Gbit/s
- 4x Intel 2.5GbE I226-V
- 2x HDMI 2.1
- 1x Display Port 1.4
- 4x USB-A 2.0, 2x USB-A 3.0
- 1x USB-C for data and display

I ordered the system without SSD and memory and added these components:

- Corsair VENGEANCE 32GB DDR5 SODIMM (CMSX32GX5M1A4800C40)
- WD Black SN770 NVMe with 1 TB
- WD Green SN350 NVMe with 2 TB
  
The whole system is fanless and consumes about 10W per hour, which makes ~30 € per year (based on the current power costs in Germany).
