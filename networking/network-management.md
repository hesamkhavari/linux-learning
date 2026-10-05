# Linux Network Management and Troubleshooting

## Network Interfaces
### Legacy: ifconfig
Older Linux systems may use:
```bash
ifconfig
ifconfig -a
ifconfig eth1
```
Bring an interface up/down:
```bash
ifconfig eth1 up
ifconfig eth1 down
```
> `ifconfig` belongs to the older net-tools suite. Modern Linux systems generally use `ip` from iproute2.
---
### ip
Display interfaces and addresses:
```bash
ip addr
```
Short form:
```bash
ip a
```
Display link status:
```bash
ip link
```
Bring an interface down:
```bash
sudo ip link set dev eth1 down
```
Bring it up:
```bash
sudo ip link set dev eth1 up
```
Display routing table:
```bash
ip route
```
Short form:
```bash
ip r
```

## Configure an IP Address
Modern syntax:
```bash
sudo ip addr add 172.16.1.50/16 dev eth1
```
Remove it:
```bash
sudo ip addr del 172.16.1.50/16 dev eth1
```
> `ip` changes are generally runtime configuration. Persistent network configuration should normally be managed by the system's network manager.

## Routing
Legacy:
```bash
route
route -n
```
Modern:
```bash
ip route
```
Show default route:
```bash
ip route show default
```
Add a route:
```bash
sudo ip route add 10.0.0.0/8 via 10.0.0.1 dev eth2
```
Delete it:
```bash
sudo ip route del 10.0.0.0/8
```

## Connectivity Testing
Test the local TCP/IP stack:
```bash
ping 127.0.0.1
ping localhost
```
Test an IP address:
```bash
ping 8.8.8.8
```
Test DNS + connectivity:
```bash
ping google.com
```
Force IPv4:
```bash
ping -4 8.8.8.8
```
A useful troubleshooting sequence is:
```Plain text
localhost
 ↓
local gateway
 ↓
remote IP
 ↓
domain name
```
### This helps distinguish local networking, routing, Internet connectivity, and DNS problems.

## Traceroute
Show the path toward a destination:
```bash
traceroute google.com
```
Depending on the distribution, `traceroute` may need to be installed separately.

## Socket and Connection Inspection
### ss
Modern Linux systems generally use `ss` instead of `netstat`.
Show TCP sockets:
```bash
ss -t
```
Show UDP sockets:
```bash
ss -u
```
Listening TCP/UDP sockets:
```bash
ss -tuln
```
Include process information:
```bash
sudo ss -tulpn
```
This is one of the most useful commands for checking which services are listening on a server.

## Legacy: netstat
```bash
netstat 
netstat -at 
netstat -au 
netstat -r
```
Learn it because it appears in older documentation, but prefer `ss` on modern systems.

## DHCP
Older environments may use:
```bash
dhclient
```
On modern distributions, DHCP is often managed by NetworkManager, systemd-networkd, or another network management service.
Do not manually run `dhclient` without first understanding which network manager owns the interface.

## Neighbor / ARP Information
Legacy:
```bash
arp -a
```
Modern:
```bash
ip neigh
```
`ip neigh` displays the kernel's neighbor table.

## DNS Troubleshooting
### host
```bash
host google.com
```
### dig
```bash
dig google.com
```
Query a specific record:
```bash
dig A google.com
dig AAAA google.com
dig MX google.com
```
### nslookup
```bash
nslookup google.com
```
`dig` and `host` are especially useful for DNS troubleshooting.

## Hostname
Display hostname:
```bash
hostname
```
Modern persistent hostname management:
```bash
hostnamectl
```
Set hostname:
```bash
sudo hostnamectl set-hostname computer2
```
> Do not edit `/usr/bin/hostname` manually. It is an executable program, not a hostname configuration file.

## Local Name Resolution
View the local hosts file:
```bash
cat /etc/hosts
```
The `/etc/hosts` file provides local hostname-to-IP address mappings.

## DNS Resolver Configuration
Depending on the distribution and network stack:
```bash
cat /etc/resolv.conf
```
> On many modern systems `/etc/resolv.conf` may be generated or managed by another service. Editing it directly may not persist.

## NetworkManager
### Modern desktop/server Linux systems may use NetworkManager.
Check its status:
```bsah
systemctl status NetworkManager
```

## nmcli
`nmcli` is the command-line interface for NetworkManager.
Show general status:
```bash
nmcli general status
```
Show devices:
```bash
nmcli device status
```
Show connections:
```
nmcli connection show
```
Show detailed device information:
```bash
nmcli device show
```
Bring a connection up:
```bash
sudo nmcli connection up "<connection-name>"
```
Bring it down:
```bash
sudo nmcli connection down "<connection-name>"
```
NetworkManager documents `nmcli` as its command-line tool for displaying and controlling network status and connections.

## nmtui
`nmtui` provides a terminal-based interface for NetworkManager:
```bash
nmtui
```
It can be used to edit and activate network connections and configure the hostname.

## Wireless
Legacy tools:
```bash
iwconfig
iwlist
```
Modern wireless management commonly uses:
```bash
nmcli device wifi list
```
With NetworkManager:
```bash
nmcli device status
```
> `iwconfig` and `iwlist` are legacy wireless tools. For modern administration, prefer NetworkManager's `nmcli`/`nmtui` where NetworkManager is in use.

## Practical Network Troubleshooting
A basic troubleshooting workflow:
```bash
ip addr
ip route
ip route show default
ping -c 3 <gateway>
ping -c 3 8.8.8.8
ping -c 3 google.com
ss -tulpn
dig google.com
```
Interpretation:
```Plain text
ip addr
 ↓
Is the interface configured?

ip route
 ↓
Is there a route/default gateway?

ping gateway
 ↓
Can we reach the local network?

ping remote IP
 ↓
Is IP connectivity working?

ping domain
 ↓
Does DNS work?

ss -tulpn
 ↓
Which local services are listening?

dig
 ↓
Is DNS resolving correctly?
```
This is more useful in real administration than memorizing individual networking commands.
