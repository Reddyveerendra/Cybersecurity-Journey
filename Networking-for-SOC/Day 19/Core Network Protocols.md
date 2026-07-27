Here is a **clean, GitHub-ready `README.md`** version for your Day 3 notes. I kept it concise and professional so it looks good in your learning repository.

# Day 3 – Core Network Protocols

Today I learned the core network protocols that are important for understanding network communication and SOC investigations.

## 1. TCP – Transmission Control Protocol

TCP is a **connection-oriented transport layer protocol** that provides reliable and ordered delivery of data.

### Three-Way Handshake

TCP establishes a connection using three steps:

1. **SYN** – Client requests a connection.
2. **SYN-ACK** – Server acknowledges the request and responds.
3. **ACK** – Client acknowledges the response.

```text
Client                    Server
  |                         |
  | -------- SYN ---------> |
  | <----- SYN + ACK ------ |
  | -------- ACK ---------> |
  |                         |
  |    Connection Ready     |
```

### Reliability

TCP provides reliable communication using:

* Sequence numbers
* Acknowledgments
* Retransmission of lost data
* Ordered delivery

> TCP reliability does not mean there is no downtime. It means TCP can reliably deliver data when the connection is available.

### Flow Control

Flow control prevents a fast sender from overwhelming a slow receiver.

```text
Fast Sender
     |
     | Data
     v
Slow Receiver
```

The receiver uses the TCP receive window to communicate how much data it can currently handle.

---

## 2. UDP – User Datagram Protocol

UDP is a **connectionless transport layer protocol**.

It does not provide TCP-style guarantees for:

* Delivery
* Ordering
* Retransmission

### Characteristics

* Connectionless
* Low overhead
* Faster for many real-time applications
* No TCP-style handshake
* No guaranteed delivery

### Use Cases

* DNS
* DHCP
* Online gaming
* VoIP
* Video streaming
* Real-time applications
* Broadcast and multicast traffic

---

## 3. DNS – Domain Name System

DNS translates domain names into IP addresses and provides other information about domains.

Example:

```text
www.example.com
       |
       v
      DNS
       |
       v
192.0.2.10
```

### Common DNS Records

| Record | Purpose                                                     |
| ------ | ----------------------------------------------------------- |
| A      | Maps a domain to an IPv4 address                            |
| AAAA   | Maps a domain to an IPv6 address                            |
| MX     | Specifies mail servers for a domain                         |
| TXT    | Stores text information such as SPF and domain verification |
| CNAME  | Creates an alias for another domain name                    |
| NS     | Identifies authoritative DNS servers                        |

### Example

```text
A Record
example.com → 192.0.2.10

AAAA Record
example.com → 2001:db8::10

CNAME
www.example.com → example.com

MX
example.com → mail.example.com
```

---

## 4. DHCP – Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to devices.

DHCP can provide:

* IP Address
* Subnet Mask
* Default Gateway
* DNS Server

### DORA Process

DORA stands for:

**Discover → Offer → Request → Acknowledge**

```text
Client                    DHCP Server
  |                           |
  | ---- DHCPDISCOVER ------> |
  | <----- DHCPOFFER -------- |
  | ---- DHCPREQUEST -------> |
  | <------ DHCPACK --------- |
  |                           |
  |  Network Configuration    |
```

---

## 5. ARP – Address Resolution Protocol

ARP is used to resolve an **IPv4 address to a MAC address** on a local network.

Example:

```text
IP Address
192.168.1.1
     |
     v
    ARP
     |
     v
MAC Address
AA:BB:CC:DD:EE:FF
```

### ARP Commands

To view the ARP cache in Windows:

```cmd
arp -a
```

---

## 6. ICMP – Internet Control Message Protocol

ICMP is used for:

* Network diagnostics
* Error reporting
* Reachability testing
* Network control messages

### Ping

The `ping` command commonly uses:

* ICMP Echo Request
* ICMP Echo Reply

```cmd
ping 8.8.8.8
```

### Tracert

Windows `tracert` can use ICMP responses to identify network hops.

```cmd
tracert 8.8.8.8
```

> A failed ping does not always mean the destination is down. ICMP traffic may be blocked by a firewall.

---

## 7. NAT – Network Address Translation

NAT translates network addresses, commonly allowing devices using private IP addresses to communicate through a public IP address.

Example:

```text
Laptop
192.168.1.10
     |
     v
Router
     |
    NAT
     |
     v
Public IP
     |
     v
Internet
```

### Private IPv4 Ranges

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

NAT allows multiple devices on a private network to share a public IP address.

---

## 8. VLAN – Virtual Local Area Network

A VLAN logically separates a network into different **Layer 2 broadcast domains**.

Example:

```text
Physical Network
       |
       +---- VLAN 10 → Employees
       |
       +---- VLAN 20 → Servers
       |
       +---- VLAN 30 → Guest Wi-Fi
```

### Benefits

* Network segmentation
* Security isolation
* Better network organization
* Reduced broadcast domains

> VLANs help with network segmentation, but additional security controls such as firewalls and access control may be required.

---

# SOC Perspective

## DNS Tunneling

Attackers can abuse DNS to create covert communication channels.

```text
Compromised Host
      |
      | Encoded Data
      v
DNS Queries
      |
      v
Attacker-Controlled DNS Server
```

### Possible Indicators

* Large number of DNS requests
* Long or unusual subdomains
* Random-looking domain names
* High-entropy DNS queries
* Frequent DNS requests
* Unusual TXT queries

---

## ARP Spoofing

An attacker sends forged ARP information to associate their MAC address with another device's IP address.

Example:

```text
Normal:

Victim → Router → Internet


ARP Spoofing:

Victim → Attacker → Router → Internet
```

This can allow an attacker to position themselves between the victim and the gateway.

ARP spoofing is commonly associated with **Man-in-the-Middle (MITM)** attacks.

---

## ICMP Tunneling

Attackers can abuse ICMP packets to carry hidden or encoded data.

```text
Compromised Host
      |
      | Hidden Data
      v
ICMP Packets
      |
      v
Attacker-Controlled System
```

### Possible Indicators

* Unusually high ICMP traffic
* Large ICMP payloads
* Periodic ICMP communication
* Continuous ICMP communication
* Unknown external destinations

---

## Rogue DHCP

A Rogue DHCP server is an unauthorized DHCP server on a network.

```text
Client
   |
   v
Rogue DHCP Server
   |
   v
Incorrect or Malicious Configuration
```

An attacker may provide:

* Malicious DNS Server
* Incorrect Default Gateway
* Incorrect IP Configuration

### SOC Investigation

A SOC analyst may investigate:

* Unexpected DHCP servers
* DHCP logs
* MAC addresses
* Switch ports
* Affected devices

---

# Quick Revision

| Protocol | Main Purpose                               |
| -------- | ------------------------------------------ |
| TCP      | Reliable and ordered data delivery         |
| UDP      | Connectionless, low-overhead communication |
| DNS      | Domain name and DNS information resolution |
| DHCP     | Automatic network configuration            |
| ARP      | IPv4 address to MAC address resolution     |
| ICMP     | Diagnostics and error reporting            |
| NAT      | Network address translation                |
| VLAN     | Logical network segmentation               |

---

# Key Takeaways

* **TCP** provides reliable and ordered communication.
* **UDP** provides connectionless communication with low overhead.
* **DNS** translates domain names and stores domain information.
* **DHCP** automatically provides network configuration using the DORA process.
* **ARP** maps IPv4 addresses to MAC addresses on local networks.
* **ICMP** is commonly used for network diagnostics and error reporting.
* **NAT** translates private and public network addresses.
* **VLANs** provide logical network segmentation.
* **DNS tunneling** can be used for covert communication.
* **ARP spoofing** can enable Man-in-the-Middle attacks.
* **ICMP tunneling** can hide data inside ICMP traffic.
* **Rogue DHCP** can provide malicious network configuration.

---
