# CyberLab
VirtualBox-based hacking lab which can be run on a single computer. The lab computers are partitioned into a LAN, WAN and a DMZ subnet. The traffic between the subnets is routed by a pfSense firewall router. Hosts include a Kali-Linux-based attacker box, Metasploitable- and OWASP-BWA victim boxes and a Windows domain controller.

![Alt text](./Illustrations/CyberLab%20Overview%20Light.drawio.svg)

Similar to an external host, all traffic between the attacker and a particular CyberLab host must be routed through the firewall. The only difference is that the attack route has no additional WAN hops. Therefore, this setup can effectively simulate an attack originating from an external network.

# Setup Instructions
The lab setup is a two-step procedure:

### 1. Download VirtualBox

Download *VirtualBox* for your host OS from www.virtualbox.org and install the software.

### 2. Import VM-Appliances into VirtualBox

Download the cyber lab VM appliance files below and import them into *VirtualBox* (File -> Import Appliance):

- firewall.ova
- kali.ova
- winxp.ova
- winserver.ova
- debserver.ova
- meta.ova
- owasp.ova

Each file contains the virtual machine (VM) of the named host. After importing, each host can be booted or shutdown separately from within *VirtualBox*. When a host is booted up, a window will open showing the host's terminal or GUI. You may now start to (counter-) hack your way through the network by interacting with the different hosts.

**Notes**: 
- Appliance files are temporarily unavailable due to servicing.

# Hosts
| Host Name    | IP (Static)       | Subnet   | Description                   | OS                | Role     | 
| :----------- | :-----------------| -------- | :---------------------------- | ----------------- | -------: | 
| firewall     | `172.20.10.10`    | WAN      | pfSense Firewall Router       | FreeBSD           | Victim   |     
| winserver    | `172.20.20.12`    | LAN      | Windows AD Domain Controller  | Win-Server 2012   | Victim   |     
| winxp        | `172.20.20.9`     | LAN      | Generic Windows Workstation   | Windows XP Pro    | Victim   |     
| debserver    | `172.20.30.40`    | DMZ      | Server (Web/Mail/FTP/SMB...)  | Debian Linux      | Victim   |     
| meta         | `172.20.30.30`    | DMZ      | This host is mega-exploitable | Ubuntu-Linux      | Victim   | 
| owasp        | `172.20.30.80`    | DMZ      | This host has broken webapps  | Ubuntu-Linux      | Victim   |     
| kali         | `172.20.40.40`    | ATCK     | This host has 'the' tools     | Debian-Linux      | Attacker |     
|              |                   |          |                               |                   |          | 

**Notes** 
- All host have static IPv4 addresses (although DHCP is offered within each subnet).
- Prepend `victim.local` to the host name to get the host's fully qualified domain name, e.g. `firewall.victim.local`.
- All hosts are currently set to use german regional settings by default. This includes the keyboard layout.

# Users
| Name              | Login as               | Admin  | Default Password  | Role      |
| :---------------- | :-------------------   | :----: | ----------------  | --------: |
| Administrator     | `VICTIM\Administrator` | x      | TryHackMe!        | Victim    |
| alice             | `VICTIM\alice`         | x      | TryHackMe!        | Victim    |
| bob               | `VICTIM\bob`           |        | TryHackMe!        | Victim    |
| trent             | `VICTIM\trent`         |        | TryHackMe!        | Victim    |
|                   |                        |        |                   |           |

**Notes**: 
- prepend `victim.local` to the user name to get the user's fully-qualified domain name (FQDN), e.g. `alice.victim.local`.

# pfSense
Host `firewall.victim.local` is the lab's central node. It runs the pfSense software and with that has routing, firewall, DHCP and DNS resolution capabilities. This host has 4 network adapter cards. Each network adapter acts as the local gateway for the attached subnet. For example, the default gateway of the `LAN` subnet has IP address `172.20.20.10`. 

| Adapter  | Name     | IP                | Subnet | VirtualBox Link Type |
| :------- | :------  | :---------------- | :------| :------------------- |
| 1        | em0      | 210.200.10.1      | WAN    | NAT-Network          |
| 2        | em1      | 172.20.20.10      | LAN    | NAT-Network          |
| 3        | em2      | 172.20.30.10      | DMZ    | NAT-Network          |
| 4        | em3      | 172.20.40.10      | ATCK   | NAT-Network          |
|          |          |                   |        |                      |

**Notes**
- The firewall router forwards DNS requests it cannot resolve to the WAN and the active directory domain controller.
