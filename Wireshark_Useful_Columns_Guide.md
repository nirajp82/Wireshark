Your document provides a solid overview of the most useful Wireshark columns, but a few small adjustments and additional details could make it even more comprehensive and accurate. Here's an updated version with minor corrections and some additions to enhance clarity and context.

---

# **Wireshark Most Useful Columns**

This document describes some of the most useful columns in Wireshark and what they represent. Understanding these columns is crucial for effective network analysis.

## **Core Columns**

1. **No. (Packet Number):**  
   The sequential number assigned to each packet captured. This is essential for referencing specific packets and is the only reliable way to restore the original capture order after sorting or filtering.

   * **Example:** `1`, `2`, `3`, etc.

2. **Time:**  
   The timestamp of when the packet was captured. Different formats (relative, delta, absolute) are available.
   - **Relative Time** shows the time since the start of the capture.
   - **Delta Time** shows the time difference between the current packet and the previous displayed packet.
   - **Absolute Time** shows the exact time the packet was captured (typically in UTC or local time).
   
   * **Example:**  
     - Relative: `0.010` (10 milliseconds after the capture started)  
     - Delta: `0.010` (Delta from previous packet)  
     - Absolute: `2024-10-27 12:00:00.000` (Exact time of capture)

3. **Source:**  
   The IP address or MAC address of the device that sent the packet.

   * **Example:** `192.168.1.10`, `00:1A:2B:3C:4D:5E`.

4. **Destination:**  
   The IP address or MAC address of the device that received the packet.

   * **Example:** `192.168.1.20`, `FF:FF:FF:FF:FF:FF`.

5. **Protocol:**  
   The highest-level protocol identified in the packet (e.g., TCP, UDP, HTTP, DNS). This is used to quickly identify the type of communication taking place.

   * **Example:** `TCP`, `UDP`, `HTTP`, `DNS`.

6. **Length:**  
   The total size of the packet in bytes, including the header and payload. This can indicate the size of data transferred in a single packet.

   * **Example:** `1500`, `64`.

7. **Info:**  
   A brief summary of the packet's contents, often protocol-specific. This is a great starting point for understanding what is happening in the packet, including high-level details about the data or action.

   * **Example:**  
     - `HTTP GET /index.html`  
     - `TCP SYN`  
     - `DNS Query A www.example.com`

## **TCP-Specific Columns**

8. **Flags:**  
   Various flags that control TCP connection behavior (e.g., SYN, ACK, FIN, RST, PSH, URG). These flags are essential for understanding TCP handshakes, connection termination, and data flow.

   * **Example:**  
     - `SYN` (initiating a connection)  
     - `ACK` (acknowledgment)  
     - `SYN-ACK` (synchronization and acknowledgment)  
     - `FIN` (connection termination)

9. **Sequence/ACK Number (TCP):**  
   Used to track the order of TCP segments and ensure reliable delivery. The sequence number identifies the data in a segment, and the acknowledgment number confirms receipt of that data. These are important for understanding how data is transmitted and received in a TCP connection.

   * **Example:**  
     - `Seq=100` (Sequence number for the current packet)  
     - `Ack=200` (Acknowledgment number, showing what the receiver expects next)

10. **Window Size (TCP):**  
    Indicates the amount of data the sender is willing to accept before needing an acknowledgment. This is crucial for flow control, ensuring the sender doesn't overwhelm the receiver with too much data.

    * **Example:** `Win=65535` (The receiver's buffer window size in bytes)

## **Timing and Network Characteristics**

11. **Delta Time:**  
    The time elapsed between the current packet and the **previous displayed packet**. This is invaluable for identifying delays and performance bottlenecks. *Note: Filtering or sorting can affect which packet is considered "previous."*

    * **Example:**  
      - `0.010` (10 milliseconds)  
      - `0.500` (500 milliseconds)  

    This is particularly useful for identifying delays between packets in a session, which can help pinpoint issues like high latency or slow processing on the sender or receiver.

12. **TTL (Time-to-Live):**  
    The number of hops a packet can take before being discarded. A decreasing TTL value as packets traverse the network can indicate routing problems or network loops, which can lead to delays.

    * **Example:** `64`, `128` (Typical TTL values).  
    Low TTL values might be worth investigating, especially if they drop unexpectedly.

13. **Absolute Date and Time:**  
    The exact date and time the packet was captured. This is useful for correlating packet captures with other logs, events, or system times for more context.

    * **Example:** `2024-10-27 12:00:00.123` (Exact time of capture in absolute terms).

14. **Cumulative Bytes:**  
    The total number of bytes transferred in a stream (often a TCP stream). This can be useful for tracking the volume of data transferred during a session, especially in long communications or data transfers.

    * **Example:** `1024`, `2048` (Total bytes transferred in a session).

15. **TCP Stream Index:**  
    A unique identifier for each TCP stream, making it easy to follow a specific conversation even if packets are interleaved with other streams. This is useful when you're troubleshooting or analyzing a specific session among multiple connections.

    * **Example:** `Stream 0`, `Stream 1`, `Stream 2`.

## **Example Scenario**

Imagine you're investigating a slow website. You capture network traffic with Wireshark and see a large **Delta Time** value (e.g., 1 second) between an HTTP request packet (Protocol: HTTP, Info: `GET /index.html`) and the corresponding HTTP response packet (Protocol: HTTP, Info: `200 OK`). This large Delta Time suggests a problem with the server, the network, or something in between.

- **Delta Time** of `1 second` between request and response: This could indicate server-side delays or high network latency.
- **TTL** values: If TTL values are unusually low, it could indicate routing issues, like loops or network congestion.
- **TCP Sequence and ACK Numbers**: If there are retransmissions (e.g., gaps in Sequence numbers), it might point to packet loss or network congestion.
- **TCP Stream Index**: Using the stream index, you can isolate the traffic of this particular session to understand if there are any unusual delays or retransmissions.

By examining other columns like TTL, Sequence/ACK numbers, or TCP Stream Index, you can further pinpoint the issue. For instance, a low TTL might suggest a routing loop, or a gap in Sequence/ACK numbers might point to packet loss or retransmissions.

## **Conclusion**

This information should help you get started with analyzing network traffic using Wireshark! Remember to customize your displayed columns as needed for your particular analysis tasks. Additionally, understanding the context and relationships between columns (such as Delta Time, Sequence/ACK Numbers, and Flags) can lead to more accurate insights when troubleshooting network issues.

