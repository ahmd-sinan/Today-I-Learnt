# Computer Networking: Ethernet Categories, Wiring Standards & Crimping 🔌

**Date:** 2026-09-21


Today I learned the detailed specifications of Ethernet networking. I explored the different performance categories of Ethernet cables, the precise T568B wiring standard for RJ45 connectors, the differences between Straight-through and Crossover cables, and the exact 10-step process for crimping and testing a network cable.

## 1. Ethernet Cable Categories
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
*   The network speed also depends on:
    *   Network cards
    *   Switches
    *   Routers
    *   Internet plan
    *   Cable length
    *   Cable quality
    *   Network standard
*   The usual maximum copper Ethernet channel length is around 100 metres.
*   Long cable runs may require fiber, switches or repeaters.

## 2. RJ45 Connector
An Ethernet cable commonly uses an 8P8C modular connector, often called an RJ45 connector. It has eight contact positions.

The eight wires are arranged in a specific order before inserting them into the connector. Correct arrangement is important because incorrect wiring can cause:
*   No connection
*   Slow connection
*   Unstable connection
*   Incorrect wire mapping
*   Cable tester failure

## 3. T568B Wiring Standard
The T568B color order is:
1. White-orange
2. Orange
3. White-green
4. Blue
5. White-blue
6. Green
7. White-brown
8. Brown

**T568B order (Pin Mapping):**
*   **Pin 1** – White/Orange
*   **Pin 2** – Orange
*   **Pin 3** – White/Green
*   **Pin 4** – Blue
*   **Pin 5** – White/Blue
*   **Pin 6** – Green
*   **Pin 7** – White/Brown
*   **Pin 8** – Brown

*Note:* The order must be the same when viewed with the connector contacts facing the correct direction.

## 4. Ethernet Cable Types

### 4.1 Straight-Through Cable
A straight-through cable uses the same wiring standard on both ends.
*   **Example:** T568B → T568B
*   **Common uses:**
    *   Computer to switch
    *   Computer to router
    *   Printer to switch
    *   Access point to switch

### 4.2 Crossover Cable
A crossover cable uses different wiring standards on each end.
*   **Example:** T568A → T568B
*   **Traditionally connects similar device types, such as:**
    *   Computer to computer
    *   Switch to switch
    *   Router to router
*   *Note:* Modern devices often support auto-MDI/MDIX, which can automatically detect and correct the need for crossover wiring. Because of this, many modern devices can communicate using a normal straight-through cable.

### 4.3 Rollover Cable
A rollover cable uses a reversed pin arrangement.
*   **Mainly used for:**
    *   Console access
    *   Network-device configuration
    *   Router and switch administration
*   *Note:* A rollover cable is not normally used for ordinary Ethernet data communication.

## 5. Ethernet Cable Crimping Process
The general process for making an Ethernet cable is:

*   **Step 1: Cut the cable.** Cut the required length of twisted-pair cable.
*   **Step 2: Remove the outer jacket.** Use a cable stripper to remove a small part of the outer protective coating. Be careful not to damage the internal wires.
*   **Step 3: Separate the wire pairs.** Untwist the four wire pairs carefully. Avoid untwisting more cable than necessary because twisting helps reduce interference.
*   **Step 4: Arrange the wires.** Arrange the eight wires according to the required standard. For T568B: White-orange, Orange, White-green, Blue, White-blue, Green, White-brown, Brown.
*   **Step 5: Straighten the wires.** Make the wires straight and place them side by side in the correct order.
*   **Step 6: Cut the wire ends evenly.** Cut the wires so that all eight ends have the same length.
*   **Step 7: Insert the wires into the RJ45 connector.** Push the wires fully into the connector. Check that:
    *   All eight wires reach the end
    *   The color order is correct
    *   The outer jacket enters the connector
    *   No wire is missing
*   **Step 8: Crimp the connector.** Place the connector inside the crimping tool and press firmly. The metal contacts inside the connector press into the wires.
*   **Step 9: Repeat on the other end.** Make the second end using the required wiring standard.
*   **Step 10: Test the cable.** Use a cable tester to check the wire connections.

## 6. Ethernet Cable Testing
A cable tester checks whether the wires are connected to the correct pins.

A correctly wired straight-through cable usually shows:

`1 → 1`, `2 → 2`, `3 → 3`, `4 → 4`, `5 → 5`, `6 → 6`, `7 → 7`, `8 → 8`

**This means:**
*   Pin 1 on one end connects to pin 1 on the other end
*   Pin 2 connects to pin 2
*   The same continues through pin 8

**Possible tester results:**
*   **Correct cable:** `1 2 3 4 5 6 7 8` matches `1 2 3 4 5 6 7 8`
*   **Open wire:** One wire is not connected.
*   **Short circuit:** Two wires are incorrectly connected together.
*   **Reversed wire:** A wire is connected to the wrong pin.
*   **Missing wire:** One of the eight wires is not properly inserted or crimped.

*Important Note:* A cable tester checks wiring continuity and pin mapping. It does not always prove that the cable can support the maximum advertised speed.