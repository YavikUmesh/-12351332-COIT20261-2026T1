# Week 06 Tutorial - ARP and Default Gateways

**Student Name:** Yavik Umesh  
**Student ID:** [Your Student ID]  
**Course:** [Course Code/Name]  
**Week:** 06  
**Date:** April 2026

---

## Task 1: Resolving IP Addresses to Hardware Addresses (ARP Basics)

### Project Used
- **Project Name:** `Setting-IP-<YourStudentID>` (or `ARP-Basics-<YourStudentID>`)

### Network Topology
- 4 × Linux Hosts (Host A, Host B, Host C, Host D)
- 1 × Ethernet Switch
- All hosts connected to the same switch (same LAN/subnet)

### IP Addressing Example
| Host   | IP Address       | Interface |
|--------|------------------|---------|
| Host A | 192.168.10.10/24 | eth0    |
| Host B | 192.168.10.20/24 | eth0    |
| Host C | 192.168.10.30/24 | eth0    |
| Host D | 192.168.10.40/24 | eth0    |

### Activities & Observations

1. **Initial ARP Table on Host A**
   ```bash
   ip neigh show ```
   
- ARP table was mostly empty or contained only the switch/router entry (if any).

2. **Ping from Host A to Host B**
Command: ping -c 4 192.168.10.20
Host A sent an ARP request (broadcast) to resolve Host B’s MAC address.

3. **ARP Table on Host A after pinging Host B**
New entry appeared for Host B’s IP → MAC address mapping.
State: REACHABLE

4. **Ping from Host C to Host A**
Command (on Host C): ping -c 4 192.168.10.10
Host C performed ARP resolution for Host A.

5. **ARP Table on Host A after Host C pinged it**
New entry for Host C’s IP → MAC address was added to Host A’s ARP table.
This shows that ARP entries are populated bidirectionally when communication occurs.


### Key Learnings

ARP (Address Resolution Protocol) dynamically maps IP addresses to MAC (hardware) addresses.
Entries are added automatically when a device needs to communicate with another IP on the same LAN.
ARP table entries have a timeout and can be removed if not used for some time.
The ip neigh show command is used to view the neighbor (ARP) table.

## Outputs - Task 1

Screenshots:
ARP-Basics-<YourStudentID>-HostA-Table1.png → ARP table before any pings
ARP-Basics-<YourStudentID>-HostA-Table2.png → ARP table after pinging Host B
ARP-Basics-<YourStudentID>-HostA-Table3.png → ARP table after Host C pings Host A
ARP-Basics-<YourStudentID>-HostA-Table4.png (optional) → Additional combinations

# Task 2: Default Gateways
## Project Name

Project: Default-Gateway-<YourStudentID>

## Network Topology

- Subnet 1: Two Linux Hosts + Linux Router 1 + Ethernet Switch
- Subnet 2: Two Linux Hosts + Linux Router 2 + Ethernet Switch
- Subnet 3: Direct link between Router 1 and Router 2

Total Devices: 4 Hosts, 2 Switches, 2 Routers → 3 subnets

## IP Addressing Scheme (Example)
### Subnet 1 (192.168.10.0/24)

- Host A: 192.168.10.10/24  (Gateway: 192.168.10.1)
- Host B: 192.168.10.20/24  (Gateway: 192.168.10.1)
- Router 1 (eth0): 192.168.10.1/24

### Subnet 2 (192.168.20.0/24)

- Host C: 192.168.20.30/24  (Gateway: 192.168.20.1)
- Host D: 192.168.20.40/24  (Gateway: 192.168.20.1)
- Router 2 (eth0): 192.168.20.1/24

### Subnet 3 (Link between Routers - 192.168.30.0/24)

- Router 1 (eth1): 192.168.30.1/24
- Router 2 (eth1): 192.168.30.2/24

## Configuration (/etc/network/interfaces)
### On Hosts (Forwarding disabled):
```Bash
auto eth0
iface eth0 inet static
    address 192.168.10.10
    netmask 255.255.255.0
    gateway 192.168.10.1
    up sysctl -w net.ipv4.ip_forward=0
```

### On Routers (Forwarding enabled):
```Bash
# Router 1
auto eth0
iface eth0 inet static
    address 192.168.10.1
    netmask 255.255.255.0
    up sysctl -w net.ipv4.ip_forward=1

auto eth1
iface eth1 inet static
    address 192.168.30.1
    netmask 255.255.255.0
```
(Similar configuration for Router 2)

## Routing Tables Verification

- Used ip route show on all devices.
- Hosts showed a default route (default via <gateway>).
- Routers showed connected routes for directly attached subnets.

## Connectivity Test

- Successful ping from Host A (Subnet 1) to Host C (Subnet 2)
  
``` Bash
ping -c 4 192.168.20.30
```
- Traffic path: Host A → Router 1 → Router 2 → Host C

## Outputs - Task 2

- Exported Project: Default-Gateway-<YourStudentID>.gns3project
- Network Screenshot: Default-Gateway-<YourStudentID>-network.png
- Routing Tables: Default-Gateway-<YourStudentID>-routing-tables.png (or multiple screenshots)
- Successful Ping: Default-Gateway-<YourStudentID>-ping.png

### Summary of Routing Tables:

|Device|Key Routes|
|Host A|default via 192.168.10.1|
|Router 1|"192.168.10.0/24 (connected), 192.168.30.0/24 (connected), 192.168.20.0/24 via 192.168.30.2"|
|Host C|default via 192.168.20.1|

## Files included in repository:

ARP-Basics-<YourStudentID>-HostA-Table*.png
Default-Gateway-<YourStudentID>.gns3project
Default-Gateway-<YourStudentID>-network.png
Default-Gateway-<YourStudentID>-routing-tables.png
Default-Gateway-<YourStudentID>-ping.png

























   
