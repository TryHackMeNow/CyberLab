# CyberLab

*VirtualBox*-based hacking lab which can be run on a single PC. All lab computers (hosts) are virtual machines executed within *VirtualBox*. The hosts are partitioned into a LAN, WAN and a DMZ network segment. The traffic between the network segments is routed by a *pfSense* firewall router. The firewall router is itself a virtual machine. Hosts include a Kali-Linux-based attacker box, Metasploitable- and OWASP-BWA victim boxes and a Windows AD domain controller.

![Network Overview](./Illustrations/CyberLab%20Overview.drawio.svg)

Similar to an external host, all traffic between the attacker and a particular CyberLab host must be routed through the firewall. The only difference is that the attack route has no additional WAN hops. Therefore, this setup can effectively simulate an attack originating from an external network.

# Setup Instructions
The lab setup is a two-step procedure:

### 1. Download VirtualBox

Download *VirtualBox* for your host OS from www.virtualbox.org and install the software. 

### 2. Import VMs into VirtualBox

Download the virtual machine files below and import them into *VirtualBox* (File -> Import Appliance):

| Virtual Machine                                                          | File Size  |
| :---------------                                                         | ---------: |
| **Version 0.9 (current)**                                                |            |
| [firewall.ova](https://drive.proton.me/urls/3QCCH2103M#FL0Mvyz7L45t)     | 496 MB     | 
| winserver.ova                                                            | 4362 MB    |
| [winxp.ova](https://drive.proton.me/urls/6VG9V701VG#zHszvhBsyNrO)        | 1037 MB    |
| debserver.ova                                                            | 2464 MB    |
| [meta.ova](https://drive.proton.me/urls/GTWSPT37BG#cSaLFrntNRT9)         | 640 MB     |
| owasp.ova                                                                | 2426 MB    |
| kali.ova                                                                 | 6831 MB    |

Each file contains the virtual machine (VM) of the named CyberLab host. 

After importing, each host can be booted or shutdown separately from within *VirtualBox*. When a host is booted up, a window will open showing the host's terminal or GUI. You may now start to (counter-) hack your way through the network by interacting with the different hosts.

**Notes:**
- CyberLab is intentionally designed for an uncomplicated and fast setup. This has the tradeoff of somewhat larger file size. You may off course choose to download only the host-VMs that are of interest to you.
- Virtual machine files might be temporarily unavailable due to servicing.

# Hosts
| Host Name    | IP (Static)       | Subnet   | Description                   | OS                | Role     | 
| :----------- | :-----------------| -------- | :---------------------------- | ----------------- | -------: | 
| firewall     | `172.20.10.10`    | WAN      | pfSense firewall router       | FreeBSD           | Victim   |     
| winserver    | `172.20.20.12`    | LAN      | AD domain controller          | Win-Server 2012   | Victim   |     
| winxp        | `172.20.20.9`     | LAN      | Generic workstation           | Windows XP Pro    | Victim   |     
| debserver    | `172.20.30.40`    | DMZ      | Server (Web/Mail/FTP/SMB...)  | Debian Linux      | Victim   |     
| meta         | `172.20.30.30`    | DMZ      | This host is meta-exploitable | Ubuntu-Linux      | Victim   | 
| owasp        | `172.20.30.80`    | DMZ      | This host has broken webapps  | Ubuntu-Linux      | Victim   |     
| kali         | `172.20.40.40`    | ATCK     | This host has 'the' tools     | Debian-Linux      | Attacker |     
|              |                   |          |                               |                   |          | 

**Notes** 
- All host have static IPv4 addresses (although DHCP is offered within each subnet).
- All hosts are currently set to use german regional language settings by default. This includes the keyboard layout.

# Users

The following user accounts (or a subset thereof) are registered on CyberLab hosts:

| Name              | Login as               | Password | Admin  |  Role    |
| :---------------- | :-------------------   | ---------| :----: |--------: |
| Administrator     | `VICTIM\Administrator` | admin    | x      | Victim   |
| alice             | `VICTIM\alice`         | alice    | x      | Victim   |
| bob               | `VICTIM\bob`           | bob      |        | Victim   |
| trent             | `VICTIM\trent`         | trent    |        | Victim   |
|                   |                        |          |        |          |

The default login password **TryHackMe!** is supported for all users on all hosts.

# Domain
Host `winserver` is an Active Directory Domain Controller. The domain name is `victim.local`. The fully qualified domain name of a network resource (user, host) is thus `<some_resource>.victim.local`. For example, by entering `http://www.debserver.victim.local` into a web browser on a lab host, the home page of the debian web server will show up.

### Shared Folders
Hosts `debserver`, `winxp` and `winserver` have access to each other's shared folders via the SMB network protocol. For example, type in `\\debserver` into windows explorer to see the folder's shared by `debserver`.

# Firewall
Host `firewall.victim.local` is the lab's central node. It runs *pfSense CE* and with that is a firewall with IP routing, DHCP and DNS resolution capabilities. The firewall is managed via web interface. The web interface is online at `http://172.20.20.10` (port 80). User: `admin`, password: `TryHackMe!`.

### Network Adapters
The firewall router has 4 network adapter cards. Each network adapter acts as the local gateway for the attached network segment. For example, the default gateway of the `LAN` subnet has IP address `172.20.20.10`. The firewall router forwards DNS requests it cannot resolve to the WAN interface (internet) and the active directory domain controller.

| Adapter  | Name     | IP                | Subnet | VirtualBox Link Type |
| :------- | :------  | :---------------- | :------| :------------------- |
| 1        | em0      | 210.200.10.1      | WAN    | NAT-Network          |
| 2        | em1      | 172.20.20.10      | LAN    | NAT-Network          |
| 3        | em2      | 172.20.30.10      | DMZ    | NAT-Network          |
| 4        | em3      | 172.20.40.10      | ATCK   | NAT-Network          |
|          |          |                   |        |                      |

# License Information
This contribution uses the following free software components:

| Software Component       | Version (amd64) |
| :----------------------- | --------------: |
| pfSense CE               | 2.7.2           |
| Kali-Linux               | 2026.1          |
| Metasploitable-2         | 2.0.0           |
| OWASP-BWA                | 1.2             |
| Debian-Linux             | 13.5.0          |
| Windows Server           | 2012 R2         |
| Windows XP               | Pro SP1         |

All above software components were downloaded as pre-build ISO images. As such they are free to use in non-commercial software projects without license restrictions. This contribution modified the above components via manual configuration and re-packaging into the new 'source' state, which are the editable virtual machines downloadable from this repository.
