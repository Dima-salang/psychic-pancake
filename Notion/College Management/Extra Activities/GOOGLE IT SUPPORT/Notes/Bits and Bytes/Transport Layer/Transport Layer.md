  

The transport layer plays a crucial role in computer networking by handling various important functions, including multiplexing and demultiplexing traffic, establishing connections, and ensuring data integrity. Here are key points to understand:

**Multiplexing and Demultiplexing:**

- Multiplexing: Multiplexing in the transport layer allows nodes on a network to send traffic to multiple different receiving services simultaneously. It involves the combining of data from multiple sources into a single stream for transmission.
- Demultiplexing: Demultiplexing, on the other hand, occurs at the receiving end. It involves taking incoming traffic, which may be destined for the same node but different services, and directing it to the appropriate receiving service for processing.

**Ports and Port Numbers:**

- Ports: Ports are 16-bit numbers used to direct network traffic to specific services running on a networked computer. Ports are essential for identifying the target service to which data should be delivered.
- Example: For HTTP traffic (unencrypted web traffic), the standard port is 80. If you want to access a web server running on IP address 10.1.1.100, you would direct traffic to 10.1.1.100:80. This combination of IP address and port is called a socket address or socket number.

  

**Importance of Ports and Multiplexing:**

- Ports are essential for enabling multiple network services to coexist on the same computer or network.
- They allow different services to listen on different ports and respond to requests independently.
- Ports, combined with IP addresses, provide a way to route incoming traffic to the correct service.

  

**Multiplexing:**

Multiplexing, in the context of the transport layer, involves combining data streams from multiple sources into a single, unified stream for transmission over a network. This consolidation of data allows multiple applications or services to share the same network connection simultaneously. Here's how multiplexing works:

1. **Data Sources:** Multiple applications or services generate data that needs to be transmitted over a network. Each source typically has its own data stream.
2. **Multiplexing Layer:** The transport layer of the sending device (usually a computer or network device) is responsible for multiplexing. It takes data from different sources and combines it into a single data stream.
3. **Port Numbers:** Port numbers play a crucial role in multiplexing. Each data stream is associated with a specific port number. Port numbers act as identifiers, allowing the receiving end to demultiplex the data and route it to the correct application or service.
4. **Combination:** The transport layer combines the data streams from different sources into a single stream. It may also add headers or metadata to identify the source and destination ports.
5. **Transmission:** The combined data stream is transmitted over the network to the destination device.

**Demultiplexing:**

Demultiplexing is the reverse process of multiplexing. It occurs at the receiving end, where the combined data stream is separated back into individual data streams and delivered to the appropriate applications or services. Here's how demultiplexing works:

1. **Received Data Stream:** The receiving device receives the combined data stream, which may contain data from multiple sources.
2. **Demultiplexing Layer:** The transport layer of the receiving device is responsible for demultiplexing. It examines the incoming data stream to determine which port number each segment of data corresponds to.
3. **Port Numbers:** Port numbers serve as the key to demultiplexing. They help identify the destination application or service for each segment of data.
4. **Separation:** The transport layer separates the incoming data stream into multiple data streams, each associated with a specific port number.
5. **Delivery:** The separated data streams are delivered to their respective applications or services running on the receiving device.

  

  

## Dissecting a TCP Segment

  

1. **Ethernet Frame and IP Datagram**:
    - An Ethernet frame contains an IP datagram as its payload.
    - The IP datagram, in turn, contains a TCP segment within its payload.
2. **TCP Segment Structure**:
    - A TCP segment comprises two main parts: a TCP header and a data section.
    - The data section is where the application layer places its data.
3. **TCP Header Fields**:
    - **Source Port and Destination Port Fields**:
        - The destination port indicates the service to which the traffic is directed.
        - The source port is a high-numbered port chosen from ephemeral ports, used to differentiate outgoing connections.
    - **Sequence Number**:
        - A 32-bit number used to track where this segment fits in a sequence of TCP segments.
    - **Acknowledgment Number**:
        - Represents the number of the next expected segment.
        - In simple terms, it informs that after this segment (sequence number), expect the next segment.
    - **Data Offset**:
        - A 4-bit field indicating the length of the TCP header.
        - It helps the receiving device locate the start of the actual data payload.
    - **TCP Control Flags**:
        - Six bits reserved for various TCP control flags, including SYN, ACK, FIN, RST, PSH, and URG.
    - **TCP Window**:
        - A 16-bit number specifying the range of sequence numbers that can be sent before an acknowledgment is required.
        - TCP heavily relies on acknowledgments to ensure data receipt.
    - **Checksum**:
        - A 16-bit field used for error-checking.
        - After the recipient ingests the entire segment, the checksum is calculated over the segment and compared to the one in the header to detect data loss or corruption.
    - **Urgent Pointer**:
        - Used in conjunction with one of the TCP control flags to highlight segments of greater importance.
        - Not commonly used in modern networking.
    - **Options Field**:
        - Rarely used in practice but occasionally utilized for complex flow control protocols.
    - **Padding**:
        
        - A sequence of zeros ensuring that the data payload section begins at the expected location.
        
          
        
        ![[/Untitled 36.png|Untitled 36.png]]
        

  

## TCP Control Flags and the Three-way Handshake

  

![[/Untitled 1 21.png|Untitled 1 21.png]]

  

**Understanding TCP Control Flags and Connection Establishment**

In networking, the Transmission Control Protocol (TCP) is responsible for establishing connections used to transmit sequences of data segments. It's crucial for IT support specialists to comprehend how TCP connections are established and maintained, as this knowledge is essential for troubleshooting network issues.

Let's explore the six TCP control flags used in a TCP header, presented in the order they appear:

1. **URG (Urgent)**: This flag, when set to one, indicates that the segment is considered urgent. The urgent pointer field contains additional information about the urgency. However, this feature of TCP has not seen widespread adoption and is not commonly used.
2. **ACK (Acknowledged)**: A value of one in this field indicates that the acknowledgment number field should be examined. It signifies that the receiving device acknowledges the receipt of data.
3. **PSH (Push)**: The PSH flag is used when the transmitting device wants the receiving device to push buffered data to the application on the receiving end as soon as possible. It's used for more efficient delivery of large data chunks and ensures timely responses for small amounts of critical data.
4. **RST (Reset)**: This flag indicates that one side of a TCP connection hasn't been able to recover from missing or malformed segments properly. It essentially requests to start the connection over from scratch.
5. **SYN (Synchronize)**: The SYN flag is used during the initial establishment of a TCP connection. It ensures that the receiving end examines the sequence number field to initiate synchronization.
6. **FIN (Finish)**: When set to one, the FIN flag signifies that the transmitting computer has no more data to send, and it initiates the process of closing the connection.

**TCP Connection Establishment**:  
To understand how TCP connections are established, consider two computers: Computer A (transmitting) and Computer B (receiving). Here's a simplified explanation of the connection establishment process:  

1. Computer A sends a TCP segment to Computer B with the SYN (Synchronize) flag set. This indicates that Computer A wants to establish a connection and provides its sequence number.
2. Computer B responds with a TCP segment containing both the SYN and ACK (Acknowledged) flags set. This signifies that Computer B acknowledges the request to establish a connection and provides its acknowledgment of Computer A's sequence number.
3. Computer A sends another TCP segment with only the ACK flag set, indicating that it acknowledges Computer B's acknowledgment. Now, the connection is established, and both computers can start sending data.

This three-step exchange, involving segments with SYN, SYN/ACK, and ACK flags, is famously known as the "three-way handshake." It ensures that both computers understand each other's protocol and are ready to exchange data. Once established, the TCP connection operates in full duplex mode, allowing data transmission in both directions.

  

When one of the devices is ready to close the connection, a "four-way handshake" occurs. The process involves sending FIN (Finish) flags and acknowledging them. While TCP connections can theoretically stay open in simplex mode with only one side closing the connection, it's not a common scenario in practice.

![[/Untitled 2 16.png|Untitled 2 16.png]]

  

  

  

## TCP Sockets

In networking, a **socket** is an essential concept that represents the endpoint of a potential TCP (Transmission Control Protocol) connection.

- A socket is a fundamental software abstraction that enables network communication between devices or applications over a network, typically using TCP or UDP protocols.

  

> [!important] **Think of a socket as an endpoint in a network, allowing data to be sent to or received from another device or application. It provides a standardized interface for network communication.**

  

1. **Instantiation of an Endpoint**: A socket is the instantiation or implementation of an endpoint defined elsewhere. In simpler terms, think of it as the **actual communication endpoint** in a potential TCP connection. While TCP defines the rules and protocol for communication, sockets are the concrete instances that implement these rules in actual programs.
2. **Contrasting with Ports**: Sockets are distinct from ports. Ports are more like virtual, descriptive labels associated with specific services or processes on a device. Ports help direct network traffic to the appropriate service or program. However, opening a socket on a specific port is what allows a program to listen for and handle incoming network connections on that port.
3. **States of TCP Sockets**: TCP sockets can exist in various states, and understanding these states is crucial for troubleshooting network connectivity issues. Here are some of the most common socket states:
    - **LISTEN**: This state indicates that a TCP socket is ready and listening for incoming connections. It's typically seen on the server side, where a server program is waiting for clients to connect.
    - **SYN_SENT**: This state signifies that a synchronization (SYN) request has been sent by the client, but the connection hasn't been fully established yet. This is observed on the client side during the initial connection setup.
    - **SYN_RECEIVED**: A socket in this state was previously in the LISTEN state and has received a SYN request from a client. It has sent a SYN/ACK response but is waiting for the final ACK from the client. This is seen on the server side.
    - **ESTABLISHED**: In this state, the TCP connection is fully established and operational. Both sides (client and server) are ready to exchange data. This state is common to both the client and server sides during the active connection.
    - **FIN_WAIT**: When a FIN (Finish) signal has been sent, but the corresponding ACK from the other end hasn't been received yet, the socket enters this state.
    - **CLOSE_WAIT**: This state indicates that the TCP connection has been closed at the TCP layer, but the application that opened the socket has not yet released its hold on the socket. This typically occurs when an application needs to perform some cleanup or final data transfer.
    - **CLOSED**: When a socket enters the CLOSED state, the connection has been fully terminated, and no further communication is possible.
4. **Variability Across Operating Systems**: It's essential to note that socket states and their specific names can vary from one operating system to another. This variation exists outside the scope of the TCP protocol itself. TCP, as a protocol, is universal and standardized to ensure interoperability across devices. However, how socket states are described at the operating system level may differ.

  

![[/Untitled 3 15.png|Untitled 3 15.png]]

  

  

## Key Characteristics of Sockets

  

1. **Address:** Sockets are identified by an **IP address and port number combination**. The IP address specifies the target device, and the port number specifies the specific service or process on that device.
2. **Protocol:** Sockets use a specific protocol, such as TCP or UDP, to define how data is transmitted and received. TCP provides reliable, connection-oriented communication, while UDP offers simpler, connectionless communication.

  

## Types of Sockets

1. **Server Socket:** Also known as a **listening socket**, this type of socket is used by server applications to wait for incoming client connections. **It listens on a specific port and accepts incoming connections, creating new sockets for each client.**
2. **Client Socket:** Client applications use client sockets to **initiate connections to server sockets**. They specify the server's IP address and port number to establish a connection.

  

  

## Connection-oriented and Connectionless Protocols

  

**TCP (Transmission Control Protocol):**

- **Connection-Oriented:** TCP establishes a connection between the sender and receiver before data transmission begins. This connection is a virtual circuit that ensures data reliability.
- **Reliable Data Transfer:** TCP guarantees the **reliable delivery of data**. It achieves this **by using sequence numbers, acknowledgments, and retransmission of lost data.**
- **Acknowledgments:** TCP requires the recipient to acknowledge the receipt of each data segment. If the sender doesn't receive an acknowledgment within a certain time, it assumes the segment was lost and retransmits it.
- **Orderly Data:** Data sent over a TCP connection arrives in the same order it was sent, thanks to sequence numbers.
- **Overhead:** As mentioned, TCP introduces overhead due to connection establishment, acknowledgments, and other control mechanisms. While this ensures data integrity, it may impact performance for applications where low latency is critical.

**UDP (User Datagram Protocol):**

- **Connectionless:** UDP does not establish a connection before sending data. Each datagram (packet) sent is independent of the others.
- **Unreliable Data Transfer:** Unlike TCP, UDP does not guarantee the delivery of data. It lacks acknowledgments and retransmission mechanisms.
- **Low Overhead:** UDP has minimal overhead because it doesn't involve connection setup, teardown, or acknowledgments. This makes it suitable for real-time applications where speed matters.
- **No Ordered Delivery:** Data sent over UDP may arrive out of order. Applications using UDP must handle reordering if necessary.
- **Use Cases:** UDP is often used for real-time applications like VoIP (Voice over IP), video streaming, online gaming, and DNS (Domain Name System) queries. In these cases, a small amount of lost or out-of-order data is acceptable as long as the application can adapt.

**When to Use TCP vs. UDP:**

- Use TCP when data integrity and reliability are crucial, such as in file transfers, web browsing, email, and most internet applications.
- Use UDP when low overhead and minimal latency are more important than guaranteed data delivery. UDP is suitable for real-time applications where occasional data loss is acceptable, and responsiveness matters.

  

  

## Types of Ports

1. **Ports and Sockets:** Ports are 16-bit numbers used to direct network traffic to specific services running on a computer. A socket is an active port that a service listens on for incoming data requests.
2. **TCP Segment:** In the Transport Layer of the TCP/IP model, a TCP segment specifies the ports used to establish a network connection. It tells a service to listen for data requests on a specific port.
3. **Three Categories of Ports:** Ports are categorized into three groups:
    - **System Ports (1-1023):** Reserved for common applications and services, such as FTP and Telnet.
    - **User Ports (1024-49151):** Registered by vendors for specific server applications.
    - **Ephemeral Ports (49152-65535):** Used as temporary ports for private transfers; typically, only clients use ephemeral ports.
4. **TCP for Data Integrity:** TCP ensures data integrity by sending acknowledgments between the service and client to confirm that data was received. It also uses checksum verification to check data consistency.
5. **Port Security:** Ports can be used for legitimate data transfer but can also be exploited by malicious actors. Firewalls are essential for securing ports and minimizing security risks.

  

## Firewalls

**Firewalls in Computer Networking:**

1. **Introduction:**
    - A firewall is a critical network device or software program designed to control and manage network traffic based on specific criteria.
    - Its primary purpose is to **enhance network security by allowing or blocking incoming and outgoing traffic based on predefined rules.**
2. **Layers of Operation:**
    - Firewalls can operate at various layers of the network, depending on their specific functionality and purpose.
    - They can be categorized into different types based on the network layer they operate on. The most common layers include:
        - **Application Layer Firewalls:** These firewalls perform deep inspection of application layer traffic. They are capable of understanding and analyzing the content of data packets, making decisions based on application-specific protocols.
        - **Transport Layer Firewalls:** Often used to control traffic based on ports and protocols, they operate at the transport layer (Layer 4) of the OSI model. These firewalls focus on the source and destination ports within data packets.
        - **Network Layer Firewalls:** These firewalls work at the network layer (Layer 3) and can block traffic based on IP addresses or subnets.
3. **Network Security:**
    - Firewalls are a fundamental component of network security, serving as a barrier between the internal network (local area network or LAN) and external networks, such as the Internet.
    - They are a critical defense mechanism to prevent unauthorized access and protect sensitive data.
4. **Use Cases:**
    - **Port-Based Access Control:** One common use of firewalls is to control access to specific ports. For example, in a small business network, a firewall can be configured to allow external access to port 80 for web traffic while blocking access to other ports like those used for file sharing.
    - **Perimeter Protection:** Firewalls are often placed at the perimeter of a network, acting as the first line of defense against incoming threats.
    - **Intrusion Detection/Prevention:** Some firewalls are equipped with intrusion detection and prevention features to identify and block malicious activity.
5. **Deployment:**
    - Firewalls can exist as standalone network devices or as software applications running on servers or individual hosts.
    - For many home users and small businesses, a combined router and firewall device is commonly used. These devices provide routing and firewall capabilities in one package.
    - Modern operating systems often include built-in firewall functionality, allowing users to control traffic at the host level.
6. **Configuration:**
    
    - Firewall rules are configured based on the organization's security policies and requirements.
    - Rules can be defined to allow or deny traffic based on source and destination IP addresses, source and destination ports, and protocol types.
    - Regular updates and monitoring of firewall rules are essential to adapt to evolving security threats.
    
      
    
    ![[/Untitled 4 12.png|Untitled 4 12.png]]
    
      
    
      
    
    ## The Application Layer
    
      
    
    **1. The Role of TCP Segments:**
    
    - At the application layer, communication is facilitated by the exchange of data carried in TCP segments.
    - The payload section of these segments carries the entire content of data that applications wish to transmit to each other.
    
    **2. Diverse Range of Protocols:**
    
    - The application layer encompasses a myriad of protocols, each designed to fulfill specific communication needs.
    - Examples include protocols for web browsing, video streaming, file transfer, and more.
    
    **3. Consistency in Protocol Standardization:**
    
    - While a multitude of application layer protocols exist, they share a common thread: standardization.
    - Standardized protocols ensure that applications, regardless of their type or origin, can effectively communicate with each other.
    
    **4. Web Browsers and Web Servers:**
    
    - Consider the example of web browsers and web servers.
    - Web browsers come in various flavors like Chrome, Safari, and more, while web servers include Microsoft IIS, Apache, and NGINX.
    - The essential requirement is that both clients (browsers) and servers (web servers) adhere to the same protocol to guarantee interoperability.
    
    **5. HTTP: The Protocol for Web Traffic:**
    
    - HTTP (Hypertext Transfer Protocol) is the application layer protocol for web traffic.
    - Regardless of the web browser or server in use, they must comply with the HTTP protocol specification to ensure seamless communication.
    
    **6. Standardization Across Application Types:**
    
    - The standardization concept extends to various application classes.
    - For instance, multiple FTP (File Transfer Protocol) clients may be available, but they all must follow the FTP protocol specification to exchange files.
    
      
    
      
    
    ## OSI Model
    
    **1. Multiple Network Layer Models:**
    
    - The networking domain presents a multitude of network layer models, each with its own structure and layer categorization.
    - As an IT support specialist, you may encounter different models in your career, each tailored to specific needs or educational purposes.
    
    **2. The OSI Model:**
    
    - The OSI model, standing for Open Systems Interconnection, is a well-defined and widely recognized model in the networking field.
    - It comprises seven layers, going beyond the five-layer model we've predominantly explored.
    
    **3. The Session Layer (Layer 5):**
    
    - In the OSI model, the fifth layer is the session layer.
    - Its role is to facilitate communication between actual applications and the transport layer.
    - The session layer acts as the intermediary that receives application layer data, which has undergone decapsulation from lower layers, and then forwards it to the next layer, the presentation layer.
    
    **4. The Presentation Layer (Layer 6):**
    
    - The presentation layer, situated immediately after the session layer in the OSI model, focuses on ensuring that the received application layer data is in a format understandable by the intended application.
    - This layer is responsible for tasks like data encryption, compression, and format conversions.
    
      
    
      
    

**Network Communication from Computer 1 to Computer 2 - A Detailed Overview**

To understand how data travels from Computer 1 to Computer 2, let's break down the entire process, step by step:

1. **Network Setup:**
    - Network A: Address space 10.1.1.0/24
    - Network B: Address space 192.168.1.0/24
    - Network C: Address space 172.16.1.0/24
    - Router A: Interfaces at 10.1.1.1 (Network A) and 192.168.1.254 (Network B)
    - Router B: Interfaces at 192.168.1.1 (Network B) and 172.16.1.1 (Network C)
    - Computer 1 (Client): IP address 10.1.1.100 (Network A)
    - Computer 2 (Server): IP address 172.16.1.100 (Network C), listening on Port 80
2. **Client Request:**
    - Computer 1's web browser requests a webpage from 172.16.1.100:80.
3. **Local Networking Stack:**
    - The web browser communicates with the local networking stack in Computer 1.
4. **Routing Decision:**
    - Computer 1's networking stack determines that 172.16.1.100 is on a remote network.
    - It identifies its gateway as 10.1.1.1.
5. **ARP Request:**
    - Computer 1 checks its ARP table for the MAC address of 10.1.1.1 but finds no entry.
    - Computer 1 sends an ARP request for 10.1.1.1 to all devices on its local network.
6. **Router A's Response:**
    - Router A, the gateway (10.1.1.1), recognizes the request as intended for itself (MAC: 00:11:22:33:44:55).
    - Router A responds to Computer 1 with its MAC address.
7. **Socket and TCP Segment:**
    - Computer 1 opens an outbound TCP connection, using ephemeral port 50,000.
    - It constructs a TCP segment with source port 50,000, destination port 80, and sets the SYN flag.
    - A sequence number is assigned, and a checksum is calculated.
8. **IP Datagram:**
    - The TCP segment is encapsulated into an IP datagram.
    - The IP header includes source and destination IP addresses and a TTL of 64.
    - An IP datagram checksum is calculated.
9. **Ethernet Frame:**
    - An Ethernet frame is created, containing source and destination MAC addresses.
    - The IP datagram is placed as the payload, and a checksum is calculated.
10. **Transmission:**
    - Computer 1 sends the Ethernet frame via a network cable to a switch.
11. **Switch Forwarding:**
    - The switch forwards the frame based on the destination MAC address.
    - The frame reaches Router A.
12. **Router A's Verification:**
    - Router A validates the frame using the checksum.
    - It extracts the IP datagram, validates it, and inspects the destination IP address.
13. **Routing Decision (Router A):**
    - Router A checks its routing table and determines that data should be sent to Router B (192.168.1.1) for the 172.16.1.0/24 network.
14. **New IP Datagram:**
    - A new IP datagram is created with a TTL adjustment, checksum update, and destination IP 192.168.1.1.
15. **New Ethernet Frame:**
    - An Ethernet frame is formed with source and destination MAC addresses for Router A and Router B, respectively.
    - The IP datagram is inserted, and a checksum is calculated.
16. **Transmission to Router B:**
    - The frame is sent across Network B to reach Router B.
17. **Router B's Processing:**
    - Router B validates the frame, extracts the IP datagram, and validates it.
    - It checks the routing table and finds that the destination IP is local (172.16.1.100).
18. **Final Steps (Router B to Computer 2):**
    - A new IP datagram is created, TTL updated, and checksum recalculated.
    - An Ethernet frame is formed with Router B as the source MAC and Computer 2's MAC as the destination.
    - The frame is sent to Network C and reaches Computer 2.
19. **Computer 2's Processing:**
    - Computer 2 verifies the Ethernet frame, IP datagram, and checksum.
    - It recognizes the packet is for itself (172.16.1.100) and processes it.
20. **Server Response:**
    
    - Computer 2's web server generates a response.
    - The response follows a similar journey back to Computer 1, involving TCP handshakes and data transmission.
    
      
    

# Module 3 Glossary

### **New terms and their definitions: Course 2 Module 3**

**ACK flag:** One of the TCP control flags. ACK is short for acknowledge. A value of one in this field means that the acknowledgment number field should be examined

**Acknowledgement number:** The number of the next expected segment in a TCP sequence

**Application layer:** The layer that allows network applications to communicate in a way they understand

**Application layer payload:** The entire contents of whatever data applications want to send to each other

**CLOSE:** A connection state that indicates that the connection has been fully terminated, and that no further communication is possible

**CLOSE_WAIT:** A connection state that indicates that the connection has been closed at the TCP layer, but that the application that opened the socket hasn't released its hold on the socket yet

**Connection-oriented protocol:** A data-transmission protocol that establishes a connection at the transport layer, and uses this to ensure that all data has been properly transmitted

**Connectionless protocol:** A data-transmission protocol that allows data to be exchanged without an established connection at the transport layer. The most common of these is known as UDP, or User Datagram Protocol

**Data offset field:** The number of the next expected segment in a TCP packet/datagram

**Demultiplexing:** Taking traffic that's all aimed at the same node and delivering it to the proper receiving service

**Destination port:** The port of the service the TCP packet is intended for

**ESTABLISHED:** Status indicating that the TCP connection is in working order, and both sides are free to send each other data

**FIN:** One of the TCP control flags. FIN is short for finish. When this flag is set to one, it means the transmitting computer doesn't have any more data to send and the connection can be closed

**FIN_WAIT:** A TCP socket state indicating that a FIN has been sent, but the corresponding ACK from the other end hasn't been received yet

**Firewall:** It is a device that blocks or allows traffic based on established rules

**FTP:** An older method used for transferring files from one computer to another, but you still see it in use today

**Handshake:** A way for two devices to ensure that they're speaking the same protocol and will be able to understand each other

**Instantiation:** The actual implementation of something defined elsewhere

**Listen:** It means that a TCP socket is ready and listening for incoming connections

**Multiplexing:** It means that nodes on the network have the ability to direct traffic toward many different receiving services

**Options field:** It is sometimes used for more complicated flow control protocols

**Port:** It is a 16-bit number that's used to direct traffic to specific services running on a networked computer

**Presentation layer:** It is responsible for making sure that the unencapsulated application layer data is actually able to be understood by the application in question

**PSH flag:** One of the TCP control flags. PSH is short for push. This flag means that the transmitting device wants the receiving device to push currently- buffered data to the application on the receiving end as soon as possible

**RST flag:** One of the TCP control flags. RST is short for reset. This flag means that one of the sides in a TCP connection hasn't been able to properly recover from a series of missing or malformed segments

**Sequence number:** A 32-bit number that's used to keep track of where in a sequence of TCP segments this one is expected to be

**Server or Service:** A program running on a computer waiting to be asked for data

**Session layer:** The network layer responsible for facilitating the communication between actual applications and the transport layer

**Socket:** The instantiation of an endpoint in a potential TCP connection

**Source port:** A high numbered port chosen from a special section of ports known as ephemeral ports

**SYN flag:** One of the TCP flags. SYN stands for synchronize. This flag is used when first establishing a TCP connection and make sure the receiving end knows to examine the sequence number field

**SYN_RECEIVED:** A TCP socket state that means that a socket previously in a listener state, has received a synchronization request and sent a SYN_ACK back

**SYN_SENT:** A TCP socket state that means that a synchronization request has been sent, but the connection hasn't been established yet

**TCP checksum:** A mechanism that makes sure that no data is lost or corrupted during a transfer

**TCP segment:** A payload section of an IP datagram made up of a TCP header and a data section

**TCP window:** The range of sequence numbers that might be sent before an acknowledgement is required

**URG flag:** One of the TCP control flags. URG is short for urgent. A value of one here indicates that the segment is considered urgent and that the urgent pointer field has more data about this

**Urgent pointer field:** A field used in conjunction with one of the TCP control flags to point out particular segments that might be more important than others

### **Terms and their definitions from previous modules**

A

**Address class system:** A system which defines how the global IP address space is split up

**Address Resolution Protocol (ARP):** A protocol used to discover the hardware address of a node with a certain IP address

**ARP table:** A list of IP addresses and the MAC addresses associated with them

**ASN:** Autonomous System Number is a number assigned to an individual autonomous system

B

**Bit:** The smallest representation of data that a computer can understand

**Border Gateway Protocol (BGP):** A protocol by which routers share data with each other

**Broadcast address:** A special destination used by an Ethernet broadcast composed by all Fs

**Broadcast:** A type of Ethernet transmission, sent to every single device on a LAN

C

**Cable categories:** Groups of cables that are made with the same material. Most network cables used today can be split into two categories, copper and fiber

**Cables:** Insulated wires that connect different devices to each other allowing data to be transmitted over them

**Carrier-Sense Multiple Access with Collision Detection (CSMA/CD):** CSMA/CD is used to determine when the communications channels are clear and when the device is free to transmit data

**Client:** A device that receives data from a server

**Collision domain:** A network segment where only one device can communicate at a time

**Computer networking:** The full scope of how computers communicate with each other

**Copper cable categories:** These categories have different physical characteristics like the number of twists in the pair of copper wires. These are defined as names like category (or cat) 5, 5e, or 6, and how quickly data can be sent across them and how resistant they are to outside interference are all related to the way the twisted pairs inside are arranged

**Crosstalk:** Crosstalk is when an electrical pulse on one wire is accidentally detected on another wire

**Cyclical Redundancy Check (CRC):** A mathematical transformation that uses polynomial division to create a number that represents a larger set of data. It is an important concept for data integrity and is used all over computing, not just network transmissions

D

**Data packet:** An all-encompassing term that represents any single set of binary data being sent across a network link

**Datalink layer:** The layer in which the first protocols are introduced. This layer is responsible for defining a common way of interpreting signals, so network devices can communicate

**Demarcate:** To set the boundaries of something

**Demarcation point:** Where one network or system ends and another one begins

**Destination MAC address:** The hardware address of the intended recipient that immediately follows the start frame delimiter

**Destination network:** The column in a routing table that contains a row for each network that the router knows about

**DHCP:** A technology that assigns an IP address automatically to a new device. It is an application layer protocol that automates the configuration process of hosts on a network

**Dotted decimal notation:** A format of using dots to separate numbers in a string, such as in an IP address

**Duplex communication:** A form of communication where information can flow in both directions across a cable

**Dynamic IP address:** An IP address assigned automatically to a new device through a technology known as Dynamic Host Configuration Protocol

E

**Ethernet frame:** A highly structured collection of information presented in a specific order

**Ethernet:** The protocol most widely used to send data across individual links

**EtherType field:** It follows the Source MAC Address in a dataframe. It's 16 bits long and used to describe the protocol of the contents of the frame

**Exterior gateway:** Protocols that are used for the exchange of information between independent autonomous systems

F

**Fiber cable:** Fiber optic cables contain individual optical fibers which are tiny tubes made of glass about the width of a human hair. Unlike copper, which uses electrical voltages, fiber cables use pulses of light to represent the ones and zeros of the underlying data

**Five layer model:** A model used to explain how network devices communicate. This model has five layers that stack on top of each other: Physical, Data Link, Network, Transport, and Application

**Flag field:** It is used to indicate if a datagram is allowed to be fragmented, or to indicate that the datagram has already been fragmented

**Fragmentation offset field:** It contains values used by the receiving end to take all the parts of a fragmented packet and put them back together in the correct order

**Fragmentation:** The process of taking a single IP datagram and splitting it up into several smaller datagrams

**Frame check sequence:** It is a 4-byte or 32-bit number that represents a checksum value for the entire frame

**Full duplex:** The capacity of devices on either side of a networking link to communicate with each other at the exact same time

H

**Half-duplex:** It means that, while communication is possible in each direction, only one device can be communicating at a time

**Header checksum field:** A checksum of the contents of the entire IP datagram header

**Header length field:** A four bit field that declares how long the entire header is. It is almost always 20 bytes in length when dealing with IPv4

**Hexadecimal:** A way to represent numbers using a numerical base of 16

**Hub:** It is a physical layer device that broadcasts data to everything computer connected to it

I

**IANA:** The Internet Assigned Numbers Authority, is a non-profit organization that helps manage things like IP address allocation

**Identification field:** It is a 16-bit number that's used to group messages together

**Interface:** For a router, the port where a router connects to a network. A router gives and receives data through its interfaces. These are also used as part of the routing table

**Interior gateway:** Interior gateway protocols are used by routers to share information within a single autonomous system

**Internet Protocol (IP):** The most common protocol used in the network layer

**Internet Service Provider (ISP):** A company that provides a consumer an internet connection

**Internetwork:** A collection of networks connected together through routers - the most famous of these being the Internet

**IP datagram:** A highly structured series of fields that are strictly defined

**IP options field:** An optional field and is used to set special characteristics for datagrams primarily used for testing purposes

L

**Line coding:** Modulation used for computer networks

**Local Area Network (LAN):** A single network in which multiple devices are connected

M

**MAC(Media Access Control) address:** A globally unique identifier attached to an individual network interface. It's a 48-bit number normally represented by six groupings of two hexadecimal numbers

**Modulation:** A way of varying the voltage of a constant electrical charge moving across a standard copper network cable

**Multicast frame:** If the least significant bit in the first octet of a destination address is set to one, it means you're dealing with a multicast frame. A multicast frame is similarly set to all devices on the local network signal, and it will be accepted or discarded by each device depending on criteria aside from their own hardware MAC address

N

**Network Address Translation (NAT):** A mitigation tool that lets organizations use one public IP address and many private IP addresses within the network

**Network layer:** It's the layer that allows different networks to communicate with each other through devices known as routers. It is responsible for getting data delivered across a collection of networks

**Network port:** The physical connector to be able to connect a device to the network. This may be attached directly to a device on a computer network, or could also be located on a wall or on a patch panel

**Network switch:** It is a level 2 or data link device that can connect to many devices so they can communicate. It can inspect the contents of the Ethernet protocol data being sent around the network, determine which system the data is intended for and then only send that data to that one system

**Next hop:** The IP address of the next router that should receive data intended for the destination networking question or this could just state the network is directly connected and that there aren't any additional hops needed. Defined as part of the routing table

**Node:** Any device connected to a network. On most networks, each node will typically act as a server or a client

**Non-routable address space:** They are ranges of IPs set aside for use by anyone that cannot be routed to

O

**Octet:** Any number that can be represented by 8 bits

**Organizationally Unique Identifier (OUI):** The first three octets of a MAC address

**OSI model:** A model used to define how network devices communicate. This model has seven layers that stack on top of each other: Physical, Data Link, Network, Transport, Session, Presentation, and Application

P

**Padding field:** A series of zeros used to ensure the header is the correct total size

**Patch panel:** A device containing many physical network ports

**Payload:** The actual data being transported, which is everything that isn't a header

**Physical layer:** It represents the physical devices that interconnect computers

**Preamble:** The first part of an Ethernet frame, it is 8 bytes or 64 bits long and can itself be split into two sections

**Protocol field:** A protocol field is an 8-bit field that contains data about what transport layer protocol is being used

**Protocol:** A defined set of standards that computers must follow in order to communicate properly is called a protocol

R

**Router:** A device that knows how to forward data between independent networks

**Routing protocols:** Special protocols the routers use to speak to each other in order to share what information they might have

S

**Server:** A device that provides data to another device that is requesting that data, also known as a client

**Service type field:** A eight bit field that can be used to specify details about quality of service or QoS technologies

**Simplex communication:** A form of data communication that only goes in one direction across a cable

**Source MAC address:** The hardware address of the device that sent the ethernet frame or data packet. In the data packet it follows the destination MAC address

**Start Frame Delimiter (SFD):** The last byte in the preamble, that signals to a receiving device that the preamble is over and that the actual frame contents will now follow

**Static IP address:** An IP address that must be manually configured on a node

**Subnet mask:** 32-bit numbers that are normally written as four octets of decimal numbers

**Subnetting:** The process of taking a large network and splitting it up into many individual smaller sub networks or subnets

T

**Time-To-Live field (TTL):** An 8-bit field that indicates how many router hops a datagram can traverse before it's thrown away

**Total hops:** The total number of devices data passes through to get from its source to its destination. Routers try to choose the shortest path, so fewest hops possible. The routing table is used to keep track of this

**Total length field:** A 16-bit field that indicates the total length of the IP datagram it's attached to

**Transmission Control Protocol (TCP):** The data transfer protocol most commonly used in the fourth layer. This protocol requires an established connection between the client and server

**Transport layer:** The network layer that sorts out which client and server programs are supposed to get the data

**Twisted pair cable:** The most common type of cabling used for connecting computing devices. It features pairs of copper wires that are twisted together

U

**Unicast transmission:** A unicast transmission is always meant for just one receiving address

**User Datagram Protocol (UDP):** A transfer protocol that does not rely on connections. This protocol does not support the concept of an acknowledgement. With UDP, you just set a destination port and send the data packet

V

**Virtual LAN (VLAN):** It is a technique that lets you have multiple logical LANs operating on the same physical equipment

**VLAN header:** A piece of data that indicates what the frame itself is. In a data packet it is followed by the EtherType