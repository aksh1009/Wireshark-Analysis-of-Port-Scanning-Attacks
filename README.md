
# Wireshark Analysis of Port Scanning Attacks

## Table of Contents

- [Overview](#overview)
- [Contents](#contents)
- [Port Scanning Techniques](#port-scanning-techniques)
  - [Vanilla Scan](#vanilla-scan)
  - [TCP Half Open (SYN Scan)](#tcp-half-open-syn-scan)
  - [TCP Connect](#tcp-connect)
  - [UDP Scan](#udp-scan)
  - [Xmas Scan](#xmas-scan)
  - [FIN Scan](#fin-scan)
- [Analysis](#analysis)
- [Legal and Ethical Considerations](#legal-and-ethical-considerations)

## Overview

This project delves into the analysis of various port scanning attacks using the powerful network analysis tool, Wireshark. Port scanning is a common reconnaissance technique employed by attackers to discover open ports on a target system. By capturing and scrutinizing the network traffic generated during these attacks, we aim to gain insights into their characteristics and behaviors.

## Contents

- **pcap_files:** This directory includes Wireshark capture files (.pcap) for different port scanning techniques.
- **visuals:** Find IO graph visualizations created using Wireshark for a better understanding of network activity during port scanning.

## Port Scanning Techniques

### Vanilla Scan

This is another basic technique and it attempts to connect to all of the 65,536 ports simultaneously. It sends a request to connect (SYN flag), then, after it receives an acknowledgment of connection (SYN-ACK), returns an ACK flag. This SYN, SYN-ACK, ACK exchange comprises a TCP handshake. As a full connection is logged by firewalls, the Vanilla port scanning technique is easy to spot.

**Nmap Command:**
```bash
nmap -p- <target>
```

**Wireshark Filter**
```bash
udp and ip.addr == <target_ip> 
```


### TCP Half Open (SYN Scan)

This one is also called an SYN scan. It sends an SYN and waits for the target to send back an SYN-ACK, but it won`t answer back. Because the TCP connection is not complete, the system will not log the process. So, a TCP Half Open is hard to detect. However, the sender does find out if the port is open or not.


**Nmap Command:**
```bash
tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size<=1024
```

![WhatsApp Image 2023-12-28 at 01 01 32_abe44d37](https://github.com/aksh1009/Wireshark-Analysis-of-Port-Scanning-Attacks/assets/143216212/f43cc913-0c7b-4c6f-9c6b-93463d01e0a2)

**Wireshark Filter**
```bash
udp and ip.addr == <target_ip> 
```


### TCP Connect

The TCP connect port scanning technique works just like the SYN scan, but it completes the TCP connection. Since it sends one more packet, it is noisier and therefore easier to spot. So, the TCP connect scan is less common and popular.
On the plus side, the user won’t need the same level of privileges to run it, as it would need for an SYN scan. It just uses the same connection protocols like any user that connects to other systems.

**Wireshark Filter**
```bash
tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size>1024	
```

### UDP Scan

This offers lots of exploitable UDP services that attackers can use. DNS exfiltration is only one of them.
For this technique, it is important to send a specific payload to the target. In order to check if a DNS server is up, you should send a DNS request.
You can send a DNS request to UDP port 53 and see if you get a DNS reply. If the system responds, it means there`s a DNS server.

**Nmap Command:**
```bash
nmap -sU <target>
```


![WhatsApp Image 2023-12-28 at 01 56 06_1516f789](https://github.com/aksh1009/Wireshark-Analysis-of-Port-Scanning-Attacks/assets/143216212/31963e7e-7ea7-48ba-8746-cba4b27d494f)

**Wireshark Filter**
```bash
udp and ip.addr == <target_ip> 
```

### Xmas Scan

The Xmas scan sends various flags – FIN, URG, and PUSH – with the intent to create an illogical interaction. The sender then analyzes the system’s response so it can find out more about the system’s ports and firewall. give me nmap code for these

**Nmap Command:**
```bash
tcp.flags.fin==1 && tcp.flags.push==1 && tcp.flags.urg==1
```

![WhatsApp Image 2023-12-28 at 01 34 36_0b1b327a](https://github.com/aksh1009/Wireshark-Analysis-of-Port-Scanning-Attacks/assets/143216212/4d8ee05d-1d56-4c93-8090-2991d5889d1d)

**Wireshark Filter**
```bash
udp and ip.addr == <target_ip> 
```


### FIN Scan

The FIN scan means you send to a port a FIN flag, although you don`t intend to end an established session. The system’s response allows you to check the state of the port or gather insights about the firewall. If a port is closed and receives an unsolicited FIN packet, it sends back an instantaneous abort packet (RST). If the port is open, it will ignore it.

**Nmap Command:**
```bash
nmap -sF <target>
```

![WhatsApp Image 2023-12-28 at 01 41 45_fb9fc69f](https://github.com/aksh1009/Wireshark-Analysis-of-Port-Scanning-Attacks/assets/143216212/1419b80b-a9f2-4108-8a11-53f408ab3cd6)

**Wireshark Filter**
```bash
tcp.flags==0x001
```

## Analysis

Explore the pcap files using Wireshark to observe the network traffic during each port scanning technique. The associated IO graphs visually represent the interactions between the scanner and the target system.

## Legal and Ethical Considerations

Ensure that you have the right permissions to use and analyze network traffic. Respect privacy and legal regulations when conducting any form of network analysis. I have performed these attacks in my local network.


