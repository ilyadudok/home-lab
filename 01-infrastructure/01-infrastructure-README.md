# 01 - Infrastructure

**Status: In Progress**

## What this is

This covers the physical and virtual foundation of the lab. Before anything else can run, I needed a hypervisor up and a network design figured out. This document tracks the planning decisions and will get updated as I actually build it out.

## Hypervisor - Proxmox VE

Going with Proxmox on the HP Pavilion AIO. A few reasons:

- Bare metal install so all the hardware goes to the lab, no host OS overhead
- Web GUI means I can manage everything from my laptop without touching the machine
- More realistic than running VMs inside Windows - this is closer to how it works in a real environment
- Free, no license headaches

The HP AIO is getting wiped. Windows 10 Home is gone.

## Attacker machine - Kali Linux

Bare metal Kali on the HP Envy. Keeping it as a dedicated physical machine for a few reasons:

- No hypervisor overhead for scanning tools
- Easier to physically isolate from the network when running attack exercises
- The battery on this machine is faulty and doesn't hold a charge reliably. It stays plugged into AC power at all times. Not a problem for a lab machine that never moves.

## Network design

### Physical

```
ISP (1Gbps fiber)
       |
  ISP router (192.168.1.1)
       |
  Home LAN (192.168.1.x)
       |
  HP AIO - Proxmox host (192.168.1.10) -- wired ethernet
  Dell D630 - sensor node               -- wired ethernet
  Lenovo Yoga - daily driver            -- wifi or wired
```

### Virtual - Proxmox bridges

**vmbr0 - lab network (10.0.10.x)**
- NAT to internet for updates
- VMs: pfSense, Wazuh, Windows Server 2022, Windows 10, Ubuntu Desktop, OpenVAS

**vmbr1 - isolated attack segment (10.0.99.x)**
- No internet route - pfSense deny rule enforces this
- VMs: Metasploitable 2, DVWA, VulnHub targets
- Kali connects here for attack exercises only

The isolation on vmbr1 is intentional and important. Running attack tools against anything with internet access is both a legal risk and just bad practice. The pfSense rule will be documented with screenshots once that VM is deployed.

## Hardware

| Device | Specs | Role | OS |
|---|---|---|---|
| HP Pavilion AIO 24 | Ryzen 5 2500U, 8GB RAM, 1TB HDD | Proxmox hypervisor | Proxmox VE |
| HP Envy TS m6 | i5 4200U, 8GB RAM, 750GB HDD | Kali attacker/scanner | Kali Linux |
| Dell Latitude D630 | Core 2 Duo T8300, 4GB RAM, 500GB | Passive sensor | Linux Mint XFCE |
| Lenovo Yoga 7 | i7 12th Gen, 32GB RAM, 1TB NVMe | Daily driver / lab mgmt | Windows 11 Home |

## ISOs

- [ ] Proxmox VE - https://www.proxmox.com/en/downloads
- [ ] Kali Linux installer - https://www.kali.org/get-kali/

## Next step

Flash Proxmox ISO to USB, install on HP AIO, configure the two virtual bridges. Will document the install process as it happens.
