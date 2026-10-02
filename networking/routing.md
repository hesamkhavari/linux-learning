# Linux Routing
## Routing Table
Traditional command:
```bash
route
```
Numeric output:
```bash
route -n
```
Search for the default route:
```bash
route -n | grep UG
```
The `UG` flags commonly indicate a route that is Up and uses a Gateway.

## Modern Alternative
On modern Linux systems, `iproute2` is preferred:
```bash
ip route
```
Default route:
```bash
ip route show default
```

## Checking Router Availability
A gateway address obtained from the routing table can be tested with:
```bash
ping -c 5 <gateway-ip>
```
Example:
```bash
ping -c 5 192.168.1.1
