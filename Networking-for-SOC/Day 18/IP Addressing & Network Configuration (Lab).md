# Day 2 – IP Addressing & Network Configuration Lab

## Objective

Learn how to identify IP configuration, troubleshoot network connectivity, understand DNS, inspect ARP tables, trace network routes, and analyze active network connections.

---

# 1. Find Your IP Address

### Command

```cmd
ipconfig
```

Look for:

```text
IPv4 Address
```

Example:

```text
IPv4 Address. . . . . . : 192.168.1.10
```

### Explanation

* The IPv4 address identifies your computer on the network.
* `192.168.1.10` is a private IP address.
* Private IP addresses are commonly used inside local networks.

---

# 2. Find Your MAC Address

### Command

```cmd
ipconfig /all
```

Look for:

```text
Physical Address
```

Example:

```text
Physical Address. . . . . : AA-BB-CC-11-22-33
```

### Explanation

The MAC address is the hardware address associated with a network interface.

### SOC Perspective

MAC addresses can help identify devices within a local network and can be useful during network investigations.

---

# 3. View DNS Servers

### Command

```cmd
ipconfig /all
```

Look for:

```text
DNS Servers
```

A DNS server can be:

### Router / Local DNS

```text
192.168.1.1
```

The router may forward DNS queries to another DNS server.

### Internal DNS / Active Directory DNS

```text
10.x.x.x
172.16.x.x
192.168.x.x
```

In a corporate environment, the DNS server may be an internal DNS server or an Active Directory Domain Controller.

### Public DNS

Examples:

```text
8.8.8.8
8.8.4.4
1.1.1.1
```

> **Note:** `google.com` is a domain name, not a DNS server. Google's public DNS servers include `8.8.8.8` and `8.8.4.4`.

---

# 4. Find the Default Gateway

### Command

```cmd
ipconfig
```

Look for:

```text
Default Gateway
```

Example:

```text
Default Gateway . . . . : 192.168.1.1
```

### Explanation

The default gateway is usually the router that forwards traffic from your local network to other networks.

Example:

```text
Your PC
192.168.1.10
     |
     ↓
Default Gateway
192.168.1.1
     |
     ↓
Internet
```

---

# 5. Release IP Address

### Command

```cmd
ipconfig /release
```

### Explanation

This releases the current DHCP-assigned IP address.

Your system may temporarily lose network connectivity.

---

# 6. Renew IP Address

### Command

```cmd
ipconfig /renew
```

### Explanation

This requests a new IP address from the DHCP server.

Basic DHCP flow:

```text
Client
   ↓
DHCP Server
   ↓
IP Address Assigned
```

---

# 7. Test Network Connectivity

## Ping Default Gateway

First find your default gateway:

```cmd
ipconfig
```

Then run:

```cmd
ping 192.168.1.1
```

If you receive:

```text
Reply from 192.168.1.1
```

Your computer can communicate with the local gateway.

---

## Ping an Internet IP

```cmd
ping 8.8.8.8
```

This tests connectivity to Google's public DNS IP address.

If you receive:

```text
Reply from 8.8.8.8
```

You likely have Internet connectivity.

---

## Ping a Domain

```cmd
ping google.com
```

This involves:

1. DNS resolution
2. Network connectivity

Example:

```text
Pinging google.com [IP_Address]
```

> **Note:** Some servers block ICMP ping. Therefore, a failed ping does not always mean that the destination is unreachable.

---

# 8. View the ARP Table

### Command

```cmd
arp -a
```

ARP maps:

```text
IP Address ↔ MAC Address
```

Example:

```text
Internet Address      Physical Address
192.168.1.1           AA-BB-CC-11-22-33
```

This shows the MAC address associated with a local IP address.

### SOC Perspective

ARP is important for understanding attacks such as:

* ARP Spoofing
* ARP Poisoning
* Man-in-the-Middle attacks

---

# 9. Trace the Packet Route

### Windows Command

```cmd
tracert google.com
```

### Explanation

`tracert` shows the network path or hops between your computer and the destination.

Example:

```text
Your PC
   ↓
Home Router
   ↓
ISP Router
   ↓
Internet Router
   ↓
Destination
```

Each hop usually represents a router or Layer 3 device.

### SOC Perspective

`tracert` can help identify:

* Network delays
* Routing problems
* Where traffic stops
* Network path issues

---

# 10. View the Routing Table

### Command

```cmd
route print
```

### Explanation

The routing table tells your computer where to send network traffic.

It contains information about:

* Destination networks
* Network interfaces
* Gateways
* Routes

Basic flow:

```text
Destination Network
        ↓
Routing Table
        ↓
Next Hop / Gateway
        ↓
Destination
```

---

# 11. DNS Troubleshooting with NSLookup

### Command

```cmd
nslookup google.com
```

### Explanation

`nslookup` is used to query DNS servers and resolve domain names into IP addresses.

You can also specify a DNS server:

```cmd
nslookup google.com 8.8.8.8
```

This asks Google's public DNS server to resolve the domain.

### SOC Perspective

DNS investigation can help analyze:

* Suspicious domains
* Malicious domains
* DNS configuration problems
* DNS resolution issues

---

# 12. View Network Connections

### Command

```cmd
netstat -ano
```

This displays:

* Active network connections
* Listening ports
* Local IP and port
* Remote IP and port
* Process ID (PID)

Example:

```text
TCP    192.168.1.10:50000    8.8.8.8:443    ESTABLISHED    1234
```

The last number is the PID:

```text
1234
```

You can identify the process using:

```cmd
tasklist /FI "PID eq 1234"
```

### SOC Perspective

This is useful for investigating suspicious network connections.

Example:

```text
Unknown Process
      ↓
ESTABLISHED Connection
      ↓
External IP
      ↓
Investigate Process
      ↓
Check Threat Intelligence
```

---

# Windows Command Summary

| Purpose                        | Command               |
| ------------------------------ | --------------------- |
| Find IP address                | `ipconfig`            |
| Detailed network configuration | `ipconfig /all`       |
| Release IP                     | `ipconfig /release`   |
| Renew IP                       | `ipconfig /renew`     |
| Test connectivity              | `ping <IP>`           |
| View ARP table                 | `arp -a`              |
| Trace network route            | `tracert <domain/IP>` |
| View routing table             | `route print`         |
| DNS lookup                     | `nslookup <domain>`   |
| View network connections       | `netstat -ano`        |

---

# Linux Command Equivalents

| Purpose               | Windows         | Linux               |
| --------------------- | --------------- | ------------------- |
| IP address            | `ipconfig`      | `ip addr`           |
| Network configuration | `ipconfig /all` | `ip addr` / `nmcli` |
| Test connectivity     | `ping`          | `ping`              |
| Trace route           | `tracert`       | `traceroute`        |
| ARP information       | `arp -a`        | `ip neigh`          |
| Routing table         | `route print`   | `ip route`          |
| DNS lookup            | `nslookup`      | `nslookup` / `dig`  |
| Network connections   | `netstat -ano`  | `ss -tuln`          |

> **Note:** On modern Linux systems, `ip neigh` is generally preferred over the older `arp` command, and `ss` is preferred over `netstat`.

---

# Day 2 – Practical Lab

Run the following commands on your Windows system:

```cmd
ipconfig /all
```

Identify:

* IPv4 Address
* Subnet Mask
* Default Gateway
* DNS Server
* MAC Address

Then run:

```cmd
ping <your-default-gateway>
ping 8.8.8.8
ping google.com
arp -a
tracert google.com
route print
nslookup google.com
netstat -ano
```

---

# SOC Troubleshooting Workflow

Use the following workflow when troubleshooting a network connectivity issue:

```text
1. Do I have an IP address?
        ↓
2. Can I reach my Default Gateway?
        ↓
3. Can I reach an Internet IP?
        ↓
4. Can DNS resolve the domain?
        ↓
5. What path does the traffic take?
        ↓
6. What devices are present in the ARP table?
        ↓
7. What network connections are currently active?
```

---

# Key Takeaways

* `ipconfig` → Check IP configuration
* `ipconfig /all` → Get detailed network information
* `ping` → Test connectivity
* `arp -a` → View IP-to-MAC mappings
* `tracert` → Trace the network path
* `route print` → View routing decisions
* `nslookup` → Troubleshoot DNS
* `netstat -ano` → Investigate network connections and processes

These commands form the foundation of **network troubleshooting and SOC investigations**.
