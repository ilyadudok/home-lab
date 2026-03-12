# Kali Linux Install

## Date
March 2026

## What I did

Flashed the Kali Linux installer ISO to a USB drive using Balena Etcher, same tool I used for Proxmox. Then installed Kali bare metal on the HP Envy.

## Hardware note

The HP Envy has a battery fault - it doesn't hold a charge reliably. This machine lives plugged into AC power permanently. Not a problem since it never moves.

## Problem - Secure Boot

When I first booted from the USB I got this error:

```
selected boot image did not authenticate
```

This is a Secure Boot issue. The HP Envy BIOS was rejecting the Kali USB because it isn't signed by Microsoft. Fixed it by going into BIOS (F10 on boot), navigating to the Security section, and disabling Secure Boot. After that the USB booted fine.

## Install settings

- Install type: Graphical
- Primary network interface: eth0 (wired ethernet)
- Hostname: kali-lab
- Domain: left blank
- Username: eli
- Partitioning: guided - use entire disk (SCSI1)

## Notes

- Wired ethernet plugged in before boot so the installer picked up the network automatically
- Disk erase took a while on the 750GB HDD, that's normal
- This machine is dedicated to offensive tools and scanning only - Nmap, Metasploit, Burp Suite, Nessus Essentials
- During attack exercises this machine connects to the isolated lab segment (10.0.99.x) only - no internet access during those sessions
