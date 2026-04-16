# Week 05 Tutorial - VLAN Configuration with OpenvSwitch and Linux Router

**Student Name:** Yavik Umesh  
**Student ID:** [Your Student ID]  
**Course:** [Course Code/Name]  
**Week:** 05  
**Date:** April 2026

---

## Task 1: Setup VLANs on Switch (Vlan-Basics)

### Project Name
- **Project:** `Vlan-Basics-<YourStudentID>`

### Network Topology
- **Devices:**
  - 4 × Linux Host
  - 1 × OpenvSwitch
- **Connections:**
  - Host 1 → Switch `eth1`
  - Host 2 → Switch `eth2`
  - Host 3 → Switch `eth3`
  - Host 4 → Switch `eth4`
  - Switch `eth0` left unused

### IP Addressing (Same Subnet - Before VLANs)
All hosts configured with IP addresses in the same subnet, for example:
- Host 1: `192.168.10.10/24`
- Host 2: `192.168.10.20/24`
- Host 3: `192.168.10.30/24`
- Host 4: `192.168.10.40/24`

### VLAN Configuration (After Testing Initial Connectivity)
VLAN IDs based on last three digits of student ID (example used: **678** and **679**):

| Host   | Switch Port | VLAN ID | VLAN Name     |
|--------|-------------|---------|---------------|
| Host 1 | eth1        | 678     | VLAN_678      |
| Host 2 | eth2        | 678     | VLAN_678      |
| Host 3 | eth3        | 679     | VLAN_679      |
| Host 4 | eth4        | 679     | VLAN_679      |

**Commands executed on OpenvSwitch:**

```bash
# Assign access ports to VLANs
ovs-vsctl set port eth1 tag=678
ovs-vsctl set port eth2 tag=678
ovs-vsctl set port eth3 tag=679
ovs-vsctl set port eth4 tag=679
```

## Verification

Connectivity test before VLANs: All hosts could ping each other.
Connectivity test after VLANs: Hosts in the same VLAN can ping each other. Hosts in different VLANs cannot ping each other.
ARP tables checked using arp -a on each host.

### Outputs - Task 1

Exported Project: Vlan-Basics-<YourStudentID>.gns3project
Network Screenshot: Vlan-Basics-<YourStudentID>-network.png
Switch Ports Screenshot: Vlan-Basics-<YourStudentID>-ports.png

### Screenshot Notes:

Vlan-Basics-<YourStudentID>-ports.png shows output of ovs-vsctl show with correct tags on eth1–eth4.


# Task 2: Setup VLANs on a Router (Vlan-Router)
## Project Name

Project: Vlan-Router-<YourStudentID>

## Network Topology

4 × Linux Host
1 × OpenvSwitch
1 × Linux Router (connected to Switch eth0)

IP Addressing (Different Subnets)

| Host   | VLAN | Subnet | IP Address    || Gateway   |
|--------|-------------|---------|---------------||---------------|
| Host 1 | 678        | 192.168.78.0/24    | 192.168.78.10      |192.168.78.1|
| Host 2 | 678        | 192.168.78.0/24   | 192.168.78.20      |192.168.78.1|
| Host 3 | 679       | 192.168.79.0/24 | 192.168.79.30     |192.168.79.1|
| Host 4 |679        | 192.168.79.0/24    | 192.168.79.40    |192.168.79.1|


### Switch Configuration
Access Ports (same as Task 1):
```Bash
ovs-vsctl set port eth1 tag=678
ovs-vsctl set port eth2 tag=678
ovs-vsctl set port eth3 tag=679
ovs-vsctl set port eth4 tag=679
```
### Trunk Port (eth0 - to Router):
```Bash
# Allow all VLANs on trunk port
ovs-vsctl set port eth0 trunks=[]
```
### Linux Router Configuration (Router-on-a-Stick)
Commands executed on the Linux Router:
```Bash
# Create VLAN sub-interfaces
ip link add link eth0 name eth0.678 type vlan id 678
ip link add link eth0 name eth0.679 type vlan id 679

# Bring sub-interfaces up
ip link set eth0.678 up
ip link set eth0.679 up

# Assign IP addresses to sub-interfaces
ip address add 192.168.78.1/24 dev eth0.678
ip address add 192.168.79.1/24 dev eth0.679

# Enable IP forwarding
sysctl -w net.ipv4.ip_forward=1
```

## Verification

All hosts can now ping each other (inter-VLAN routing working via the Linux Router).
Hosts in VLAN 678 can reach hosts in VLAN 679 and vice versa.
ping tests performed between all combinations.
ARP tables verified on hosts and router.

## Outputs - Task 2

Exported Project: Vlan-Router-<YourStudentID>.gns3project
Network Screenshot: Vlan-Router-<YourStudentID>-network.png
Switch Ports Screenshot: Vlan-Router-<YourStudentID>-ports.png

### Screenshot Notes:

Vlan-Router-<YourStudentID>-ports.png clearly shows:
eth1 and eth2 tagged with VLAN 678
eth3 and eth4 tagged with VLAN 679
eth0 configured as trunk (trunks=[])


## Files included in this repository:

Vlan-Basics-<YourStudentID>.gns3project
Vlan-Router-<YourStudentID>.gns3project
Vlan-Basics-<YourStudentID>-network.png
Vlan-Basics-<YourStudentID>-ports.png
Vlan-Router-<YourStudentID>-network.png
Vlan-Router-<YourStudentID>-ports.png
























