---
layout: post
title: "Day 2: Learning Nmap and Building My First Security Tool"
date: 2026-02-18
categories: learning tools
---

## What I Did Today

Today I dove deeper into network security by learning Nmap and building my first Python security tool - a banner grabber.

## Learning Nmap

Nmap is a powerful network scanning tool used for reconnaissance in penetration testing. Through the TryHackMe room, I learned:

- How to perform basic network scans to discover open ports
- Different scan types (TCP connect, SYN scan, UDP scan)
- How to identify services running on specific ports
- The importance of reconnaissance as the first phase of penetration testing

**Key takeaway:** Knowing what services are running and their versions is crucial for finding potential vulnerabilities.

## Building a Banner Grabber

I built my first security tool in Python - a banner grabber that identifies services running on network ports.

### What Banner Grabbing Is

When you connect to a network service (like SSH, FTP, or a web server), many services automatically send a "banner" - a message that identifies what they are and what version they're running. Banner grabbing is the technique of capturing this information.

**Why it matters:** Service banners reveal:
- What software is running (Apache, OpenSSH, etc.)
- Version numbers (which can have known vulnerabilities)
- Operating system information

### The Building Process

I wrote a Python script using the `socket` library to:
1. Connect to a target IP address and port
2. Receive the banner sent by the service
3. Display the information to the user

The hardest part was understanding how network sockets work and handling errors when connections fail (closed ports, timeouts, etc.).

### Code Highlights

The core functionality uses Python's socket library:
```python
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.settimeout(2)
s.connect((ip, port))
banner = s.recv(1024).decode().strip()
s.close()
```

This creates a TCP connection, receives up to 1024 bytes of data (the banner), converts it to readable text, and closes the connection.

### Testing Results

I tested the tool on multiple targets:

**Success:** 
- scanme.nmap.org port 22 - Successfully grabbed SSH banner: `SSH-2.0-OpenSSH_6.6.1p1 Ubuntu`

**Failed:**
- Port 21 (FTP) - No banner received (port likely closed)
- TryHackMe machine - All ports failed (not connected to VPN)

**What I learned:** Banner grabbing only works on open ports with services that send banners immediately. Some services like HTTP require you to send a request first before they respond.

## Challenges I Faced

1. **Understanding Python sockets** - Had to research how network connections work at a low level
2. **Error handling** - Learning to handle timeouts and connection failures gracefully
3. **Testing limitations** - Realized I needed VPN connection to test on TryHackMe machines
4. **Not all services cooperate** - Some services don't send banners automatically

## Key Learnings

- Banner grabbing is a fundamental reconnaissance technique
- Python's socket library makes network programming accessible
- Error handling is crucial in network tools
- Real-world testing reveals limitations you don't see in theory
- Understanding how tools work makes you better at using them

## What's Next

Tomorrow I'll be learning Burp Suite and diving into web application security. I'm also planning to enhance my banner grabber to:
- Scan multiple ports at once
- Handle HTTP requests properly
- Add multi-threading for faster scans

---

**Check out the tool on GitHub:** [banner-grabber](https://github.com/Giftanne01/banner-grabber)

**Time spent today:** ~4 hours of focused learning and building
