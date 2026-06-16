# CyberLab
VirtualBox-based hacking lab which can be run on a single computer. The lab has a top-level domain which is organised into LAN, WAN and DMZ network segments. The network segments are routed via a pfSense firewall. Hosts include a Kali-Linux-based villain box, Metasploitable2 and OWASP-BWA victim boxes and a Windows domain controller.

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

**Note**: prepend `victim.local` to the host name to get the fully qualified domain name, e.g. `firewall.victim.local`.

**Note**: all hosts (virtualbox Guest-VMs) use german regional settings, such as keyboard layout, by default.

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




    





