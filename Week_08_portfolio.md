# Week 08 Tutorial: HTTP Clients

**Student ID:** [Your Student ID]  
**Week:** 08

---

## Task 1: HTTP Client with GUI

### Aim
Use a GUI web browser (Firefox) as an HTTP client to access a web server and capture HTTP traffic.

### Network Topology
- **Subnet A**: Firefox Host (HTTP Client) + Switch
- **Subnet B**: Intermediate network connecting Router 1 and Router 2 (Switch)
- **Subnet C**: Linux Server (HTTP Server) + Switch
- Routers: Router 1 (between A and B), Router 2 (between B and C)

![Network Topology](HTTPClient-GUI-<studentid>-network.png)

### Configuration
All nodes used static IP configuration via `/etc/network/interfaces`:

**Example IP Scheme:**
- Firefox Host (Subnet A): `192.168.1.10/24`, Gateway: `192.168.1.1`
- Router 1: Appropriate interfaces on Subnet A and B
- Router 2: Appropriate interfaces on Subnet B and C
- Linux Server (Subnet C): `192.168.3.10/24`, Gateway: `192.168.3.1`

**Connectivity Test:**
```bash
ping <Linux-Server-IP>
```

### Packet Capture

- Capture started on link in Subnet B (between Router 1 and Router 2).
- Accessed the web server using Firefox via noVNC on the Firefox Host.
- Visited http://<Linux-Server-IP> (or http://<Linux-Server-IP>/index.html).
- Capture stopped after page loaded.

[Screenshot] Packet Capture File: HTTPClient-GUI-<studentid>-subnetB.pcap

#### Outputs:

- Project: HTTPClient-GUI-<studentid>.gns3project
- Topology Screenshot: HTTPClient-GUI-<studentid>-network.png
- Packet Capture: HTTPClient-GUI-<studentid>-subnetB.pcap


## Task 2: HTTP Client with Command Line Interface

### Aim

Use command-line HTTP clients (wget and curl) to access a web server.

### Network Topology

Same topology as Task 1, with the Firefox Host replaced by a Linux Host (same IP address).

<img src="HTTPClient-CLI-%3Cstudentid%3E-network.png" alt="Network Topology">
Activities

#### 1. wget Command:
```
wget http://<Linux-Server-IP>/
# or
wget http://<Linux-Server-IP>/index.html
```
#### 2. curl Command:
```
curl -o index.html http://<Linux-Server-IP>/
# or simply view output:
curl http://<Linux-Server-IP>/
```

### Packet Capture

- Capture performed on Subnet B link while using wget.
- Second access performed with curl (no capture required).

#### Packet Capture File: HTTPClient-CLI-<studentid>-subnetB.pcap

#### Screenshots:

- Screenshot of wget command and output.
- Screenshot of curl command and output.

#### Outputs:

- Project: HTTPClient-CLI-<studentid>.gns3project
- Topology Screenshot: HTTPClient-CLI-<studentid>-network.png
- Packet Capture: HTTPClient-CLI-<studentid>-subnetB.pcap
- Screenshot(s) of wget and curl


### Key Learnings

- GUI HTTP Client: Firefox (via noVNC) – user-friendly but resource-heavy.
- CLI HTTP Clients:
  - wget: Simple downloading of web pages (good for basic retrieval).
  - curl: More powerful and flexible (better for scripting and advanced requests).

- HTTP traffic can be observed in packet captures across routers.
- Command-line tools are preferred for automation and server testing.




