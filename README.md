# CyberLab
VirtualBox-based hacking lab which can be run on a single computer. The lab has a top-level domain which is organized into LAN, WAN and DMZ network segments. The traffic between the network segments is routed by a pfSense firewall router. Hosts include a Kali-Linux-based attacker box, Metasploitable- and OWASP-BWA victim boxes and a Windows domain controller.

![Alt text](./Illustrations/CyberLab%20Overview%20Light.drawio.svg)

# Setup Instructions

### Download VirtualBox

Download Oracle VirtualBox for your host OS from www.virtualbox.org.

### Import VM-Appliances into VirtualBox

Download the cyber lab VM appliance files. The lab consists of the following VM-appliances (or lab hosts):
```
firewall.ova
kali.ova
winxp.ova
winserver.ova
debserver.ova
meta.ova
owasp.ova
```
Import the VM-appliances into VirtualBox (File -> Import Appliance). 

**Note**: all hosts (virtualbox Guest-VMs) are currently set to use german regional settings by default. This includes keyboard layout.

**Note**: Appliance files are temporarily unavailable due to servicing.

### Done

Setup is complete. After importing, each VM can be started or stopped separately from within VirtualBox. When a VM is started, the terminal of the affiliated host will come up in a new window. You may now start to BLUE- or RED-team your way through the network from the provided terminals.

# Subnets
| IP                | NetMask  | DHCP  | Default Gateway  | Type  | Domain      |
| :-----------------| :------: | :---: | :--------------: | :-----|-----------: |
| `210.200.100.1`   | 32       |       | `210.200.100.1`  | WAN   |             |
| `127.20.20.x`     | 24       | x     | `172.20.20.10`   | LAN   |victim.local |
| `127.20.30.x`     | 24       | x     | `172.20.30.10`   | DMZ   |victim.local |
| `127.20.40.x`     | 24       | x     | `172.20.40.10`   | LAN   |victim.local |

**Note**: Similar to an external host, all traffic between the attacker and a particular CyberLab host must be routed through the firewall. The only difference is that the attack route has no additional WAN hops. Therefore, this setup can effectively simulate an attack originating from an external network.

# Hosts
| IP (Static)       | Host Name        | Description                   | OS                     | Role     | 
| :-----------------| ---------------- | :---------------------------- | ---------------------- | -------: | 
| `172.20.10.10`    | firewall         | pfSense Firewall Server       | FreeBSD                | Victim   |     
| `172.20.20.12`    | winserver        | Windows AD Domain Controller  | Win-Server 2012        | Victim   |     
| `172.20.20.9`     | winxp            | Generic Windows Workstation   | Windows XP Pro         | Victim   |     
| `172.20.30.40`    | debserver        | Server (Web/Mail/FTP/SMB...)  | Debian Linux           | Victim   |     
| `172.20.30.30`    | meta             | This host is mega-exploitable | Ubuntu-Linux           | Victim   | 
| `172.20.30.80`    | owasp            | This host has broken webapps  | Ubuntu-Linux           | Victim   |     
| `172.20.40.40`    | kali             | This host has 'the' tools     | Debian-Linux           | Attacker |     
|                   |                  |                               |                        |          | 

**Note** All host-IP's are configured as static IPv4 addresses (although DHCP is offered within each subnet).

**Note**: prepend `victim.local` to the host name to get the host's fully qualified domain name, e.g. `firewall.victim.local`.

# Users
| Name              | Login Name             | Admin  | Default Password  | Role      |
| :---------------- | :-------------------   | :----: | ----------------  | --------: |
| Administrator     | `VICTIM\Administrator` | x      | TryHackMe!        | Victim    |
| alice             | `VICTIM\alice`         | x      | TryHackMe!        | Victim    |
| bob               | `VICTIM\bob`           |        | TryHackMe!        | Victim    |
| trent             | `VICTIM\trent`         |        | TryHackMe!        | Victim    |
|                   |                        |        |                   |           |

**Note**: prepend `VICTIM\` to the user name to get the windows netbios login name or `victim.local` to get the fully-qualified domain name (FQDN), e.g. `alice.victim.local`.

# Firewall

Host `firewall.victim.local` has 4 network adapter cards and is running a pfSense software firewall.

### Network Adapter Configuration
| IP                | Link  | Adapter  | Subnet     | VirtualBox Link Type |
| :---------------- | :---- | :------  | :--------- | -------------------: |
| 210.200.10.1      | 1     | em0      | Victim WAN | NAT-Network          |
| 172.20.20.10      | 2     | em1      | Victim LAN | NAT-Network          |
| 172.20.30.10      | 3     | em2      | Victim DMZ | NAT-Network          |
| 172.20.40.10      | 4     | em3      | Attacker   | NAT-Network          |
|                   |       |          |            |                      |

**Note**: Each network adapter acts as the local gateway for the attached subnet. For example, default gateway of the `Victim-LAN` subnet has IP address `172.20.20.10`.

