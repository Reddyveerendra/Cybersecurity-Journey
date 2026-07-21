# Day 1 – Networking Fundamentals (Learning)

## 1. What is a Network?

A **network** is a connection between multiple computers or devices that allows them to communicate and transfer data with each other.

Example:

```text
Computer A ─── Switch ─── Computer B
```

The devices can communicate and share resources over the network.

---

## 2. LAN, WAN, MAN, Internet, Intranet

### LAN – Local Area Network

A network that covers a **small geographical area**.

Examples:
- Home
- Office
- School computer lab

### WAN – Wide Area Network

A network that covers a **large geographical area**, connecting multiple LANs.

Example:
- A company connecting offices in Bangalore, Hyderabad, and Mumbai.

### MAN – Metropolitan Area Network

A network that covers a **city or metropolitan area**.

Example:
- A city-wide network connecting multiple offices or buildings.

### Internet

The **Internet is a global network of interconnected networks** that allows devices around the world to communicate.

### Intranet

An **intranet is a private network used within an organization**.

Example:

```text
Employee → Company Intranet → Internal HR Portal
```

> **Note:** An intranet is not necessarily a network tunnel. A **VPN** is a technology that can create a secure tunnel to access an organization's private network.

---

## 3. Client vs Server

### Client

A **client** is a device or application that requests a service or resource.

Example:

```text
Browser → Request → Web Server
```

### Server

A **server** is a system that provides services or resources to clients.

Example:

```text
Client → Request → Server
Client ← Response ← Server
```

---

# 4. Network Devices

## Hub

A **hub** is an older networking device that sends incoming data to **all connected ports**.

```text
PC1 → Hub
       ↓
   PC2 + PC3 + PC4
```

Because traffic is broadcast to all ports, a device connected to the network could potentially capture traffic using packet-sniffing techniques.

> **SOC Note:** An attacker cannot automatically decrypt data just because it is broadcast. They may be able to **capture/sniff** the traffic. Whether they can read it depends on whether the traffic is encrypted.

---

## Switch

A **switch** connects devices in a LAN and forwards Ethernet frames using **MAC addresses**.

```text
PC1 ─┐
PC2 ─┼── Switch ── Server
PC3 ─┘
```

A switch uses its MAC address table to forward frames toward the correct destination port.

**SOC Perspective:** Switches are important when understanding **internal network communication and lateral movement**.

---

## Router

A **router** connects **different networks** and forwards packets using **IP addresses**.

Example:

```text
Internal Network
       ↓
     Router
       ↓
      ISP
       ↓
   Internet
```

A router is not simply a device used to connect to an ISP. It can connect many different networks. In a home network, it commonly connects the LAN to the ISP.

---

## Firewall

A **firewall** can be hardware, software, or a virtual appliance that controls network traffic according to security rules or policies.

Example:

```text
Internet
    ↓
Firewall
    ↓
Internal Network
```

Example rule:

```text
Allow TCP 443
Block TCP 23
```

**SOC Perspective:** Firewall logs are useful for investigating:

- Source IP
- Destination IP
- Source Port
- Destination Port
- Protocol
- Allow/Block action

---

## Access Point

An **Access Point (AP)** allows wireless devices to connect to a wired network.

```text
Laptop )))
          \
           Access Point ── Switch ── Network
          /
Phone   )))
```

An AP provides **wireless network access** and usually connects to the LAN through Ethernet.

---

## Modem

A **modem** connects your network to the **Internet Service Provider (ISP)**.

```text
Your Network → Modem → ISP → Internet
```

In modern home networks, the modem, router, switch, and wireless AP may be combined into one device.

---

# 5. OSI Model – 7 Layers

| Layer | Name | Examples / Function |
|---|---|---|
| 7 | Application | HTTP, HTTPS, DNS, SMTP |
| 6 | Presentation | Encryption, decryption, data formatting |
| 5 | Session | Establish, manage, terminate sessions |
| 4 | Transport | TCP, UDP, Ports |
| 3 | Network | IP addressing, Routing |
| 2 | Data Link | MAC addresses, Ethernet frames |
| 1 | Physical | Cables, radio signals, physical transmission |

### Important SOC Focus

```text
Layer 7 → Application traffic
Layer 4 → Ports and TCP/UDP
Layer 3 → IP addresses
Layer 2 → MAC addresses
```

For a SOC Analyst, **Layers 3, 4, and 7** are particularly important during network investigations.

---

# 6. TCP/IP Model

The TCP/IP model maps to the OSI model as follows:

```text
TCP/IP Model        OSI Model

Application      →  Application
                    Presentation
                    Session

Transport        →  Transport

Internet         →  Network

Network Access   →  Data Link
                    Physical
```

The TCP/IP model consists of:

```text
Application
    ↓
Transport
    ↓
Internet
    ↓
Network Access
```

### Example

When accessing an HTTPS website:

```text
Application → HTTPS
Transport   → TCP
Internet    → IP
Network     → Ethernet / Wi-Fi
```

---

# 7. Data Flow Between Two Computers

A simplified data flow is:

```text
Application
HTTP/HTTPS
     ↓
Transport
TCP
Source Port + Destination Port
     ↓
Network
Source IP + Destination IP
     ↓
Data Link
Source MAC + Destination MAC
     ↓
Physical
Ethernet Cable / Wi-Fi
```

### Example

Suppose:

```text
PC1
IP: 192.168.1.10

PC2
IP: 192.168.1.20
```

PC1 wants to communicate with PC2.

```text
PC1
 ↓
Application Data
 ↓
TCP
Source Port: 50000
Destination Port: 443
 ↓
IP
Source: 192.168.1.10
Destination: 192.168.1.20
 ↓
MAC
Source MAC → Destination MAC
 ↓
Ethernet/Wi-Fi
 ↓
PC2
```

The receiving computer then processes the data in the **reverse direction**.

---

# 8. SOC Perspective

Networking knowledge is essential for SOC Analysts because most security incidents involve network communication.

When investigating a network alert, ask:

### WHO?

```text
Source IP
```

### WHERE?

```text
Destination IP
```

### HOW?

```text
TCP / UDP
```

### WHICH SERVICE?

```text
Destination Port
```

### WHAT APPLICATION?

```text
HTTP / DNS / SMB / RDP / SSH
```

### ALLOWED OR BLOCKED?

```text
Firewall Action
```

### WHAT HAPPENED NEXT?

Check:

```text
Endpoint Logs
Authentication Logs
Process Creation Logs
DNS Logs
Firewall Logs
```

---

# 9. Mini SOC Scenario

Suppose you receive this alert:

```text
Internal PC: 10.10.10.15
External IP: 185.x.x.x
Destination Port: TCP 4444
```

Your investigation questions should be:

1. Who owns `10.10.10.15`?
2. Why did the system connect to the external IP?
3. Is TCP port `4444` expected in the environment?
4. What process created the connection?
5. Was the connection allowed or blocked?
6. Has this external IP been seen before?
7. Is the destination IP malicious?
8. What happened before and after the connection?
9. Are there related endpoint or authentication events?

---

# 10. Day 1 Key Takeaways

Remember these important concepts:

- **LAN** → Small geographical area
- **WAN** → Large geographical area
- **MAN** → City/metropolitan area
- **Internet** → Global public network
- **Intranet** → Private internal organizational network
- **Client** → Requests services
- **Server** → Provides services
- **Hub** → Broadcasts traffic to all ports
- **Switch** → Uses MAC addresses to forward frames
- **Router** → Connects networks and routes using IP addresses
- **Firewall** → Controls network traffic using security rules
- **Access Point** → Provides wireless network access
- **Modem** → Connects the network to the ISP
- **OSI** → 7-layer networking reference model
- **TCP/IP** → Practical networking model
- **IP Address** → Layer 3 logical addressing
- **MAC Address** → Layer 2 hardware/network interface addressing
- **Port** → Identifies a service/application endpoint
- **TCP/UDP** → Transport-layer protocols

## SOC Investigation Mindset

```text
Source IP
    ↓
Destination IP
    ↓
Protocol
    ↓
Port
    ↓
Application
    ↓
Firewall Action
    ↓
Endpoint Activity
    ↓
Authentication Activity
    ↓
Determine: Benign or Malicious?
```
