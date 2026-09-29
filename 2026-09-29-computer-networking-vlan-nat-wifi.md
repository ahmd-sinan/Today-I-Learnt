# Computer Networking: VLANs, NAT & Wireless Networking 

**Date:** 2026-09-29

Today I learned about advanced network configuration protocols and the foundational concepts of wireless LANs. I explored how VLANs logically separate physical switches, how NAT bridges private and public IP addresses, and the distinct characteristics of different Wi-Fi frequency bands.

## VLAN (Virtual Local Area Network)
A VLAN, or Virtual Local Area Network, logically separates devices inside a physical switching network. 

*Note: Devices in different VLANs normally require routing to communicate with each other.*

**Example:**
*   VLAN 10 – Staff
*   VLAN 20 – Students
*   VLAN 30 – Servers

**Benefits:**
*   Better security
*   Reduced broadcast traffic
*   Easier network management
*   Logical separation without separate physical switches

## NAT (Network Address Translation)
NAT translates private IP addresses into public IP addresses and vice versa. It allows many private devices to share one single public IPv4 address, which is crucial for preserving the limited supply of IPv4 addresses.

**Example:**
1. Private device: `192.168.1.10`
2. ↓
3. Router performs NAT
4. ↓
5. Public internet

## Wireless Networking (Wi-Fi)
Wi-Fi is a wireless LAN (WLAN) technology. 

**Important Wi-Fi concepts include:**
*   SSID (Service Set Identifier)
*   Access point
*   Wireless channel
*   Frequency band
*   Signal strength
*   Encryption
*   Authentication

### Common Frequency Bands

**2.4 GHz**
*   Longer range
*   Better wall penetration
*   More interference
*   Usually lower speed

**5 GHz**
*   Higher speed
*   More channels
*   Shorter range than 2.4 GHz

**6 GHz**
*   More available spectrum in supported regions
*   Requires compatible devices
*   Useful for newer Wi-Fi technologies