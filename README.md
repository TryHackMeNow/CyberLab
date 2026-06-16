# CyberLab
VirtualBox-based hacking lab which can be run on a single computer. The lab has a top-level domain which is organized into LAN, WAN and DMZ network segments. The network segments are routed via a pfSense firewall. Hosts include a Kali-Linux-based attacker box, Metasploitable and OWASP-BWA victim boxes and a Windows domain controller.

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

# Hosts
| IP (Static)       | Host Name        | Role      | Description                   | OS                     |
| :-----------------| ---------------- | --------  | :---------------------------- | ---------------------- |
| `172.20.10.10`    | firewall         | Victim    | pfSense Firewall Server       | FreeBSD                |    
| `172.20.20.12`    | winserver        | Victim    | Windows AD Domain Controller  | Win-Server 2012        |    
| `172.20.20.9`     | winxp            | Victim    | Generic Windows Workstation   | Windows XP Pro         |    
| `172.20.30.40`    | debserver        | Victim    | Server (Web/Mail/FTP/SMB...)  | Debian Linux           |    
| `172.20.30.30`    | meta             | Victim    | This host is mega-exploitable | Ubuntu-Linux           |
| `172.20.30.80`    | owasp            | Victim    | This host has broken webapps  | Ubuntu-Linux           |    
| `172.20.40.40`    | kali             | Attacker  | This host has 'the' tools     | Debian-Linux           |    
|                   |                  |           |                               |                        |

**Note** All host-IP's are configured as static IPv4 addresses (although DHCP is offered within each subnet).

**Note**: prepend `victim.local` to the host name to get the host's fully qualified domain name, e.g. `firewall.victim.local`.

# Subnets
| IP                | NetMask  | DHCP  | Default Gateway  | Simulated Type  | Role      |
| :-----------------| :------: | :---: | :--------------: | :-------------  | --------: |
| `210.200.100.1`   | 32       |       | `210.200.100.1`  | WAN             | Victim    |
| `127.20.20.x`     | 24       | x     | `172.20.20.10`   | LAN             | Victim    |
| `127.20.30.x`     | 24       | x     | `172.20.30.10`   | DMZ             | Victim    |
| `127.20.40.x`     | 24       | x     | `172.20.40.10`   | WAN             | Attacker  |

# Users
| Name              | Login Name             | Admin  | Default Password  | Role      |
| :---------------- | :-------------------   | :----: | ----------------: | --------: |
| Administrator     | `VICTIM\Administrator` | x      | TryHackMe!        | Victim    |
| alice             | `VICTIM\alice`         | x      | TryHackMe!        | Victim    |
| bob               | `VICTIM\bob`           |        | TryHackMe!        | Victim    |
| trent             | `VICTIM\trent`         |        | TryHackMe!        | Victim    |
|                   |                        |        |                   |           |

**Note**: prepend `VICTIM\` to the user name to get the windows netbios login name or `victim.local` to get the fully-qualified domain name (FQDN), e.g. `alice.victim.local`.
