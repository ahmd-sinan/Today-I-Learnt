# Computer Networking: Protocols, OSI Model, Troubleshooting & Security 

**Date:** 2026-09-28

Today I learned the foundational protocols that govern network communication, the standard port numbers they use, and the 7-layer OSI model that explains how data moves. I also documented a comprehensive 10-step troubleshooting methodology, common network failures, and essential security practices.

## Important Network Protocols

*   **ARP (Address Resolution Protocol):** Finds the MAC address associated with an IPv4 address inside a local network.
    *   *Example:* "Who has 192.168.1.20?" The device with that IP responds with its MAC address.
*   **DNS (Domain Name System):** Converts domain names into IP addresses.
    *   *Example:* `google.com` → IP address
*   **DHCP (Dynamic Host Configuration Protocol):** Automatically provides network configuration such as: IP address, Subnet mask, Default gateway, and DNS server.
*   **HTTP:** Used for web communication.
*   **HTTPS:** HTTP protected by encryption using TLS.
*   **TCP:** Provides reliable, ordered data delivery.
*   **UDP:** Provides faster, connectionless communication without the same reliability guarantees as TCP.
*   **ICMP:** Used for network control and diagnostic messages. `ping` uses ICMP in normal IPv4 operation.

## TCP vs. UDP Comparison

### TCP (Transmission Control Protocol)
**TCP provides:**
*   Reliable delivery
*   Ordered data
*   Error detection
*   Retransmission
*   Flow control
*   Connection-oriented communication

**Common uses:** Web traffic, Email, File transfer, Database communication.

### UDP (User Datagram Protocol)
**UDP provides:**
*   Low overhead
*   Connectionless communication
*   No built-in guarantee of delivery
*   No built-in guarantee of order

**Common uses:** DNS queries, Streaming, Online games, Voice and video communication, Real-time applications.

## Port Numbers
A port number identifies a service or application on a device. *Note: A port is not the same as a physical switch port. A network port is a logical software communication endpoint.*

| Port | Common Service |
| :--- | :--- |
| **20/21** | FTP (File Transfer Protocol) |
| **22** | SSH (Secure Shell) |
| **23** | Telnet |
| **25** | SMTP (Simple Mail Transfer Protocol) |
| **53** | DNS (Domain Name System) |
| **67/68** | DHCP (Dynamic Host Configuration Protocol) |
| **80** | HTTP (Hypertext Transfer Protocol) |
| **110** | POP3 (Post Office Protocol version 3) |
| **143** | IMAP (Internet Message Access Protocol) |
| **443** | HTTPS (HTTP Secure) |
| **3389** | Remote Desktop Protocol (RDP) |

## The OSI Model
The OSI model explains network communication using seven layers.

*   **Layer 7 – Application:** Provides network services to applications.
    *   *Examples:* HTTP, DNS, SMTP
*   **Layer 6 – Presentation:** Handles data format, encryption, compression, and translation.
*   **Layer 5 – Session:** Manages communication sessions.
*   **Layer 4 – Transport:** Provides end-to-end communication.
    *   *Examples:* TCP, UDP
    *   *Data unit:* Segment or datagram
*   **Layer 3 – Network:** Handles logical addressing and routing.
    *   *Examples:* IPv4, IPv6, Routers
    *   *Data unit:* Packet
*   **Layer 2 – Data Link:** Handles local network communication.
    *   *Examples:* Ethernet, MAC addresses, Switches
    *   *Data unit:* Frame
*   **Layer 1 – Physical:** Handles physical transmission.
    *   *Examples:* Cables, Electrical signals, Light, Radio signals, Connectors
    *   *Data unit:* Bits

## Encapsulation
When an application sends data, each networking layer adds its own information. The general process is:
1.  **Application data** ↓
2.  **Transport segment** ↓
3.  **Network packet** ↓
4.  **Data-link frame** ↓
5.  **Bits on the cable**

At the receiving device, the process is reversed. This is called **decapsulation**.

## Network Troubleshooting Method
A simple, 10-step troubleshooting process:

*   **Step 1: Check the physical connection.** Is the cable connected? Are the connector lights on? Is the cable damaged? Is the correct port used?
*   **Step 2: Check the network adapter.** Is Ethernet or Wi-Fi enabled? Is the adapter detected? Is the driver installed?
*   **Step 3: Check IP configuration.** Run `ipconfig` and check the IP address, Subnet mask, and Default gateway.
*   **Step 4: Test the local computer.** Run `ping 127.0.0.1` to test the local TCP/IP stack.
*   **Step 5: Test the computer’s own IP address.** Run `ping YOUR_IP_ADDRESS`.
*   **Step 6: Test another device in the same LAN.** Run `ping 192.168.1.20`.
*   **Step 7: Test the default gateway.** Run `ping 192.168.1.1`.
*   **Step 8: Test DNS.** Try `nslookup example.com`.
*   **Step 9: Test a service or port.** For PowerShell, run `Test-NetConnection example.com -Port 443`.
*   **Step 10: Check firewall and configuration.** A firewall, wrong subnet, wrong gateway, or incorrect DNS setting may block communication.

## Common Network Problems
*   **Wrong cable wiring:** Caused by incorrect color order, wire not inserted fully, poor crimping, or damaged connector.
*   **IP address conflict:** Two devices use the same IP address resulting in unstable communication, intermittent connection, and incorrect network behavior.
*   **Wrong subnet mask:** Devices may believe they are in different networks even when physically connected.
*   **Wrong default gateway:** The device may communicate inside the LAN but fail to access outside networks.
*   **DNS failure:** The internet may work by IP address but domain names may not open.
*   **Firewall blocking:** A firewall may block ping, ports, or applications.
*   **Damaged cable:** A broken wire or poor connector may cause no connection, packet loss, low speed, or intermittent failure.
*   **Duplex or speed mismatch:** Network devices may negotiate different speeds or duplex modes, causing poor performance.

## Network Security Basics

**Important network security practices include:**
*   Use strong passwords and change default router passwords.
*   Keep firmware and software updated.
*   Use firewalls and encryption.
*   Disable unused services.
*   Use secure Wi-Fi settings.
*   Avoid open public Wi-Fi when handling sensitive data.
*   Separate important devices using VLANs or network segmentation.
*   Monitor unusual traffic.
*   Use multi-factor authentication (MFA).
*   Keep backups and limit administrator access.

**Common threats:**
*   Malware
*   Phishing
*   Password attacks
*   Denial-of-service (DoS) attacks
*   Man-in-the-middle attacks
*   Packet sniffing
*   Unauthorized access
*   Rogue access points
*   IP spoofing
*   MAC spoofing