# home-lab

Personal cybersecurity home lab built on spare hardware. I use this to get hands-on with the tools and techniques that matter for SOC and vulnerability analyst work. Everything here is documented as I build it.

## Background

I have the CompTIA trifecta (A+, Network+, Security+) and wanted a place to actually practice what I studied. The goal is to build real skills, not just pass tests. I'm targeting SOC Analyst and Vulnerability Analyst roles right now, with a plan to move into cloud security over the next year or so.

## Hardware

|Machine|Specs|Role|
|-|-|-|
|HP Pavilion AIO 24|AMD Ryzen 5 2500U, 8GB RAM, 1TB HDD|Proxmox hypervisor - runs all lab VMs|
|HP Envy TS m6|Intel Core i5 4200U, 8GB RAM, 750GB HDD|Kali Linux - offensive tools and scanning|
|Dell Latitude D630|Core 2 Duo T8300, 4GB RAM, 500GB|Passive network sensor - Wireshark, packet capture|
|Lenovo Yoga 7|i7 12th Gen, 32GB RAM, 1TB NVMe|Daily driver - used for lab management only|

## Network

Two segments:

**Lab network (10.0.10.x)** - Proxmox virtual bridge with NAT to internet. This is where the SIEM, Active Directory, endpoints, and scanner VMs live.

**Isolated attack segment (10.0.99.x)** - No internet route. Intentionally vulnerable targets live here. pfSense enforces a deny rule on this segment. Nothing in this range touches the internet.

The Proxmox host itself sits on the home LAN at 192.168.100.10. The home network runs on 192.168.100.x.

## Tools (planned)

* Proxmox VE - hypervisor
* Wazuh - SIEM/XDR
* OpenVAS / Greenbone - vulnerability scanning
* Nessus Essentials - vulnerability scanning
* pfSense CE - firewall and network segmentation
* Suricata - IDS/IPS
* Kali Linux - offensive toolset
* Windows Server 2022 - Active Directory
* Microsoft Azure + Sentinel - cloud SIEM
* Metasploitable 2 / DVWA - vulnerable targets

## Projects

|#|Project|Status|
|-|-|-|
|01|Infrastructure - Proxmox install and network setup|Proxmox installed|
|02|SIEM - Wazuh deployment and endpoint agents|Planned|
|03|Vulnerability Management - OpenVAS, Nessus, VA report|Planned|
|04|Active Directory - domain setup and attack detection|Planned|
|05|Network Security - pfSense and Suricata|Planned|
|06|Packet Analysis - Wireshark captures|Planned|
|07|Incident Response - IR simulation and NIST format report|Planned|
|08|Cloud - Azure Sentinel and Defender for Cloud|Planned|

## Certs

* CompTIA A+ - done
* CompTIA Network+ - done
* CompTIA Security+ - done
* CompTIA CySA+ - studying
* Microsoft AZ-500 - planned

## Links

[LinkedIn](https://www.linkedin.com/in/eli-dudok-csis-40261119a/) · [GitHub](https://github.com/ilyadudok)

