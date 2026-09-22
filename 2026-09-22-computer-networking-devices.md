# Computer Networking: Core Intermediary Devices & Servers 🖥️

**Date:** 2026-09-22


Today I learned the functional definitions and operational mechanics of core network hardware. I explored how Switches manage internal LAN traffic, how Routers navigate between different networks, the various classifications of Servers, and the foundational rules of Firewalls.

## 1. Switch
A network switch connects devices inside a LAN.

**Examples of connected devices:**
*   Computers
*   Servers
*   Printers
*   Access points
*   IP cameras

A switch uses MAC addresses to forward Ethernet frames.

**How a switch works:**
1.  A frame enters a switch port.
2.  The switch checks the source MAC address.
3.  The switch learns which port belongs to that MAC address.
4.  The switch checks the destination MAC address.
5.  It forwards the frame through the correct port when possible.

**Important terms:**
*   MAC address table
*   Ethernet frame
*   Switch port
*   Forwarding
*   Flooding
*   Broadcast

*Note:* A switch normally works mainly at Layer 2 of the OSI model, although Layer 3 switches can also perform routing.

## 2. Router
A router connects different networks.

**Example:**
Home LAN → Router → Internet

A router uses IP addresses and routing information to decide where packets should go.

**Main router functions:**
*   Connect different networks
*   Select paths for packets
*   Provide a default gateway
*   Perform NAT in many home networks
*   Support DHCP in many home networks
*   Connect LANs to the internet
*   Apply routing and security rules

### Switch vs Router Comparison

| Feature | Switch | Router |
| :--- | :--- | :--- |
| **Main purpose** | Connect devices in one LAN | Connect different networks |
| **Main address** | MAC address | IP address |
| **Main data unit** | Frame | Packet |
| **Common OSI layer** | Layer 2 | Layer 3 |
| **Example** | Connect computers in a lab | Connect LAN to internet |

## 3. Server
A server is a computer or software system that provides services to other devices.

**A server can be:**
*   Physical
*   Virtual
*   Cloud-based
*   A software application running on a normal computer

**Common server types:**
*   **Web server:** Provides websites and web content.
    *   *Examples of web-server software:* Apache, Nginx, Microsoft IIS
*   **File server:** Stores and shares files.
*   **Database server:** Stores and manages structured data.
    *   *Examples:* MySQL, PostgreSQL, Microsoft SQL Server
*   **DNS server:** Converts domain names into IP addresses.
    *   *Example:* `example.com` → IP address
*   **DHCP server:** Automatically provides network configuration to devices.
*   **Mail server:** Handles email delivery and storage.
*   **Authentication server:** Manages user login and access.
*   **Application server:** Runs application logic for clients.

## 4. Firewall
A firewall controls network traffic based on security rules.

**It can allow or block traffic according to:**
*   Source IP address
*   Destination IP address
*   Port number
*   Protocol
*   Network interface
*   Application
*   User or identity
*   Connection state

**Example:**
A firewall may allow HTTPS traffic on port 443, but block unwanted incoming traffic.

**Firewalls can be:**
*   Hardware firewalls
*   Software firewalls
*   Network firewalls
*   Host-based firewalls
*   Cloud security controls

*Note:* A firewall is an important part of network security, but it is not the only security mechanism.