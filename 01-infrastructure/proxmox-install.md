# Proxmox Install

## Date
March 2026

## What I did

Wiped the HP Pavilion AIO and installed Proxmox VE bare metal. Used Balena Etcher to flash the Proxmox ISO to a USB drive, booted from it on the AIO, and went through the graphical installer.

During install I set:
- Hostname: proxmox.home.lab
- IP: 192.168.100.10/24
- Gateway: 192.168.100.1
- DNS: 8.8.8.8

## Problem I ran into

When I first set the IP during install I used 192.168.1.10 because I assumed that was my home network range. It wasn't. My ISP router actually runs 192.168.100.x so Proxmox ended up on the wrong subnet and I couldn't reach the web GUI from my laptop at all.

To fix it I edited the network config file directly on the Proxmox terminal:

```
nano /etc/network/interfaces
```

Changed the address from 192.168.1.10/24 to 192.168.100.10/24 and the gateway from 192.168.1.1 to 192.168.100.1.

The config file change alone didn't take effect immediately so I flushed and reset the IP manually to test it:

```
ip addr flush dev vmbr0
ip addr add 192.168.100.10/24 dev vmbr0
ip route add default via 192.168.100.1
```

That got it working. Then rebooted to confirm the config file change was permanent. It was.

## How the networking works in Proxmox

Proxmox doesn't put an IP directly on the physical ethernet port (enp1s0). Instead it creates a virtual bridge called vmbr0 and bridges enp1s0 into it. The IP address sits on vmbr0. This is what allows VMs to share the same physical network connection - they all plug into the bridge.

## End result

- Web GUI accessible at https://192.168.100.10:8006 from my laptop
- Subscription popup on login - this is normal, clicked X, free version has no real limitations
- Ran apt update and apt upgrade to fully patch the system

## Notes

- USB can be removed after install, Proxmox boots from internal drive
- Manage everything through the web GUI, no need to touch the physical machine
- Storage shows as local (for ISOs) and local-lvm (for VM disks)
