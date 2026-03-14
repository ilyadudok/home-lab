# ISO Inventory

ISOs currently loaded in Proxmox local storage and why each one is there.

## Ubuntu Server 24.04.4 LTS
- Source: https://releases.ubuntu.com/24.04/
- Used for: Wazuh SIEM VM and OpenVAS vulnerability scanner VM
- Why: Lightweight, stable, well documented, good community support for security tools

## Windows Server 2022 (180 day evaluation)
- Source: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
- Used for: Active Directory domain controller VM
- Why: Industry standard for AD environments, free eval ISO from Microsoft, 180 day license is plenty for lab purposes

## Windows 10 Pro
- Source: https://www.microsoft.com/en-us/software-download/windows10
- Used for: Domain joined endpoint VM, target for attack simulations and Wazuh agent monitoring
- Why: Pro required for domain join, most common enterprise endpoint OS

## Notes
- ISOs are stored in Proxmox local storage, not on any VM
- When a VM is built the ISO gets attached during install then detached after
- Evaluation licenses will need to be renewed or replaced after 180 days
