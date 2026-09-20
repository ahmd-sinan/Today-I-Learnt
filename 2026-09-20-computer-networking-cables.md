# Computer Networking: Physical Cables & Connectors 🔌

**Date:** 2026-09-20

Today I learned about the physical transmission media used in computer networks from 1 Day Hands-on Workshop. I explored the internal components, use cases, and classifications of Coaxial, Twisted-Pair, and Optical-Fiber cables, along with the critical differences between Ethernet categories and RJ45 connectors.

## Network Cables
Network cables carry data between devices. The main cable types studied are:
1. Coaxial cable
2. Twisted-pair cable
3. Optical-fiber cable

## Coaxial Cable
A coaxial cable contains:
1. Central copper conductor
2. Insulating layer
3. Metallic shield
4. Outer protective jacket

The central conductor carries the signal. The shield helps reduce interference.

**Uses:**
*   Cable television
*   Broadband internet
*   CCTV systems
*   Older computer networks

**Advantages:**
*   Good shielding
*   Strong physical structure
*   Can carry signals over reasonable distances

**Disadvantages:**
*   Thicker than twisted-pair cable
*   More difficult to install
*   Less common in modern Ethernet LANs

## Twisted-Pair Cable
Twisted-pair cable contains pairs of copper wires twisted around each other.

**Twisting helps reduce:**
*   Electromagnetic interference
*   Crosstalk
*   Signal noise

A common Ethernet cable contains four twisted pairs, which means eight individual wires. There are two important types: UTP and STP.

### UTP – Unshielded Twisted Pair
UTP cable has twisted wire pairs but does not have an additional metallic shield around the pairs.

**Advantages:**
*   Less expensive
*   Lightweight
*   Flexible
*   Easy to install
*   Common in homes, schools and offices

**Disadvantages:**
*   More affected by electromagnetic interference
*   Not ideal for areas with heavy electrical noise

### STP – Shielded Twisted Pair
STP cable contains additional shielding to reduce interference. The shielding may be placed around individual wire pairs, around all pairs, or around both individual pairs and the complete cable.

**Advantages:**
*   Better protection against interference
*   Useful near motors, machines and electrical equipment
*   More suitable for noisy environments

**Disadvantages:**
*   More expensive
*   Thicker and less flexible
*   Installation may be more difficult
*   Proper grounding may be required

### UTP vs STP Comparison

| Feature | UTP | STP |
| :--- | :--- | :--- |
| **Full form** | Unshielded Twisted Pair | Shielded Twisted Pair |
| **Shielding** | No additional shield | Has additional shield |
| **Cost** | Lower | Higher |
| **Flexibility** | More flexible | Less flexible |
| **Installation** | Easier | More difficult |
| **Interference protection** | Lower | Higher |
| **Common use** | Homes, offices, labs | Industrial and electrically noisy areas |

## Optical-Fiber Cable
Optical fiber carries data using light instead of electrical signals.

**It contains:**
1. Core
2. Cladding
3. Protective coating
4. Outer jacket

The core carries light. The cladding helps keep the light inside the core through a process called *total internal reflection*.

### Types of Fiber
*   **Single-mode fiber:** Small core, usually used for long distances, common in telecommunications and service-provider networks.
*   **Multimode fiber:** Larger core, usually used for shorter distances, common inside buildings and data centers.

**Advantages:**
*   Very high speed
*   Long transmission distance
*   Resistant to electromagnetic interference
*   More secure against electrical interference
*   Useful for backbone connections

**Disadvantages:**
*   More expensive equipment
*   Installation requires special tools
*   Cable can be damaged if bent too much
*   Repairing fiber requires special skills

## Ethernet Cable Categories
Ethernet cables are classified into categories such as Cat5e, Cat6 and Cat6a. The category indicates the cable’s performance capability.

| Category | Common speed capability | Frequency | Typical maximum Ethernet length |
| :--- | :--- | :--- | :--- |
| **Cat1** | Telephone signals | Very low | Not used for modern Ethernet |
| **Cat2** | Older low-speed networks | Up to 1 MHz | Obsolete |
| **Cat3** | 10 Mbps Ethernet | 16 MHz | Up to 100 m |
| **Cat4** | Older token-ring networks | 20 MHz | Obsolete |
| **Cat5** | Older Ethernet | 100 MHz | Up to 100 m |
| **Cat5e** | Up to 1 Gbps commonly | 100 MHz | Up to 100 m |
| **Cat6** | 1 Gbps commonly; higher speeds over shorter distances | 250 MHz | Up to 100 m depending on speed |
| **Cat6a** | Up to 10 Gbps | 500 MHz | Up to 100 m |
| **Cat7** | Higher shielding and frequency capability | 600 MHz | Depends on standard and installation |
| **Cat8** | High-speed data-center connections | Up to 2000 MHz| Usually up to 30 m for supported high-speed Ethernet |

**Important points:**
*   A higher category does not automatically mean faster internet.
*   The network speed also depends on: Network cards, Switches, Routers, Internet plan, Cable length, Cable quality, and Network standard.
*   The usual maximum copper Ethernet channel length is around 100 metres.
*   Long cable runs may require fiber, switches or repeaters.

## RJ45 Connector
An Ethernet cable commonly uses an 8P8C modular connector, often called an RJ45 connector. It has eight contact positions.

The eight wires are arranged in a specific order before inserting them into the connector. Correct arrangement is important because incorrect wiring can cause:
*   No connection
*   Slow connection
*   Unstable connection
*   Incorrect wire mapping
*   Cable tester failure