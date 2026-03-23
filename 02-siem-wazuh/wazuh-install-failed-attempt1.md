# Wazuh Install - Attempt 1 (Failed)

## Date
March 2026

## Goal
Install Wazuh all-in-one on a fresh Ubuntu Server 24.04 VM using the official Wazuh quick install script. The plan was to get the full stack running: manager, indexer, and dashboard all on one VM.

## VM Specs

| Setting | Value |
|---|---|
| RAM | 3072MB (3GB) |
| CPU Cores | 2 |
| Disk | 50GB |
| Network | vmbr0 (DHCP, 192.168.100.46) |
| OS | Ubuntu Server 24.04.4 LTS |

## What I did

Built a fresh Ubuntu Server VM in Proxmox with 2 cores, 3GB RAM, and 50GB disk. After install I SSHed in from my laptop and ran the standard update first:

```
sudo apt update && sudo apt upgrade -y
```

Then ran the Wazuh all-in-one install script from the official docs. The first attempt with 2GB RAM got rejected immediately with a warning that the system did not meet the minimum requirements. Bumped the VM to 3GB RAM and ran it again with the -i flag to ignore the warning:

```
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i
```

The installer ran through the indexer and manager components fine. It got to the dashboard install and failed. The installer then automatically removed everything it had installed and left the system clean.

## Problems I hit

**Problem 1 - RAM warning on first run**

The installer threw this right away:

```
Error: insufficient RAM. Minimum required is 4GB.
```

Fixed by bumping VM RAM from 2GB to 3GB in Proxmox hardware settings and adding the -i flag to ignore the check.

**Problem 2 - Dashboard install failed**

The installer got through the indexer and manager but crashed during the dashboard component. It printed:

```
Error: wazuh dashboard installation failed
```

Then started auto-removing everything.

**Problem 3 - Write errors in the log**

Checked the install log with:

```
sudo grep -i "error" /var/log/wazuh-install.log
```

Saw write errors and no space left on device errors even though df -h showed only 31% disk usage. Checked /tmp separately and that was fine too.

**Problem 4 - wazuh-manager service failed with exit code 127**

Dug deeper with:

```
sudo journalctl -xeu wazuh-manager.service | tail -30
```

Exit code 127 means command not found. This pointed to a corrupted install where some files did not get written correctly during the install process.

## What failed and why

The root cause was insufficient RAM. 3GB is not enough for the Wazuh all-in-one stack. During install the system ran out of available RAM, which caused disk write failures as the system struggled, which corrupted the package installation partway through. The wazuh-manager exit code 127 was a symptom of that corruption, not the root cause.

The -i flag lets you bypass the RAM check but that does not mean the system can actually handle the install. The warning exists for a reason.

## What I would do differently

Give the VM the full 4GB RAM that Wazuh actually requires. At the time of this attempt the other VMs had not been built yet so there was nothing competing for the HP AIO's 8GB. There was no good reason to be conservative with RAM on this VM.

Deleted the VM entirely in Proxmox and rebuilt with 4096MB RAM for attempt 2.

## References
- Wazuh quickstart guide - https://documentation.wazuh.com/current/quickstart.html
- Wazuh package repositories - https://documentation.wazuh.com/current/installation-guide/packages-list.html
