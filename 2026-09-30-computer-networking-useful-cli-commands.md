# Computer Networking: Useful CLI Commands (Windows & Linux) 

**Date:** 2026-09-30

Today I compiled a master list of the most essential command-line networking tools for both Windows and Linux environments. These commands are critical for daily administration, network diagnostics, and connectivity troubleshooting.

## Useful Commands to Learn (Windows)
Here are the fundamental networking commands used in the Windows Command Prompt or PowerShell, along with a brief explanation of their purpose:

*   **`ipconfig`**: Displays the basic network configuration (IPv4, IPv6, Subnet Mask, Default Gateway) for all active network adapters.
*   **`ipconfig /all`**: Displays an exhaustive list of network configurations, including the physical MAC address, DHCP server details, and DNS server addresses.
*   **`ping`**: Tests basic connectivity to another network device by sending ICMP Echo Requests and waiting for replies.
*   **`tracert`**: Traces the exact route (showing every router hop) that data packets take to reach a specific destination.
*   **`nslookup`**: Queries Domain Name System (DNS) servers to resolve a domain name (like google.com) into an IP address, or vice versa.
*   **`arp -a`**: Displays the local Address Resolution Protocol (ARP) table, which lists the known mappings of IP addresses to physical MAC addresses on the local network.
*   **`netstat`**: Displays active TCP connections, ports on which the computer is listening, and local routing tables.
*   **`hostname`**: Displays the computer's network device name.
*   **`getmac`**: Quickly displays the physical MAC addresses of the system's network adapters.

## Useful Commands to Learn (Linux)
Here are the fundamental networking commands used in Linux terminal environments:

*   **`ip addr`**: Shows IP addresses and detailed network interface hardware information (the modern replacement for `ifconfig`).
*   **`ip route`**: Displays the system's IP routing table, showing where traffic is directed.
*   **`ping`**: Tests connectivity to another device (Note: on Linux, `ping` will run infinitely by default until you press `Ctrl+C`).
*   **`traceroute`**: Maps the journey of packets to a destination, showing router hops and latency across the network.
*   **`tracepath`**: Similar to `traceroute`, but traces the path to a host discovering the MTU (Maximum Transmission Unit) along the way. It does not require root/sudo privileges.
*   **`ss`**: Dumps socket statistics. It is the modern, faster replacement for `netstat` used to check active network connections and listening ports.
*   **`ip neigh`**: Shows the neighbor table, which is the Linux equivalent of the ARP table (mapping IP addresses to MAC addresses).
*   **`dig`**: A highly flexible and powerful command-line tool for interrogating DNS name servers and troubleshooting DNS issues.
*   **`nslookup`**: Queries DNS servers to resolve domain names (an older tool, but still widely available and used).
*   **`hostname`**: Shows or sets the system's host name.
*   **`curl`**: Transfers data to or from a server without user interaction. Highly used for testing HTTP/HTTPS web services and APIs.
*   **`wget`**: A non-interactive network downloader used to retrieve and download files directly from the web to the local hard drive.

## ractical Examples
Here are practical, real-world examples of how to execute these commands in a terminal:

**Show IP addresses:**
```bash
ip addr