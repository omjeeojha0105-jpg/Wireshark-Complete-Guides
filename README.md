# 🦈 Wireshark – Complete Network Traffic Analysis Guide

<p align="center">
  <img src="images/01-wireshark-banner.png" width="100%" alt="Wireshark Network Analysis Banner">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Tool-Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white">
  <img src="https://img.shields.io/badge/Domain-Network%20Security-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/Documentation-Self--Learning-orange?style=for-the-badge">
</p>

<h3 align="center">Understand Packets. Analyse Traffic. Investigate Networks.</h3>

<p align="center">
  <b>Created by Omjee Ojha</b><br>
  Integrated B.Tech/M.Tech CSE – Cybersecurity | UPSIFS<br>
  Cybersecurity Intern – NetworkWalks
</p>

---

## 👨‍💻 About Me

Hello! I'm **Omjee Ojha**, an Integrated B.Tech/M.Tech Computer Science student specialising in Cybersecurity at Uttar Pradesh State Institute of Forensic Sciences (UPSIFS).

I'm currently developing my practical cybersecurity skills through my internship at **NetworkWalks**.

I created this repository to document my understanding of **Wireshark and Network Traffic Analysis**, starting from networking fundamentals and moving towards advanced packet investigation.

My approach is simple:

- Understand how network communication works.
- Learn how packets travel between devices.
- Identify protocols, ports, IP addresses and communication patterns.
- Use display filters to investigate specific traffic.
- Understand what packet evidence actually tells us.

This is a continuously evolving technical learning resource.

---

## 📚 Table of Contents

1. [Introduction to Wireshark](#-1-introduction-to-wireshark)
2. [What Does Wireshark Capture?](#-2-what-does-wireshark-capture)
3. [Network Interfaces](#-3-understanding-network-interfaces)
4. [How Packet Capture Works](#-4-how-packet-capture-works)
5. [Understanding Packet Structure](#-5-understanding-packet-structure)
6. [OSI and TCP/IP Models](#-6-osi-and-tcpip-models)
7. [Wireshark Interface](#-7-wireshark-interface-explained)
8. [Capture Filters vs Display Filters](#-8-capture-filters-vs-display-filters)
9. [Display Filter Cheat Sheet](#-9-wireshark-display-filter-cheat-sheet)
10. [Protocol Analysis](#-10-protocol-wise-analysis)
11. [TCP Deep Dive](#-11-tcp-deep-dive)
12. [DNS Investigation](#-12-dns-investigation)
13. [HTTP and TLS Analysis](#-13-http-and-tls-analysis)
14. [Network Security Investigation](#-14-network-security-investigation)
15. [Advanced Wireshark Features](#-15-advanced-wireshark-features)
16. [Statistics and Graphs](#-16-statistics-and-graphs)
17. [Investigation Workflow](#-17-network-investigation-workflow)
18. [Common Mistakes and Limitations](#-18-common-mistakes-and-limitations)
19. [Resources](#-19-learning-resources)

---

# 🌐 1. Introduction to Wireshark

**Wireshark** is an open-source network protocol analyser used to capture, inspect, filter and analyse network traffic.

It allows us to examine individual packets and understand how devices communicate over a network.

### 🔍 What can Wireshark do?

| Function | Explanation |
|---|---|
| Packet Capture | Capture packets available to a selected interface |
| Protocol Analysis | Analyse TCP, UDP, DNS, HTTP, TLS and other protocols |
| Network Troubleshooting | Investigate communication failures |
| Traffic Analysis | Examine source IPs, destination IPs and ports |
| Security Monitoring | Investigate suspicious traffic patterns |
| Network Forensics | Examine saved packet captures |

### 💡 Real-world example

Suppose I open Instagram on my laptop.

Behind the scenes:

1. My laptop may perform DNS resolution.
2. It connects to an Instagram server.
3. TCP or QUIC carries the communication.
4. TLS protects application data.
5. The server sends responses back.

Wireshark helps investigate the network communication involved in these activities.

**Important:** Wireshark does not automatically reveal encrypted passwords, private messages or session cookies.

---

# 📡 2. What Does Wireshark Capture?

Wireshark captures packets made available by the selected network interface and the underlying packet-capture mechanism.

It does not automatically capture every packet travelling across the internet.

### 🖥️ Example Network

```text
                  INTERNET
                      |
                 WIFI ROUTER
                /     |     \
               /      |      \
          LAPTOP    PHONE    PC
             |
         WIRESHARK
```

### Which traffic can be visible?

| Traffic Type | Visibility |
|---|---|
| My laptop's outgoing traffic | Usually visible |
| My laptop's incoming traffic | Usually visible |
| Broadcast traffic delivered to my interface | Usually visible |
| Relevant multicast traffic | Depends on membership and configuration |
| Other devices' unicast traffic | Normally not visible |
| Traffic on an unrelated network | Not visible |

### 🔹 Promiscuous Mode

Promiscuous mode allows a network interface to accept frames that are not addressed to its own MAC address, where the underlying network delivers those frames to it.

However, it does not bypass normal Ethernet switch forwarding.

### 🔹 Monitor Mode

Monitor mode is a wireless capture mode that allows a compatible Wi-Fi adapter to capture supported 802.11 frames in its radio range.

### Promiscuous Mode vs Monitor Mode

| Promiscuous Mode | Monitor Mode |
|---|---|
| Used with supported network interfaces | Used with compatible wireless adapters |
| Accepts additional Ethernet frames delivered to the interface | Captures supported wireless 802.11 frames |
| Does not automatically expose switched unicast traffic | Depends on channel, hardware and wireless conditions |

To monitor other devices' traffic in an authorised environment, network administrators may configure:

- Switch Port Mirroring (SPAN)
- Network TAP
- Wireless monitoring sensor
- Dedicated monitoring interface

### 📷 Visual Reference

![Wireshark Capture](images/04-packet-capture.png)

---

# 🖥️ 3. Understanding Network Interfaces

A network interface is a connection point through which a device sends and receives network traffic.

Wireshark displays the available interfaces when opening the application.

### Common Linux Interfaces

| Interface | Meaning |
|---|---|
| eth0 | Ethernet |
| wlan0 | Wireless LAN |
| lo | Loopback |
| any | Linux pseudo-interface for supported interfaces |
| tun0 | TUN VPN interface |
| tap0 | TAP virtual Ethernet interface |
| docker0 | Docker bridge |
| vboxnet0 | VirtualBox host-only interface |
| nflog | Netfilter logging interface |
| nfqueue | Netfilter queue interface |

### 🔹 1. Ethernet (eth0)

Used for wired network communication.

### 🔹 2. Wireless (wlan0)

Used for Wi-Fi communication.

### 🔹 3. Loopback (lo)

Used for communication within the same device.

Example:

```bash
ping 127.0.0.1
```

### 🔹 4. TUN Interface

A TUN interface handles routed IP packets and is commonly used by VPN applications.

### 🔹 5. TAP Interface

A TAP interface handles virtual Ethernet frames.

### 🔹 6. Any Interface

On Linux, `any` is a pseudo-interface that can capture traffic from multiple supported interfaces.

It is not a physical network card.

### 🔧 Commands to identify interfaces

```bash
ip link show
```

```bash
ip addr
```

```bash
ip route
```

```bash
ip -br addr
```

### 📷 My Interface View

![Wireshark Interfaces](images/03-network-interfaces.png)

---

# ⚙️ 4. How Packet Capture Works

Wireshark relies on a packet-capture mechanism such as Npcap on Windows or libpcap-compatible facilities on Linux.

### Packet Capture Flow

```text
NETWORK TRAFFIC
      |
      v
NETWORK INTERFACE
      |
      v
PACKET CAPTURE DRIVER / LIBRARY
      |
      v
WIRESHARK CAPTURE ENGINE
      |
      v
PROTOCOL DISSECTION
      |
      v
DISPLAY FILTER
      |
      v
PACKET INVESTIGATION
      |
      v
SAVE PCAP / PCAPNG
```

### Step-by-step process

1. Select the correct network interface.
2. Start packet capture.
3. Receive available packets through the capture mechanism.
4. Wireshark dissects the packet protocols.
5. Apply display filters.
6. Inspect packet details and raw bytes.
7. Save the capture for later investigation.

![Packet Capture](images/04-packet-capture.png)

---

# 📦 5. Understanding Packet Structure

A packet contains information that helps deliver data across a network.

A simplified Ethernet + IPv4 + TCP packet can be represented as:

```text
+----------------------------+
| Ethernet Header            |
+----------------------------+
| IPv4 Header                |
+----------------------------+
| TCP Header                 |
+----------------------------+
| Application Data           |
+----------------------------+
```

### Important Fields

| Field | Meaning |
|---|---|
| Source MAC | Sender's hardware address |
| Destination MAC | Receiver's hardware address |
| Source IP | Sender's IP address |
| Destination IP | Receiver's IP address |
| Source Port | Source application port |
| Destination Port | Destination application port |
| Protocol | TCP, UDP, ICMP, etc. |
| Payload | Carried application data |

### Packet Investigation

When inspecting a packet, ask:

1. Who sent the packet?
2. Who received it?
3. Which protocol is used?
4. Which ports are involved?
5. What information is carried?
6. Is the communication encrypted?

![Packet Details](images/05-packet-details.png)

---

# 🧱 6. OSI and TCP/IP Models

Understanding networking layers makes packet analysis easier.

### OSI Model

| Layer | Name | Examples |
|---|---|---|
| 7 | Application | HTTP, DNS, SMTP |
| 6 | Presentation | Encoding, encryption |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IPv4, IPv6, ICMP |
| 2 | Data Link | Ethernet, ARP |
| 1 | Physical | Cables, radio signals |

### Visual Representation

```text
+-----------------------+
| 7. Application        |
+-----------------------+
| 6. Presentation       |
+-----------------------+
| 5. Session            |
+-----------------------+
| 4. Transport          |
+-----------------------+
| 3. Network            |
+-----------------------+
| 2. Data Link          |
+-----------------------+
| 1. Physical           |
+-----------------------+
```

### Why is this important?

Suppose a website does not load.

You can investigate:

- DNS: Is the domain resolving?
- IP: Is the destination reachable?
- TCP: Is the connection established?
- TLS: Is the secure handshake succeeding?
- HTTP: Is the server returning an error?

![OSI and Packet Analysis](images/06-osi-model.png)

---

# 🦈 7. Wireshark Interface Explained

The main Wireshark window contains several important sections.

### 1. Interface List

Displays available capture interfaces.

### 2. Capture Filter Bar

Restricts packets before they are captured.

### 3. Display Filter Bar

Filters already captured packets.

### 4. Packet List Pane

Displays:

- Packet Number
- Time
- Source
- Destination
- Protocol
- Length
- Information

### 5. Packet Details Pane

Displays decoded packet information.

### 6. Packet Bytes Pane

Displays hexadecimal and ASCII packet data.

### 7. Status Bar

Displays packet and capture information.

![Wireshark Main Interface](images/02-wireshark-interface.png)

---

# 🔍 8. Capture Filters vs Display Filters

These are two different types of Wireshark filters.

| Feature | Capture Filter | Display Filter |
|---|---|---|
| Applied | Before and during capture | After packets are captured |
| Purpose | Restrict captured traffic | Show selected packets |
| Syntax | BPF | Wireshark display-filter syntax |
| Can recover excluded packets? | No | Only if they were captured |
| Example | `port 53` | `dns` |

### 🔹 Capture Filter Examples

Capture DNS:

```bash
port 53
```

Capture TCP:

```bash
tcp
```

Capture a specific host:

```bash
host 192.168.1.10
```

Capture a network:

```bash
net 192.168.1.0/24
```

Capture HTTPS over TCP:

```bash
tcp port 443
```

Exclude SSH:

```bash
not port 22
```

Capture only traffic from a host:

```bash
src host 192.168.1.10
```

Capture only traffic going to a host:

```bash
dst host 192.168.1.20
```

### 🔹 Display Filter Examples

```wireshark
dns
```

```wireshark
tcp
```

```wireshark
ip.addr == 192.168.1.10
```

```wireshark
tcp.port == 443
```

---

# 🧰 9. Wireshark Display Filter Cheat Sheet

## 🌐 Basic Protocol Filters

| Purpose | Filter |
|---|---|
| TCP | `tcp` |
| UDP | `udp` |
| DNS | `dns` |
| HTTP | `http` |
| TLS | `tls` |
| ICMP | `icmp` |
| IPv4 | `ip` |
| IPv6 | `ipv6` |
| ARP | `arp` |
| DHCP | `dhcp` |
| QUIC | `quic` |
| FTP | `ftp` |
| SSH | `ssh` |
| SMTP | `smtp` |
| SMB | `smb` |
| NTP | `ntp` |

## 🖥️ IP Address Filters

| Purpose | Filter |
|---|---|
| Specific IP | `ip.addr == 192.168.1.10` |
| Source IP | `ip.src == 192.168.1.10` |
| Destination IP | `ip.dst == 192.168.1.20` |
| Specific network | `ip.addr == 192.168.1.0/24` |
| Exclude IP | `!(ip.addr == 192.168.1.10)` |
| IPv6 address | `ipv6.addr == 2001:db8::1` |

## 🔌 Port Filters

| Purpose | Filter |
|---|---|
| Port 80 | `tcp.port == 80` |
| Port 443 | `tcp.port == 443` |
| Port 53 | `udp.port == 53` |
| Source port 443 | `tcp.srcport == 443` |
| Destination port 80 | `tcp.dstport == 80` |
| Port range | `tcp.port >= 1 && tcp.port <= 1024` |
| Multiple ports | `tcp.port == 80 || tcp.port == 443` |

## 🔄 TCP Filters

| Purpose | Filter |
|---|---|
| SYN | `tcp.flags.syn == 1` |
| SYN without ACK | `tcp.flags.syn == 1 && tcp.flags.ack == 0` |
| ACK | `tcp.flags.ack == 1` |
| RST | `tcp.flags.reset == 1` |
| FIN | `tcp.flags.fin == 1` |
| PSH | `tcp.flags.push == 1` |
| Retransmissions | `tcp.analysis.retransmission` |
| Duplicate ACK | `tcp.analysis.duplicate_ack` |
| Out of order | `tcp.analysis.out_of_order` |
| Zero window | `tcp.analysis.zero_window` |

## 📡 DNS Filters

| Purpose | Filter |
|---|---|
| DNS queries | `dns.flags.response == 0` |
| DNS responses | `dns.flags.response == 1` |
| Specific domain | `dns.qry.name == "example.com"` |
| Domain contains text | `dns.qry.name contains "google"` |
| DNS errors | `dns.flags.rcode != 0` |
| A record | `dns.qry.type == 1` |
| AAAA record | `dns.qry.type == 28` |
| MX record | `dns.qry.type == 15` |
| TXT record | `dns.qry.type == 16` |
| CNAME record | `dns.qry.type == 5` |

## 🌍 HTTP Filters

| Purpose | Filter |
|---|---|
| HTTP requests | `http.request` |
| HTTP responses | `http.response` |
| GET requests | `http.request.method == "GET"` |
| POST requests | `http.request.method == "POST"` |
| HTTP 200 | `http.response.code == 200` |
| HTTP 404 | `http.response.code == 404` |
| HTTP 500 | `http.response.code == 500` |
| Host header | `http.host` |
| URI | `http.request.uri` |
| User-Agent | `http.user_agent` |

## 🔐 TLS Filters

| Purpose | Filter |
|---|---|
| TLS traffic | `tls` |
| Client Hello | `tls.handshake.type == 1` |
| Server Hello | `tls.handshake.type == 2` |
| Certificate | `tls.handshake.type == 11` |
| TLS alerts | `tls.record.content_type == 21` |
| SNI hostname | `tls.handshake.extensions_server_name` |

## 📶 ARP, DHCP and ICMP Filters

| Purpose | Filter |
|---|---|
| ARP requests | `arp.opcode == 1` |
| ARP replies | `arp.opcode == 2` |
| DHCP | `dhcp` |
| DHCP Discover | `dhcp.option.dhcp == 1` |
| DHCP Offer | `dhcp.option.dhcp == 2` |
| DHCP Request | `dhcp.option.dhcp == 3` |
| DHCP ACK | `dhcp.option.dhcp == 5` |
| ICMP Echo Request | `icmp.type == 8` |
| ICMP Echo Reply | `icmp.type == 0` |
| Destination Unreachable | `icmp.type == 3` |
| Time Exceeded | `icmp.type == 11` |

## 🚨 Investigation Filters

| Purpose | Filter |
|---|---|
| TCP connection attempts | `tcp.flags.syn == 1 && tcp.flags.ack == 0` |
| TCP resets | `tcp.flags.reset == 1` |
| Retransmissions | `tcp.analysis.retransmission` |
| Failed DNS responses | `dns.flags.rcode != 0` |
| HTTP POST | `http.request.method == "POST"` |
| Large TCP payload | `tcp.len > 1000` |
| Large frames | `frame.len > 1400` |
| Broadcast | `eth.dst == ff:ff:ff:ff:ff:ff` |

### 🔗 Combined Filters

DNS or HTTP:

```wireshark
dns || http
```

TCP communication involving a specific IP:

```wireshark
tcp && ip.addr == 192.168.1.10
```

HTTP errors:

```wireshark
http.response.code >= 400
```

Specific DNS queries:

```wireshark
dns.flags.response == 0 && dns.qry.name contains "example"
```

TCP SYN packets to a particular destination:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0 && ip.dst == 192.168.1.20
```

**Note:** These filters should be tested against your installed Wireshark version. A matching packet is a lead for investigation, not automatic proof of an attack.

---

# 🔬 10. Protocol-Wise Analysis

## A. DNS – Domain Name System

DNS translates domain names into IP addresses.

Example:

```text
User enters example.com
          |
          v
      DNS Query
          |
          v
     DNS Resolver
          |
          v
     DNS Response
          |
          v
     IP Address
```

Useful filters:

```wireshark
dns
```

```wireshark
dns.flags.response == 0
```

```wireshark
dns.flags.response == 1
```

```wireshark
dns.qry.name contains "instagram"
```

Investigate:

- Requested domain.
- Query type.
- DNS server.
- Response code.
- Returned IP address.
- Follow-up connections.

![DNS Analysis](images/08-dns-analysis.png)

## B. TCP – Transmission Control Protocol

TCP is connection-oriented and provides reliable data delivery.

### TCP Three-Way Handshake

```text
CLIENT                      SERVER
  |                            |
  |---------- SYN ------------>|
  |                            |
  |<-------- SYN + ACK --------|
  |                            |
  |----------- ACK ----------->|
  |                            |
  |                     CONNECTED
```

Useful filters:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

```wireshark
tcp.flags.reset == 1
```

Investigate:

- Source and destination IP.
- Source and destination port.
- SYN, SYN-ACK and ACK.
- RST and FIN.
- Retransmissions.

![TCP Handshake](images/07-tcp-handshake.png)

## C. UDP – User Datagram Protocol

UDP is connectionless and does not use a TCP-style three-way handshake.

Common applications:

- DNS
- DHCP
- VoIP
- Streaming
- QUIC

Filter:

```wireshark
udp
```

## D. HTTP – Hypertext Transfer Protocol

HTTP is used for web communication.

Example:

```http
GET /index.html HTTP/1.1
Host: example.com
```

Useful filters:

```wireshark
http.request
```

```wireshark
http.response
```

```wireshark
http.request.method == "POST"
```

Investigate:

- Request method.
- Host.
- URI.
- Response code.
- Content type.
- User-Agent.

![HTTP Analysis](images/09-http-analysis.png)

## E. TLS – Transport Layer Security

TLS protects communication by encrypting application data.

A simplified TLS handshake includes Client Hello and Server Hello, followed by the rest of the handshake.

Useful filters:

```wireshark
tls
```

```wireshark
tls.handshake.type == 1
```

```wireshark
tls.handshake.type == 2
```

Investigate:

- TLS version.
- Client Hello.
- Server Hello.
- SNI, when available.
- Certificates, when sent.
- TLS alerts.

**Important:** TLS usually prevents reading application payload contents from a normal encrypted capture.

![TLS Analysis](images/10-tls-handshake.png)

## F. ARP – Address Resolution Protocol

ARP resolves IPv4 addresses into MAC addresses on a local network.

```text
Who has 192.168.1.1?
Tell 192.168.1.10
```

Useful filters:

```wireshark
arp
```

```wireshark
arp.opcode == 1
```

```wireshark
arp.opcode == 2
```

## G. DHCP – Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to clients.

### DORA Process

```text
CLIENT                  DHCP SERVER

Discover  ------------>

          <------------ Offer

Request   ------------>

          <------------ Acknowledge
```

Useful filters:

```wireshark
dhcp
```

```wireshark
dhcp.option.dhcp == 1
```

```wireshark
dhcp.option.dhcp == 2
```

```wireshark
dhcp.option.dhcp == 3
```

```wireshark
dhcp.option.dhcp == 5
```

Investigate:

- Client MAC.
- Offered IP.
- DHCP server.
- Default gateway.
- DNS server.
- Lease duration.

## H. ICMP – Internet Control Message Protocol

ICMP is used for network diagnostics and error reporting.

Useful filters:

```wireshark
icmp
```

```wireshark
icmp.type == 8
```

```wireshark
icmp.type == 0
```

```wireshark
icmp.type == 3
```

Investigate echo requests, echo replies and unreachable messages.

---

# 🚨 11. TCP Deep-Dive

### TCP Flags

| Flag | Meaning |
|---|---|
| SYN | Starts a connection |
| ACK | Acknowledges received data |
| FIN | Gracefully closes a connection |
| RST | Resets a connection |
| PSH | Pushes data towards the application |
| URG | Marks urgent data |

### What does a closed port look like?

A connection attempt to a closed TCP port commonly receives a TCP RST response, often RST-ACK.

A filtered port may produce no response or an ICMP error, depending on firewall behaviour.

### Retransmissions

TCP retransmissions may happen due to:

- Packet loss.
- Network congestion.
- Receiver problems.
- Delayed acknowledgements.

Useful filters:

```wireshark
tcp.analysis.retransmission
```

```wireshark
tcp.analysis.duplicate_ack
```

```wireshark
tcp.analysis.out_of_order
```

```wireshark
tcp.analysis.zero_window
```

### Follow TCP Stream

Right-click a TCP packet:

**Follow → TCP Stream**

This helps reconstruct the available TCP conversation.

![TCP Stream](images/15-follow-tcp-stream.png)

---

# 🕵️ 12. DNS Investigation Workflow

When investigating DNS traffic, ask:

1. Which device made the query?
2. Which domain was requested?
3. Which DNS server received it?
4. What response was returned?
5. Which IP address was resolved?
6. Did the client connect to that IP?

```text
DNS QUERY
    |
    v
IDENTIFY CLIENT
    |
    v
IDENTIFY DOMAIN
    |
    v
CHECK DNS RESPONSE
    |
    v
EXTRACT RESOLVED IP
    |
    v
CHECK FOLLOW-UP CONNECTION
    |
    v
DOCUMENT FINDINGS
```

---

# 🌍 13. HTTP and HTTPS Investigation

### HTTP Checklist

- Identify request method.
- Identify hostname.
- Inspect URI.
- Check response status.
- Inspect headers.
- Identify content type.
- Follow the TCP stream where useful.

### HTTPS Checklist

- Identify TLS Client Hello.
- Identify TLS Server Hello.
- Inspect SNI if available.
- Inspect certificates if available.
- Check TLS alerts.
- Check connection resets and retransmissions.

### Important Difference

| HTTP | HTTPS |
|---|---|
| Application data is not encrypted by HTTP itself | TLS protects application data |
| Contents may be readable in a capture | Contents are normally encrypted |
| Sensitive information can be exposed | Provides confidentiality in transit |

---

# 🛡️ 14. Network Security Investigation

Wireshark can help investigate suspicious network activity.

## A. Port Scanning

Possible indicators:

- Multiple connection attempts.
- Multiple destination ports.
- Repeated SYN packets.
- RST responses.

Filter:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Questions:

- Which IP initiated the connection?
- Which IP was targeted?
- How many destination ports were attempted?
- What responses did the target return?

## B. Brute-Force Pattern Investigation

Possible indicators:

- Repeated authentication requests.
- Repeated failures.
- Unusual connection frequency.

Example filter for visible HTTP login requests:

```wireshark
http.request.method == "POST"
```

This is not sufficient by itself to confirm brute force.

## C. Cleartext Exposure

In an authorised HTTP lab, inspect POST requests:

```wireshark
http.request.method == "POST"
```

Then inspect the available request body and use Follow TCP Stream where appropriate.

## D. Suspicious DNS

Investigate unusual domains, repeated queries, failed resolutions and unexpected DNS servers.

```wireshark
dns.flags.rcode != 0
```

## E. Network Performance

Investigate retransmissions, duplicate ACKs and zero-window events.

```wireshark
tcp.analysis.retransmission
```

```wireshark
tcp.analysis.duplicate_ack
```

```wireshark
tcp.analysis.zero_window
```

**Reminder:** Suspicious-looking traffic is an indicator to investigate, not proof of malicious activity.

---

# ⚙️ 15. Advanced Wireshark Features

| Feature | Purpose |
|---|---|
| Follow TCP Stream | Reconstruct TCP conversations |
| Follow UDP Stream | Inspect supported UDP conversations |
| Follow HTTP/2 Stream | Inspect HTTP/2 streams |
| Expert Information | View protocol warnings and errors |
| Decode As | Apply a different protocol dissector |
| Packet Bytes | Inspect raw packet bytes |
| Conversations | View communication pairs |
| Endpoints | View network endpoints |
| Profiles | Save custom Wireshark settings |
| Time Display Format | Change packet timestamp display |

### Expert Information

Go to:

**Analyze → Expert Information**

### Decode As

Go to:

**Analyze → Decode As**

### Follow Stream

Right-click a relevant packet:

**Follow → TCP Stream**

---

# 📊 16. Statistics and Graphs

Wireshark has useful statistics tools under the **Statistics** menu.

| Feature | Purpose |
|---|---|
| Protocol Hierarchy | Shows protocol distribution |
| Endpoints | Shows communicating endpoints |
| Conversations | Shows communication pairs |
| I/O Graphs | Displays packet/byte activity over time |
| Flow Graph | Visualises communication flow |
| TCP Stream Graphs | Analyses TCP sequence and throughput |

### Protocol Hierarchy

Helps understand which protocols are present in a capture.

![Protocol Hierarchy](images/12-protocol-hierarchy.png)

### Endpoints

Helps identify IP and MAC endpoints, packet counts and bytes.

![Endpoints](images/13-endpoints.png)

### I/O Graph

Helps visualise network traffic over time.

![I/O Graph](images/14-io-graph.png)

---

# 🧪 17. Network Investigation Workflow

My preferred investigation workflow:

```text
1. DEFINE OBJECTIVE
        |
        v
2. SELECT INTERFACE
        |
        v
3. CAPTURE TRAFFIC
        |
        v
4. IDENTIFY PROTOCOLS
        |
        v
5. IDENTIFY IPS AND PORTS
        |
        v
6. ANALYSE DNS / TCP / HTTP / TLS
        |
        v
7. APPLY RELEVANT FILTERS
        |
        v
8. INVESTIGATE SUSPICIOUS PATTERNS
        |
        v
9. REVIEW STATISTICS
        |
        v
10. DOCUMENT FINDINGS
```

### Investigation Checklist

- [ ] Identify the correct capture interface.
- [ ] Start packet capture.
- [ ] Identify protocols.
- [ ] Identify source and destination IPs.
- [ ] Identify ports.
- [ ] Analyse DNS.
- [ ] Analyse TCP communication.
- [ ] Inspect HTTP/TLS traffic.
- [ ] Check retransmissions and resets.
- [ ] Review protocol hierarchy.
- [ ] Review endpoints and conversations.
- [ ] Document findings.

---

# ⚠️ 18. Common Mistakes and Limitations

| Misconception | Correct Understanding |
|---|---|
| Wireshark captures all internet traffic | It captures traffic available to the selected interface |
| Promiscuous mode sees every device | Switch forwarding still limits visibility |
| HTTPS is completely readable | Encrypted application data is normally protected |
| SYN means an attack | SYN is part of normal TCP communication |
| Retransmission means malware | It may indicate packet loss or congestion |
| DNS query proves a website was visited | A query alone does not prove successful access |
| Port number always identifies an application | Applications can use non-standard ports |
| Display filter deletes packets | It only changes displayed packets |

### 🔐 Privacy and Security

- Never publish passwords, cookies or session tokens.
- Do not upload private packet captures without reviewing them.
- Only monitor networks you own or are authorised to inspect.
- Mask personal and sensitive information in screenshots.

---

# 📚 19. Learning Resources

| Resource | Link |
|---|---|
| Wireshark Official Website | https://www.wireshark.org/ |
| Official Documentation | https://www.wireshark.org/docs/ |
| Display Filter Reference | https://www.wireshark.org/docs/dfref/ |
| Wireshark Wiki | https://wiki.wireshark.org/ |
| Sample Captures | https://wiki.wireshark.org/SampleCaptures |
| User's Guide | https://www.wireshark.org/docs/wsug_html_chunked/ |

---

## 🚀 Final Thoughts

Wireshark is not just about applying filters and reading packet outputs.

It is about understanding how devices communicate, why a packet exists, what information it carries and what the available evidence tells us.

I created this repository to build and document my understanding of network traffic analysis, one concept at a time.

**LEARN → CAPTURE → FILTER → ANALYSE → INVESTIGATE → DOCUMENT**

---

<p align="center">
  <b>Made with 💙 by Omjee Ojha</b><br>
  Cybersecurity Student | UPSIFS<br>
  Cybersecurity Intern | NetworkWalks
</p>

<p align="center">
  ⭐ If this guide helps you understand Wireshark, consider giving this repository a star!
</p>
