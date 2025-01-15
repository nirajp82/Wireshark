# Wireshark Command/Cheatsheet

This README provides an overview of essential Wireshark commands, filters, keyboard shortcuts, and more, helping you effectively capture, analyze, and troubleshoot network traffic.

---

### Table of Contents

1. [Default Columns in a Packet Capture Output](#default-columns-in-a-packet-capture-output)
2. [Logical Operators](#logical-operators)
3. [Filtering Packets (Display Filters)](#filtering-packets-display-filters)
4. [Filter Types](#filter-types)
5. [Wireshark Capturing Modes](#wireshark-capturing-modes)
6. [Miscellaneous Features](#miscellaneous)
7. [Capture and Display Filter Syntax](#capture-and-display-filter-syntax)
8. [Keyboard Shortcuts - Main Display Window](#keyboard-shortcuts---main-display-window)
9. [Protocols - Values](#protocols---values)
10. [Common Filtering Commands](#common-filtering-commands)
11. [Special Operators](#Special-Operators)
12. [Wireshark Packet Analysis - Filters and Answers](#wireshark-packet-analysis---filters-and-answers)


## Default Columns in a Packet Capture Output

In Wireshark, the packet capture output contains a variety of columns to help you analyze network traffic. Below are the default columns displayed in a packet capture output.

| **Name**        | **Description**                                                                          |
|-----------------|------------------------------------------------------------------------------------------|
| **No.**         | Frame number from the beginning of the packet capture.                                  |
| **Time**        | Seconds from the first frame.                                                           |
| **Source (src)**| Source address, commonly an IPv4, IPv6, or Ethernet address.                           |
| **Destination (dst)**| Destination address.                                                                   |
| **Protocol**    | Protocol used in the Ethernet frame, IP packet, or TCP segment (e.g., TCP, UDP, HTTP).   |
| **Length**      | Length of the frame in bytes.                                                           |

These columns provide basic information about each captured packet. You can customize the columns displayed in Wireshark to suit your specific needs.

---

## Logical Operators

Logical operators in Wireshark display filters allow you to combine multiple conditions in a filter expression. Here are the most commonly used logical operators:

| **Operator**       | **Description**                          | **Example**                                                                                   |
|--------------------|------------------------------------------|-----------------------------------------------------------------------------------------------|
| **and** or **&&**   | **Logical AND**                          | All the conditions should match. Example: `ip.src == 192.168.1.1 and tcp.port == 80`         |
| **or** or **||**    | **Logical OR**                           | Either all or one of the conditions should match. Example: `ip.src == 192.168.1.1 or tcp.port == 443` |
| **xor** or **^^**   | **Logical XOR (Exclusive OR)**           | Exclusive alterations - only one of the two conditions should match, not both. Example: `ip.src == 192.168.1.1 xor ip.dst == 192.168.1.2` |
| **not** or **!**    | **Negation**                             | Inverts the condition. Example: `not ip.addr == 192.168.1.1` (show packets not from this IP) |
| **[n]** or **[...]**| **Substring operator**                   | Filters for specific words or text within the packet. Example: `http contains "GET"`              |

---

## Filtering Packets (Display Filters)

Display filters in Wireshark are used to selectively show packets based on various conditions. Below are common filter operators with examples:

| **Operator**       | **Description**                           | **Example**                                |
|--------------------|-------------------------------------------|--------------------------------------------|
| **eq** or **==**   | **Equal**                                 | `ip.dst == 192.168.1.1`                    |
| **ne** or **!=**   | **Not equal**                             | `ip.dst != 192.168.1.1`                    |
| **gt** or **>**    | **Greater than**                          | `frame.len > 10`                           |
| **lt** or **<**    | **Less than**                             | `frame.len < 10`                           |
| **ge** or **>=**   | **Greater than or equal**                 | `frame.len >= 10`                          |
| **le** or **<=**   | **Less than or equal**                    | `frame.len <= 10`                          |

### Explanation of Filtering Operators

1. **Equal (`eq`, `==`)**:
   - Filters packets that match a specific value.
   - **Example**: 
     ```bash
     ip.dst == 192.168.1.1
     ```
     This filter will show packets where the destination IP address is exactly `192.168.1.1`.

2. **Not Equal (`ne`, `!=`)**:
   - Filters packets that do **not** match a specific value.
   - **Example**:
     ```bash
     ip.dst != 192.168.1.1
     ```
     This filter will exclude packets where the destination IP address is `192.168.1.1`.

3. **Greater Than (`gt`, `>`)**:
   - Filters packets where the value is greater than the specified threshold.
   - **Example**:
     ```bash
     frame.len > 10
     ```
     This filter will show packets where the frame length is greater than 10 bytes.

4. **Less Than (`lt`, `<`)**:
   - Filters packets where the value is less than the specified threshold.
   - **Example**:
     ```bash
     frame.len < 10
     ```
     This filter will show packets where the frame length is less than 10 bytes.

5. **Greater Than or Equal (`ge`, `>=`)**:
   - Filters packets where the value is greater than or equal to the specified threshold.
   - **Example**:
     ```bash
     frame.len >= 10
     ```
     This filter will show packets with frame lengths greater than or equal to 10 bytes.

6. **Less Than or Equal (`le`, `<=`)**:
   - Filters packets where the value is less than or equal to the specified threshold.
   - **Example**:
     ```bash
     frame.len <= 10
     ```
     This filter will show packets with frame lengths less than or equal to 10 bytes.

---

## Filter Types

Wireshark supports two types of filters:

| **Name**            | **Description**                                                           |
|---------------------|---------------------------------------------------------------------------|
| **Capture filter**   | Filters packets during the capture process, reducing the amount of traffic stored in the capture file. |
| **Display filter**   | Filters packets after they have been captured, allowing you to hide unwanted packets from the display. |

---

## Wireshark Capturing Modes

Wireshark can be configured to capture packets in different modes, depending on the network interface.

| **Name**            | **Description**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| **Promiscuous mode**| Configures the network interface to capture all packets on the network segment to which it is connected. |
| **Monitor mode**    | Enables wireless interfaces (only on Unix/Linux systems) to capture all traffic that the interface can receive, including traffic not directed to the capturing device. |

---

## Miscellaneous

| **Name**                | **Description**                                                                 |
|-------------------------|---------------------------------------------------------------------------------|
| **Slice Operator**       | `[...]` - A range of values, e.g., `frame[0:4]` extracts the first 4 bytes. |
| **Membership Operator**  | `{}` - "In" operator to check if a value belongs to a set (e.g., `ip.addr in {192.168.1.1, 192.168.1.2}`). |
| **CTRL+E**               | Start or stop packet capture.                                                  |

---

## Capture and Display Filter Syntax

Understanding the syntax for capture and display filters is crucial for narrowing down packets of interest. Below are examples of filter syntax for both capture and display filters:

### Capture Filter Syntax

Capture filters are used to capture only the packets that meet specific conditions. They are applied before data is captured.

| **Syntax**                     | **Protocol**   | **Direction**   | **Hosts**             | **Value**         | **Logical Operator** | **Expressions**           |
|---------------------------------|----------------|-----------------|-----------------------|-------------------|----------------------|---------------------------|
| **Example**                    | `tcp`          | `src`           | `192.168.1.1`         | `80`              | `and`                | `tcp dst 202.164.30.1`    |

### Display Filter Syntax

Display filters are applied after data is captured, allowing you to refine your packet display.

| **Syntax**                     | **Protocol**   | **String 1**   | **String 2**  | **Comparison Operator** | **Value**         | **Logical Operator** | **Expressions**           |
|---------------------------------|----------------|----------------|---------------|-------------------------|-------------------|----------------------|---------------------------|
| **Example**                    | `http`          | `dest`         | `ip`           | `==`                    | `192.168.1.1`     | `and`                | `tcp port`                |

---

## Keyboard Shortcuts - Main Display Window

Wireshark provides several keyboard shortcuts to navigate efficiently through the packet list and details:

| **Accelerator**        | **Description**                                                                 |
|------------------------|---------------------------------------------------------------------------------|
| **Tab** or **

Shift+Tab** | Move between screen elements (e.g., from the toolbar to the packet list or packet detail). |
| **↓**                   | Move to the next packet or detail item.                                          |
| **↑**                   | Move to the previous packet or detail item.                                      |
| **Ctrl+↓** or **F8**    | Move to the next packet, even if the packet list isn't focused.                 |
| **Ctrl+↑** or **F7**    | Move to the previous packet, even if the packet list isn't focused.             |
| **Ctrl+→**              | In the packet detail, open the selected tree item.                             |
| **Ctrl+←**              | In the packet detail, close all the tree items.                                |
| **Ctrl+.**              | Move to the next packet of the conversation (TCP, UDP, or IP).                 |
| **Ctrl+,**              | Move to the previous packet of the conversation.                              |
| **Return or Enter**     | In the packet detail, toggle the selected tree item.                           |
| **Backspace**           | In the packet detail, jump to the parent node.                                |

---

## Protocols - Values

Wireshark supports a variety of protocols that can be filtered using their protocol names. Here is a list of common protocol values:

---

## Protocols - Values

Ah, I understand now! You want the "Notes on Common Usage" to be incorporated into the existing columns of the table, without adding a new row. I’ve added the relevant notes directly into the appropriate columns, such as **Description**, **When It's Used**, and **Key Features**. Here's the updated table:

---

| **Protocol**     | **Description**                                                                                                                                      | **When It's Used**                                                                                                                                 | **Key Features**                                                                                                  | **Wireshark Display Filter** |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|------------------------------|
| **Ethernet** (`ether`)  | A widely used **Layer 2** protocol for wired networking (LAN communications). It defines how data is framed and transmitted over a network. Typically used for LAN communications.                                | Used in most local area networks (LANs) and data center environments.                                                                             | - Standard for wired networking (homes, businesses).<br>- Uses MAC addresses for device identification.<br>- Supports IPv4 and IPv6.  | `ether`                       |
| **FDDI** (`fddi`)       | A high-speed (100 Mbps) network protocol designed for fiber-optic backbone networks.                                                                | Used in large enterprise networks for connecting routers, switches, and other devices over a high-speed backbone (less common today).              | - Operates on fiber-optic cables.<br>- Uses token-passing ring topology.<br>- Primarily for backbone and MAN networks. | `fddi`                        |
| **Internet Protocol** (`ip`) | A **Layer 3** (Network Layer) protocol responsible for addressing and routing packets across networks.                                                | Used in nearly all IP-based communications over the internet and intranets.                                                                       | - Core protocol for communication over the internet.<br>- Supports IPv4 (32-bit) and IPv6 (128-bit) addressing.<br>- Routes packets between devices. | `ip`                          |
| **Address Resolution Protocol** (`arp`) | Resolves **IP** addresses to **MAC** addresses in Ethernet networks.                                                                               | Used within a local network (LAN) to map 32-bit IP addresses to 48-bit MAC addresses.                                                             | - Resolves IP to MAC addresses.<br>- Operates only within local networks.<br>- Used by devices to communicate within the same network. | `arp`                         |
| **Reverse Address Resolution Protocol** (`rarp`) | Resolves **MAC** addresses to **IP** addresses (reverse of ARP).                                                                                  | Used in legacy systems, typically by diskless workstations, to discover their IP address based on their MAC address.                               | - Reverse of ARP.<br>- Obsolete, replaced by other methods like DHCP. | `rarp`                        |
| **DECnet** (`decnet`)   | A proprietary protocol suite for network communication in **Digital Equipment Corporation** (DEC) systems.                                            | Used in older DEC systems (1970s-1980s) for device communication in proprietary networks.                                                         | - Operated on various physical media.<br>- Largely obsolete, replaced by TCP/IP. | `decnet`                      |
| **LAT (Local Area Transport)** (`lat`) | A protocol for remote terminal access over a **LAN** in DEC systems.                                                                                 | Used in older DEC VAX systems and terminals for remote access.                                                                                      | - Supports terminal-to-host communication.<br>- Obsolete, replaced by modern protocols like SSH and RDP. | `lat`                         |
| **SCA (Serial Control and Alarm)** (`sca`) | Used for managing serial communications and alarms in control systems.                                                                               | Found in industrial and control systems (often legacy systems).                                                                                    | - Used in control systems for managing serial communication.<br>- Found in industrial applications. | `sca`                         |
| **MOP (Maintenance Operations Protocol)** (`moprc`, `mopdl`) | Used for diagnostics and maintenance of DEC systems (network maintenance and software download).                                                      | Primarily used in DEC systems for network management and troubleshooting.                                                                          | - Remote diagnostics and maintenance.<br>- Largely obsolete in modern networks. | `moprc`, `mopdl`              |
| **Transmission Control Protocol** (`tcp`) | A **Layer 4** protocol that ensures reliable, ordered, and error-checked delivery of data between applications (Provides reliability and error-checking).                                        **TCP** is used for reliable communication, such as web browsing, email, and file transfer.                                       | Used for most internet communication (e.g., web browsing, email, file transfer).                                                                  | - Provides reliable, ordered communication.<br>- Ensures error checking and retransmission of lost packets.<br>- Used with IP. | `tcp`                         |
| **User Datagram Protocol** (`udp`) | A **Layer 4** protocol that provides connectionless communication without guaranteed delivery, often used for real-time applications (used for faster, less reliable communication).                 **UDP** is used for faster, less reliable communication, such as video streaming and VoIP.                 | Used in applications where low latency is critical, and some data loss is acceptable (e.g., video streaming, VoIP).                                | - Faster than TCP due to no error-checking or retransmission.<br>- Used in streaming, gaming, and real-time applications.<br>- Does not guarantee delivery. | `udp`                         |
| **Hypertext Transfer Protocol** (`http`) | A protocol for transferring **hypertext** (web page content) between clients and servers.                                                              | Used for web browsing (requesting and serving webpages).                                                                                           | - Forms the foundation of the web.<br>- Uses TCP, typically on port 80.<br>- Transmits web pages, images, etc. | `http`                        |
| **Domain Name System** (`dns`) | A protocol used to resolve **domain names** (e.g., www.example.com) to **IP addresses**.                                                                | Used when clients need to resolve domain names to IP addresses for internet communication (e.g., browsing websites, sending emails).              | - Resolves domain names to IP addresses.<br>- Uses UDP on port 53 for queries.<br>- Critical for internet functionality. | `dns`                         |
| **Secure Sockets Layer / Transport Layer Security** (`ssl`, `tls`) | A protocol for **encrypted communication** over networks, primarily used to secure HTTPS web traffic.                                                | Used in secure communications over the internet (e.g., HTTPS).                                                                                     | - Provides encryption, integrity, and authentication.<br>- Secures data transmission (e.g., web browsing, email).<br>- Often used for secure web (HTTPS) connections. | `ssl`, `tls`                  |
| **Internet Control Message Protocol** (`icmp`) | A **Layer 3** protocol used to send error messages and operational information about network communication ( ICMP (icmp) is used for diagnostic purposes, such as ping and traceroute).                                             | Used for network diagnostics (e.g., ping, traceroute) and error reporting.                                                                        | - Commonly used in tools like **ping** and **traceroute**.<br>- Sends messages like "Destination unreachable" or "Echo request/reply". | `icmp`                        |

---

---

### Summary of Key Points:

- **Ethernet** is the most common Layer 2 protocol for wired networks.
- **FDDI** was historically used for high-speed backbone networks but has mostly been replaced by Ethernet.
- **IP** is the core protocol of the internet and is used to route packets between devices.
- **ARP** is crucial for mapping IP addresses to MAC addresses in a local network.
- **RARP** was once used for diskless workstations to obtain an IP address but is largely obsolete today.
- **DECnet**, **LAT**, **MOP**, and their variants (**MOPRC**, **MOPDL**) were once proprietary protocols used in DEC systems, but they are now outdated and rarely used.
- **TCP** is used for reliable, ordered communications in most applications, while **UDP** is used for fast, low-latency applications where speed is prioritized over reliability.

---

## Common Filtering Commands

| **Usage**                          | **Filter Syntax**                                       | **Description**                                                                                                                                                     |
|------------------------------------|---------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Wireshark Filter by IP**         | `ip.addr == 10.10.50.1`                                 | Matches packets where the **IP address** (source or destination) is **10.10.50.1**. Useful for filtering traffic to or from a specific IP address.               |
| **Filter by Destination IP**       | `ip.dest == 10.10.50.1`                                 | Filters packets where the **destination IP address** is **10.10.50.1**. Ideal for analyzing traffic sent to a particular host.                                    |
| **Filter by Source IP**           | `ip.src == 10.10.50.1`                                  | Filters packets where the **source IP address** is **10.10.50.1**. Useful for tracking the origin of traffic from a specific device.                             |
| **Filter by IP Range**             | `ip.addr >= 10.10.50.1 and ip.addr <= 10.10.50.100`     | Filters packets where the **IP address** falls within the range **10.10.50.1 to 10.10.50.100**. Useful for subnet or range-based traffic analysis.                  |
| **Filter by Multiple IPs**        | `ip.addr == 10.10.50.1 and ip.addr == 10.10.50.100`      | Filters packets where the **IP address** is **either 10.10.50.1 or 10.10.50.100**. Used to capture traffic from multiple specific IP addresses.                     |
| **Filter out an IP address**      | `! (ip.addr == 10.10.50.1)`                             | Excludes packets where the **IP address** is **10.10.50.1**. Useful when you want to ignore traffic from a particular device.                                      |
| **Filter by Subnet**               | `ip.addr == 10.10.50.1/24`                              | Filters packets where the **IP address** belongs to the **10.10.50.1/24 subnet**. Useful for analyzing traffic within a specific subnet or network segment.        |
| **Filter by Port**                 | `tcp.port == 25`                                        | Filters packets where the **TCP port** is **25**. Useful for analyzing traffic related to a specific service, like SMTP (email).                                   |
| **Filter by Destination Port**    | `tcp.dstport == 23`                                     | Filters packets where the **destination TCP port** is **23** (typically used for Telnet). Ideal for capturing Telnet traffic.                                      |
| **Filter by IP address and Port** | `ip.addr == 10.10.50.1 and tcp.port == 25`              | Filters packets where the **IP address** is **10.10.50.1** and the **TCP port** is **25**. Useful for analyzing specific communication from a particular device on a specific port. |
| **Filter by URL**                  | `http.host == "hostname"`                               | Filters HTTP packets where the **host** in the URL matches **hostname**. Useful for capturing traffic related to a specific website or web server.                 |
| **Filter by Timestamp**            | `frame.time >= "June 02, 2019 18:04:00"`                | Filters packets captured **on or after the specified timestamp** (June 02, 2019 18:04:00). Useful for analyzing packets captured during a specific time window.    |
| **Filter by SYN Flag**             | `tcp.flags.syn == 1 and tcp.flags.ack == 0`             | Filters packets where the **SYN flag** is set and the **ACK flag** is not set. Commonly used for capturing **TCP connection  **SYN flag == 1**: Typically used to capture **initial TCP handshake packets** when a initiation** (SYN packets).
| **Wireshark Beacon Filter**        | `wlan.fc.type_subtype == 0x08`                          | Filters **Wi-Fi beacon frames** (subtype 0x08). Useful for analyzing Wi-Fi networks and their beacon frames that advertise network information.                      |
| **Wireshark Broadcast Filter**     | `eth.dst == ff:ff:ff:ff:ff:ff`                           | Filters **broadcast packets** where the **destination MAC address** is set to **ff:ff:ff:ff:ff:ff**. Useful for capturing broadcast traffic on Ethernet networks.    |
| **Wireshark Multicast Filter**     | `(eth.dst[0] & 1)`                                       | Filters **multicast packets**. This checks if the **destination MAC address** starts with an odd number (which indicates multicast traffic).                        |
| **Host Name Filter**               | `ip.host == "hostname"`                                   | Filters packets where the **host** matches **hostname**. Useful for filtering traffic to or from a specific device identified by its hostname.                      |
| **MAC Address Filter**             | `eth.addr == 00:70:f4:23:18:c4`                         | Filters packets where the **MAC address** is **00:70:f4:23:18:c4**. Useful for tracking traffic to or from a specific device on the local network.                 |
| **RST Flag Filter**                | `tcp.flags.reset == 1`                                  | Filters packets where the **TCP RST (reset)** flag is set. Useful for detecting and analyzing connection resets in TCP communication.                             |

# Special Operators

| **Operator**   | **Description**                                                                                                                                   | **Example**                                            |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| **contains**   | Filters packets by checking if a field contains a specific string (case-sensitive). It searches the entire frame, not limited to a protocol.     | `frame contains "Google"`                              |
| **matches**    | Allows filtering using regular expressions (case-insensitive). It’s useful for more flexible string matching, such as domain names or patterns. | ` http.request.uri matches "gl=se$" ` or `frame mathes "Admin" ` or `dns matches "udemy"` |
| **in**         | Checks if a field value is within a specified set or range of values. It is useful for filtering multiple values or ranges in a field.           | `tcp.port in {80, 443, 8000..8004}` `http.request.method in {GET, POST}`       |

## Examples

| **Operator**   | **Example Usage**                                                                                                                                          |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **contains**   | To filter frames containing the string "Google" anywhere in the packet:                                                                                    |
|                | `frame contains "Google"`                                                                                                                                  |
| **matches**    | Match HTTP requests where the last characters in the uri are the characters "gl=se": |
|                | `http.request.uri matches "gl=se$" `                                 |
|                | Matches DNS queries where the domain name ends with `.com`, `.co.uk`, or `.org`:  \\.(com|co.uk|org)$|
|                | ``dns.qry.name matches  "\\.(com|co.uk|org)$"`  `                                 |
| **in**         | To filter packets with TCP ports 80 or 443 or between 8000 to 8004:                                                                                        |
|                |  tcp.port in {80, 443, 8000..8004} |
|                | Matches TLS handshake types 1 (Client Hello) or 2 (Server Hello): |
|                | `tls.handshake.type in {1,2}`                                                                          |

## Notes
| **Operator**   | **Details**                                                                                                      |
|----------------|------------------------------------------------------------------------------------------------------------------|
| **contains**   | Case-sensitive and matches exact strings.                                                                        |
| **matches**    | Case-insensitive and supports regular expressions (Perl-compatible).                                             |
| **in**         | Checks if a field’s value is in a specified list or a defined range.                                             |

## Summary of Operators
| **Operator**   | **Purpose**                                                     |
|----------------|-----------------------------------------------------------------|
| **contains**   | Exact string match, case-sensitive.                             |
| **matches**    | Regular expression match, case-insensitive.                     |
| **in**         | Matches if a value is in a specified list or range.             |

---

### # Wireshark Packet Analysis - Filters and Answers

This document covers various Wireshark packet filters and the corresponding answers to questions related to packet analysis.

## 1. Find DNS Packets (Only DNS, No ICMP)
**Filter**: `dns and !icmp`

This filter will display only DNS packets while excluding any ICMP packets.

---

## 2. DNS Packets Including the Word "Foundation" Regardless of Case
**Filter**: `dns && frame matches "foundation"`

This filter captures DNS packets that contain the word "Foundation" (case insensitive).

---

## 3. DNS Requests for Domains That Do Not Have the Word "Foundation"
**Filter**: `dns && !dns matches "foundation"`

This filter will show DNS requests for domains that do not contain the word "foundation."

---

## 4. ICMP Packets
**Filter**: `icmp`

This simple filter shows all ICMP packets in the capture.

---

## 5. Packets on TCP Port 443
**Filter**: `tcp.port == 443`

This filter will show all packets that use TCP port 443, which is commonly used for HTTPS traffic.

---

## 6. How Many Packets Are in the Top IP Conversation?
**Steps**:
1. Go to **Statistics** menu.
2. Choose **Conversations**.
3. Select the **IPv4** tab.
4. Sort by **Packets**.

This will give you a summary of the top IP conversations sorted by the number of packets exchanged.

---

## 7. What is the Stream ID? Build a Filter for This Stream and Apply It.
To find the stream ID:
1. Go to **Statistics** menu.
2. Choose **Conversations**.
3. Select the **IPv4** tab.
4. Sort by **Packets**. -> Right click -> Apply as filter -> Selected -> Filter on Stream Id
   
---

## 8. In This Stream, How Many Packets Are Larger Than 100 Bytes?
**Filter**: `ip.stream eq 3 && frame.cap_len > 100`

This filter counts packets in stream 3 where the packet size is greater than 100 bytes. Adjust the stream number as necessary.

---

## 9. How Many Packets in the PCAP Have the TCP SYN Bit Set to 1?
**Filter**: `tcp.flags.syn == 1`

### Note:
- **SYN**: The client sends a packet with the SYN flag set to initiate a connection.
- **SYN-ACK**: The server responds with a packet that has both SYN and ACK flags set.
- **ACK**: The client acknowledges the server’s response with a packet containing the ACK flag.

This filter identifies the packets where the SYN flag is set, indicating the initiation of a TCP connection.

---

## 10. How Many Packets Have the TCP Reset Bit Set to 1?
**Filter**: `tcp.flags.reset == 1`

### Note:
When the **RST (Reset)** flag is set, it indicates that the TCP connection is being forcibly reset. This can occur in several scenarios:
- **Connection refused**: If the server doesn’t want to establish a connection (e.g., because the port is closed).
- **Connection termination**: If one side of the connection encounters an error or is in an abnormal state and wants to abruptly terminate the session.
- **Protocol violations**: If there is an attempt to communicate on a connection that is already closed or not open.
- **Unexpected data**: A device receives data for a connection that has not been established.

---

## 11. How Many Packets Have the TCP SYN-ACK Flags Set to 1?
**Filter**: `tcp.flags.syn == 1 && tcp.flags.ack == 1`

This filter captures packets that have both the SYN and ACK flags set, indicating a response from the server during the TCP 3-way handshake (SYN-ACK).

---

Reference: 
https://www.stationx.net/wireshark-cheat-sheet/
https://scadahacker.com/library/Documents/Cheat_Sheets/Networking%20-%20Wireshark%20-%20Display%20Filters%201.pdf
https://www.udemy.com/course/wireshark-ultimate-hands-on-course


 ![image](https://github.com/user-attachments/assets/b498e5fd-c9ae-4a2c-9bc8-d1f6fe542ee3)
