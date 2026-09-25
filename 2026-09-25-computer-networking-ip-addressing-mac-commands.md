# Computer Networking: IP Addressing, Subnets, MAC & CLI Tools 

**Date:** 2026-09-25

Today I learned the foundational mechanics of network addressing and troubleshooting. I explored the structure of IPv4, historical classful ranges vs. modern CIDR notation, the difference between physical MAC addresses and logical IP addresses, and how to use basic command-line tools to diagnose network connectivity.

## 1. IP Address
An IP address identifies a network interface at the IP layer. There are two major versions: IPv4 and IPv6.

### IPv4
IPv4 uses 32 bits. It is written as four decimal numbers separated by dots.
*   **Example:** `192.168.1.10`
*   Each part is called an octet and can range from: `0` to `255`

**An IPv4 address contains:**
1.  Network portion (identifies the network)
2.  Host portion (identifies a device inside that network)

## 2. IPv4 Address Classes
Traditional IPv4 classful addressing divides addresses into Classes A, B and C.

*   **Class A:**
    *   First octet range: 1–126
    *   Default subnet mask: 255.0.0.0
    *   Default prefix: /8
    *   *Designed for very large networks.*
*   **Class B:**
    *   First octet range: 128–191
    *   Default subnet mask: 255.255.0.0
    *   Default prefix: /16
    *   *Designed for medium-sized networks.*
*   **Class C:**
    *   First octet range: 192–223
    *   Default subnet mask: 255.255.255.0
    *   Default prefix: /24
    *   *Designed for smaller networks.*
*   **Class D:** Range 224–239. Used for multicast.
*   **Class E:** Range 240–255. Reserved for experimental or special purposes.

*Important note:* Classful addressing is mainly historical. Modern networks use CIDR and subnetting, which provide more flexible address allocation.

## 3. Private IPv4 Address Ranges
Private IP addresses are used inside local networks and are not directly routable across the public internet. Home routers commonly use private IP addresses and perform NAT to communicate with the internet.

**The main private ranges are:**
*   `10.0.0.0/8`
*   `172.16.0.0/12`
*   `192.168.0.0/16`

**Examples:**
*   `10.0.0.5`
*   `172.16.10.20`
*   `192.168.1.25`

## 4. Public and Private IP Addresses
*   **Private IP address:** Used inside a local or private network. (Example: `192.168.1.10`)
*   **Public IP address:** Used to identify a network or device on the public internet. A public IP address must be globally coordinated and routable.

*Important:* A computer can have a private IP address inside the LAN, and simultaneously use a public IP address through the router or cloud service.

## 5. Subnet Mask
A subnet mask tells us which part of an IPv4 address represents the network and which part represents the host.

**Example:**
*   **IP address:** `192.168.1.10`
*   **Subnet mask:** `255.255.255.0`
*   **Prefix:** `/24`

In this example:
*   Network portion: `192.168.1`
*   Host portion: `10`
*   The `/24` means that 24 bits are used for the network portion.

## 6. CIDR Notation
CIDR means Classless Inter-Domain Routing. CIDR writes an IP address with a prefix length.

**Example: `192.168.1.0/24`**
The `/24` means: 24 network bits and 8 host bits.
The number of total addresses is: 2⁸ = 256 addresses.

In a normal IPv4 subnet, two addresses are usually reserved: the Network address and the Broadcast address. Therefore, a `/24` network normally provides: `256 − 2 = 254` usable host addresses.

### Common Examples

| Prefix | Total IPv4 addresses | Usable host addresses in a normal subnet |
| :--- | :--- | :--- |
| **/30** | 4 | 2 |
| **/29** | 8 | 6 |
| **/28** | 16 | 14 |
| **/27** | 32 | 30 |
| **/26** | 64 | 62 |
| **/25** | 128 | 126 |
| **/24** | 256 | 254 |
| **/16** | 65,536 | 65,534 |
| **/8**  | 16,777,216 | 16,777,214 |

*Note:* The exact usable-address rule can differ for special-purpose networks and modern point-to-point configurations.

## 7. Network, Host, and Broadcast Addresses
Consider the network: `192.168.1.0/24`

*   **Network address:** `192.168.1.0` (Identifies the network).
*   **Usable host range:** `192.168.1.1` – `192.168.1.254` (Can be assigned to devices).
*   **Broadcast address:** `192.168.1.255` (Used to send traffic to all devices in the subnet).

## 8. Default Gateway
A default gateway is the device a computer uses to communicate with destinations outside its local network. Usually, the default gateway is the router’s LAN IP address.

**Example:**
*   Computer IP: `192.168.1.10`
*   Subnet mask: `255.255.255.0`
*   Default gateway: `192.168.1.1`

If the computer wants to contact `192.168.1.20`, it may communicate directly because the device is in the same subnet. If it wants to contact a public internet server, it sends the traffic to the default gateway.

## 9. MAC Address
A MAC address is a hardware-level address associated with a network interface. It is commonly written in hexadecimal and mainly used inside local Ethernet networks.
*   **Example:** `00-1A-2B-3C-4D-5E`

### IP vs MAC address

| Feature | IP address | MAC address |
| :--- | :--- | :--- |
| **Main purpose** | Logical network identification | Local hardware/interface identification |
| **Used by** | Routers and IP networks | Ethernet switches and local networks |
| **Can change?** | Yes | Usually fixed by manufacturer, but can be changed or spoofed |
| **Example** | `192.168.1.10` | `00:1A:2B:3C:4D:5E` |

## 10. `ipconfig` Command
On Windows, the `ipconfig` command displays network configuration.
It commonly shows:
*   IPv4 address
*   IPv6 address
*   Subnet mask
*   Default gateway

For more detailed information, run `ipconfig /all`. This can show:
*   MAC address
*   DHCP status
*   DNS servers
*   DHCP server
*   Network adapter details
*   Lease information

## 11. `ping` Command
The `ping` command checks whether another device can be reached through an IP network. Ping normally uses ICMP Echo Request and ICMP Echo Reply messages.
*   **Example:** `ping 192.168.1.20`

A successful response may show the Reply received, Time taken, and TTL value.
*   **Example result:** `Reply from 192.168.1.20: bytes=32 time<1ms TTL=128`

**What ping can help check:**
*   Basic network connectivity
*   Whether an IP address responds
*   Approximate round-trip time
*   Possible packet loss
*   Basic troubleshooting

**What ping cannot prove:**
A successful ping does not prove that:
*   A website is working
*   A specific port is open
*   A service is running
*   The internet is fully working
*   The application is functioning correctly

*Note:* Some devices block ICMP traffic, so a failed ping does not always mean the device is offline.