# DHCP and DNS Configuration Tutorial

**Student ID:** [Your Student ID]  
**Week:** 07

---

## Task 1: DHCP Client

### Network Topology
- 3 Linux Host nodes (Host 1, Host 2, Host 3)
- 1 OpenWRT Router
- All connected via an Ethernet switch to `eth0` of the OpenWRT router

![Network Topology](DHCP-Client-<studentid>-network.png)

### Activities Summary

**OpenWRT Router (`eth0`) IP Address:**
```bash
ifconfig eth0
# or
ip addr show eth0
```
(Router acts as DHCP server by default)

## Host 1 - Manual DHCP Client:
```
# Before DHCP
ifconfig eth0

# Run DHCP client
udhcpc

# After DHCP
ifconfig eth0
```
<img src="DHCP-Client-%3Cstudentid%3E-host1.png" alt="Host 1 DHCP Client">

### Host 2 - Automatic DHCP via /etc/network/interfaces

#### Configuration added:
```
auto eth0
iface eth0 inet dhcp
    hostname Host2
```
### Host 3 - Packet Capture of DHCP Process

- Started capture on link from Host 3 to switch.
- Ran udhcpc on Host 3.
- Stopped capture.

#### Packet Capture Analysis (DHCP Messages):

- DHCP Discover (Broadcast from Host 3)
- DHCP Offer (from OpenWRT)
- DHCP Request (from Host 3)
- DHCP ACK (from OpenWRT)

#### Files:

- Project: DHCP-Client-<studentid>.gns3project
- Topology: DHCP-Client-<studentid>-network.png
- Host 1: DHCP-Client-<studentid>-host1.png
- Host 3 capture: DHCP-Client-<studentid>-host3.pcap


## Task 2: DHCP Server Basics

Project: DHCP-Server-Basics-<studentid>

### Original DHCP Configuration (via UCI)
```
uci set dhcp.lan.leasetime='2h'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

### Final DHCP Configuration

<img src="DHCP-Server-Basics-%3Cstudentid%3E-config-final.png" alt="Final DHCP Config">

### Static Lease for Host 3
```
uci add dhcp host
uci set dhcp.@host[-1].name='Host3'
uci set dhcp.@host[-1].mac='[Host3-MAC-Address]'
uci set dhcp.@host[-1].ip='192.168.1.50'
uci set dhcp.@host[-1].leasetime='12h'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

### Leases on DHCP Server
```
cat /etc/dhcp.leases
```
<img src="DHCP-Server-Basics-%3Cstudentid%3E-leases.png" alt="DHCP Leases">

#### Summary of Leases:

- Host 1: Dynamic lease (standard range)
- Host 2: Dynamic lease (2-hour duration)
- Host 3: Static lease (192.168.1.50)


## Task 3: Hosts File and Simple DNS Entries

Project: DNS-Hosts-<studentid>

### 1. Local /etc/hosts on Host 1

Entry added to /etc/hosts on Host 1:
```
192.168.1.[Host2-IP]    host2.example.com
```

### Test:
```
ping host2.example.com
```
### 2. DNS Host Record on OpenWRT Router

Added to OpenWRT DHCP/DNS configuration (/etc/config/dhcp):
```
config dnsmasq
    ...
config host
    option name 'www'
    option ip '192.168.1.[Host3-IP]'
    option domain 'example.com'
```
#### Alternative using UCI:

```
uci add dhcp host
uci set dhcp.@host[-1].name='www'
uci set dhcp.@host[-1].ip='192.168.1.[Host3-static-IP]'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

#### Tests from Host 2 and Host 3:

```
ping host2.example.com      # Resolved via Host 1's /etc/hosts
ping www.example.com        # Resolved via OpenWRT DNS
```







