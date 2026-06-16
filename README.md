# CyberLab
VirtualBox-based hacking lab which can be run on a single computer. The lab has a top-level domain which is organized into LAN, WAN and DMZ network segments. The network segments are routed via a pfSense firewall. Hosts include a Kali-Linux-based villain box, Metasploitable and OWASP-BWA victim boxes and a Windows domain controller.

![Alt text](illustrations/CyberLab%20Overview%20Light.drawio.svg)

# Hosts
| IP                | Host Name         | Description                   | OS                     |
| :-----------------| ----------------  | :---------------------------- | ---------------------- |
| `172.20.10.10`    | firewall          | pfSense Firewall Server       | FreeBSD                |    
| `172.20.20.12`    | winserver         | Windows AD Domain Controller  | Win-Server 2012        |    
| `172.20.20.9`     | winxp             | Windows PC                    | Windows XP Pro         |    
| `172.20.30.30`    | meta              | Metasploitable-2 Victim-Box   | Ubuntu-Linux           |    
| `172.20.30.40`    | debserver         | Web/Mail/SMB-Server           | Debian Linux           |    
| `172.20.30.80`    | owasp             | OWASP-BWA Victim-Box          | Ubuntu-Linux           |    
| `172.20.40.40`    | kali              | Kali-Linux Attack-Box         | Debian-Linux           |    
|                   |                   |                               |                        |

**Note**: prepend `victim.local` to the host name to get the host's fully qualified domain name, e.g. `firewall.victim.local`.

# Subnets
| IP                | NetMask  | DHCP  | Default Gateway  | Simulated Type  | Team      |
| :-----------------| :------: | :---: | :--------------: | :-------------  | --------: |
| `127.20.10.x`     | 24       |       | `172.20.10.10`   | WAN             | Victim    |
| `127.20.20.x`     | 24       | x     | `172.20.20.10`   | LAN             | Victim    |
| `127.20.30.x`     | 24       | x     | `172.20.30.10`   | DMZ             | Victim    |
| `127.20.40.x`     | 24       | x     | `172.20.40.10`   | WAN             | Attacker  |

# Users
| Name              | Login Name             | Admin  | Default Password  |
| :---------------- | :-------------------   | :----: | ----------------: |
| Administrator     | `VICTIM\Administrator` | x      | TryHackMe!        |
| alice             | `VICTIM\alice`         | x      | TryHackMe!        |
| bob               | `VICTIM\bob`           |        | TryHackMe!        |
| trent             | `VICTIM\trent`         |        | TryHackMe!        |
|                   |                        |        |                   |

**Note**: prepend `VICTIM\` to the user name to get the windows netbios login name or `victim.local` to get the fully-qualified domain name, e.g. `alice.victim.local`.

# Setup Instructions

### Download VirtualBox

Download Oracle VirtualBox for your host OS from www.virtualbox.org.

### Import VM-Appliances into VirtualBox

Download the cyber lab VM appliance files. The lab consists of the following 7 VM-appliances (or lab hosts):
```
firewall.ova
kali.ova
winxp.ova
winserver.ova
debserver.ova
meta.ova
owasp.ova
```
Import the VM-appliances into VirtualBox (File -> Import Appliance). After importing, each VM can be started or stopped separately from within VirtualBox. When a VM is started, the OS/Shell of the affiliated host will come up in a new window. Setup is complete. You may now start to BLUE- or RED-team your way through the network.

**Note**: all hosts (virtualbox Guest-VMs) are currently set to use german regional settings by default. This includes keyboard layout.

**Note**: Appliance files are temporarily unavailable due to servicing.






    





