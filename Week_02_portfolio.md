#  Week 02 Tutorial – Setting IP & Ping Testing  

##  Task 1: Setting Static IP Addresses  

##  Aim  
To configure static IP addresses on Linux hosts using three different methods:
- GNS3 Configure GUI  
- `/etc/network/interfaces` file  
- `ip` command  

##  Network Topology  

- 4 × Linux Hosts  
- 1 × Ethernet Switch  
- Network: `10.10.2.0/24`  
   **Network Diagram**  
![Network Topology](Setting-IP-<studentid>-network.png)
---
##  IP Address Allocation  

| Host | Method Used | IP Address |
|------|------------|------------|
| Host 1 | GNS3 Configure | 10.10.2.1 |
| Host 2 | GNS3 Configure | 10.10.2.2 |
| Host 3 | /etc/network/interfaces | 10.10.2.3 |
| Host 4 | ip command | 10.10.2.4 |

---

##  Configuration Steps  

###  Host 1 & Host 2 (GNS3 Configure)
- Used GNS3 GUI to assign static IP addresses before starting nodes.

 **Host 1 IP Configuration**  
![Host 1](Setting-IP-<studentid>-host1.png)

 **Host 2 IP Configuration**  
![Host 2](Setting-IP-<studentid>-host2.png)

---

### Host 3 (/etc/network/interfaces)

Edited network configuration file:

```bash
sudo nano /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 10.1.1.3
    netmask 255.255.255.0

### Host 3 Configuration Output
![Host 3](Setting-IP-<studentid>-host2.png)

### Host D (ip command)

Assigned IP using:

sudo ip address add 10.1.1.4/24 dev eth0

 ### Host 4 Configuration Output

### Verification

Used command on all hosts:

ip address show

All hosts successfully configured with correct IP addresses.

![Host 3](Setting-IP-<studentid>-host2.png)

Basic Ping Test

Command used:

ping 10.10.2.2

Sent multiple packets from Host A to Host B

Stopped after 5 responses using Ctrl + C

![Host 3](Setting-IP-<studentid>-host2.png)

 #### Ping Basic Output

![Host 3](Setting-IP-<studentid>-host2.png)

#### Result:

Successful replies received

0% packet loss

RTT observed

Ping to Non-Existent Host

Command:

ping 10.1.1.99

### Ping Error Output

![Host 3](Setting-IP-<studentid>-host2.png)

 Result:

No replies received

100% packet loss

Destination unreachable

Ping with Options

Command:

ping -c 5 -i 2 -s 100 10.1.1.2

 ### Ping with Options Output

![Host 3](Setting-IP-<studentid>-host2.png)

#### Explanation:

-c 5 → send 5 packets

-i 2 → 2-second interval

-s 100 → packet size = 100 bytes
