  

**The Importance of Computer Networking**

- Computers communicate with each other much like humans do, following specific rules or protocols.
- Networking ensures that computers can understand each other's communication protocols, and messages are reliably delivered.
- Human communication parallels: language comprehension, clarity, addressing, and rules in communication.
- Computers follow a defined set of standards and protocols for effective communication.

**Network Models**

- Computer networking involves multiple layers, and models help understand this complexity.
- Two primary models discussed in the course: TCP/IP (five-layer) and OSI (seven-layer).
- TCP/IP model used in this course provides a practical framework for understanding networking.

  

“Please Do Not Throw Ants”

  

![[/Untitled 34.png|Untitled 34.png]]

![[/Untitled 1 19.png|Untitled 1 19.png]]

  

- **Description**: This layer involves the actual hardware components that make up the physical network, such as cables, switches, and network interface cards (NICs).
- **Responsibilities**: It defines the physical connections, the type of cables to use (e.g., Ethernet, fiber optics), and the electrical or optical characteristics of the signals that flow through them.
- **Key Concepts**: Signal modulation, cable types (e.g., Ethernet cables, fiber-optic cables), connectors (e.g., RJ45 connectors), physical topologies (e.g., star, bus, ring), and transmission media (e.g., copper, fiber).

**2. Data Link Layer**:

- **Description**: This layer deals with addressing and error detection within the local network.
- **Responsibilities**: It ensures that data frames are delivered to the correct device on the local network segment. It also manages flow control and error detection.
- **Key Concepts**: MAC addresses (Media Access Control), Ethernet frames, switching, Ethernet protocols (e.g., Ethernet II, IEEE 802.3), and LAN technologies (e.g., Ethernet, Wi-Fi).

**3. Network Layer**:

- **Description**: The network layer focuses on routing data packets across different networks or subnets.
- **Responsibilities**: It determines the optimal path for data to travel from the source to the destination, using logical addressing (e.g., IP addresses) and routing tables.
- **Key Concepts**: IP addresses (IPv4 and IPv6), routing protocols (e.g., OSPF, BGP), routers, subnets, and logical addressing.

**4. Transport Layer**:

- **Description**: This layer ensures end-to-end communication and data delivery reliability.
- **Responsibilities**: It manages data segmentation, reassembly, error correction, and flow control. It also determines whether to use reliable (TCP) or unreliable (UDP) transport protocols.
- **Key Concepts**: TCP (Transmission Control Protocol), UDP (User Datagram Protocol), ports (e.g., TCP/UDP port numbers), segments, and connection establishment/termination.

**5. Application Layer**:

- **Description**: The top layer interacts directly with end-user applications.
- **Responsibilities**: It hosts various application-specific protocols and services, enabling communication between software applications running on different devices.
- **Key Concepts**: HTTP, FTP, SMTP, POP3, IMAP, DNS, DHCP, application-specific data formats, and user interfaces.

  

**Networking Cables**:

- **Copper Cables**:
    - **Description**: Copper cables are commonly used in networking. They consist of multiple pairs of copper wires enclosed in plastic insulation.
    - **Binary Communication**: Computers communicate using binary data (1s and 0s), and copper wires transmit this data by varying voltage between two ranges. The receiving device interprets these voltage changes as binary information.
    - **Common Types**:
        - **Cat5, Cat5e, and Cat6 Cables**: These are shorthand for Category 5, Category 5e, and Category 6 cables. They differ in terms of the number of twists in the copper wire pairs, which affects usable lengths and transfer rates.
    - **Differences**: The internal arrangement of twisted pairs significantly influences data transmission speed and resistance to interference. Cat5e and Cat6 cables are preferred over Cat5 due to reduced Crosstalk, which can cause network errors. Cat6 cables have even stricter specifications to minimize Crosstalk, enabling faster and more reliable data transfer.

![[/Untitled 2 14.png|Untitled 2 14.png]]

- **Fiber Optic Cables**:
    - **Description**: Fiber optic cables consist of individual optical fibers made of glass, each about the width of a human hair. These fibers transport data using pulses of light.
    - **Advantages**:
        - **High Speed**: Fiber cables can transmit data faster than copper.
        - **Immunity to Electromagnetic Interference**: Fiber is often used in environments with electromagnetic interference, as it's unaffected by outside sources.
        - **Long-Distance**: Fiber can transmit data over much longer distances than copper without data loss.
    - **Disadvantages**:
        - **Cost**: Fiber cables are more expensive than copper cables.
        - **Fragility**: Fiber is delicate and can be damaged easily.
    - **Use Cases**: Fiber cables are commonly found in data centers and environments where high-speed, long-distance, and interference-free data transmission is crucial.

![[/Untitled 3 13.png|Untitled 3 13.png]]

**Networking Devices**:

- **Router**:
    - **Description**: Routers are essential devices that connect different networks and facilitate data traffic between them. They determine the optimal path for data to reach its destination across various networks.
- **Switch**:
    - **Description**: Switches are used to create local area networks (LANs) by connecting multiple devices within the same network. They operate at the data link layer and use MAC addresses to forward data to the correct device.
- **Modem**:
    - **Description**: A modem (short for modulator-demodulator) converts digital data from computers into analog signals for transmission over analog communication lines like telephone lines or cable systems. It also demodulates incoming analog signals back into digital data.
- **Access Point (AP)**:
    - **Description**: Access points are devices that allow wireless clients to connect to a wired network. They're commonly used in Wi-Fi networks to extend network coverage.
- **Hub**:
    
    - **Description**: Hubs are older networking devices that work at the physical layer and simply broadcast incoming data to all connected devices. They're less efficient than switches and are rarely used today.
    
      
    
      
    
    **Cables and Point-to-Point Networking**:
    
    - Cables are used to create point-to-point networking connections where only two devices are connected directly.
    - While point-to-point connections are useful, they become impractical in a world with billions of computers.
    
    **Hub**:
    
    - A hub is a basic network device that operates at the physical layer (Layer 1) of the OSI model.
    - It allows multiple computers to connect simultaneously, creating a shared network segment.
    - All devices connected to a hub receive all transmitted data simultaneously.
    - Each device must determine if incoming data is intended for them or ignore it if it's not.
    - Hubs create a "collision domain," where only one device can transmit data at a time.
    - If multiple devices attempt to transmit simultaneously, electrical pulses can interfere, leading to data collisions.
    - Collisions cause devices to wait for a quiet period before attempting retransmission.
    - Hubs are less common today due to their limitations and network noise.
    
    **Network Switch**:
    
    - A network switch is a more advanced device used to connect multiple computers.
    - It operates at the data link layer (Layer 2) of the OSI model.
    - Like a hub, it allows numerous devices to connect, but it's more intelligent in handling data.
    - Unlike hubs, switches can inspect the content of Ethernet protocol data.
    - Switches determine the intended recipient of data and forward it only to the relevant device.
    - This reduces or eliminates collision domains in the network.
    - With fewer retransmissions, switches lead to higher network throughput and efficiency.
    
    ![[/Untitled 4 10.png|Untitled 4 10.png]]
    
      
    
      
    
    **Routers** are crucial devices for connecting computers across different networks, enabling data transmission between them. While hubs and switches are used for local area networks (LANs), routers are essential for routing data between independent networks.
    
    - A router operates at **Layer 3**, the network layer of the OSI model.
    - It can inspect **IP data** to determine where to route it, just as switches analyze Ethernet data.
    - Routers maintain internal tables with information on how to route traffic across various networks worldwide.
    
    **Types of Routers**:
    
    1. **Home or Small Office Routers**: These routers are commonly found in homes or small offices. They have relatively simple routing tables and are responsible for forwarding traffic from the local network (LAN) to the Internet via the Internet Service Provider (ISP).
    2. **Core ISP Routers**: Core routers serve as the backbone of the Internet. They handle vast amounts of traffic and make complex routing decisions. Core routers have multiple connections to other routers and exchange data using the **Border Gateway Protocol (BGP)**. BGP helps routers learn optimal paths for forwarding traffic.
    
    ![[/Untitled 5 10.png|Untitled 5 10.png]]
    
      
    
      
    
      
    
    In networking, it's essential to understand the roles of **servers** and **clients** in facilitating communication between devices. While we often refer to these devices as nodes, they can also be individual computer programs running on the same node, serving as both servers and clients as needed.
    
    Here are the key concepts:
    
    - **Server**: Think of a server as something that provides data in response to requests. Servers serve data to clients, fulfilling their data requests. Servers can be dedicated hardware or software applications running on a node. Examples include email servers, web servers, and file servers.
    - **Client**: A client is a device or program that requests data from a server. Clients initiate data retrieval or interaction with servers to obtain the information they need. Clients can be dedicated hardware devices like laptops, smartphones, or software applications like web browsers or email clients.
    - **Hybrid Roles**: While nodes can act as both servers and clients, most devices primarily serve one role within a network topology. For example, a web server primarily serves web pages to clients (web browsers), but it may also function as a client when connecting to a database server to retrieve data for those web pages.
    
      
    
      
    
    Physical layer
    
      
    
    1. **Bit Transmission**: At its core, the physical layer is all about transmitting bits of data. In computing, a bit is the smallest unit of information, represented as either a one or a zero. These binary digits are the building blocks of all data processed and transmitted by computers.
    2. **Frames and Packets**: The ones and zeros transmitted at the physical layer form the basis of larger data structures, such as frames and packets, which are essential for data communication across networks. Frames and packets contain not only the data but also control information for routing and error checking.
    3. **Network Cables**: Network cables, such as Ethernet cables, serve as the physical medium for transmitting data between devices. Once connected at both ends, these cables carry a constant electrical charge, facilitating data transmission.
    4. **Modulation and Line Coding**: Data transmission over network cables involves a process called modulation, specifically known as line coding in the context of computer networks. Modulation varies the voltage of the electrical charge moving across the cable. Different voltage states represent binary ones and zeros, allowing devices at each end of the link to interpret the data accurately.
    
      
    
      
    
    **Twisted pair cables**, commonly used for connecting computing devices, are a fundamental component of networking. These cables derive their name from the pairs of copper wires within them, which are twisted together. Twisted pairs are renowned for their ability to serve as a reliable conduit for information, and their twisted design serves various important functions in network communication:
    
    1. **Electromagnetic Interference (EMI) Protection**: The twisting of wire pairs helps safeguard against electromagnetic interference, a phenomenon where external electromagnetic fields disrupt the transmission of data. By twisting the wires, EMI is less likely to affect the signals traveling through them.
    2. **Cross-Talk Mitigation**: Cross-talk occurs when signals from one pair of wires interfere with signals from neighboring pairs. Twisting the pairs helps minimize cross-talk, ensuring that each pair's data remains intact and distinct from others.
    
    A standard **Cat6 cable** typically contains eight wires arranged as four twisted pairs within a single protective jacket. The number of pairs used depends on the specific networking technology employed, but it's essential to recognize that these cables support **duplex communication**.
    
    **Duplex communication** means that information can flow in both directions across the cable simultaneously. To facilitate duplex communication, networking cables reserve one or two pairs for communication in one direction while using the other one or two pairs for communication in the opposite direction. Devices at each end of the cable can transmit and receive data simultaneously, a configuration known as **full duplex**.
    
    However, if there's an issue with the connection or a device's capabilities, it might operate in **half-duplex** mode. In a half-duplex configuration, communication is still possible in both directions, but only one device can transmit data at any given time, leading to a slower and less efficient form of communication.
    
      
    
      
    
    **1. Unshielded Twisted Pair (UTP):**
    
    - **Common Use:** UTP is the most widespread and cost-effective Ethernet cable type, commonly used in both business and home networks.
    - **Protection:** Offers basic protection against Electromagnetic Interference (EMI), Radio Frequency Interference (RFI), and crosstalk interference.
    
    **2. Shielded Twisted Pair (STP):**
    
    - **Usage:** Employed in environments where EMI, RFI, and crosstalk have been identified as challenges for network communication.
    - **Shielding:** STP cables include a shielding layer made of braided aluminum and/or copper, surrounding the four twisted pairs beneath the outer jacket.
    
    **3. Foiled Twisted Pair (FTP):**
    
    - **Application:** Used in settings with EMI, RFI, or crosstalk concerns, similar to STP.
    - **Shielding Type:** FTP cables utilize a thin foil shield that wraps around the bundle of twisted pairs beneath the outer jacket.
    
    The terms "STP" and "FTP" are sometimes used interchangeably to describe shielded or foiled cables. Some cables even incorporate both braided and foiled shields for enhanced interference protection. Manufacturers specify the interference-reduction method used in their cables, so it's crucial to check product descriptions.
    
      
    
      
    
    **Straight-Through Cable (Patch Cable):**
    
    Straight-through cables, also known as patch cables, are the predominant Ethernet cable type used in computer networks. They serve various purposes, including connecting computers and routers to hubs, Ethernet switches, or even servers to Ethernet switches. Key characteristics of straight-through cables:
    
    - **Identification:** A straight-through cable is recognized by comparing both ends, where the color and stripe order of the twisted pairs are consistent.
    - **Color Coding (For 100Base-T networks):**
        - Computers and routers use:
            - Pins 1 & 2: Orange wires for sending data.
            - Pins 3 & 6: Green wires for receiving data.
        - Hubs and switches use:
            
            - Pins 1 & 2: Green wires for sending data.
            - Pins 3 & 6: Orange wires for receiving data.
            
            ![[/Untitled 6 9.png|Untitled 6 9.png]]
            
              
            
    
      
    
    Cross-over Cables are more used in older environments
    
      
    
      
    
      
    
    **Termination with RJ-45 Plugs:**
    
    - Twisted pair network cables are typically terminated with plugs that expose the individual internal wires.
    - The most prevalent plug specification in computer networking is the RJ-45 (Registered Jack 45). It is widely used for connecting network cables to network ports.
    
    **Network Ports:**
    
    - Network ports are interfaces attached directly to devices within a computer network.
    - Switches often have numerous network ports since their role is to connect multiple devices.
    - In contrast, servers and desktop computers usually possess only one or two network ports.
    - Devices like laptops, tablets, and phones may not have any dedicated network ports.
    - Most network ports feature two small LEDs: a link light and an activity light.
        - **Link Light:** Illuminates when a cable is properly connected between two powered-on devices.
        - **Activity Light:** Flashes to indicate active data transmission across the cable.
    - In contemporary high-speed networks, activity lights no longer directly correlate with the actual ones and zeros being sent due to network speed.
    - Switches may employ a single LED for both link and activity status or even include indicators for link speed, depending on the hardware.
    - Troubleshooting information about port status is often accessible through these port lights.
    
    **Wall-Mounted Network Ports and Patch Panels:**
    
    - Some network ports are installed in walls or beneath desks instead of directly on devices.
    - These ports are typically connected to the network through cables running within walls, ultimately terminating at a patch panel.
    - A patch panel is a device equipped with multiple network ports that serves as a central location for cable endpoints.
    - Patch panels do not perform any data processing; they function solely as connectors for cable runs.
    - Additional cables extend from the patch panel to switches or routers, enabling network access for the connected computers at the far end.

  

![[/Untitled 7 6.png|Untitled 7 6.png]]

  

## Ethernet

- Traditional cable networks are still common in workplaces and data centers.
- Wireless and cellular internet access are prevalent for personal use.

**2. Ethernet and the Data Link Layer:**

- Ethernet is a widely used protocol for sending data over individual network links.
- The Data Link Layer abstracts away hardware details, allowing higher-level software to send and receive data consistently, regardless of the underlying physical connection.

**3. MAC Addresses:**

- MAC (Media Access Control) addresses are used to identify individual network interfaces on computers and devices.
- They consist of a 48-bit number represented by six groups of two hexadecimal digits.
- Hexadecimal uses the numbers 0-9 and letters A-F to represent values 0-15.
- MAC addresses are globally unique due to the large number of possible combinations (2^48).
- The first three octets of a MAC address are the Organizationally Unique Identifier (OUI), assigned to hardware manufacturers by the IEEE.
- The last three octets can be assigned by the manufacturer as long as they remain unique.

**4. Ethernet Frame and Data Transmission:**

- An Ethernet frame is the basic unit of data transmission in Ethernet networks.
- Ethernet frames include source and destination MAC addresses.
- Ethernet frames can be sent in three ways: Unicast (to a specific device), Multicast (to multiple devices), or Broadcast (to all devices on a network segment).
- Ethernet frames also include a cyclic redundancy check (CRC) to ensure data integrity.

  

> [!important] Basically, we use MAC addresses in a LAN while IP for connecting to other networks.

  

**Local Network Communication (Same LAN):**

- When devices communicate within the same local area network (LAN), they typically use **ARP (Address Resolution Protocol)** to discover each other's MAC addresses.
- ARP is responsible for mapping an IP address to the corresponding MAC address within the same network segment.
- Here's how it works:
    
    1. A device (let's call it Device A) wants to communicate with another device (Device B) on the same LAN but only knows Device B's IP address.
    2. Device A sends out an ARP broadcast message to the entire local network, asking, "Who has IP address X?"
    3. Device B, recognizing that the ARP request matches its IP address, responds with its MAC address.
    4. Device A now knows Device B's MAC address and can send Ethernet frames directly to it.
    
      
    

**Remote Network Communication (Different LANs):**

- When devices need to communicate across different LANs or networks, they use routers.
- In this case, the sender device knows the recipient's IP address but not the MAC address.
- The router receives the frame, examines the IP destination address, and performs a routing decision.
- The sender encapsulates the data in an Ethernet frame but sets the destination MAC address of the router (gateway) that connects the sender's LAN to the wider network, like the internet.
- If the router determines that the destination is on a different network segment, it forwards the frame to the appropriate interface, replacing the MAC address as it goes.
- This process repeats at each router until the frame reaches the final destination's network segment.
- Finally, the recipient device recognizes that the frame contains its IP address and accepts the data payload.

  

## Unicast, Multicast, and Broadcasting

  

**1. Unicast Transmission:**

- Unicast transmission involves sending data from one sender to one specific receiver.
- In Ethernet, unicast frames have a destination MAC address with the least significant bit in the first octet set to zero.
- Unicast frames are sent to all devices within the same collision domain but are processed only by the device with the matching destination MAC address.

**2. Multicast Transmission:**

- Multicast transmission involves sending data from one sender to multiple specific receivers.
- In Ethernet, multicast frames have a destination MAC address with the least significant bit in the first octet set to one.
- Multicast frames are sent to all devices within the same network segment but are accepted or discarded by individual devices based on configured multicast address lists.

**3. Broadcast Transmission:**

- Broadcast transmission involves sending data from one sender to all devices on a local area network (LAN).
- In Ethernet, broadcast frames use a special destination MAC address composed of all F's (FF:FF:FF:FF:FF:FF).
- Ethernet broadcasts are received by every device within the same LAN.

**4. Purpose of Broadcasts:**

- Broadcasts are typically used to allow devices on a LAN to learn more about each other and discover network-related information.
- They can be used for purposes such as address resolution (ARP) and DHCP (Dynamic Host Configuration Protocol) requests.

  

  

## Ethernet Dissection

  

![[/Untitled 8 5.png|Untitled 8 5.png]]

**1. Preamble:**

- An 8-byte (64-bit) sequence used for synchronization and clock alignment.
- The first 7 bytes alternate between ones and zeros.
- The last byte is the Start Frame Delimiter (SFD) that signals the end of the preamble.

**2. Destination MAC Address:**

- A 48-bit (6-byte) address representing the intended recipient's hardware address.

**3. Source MAC Address:**

- A 48-bit (6-byte) address representing the origin of the frame.

**4. Ether-type Field:**

- A 16-bit (2-byte) field indicating the protocol of the frame's contents.
- Alternatively, it may contain a VLAN header if the frame is part of a Virtual LAN (VLAN).

**5. Data Payload:**

- Contains the actual data to be transmitted, including information from higher layers (e.g., IP, transport, application layers).
- The size of the payload can vary from 46 to 1500 bytes.

**6. Frame Check Sequence (FCS):**

- A 32-bit (4-byte) value representing a checksum computed using a cyclic redundancy check (CRC).
- Used to verify the integrity of the received data.
- If the FCS at the receiver's end doesn't match the computed CRC, the data is considered corrupted and discarded.

  

  

**Frame Check Sequence (FCS):**  
The FCS is a crucial part of an Ethernet frame, and its primary purpose is to ensure the integrity of the transmitted data. In essence, it's a checksum, or a value computed from the entire frame's data. When the frame is received, the FCS at the receiving end is recalculated, and if it matches the FCS value in the frame, the data is considered to be error-free and intact. However, if the recalculated FCS doesn't match the FCS in the frame, it signifies that the data has been corrupted during transmission, and the frame is discarded.  

**Cyclic Redundancy Check (CRC):**  
The CRC is the specific algorithm used to calculate the FCS. It's a mathematical process that involves polynomial division, which results in a fixed-size value (the CRC value) that represents the entire frame's contents. Here's how it works:  

1. **Polynomial Division:** The data in the frame is treated as a polynomial, with each bit representing a coefficient of the polynomial.
2. **Division:** The polynomial is divided by another fixed polynomial known as the "divisor polynomial." The result is the CRC value.
3. **Appending the CRC:** The calculated CRC value is appended to the end of the frame as the FCS.
4. **Verification:** When the frame is received, the recipient performs the same CRC calculation on the received data, including the FCS. If the calculated CRC matches the FCS in the frame, the data is considered valid; otherwise, it's deemed corrupt.

  

The receiving device doesn't need to know the precise formula used by the sender to compute the checksum (CRC). Instead, both the sending and receiving devices follow a standardized CRC algorithm that is well-defined in networking protocols.

Here's how it works:

1. **Standardized Algorithm:** In Ethernet and other networking protocols, the CRC algorithm is standardized. All devices that communicate over the network, whether they are sending or receiving data, use the same CRC algorithm. This standardized algorithm ensures compatibility across different network devices and vendors.
2. **Predefined Parameters:** The CRC algorithm is defined by a set of parameters, including the polynomial used for division and the initial conditions for the algorithm. These parameters are part of the Ethernet standard and are known to all networking equipment manufacturers.
3. **CRC Generator:** When a device sends data, it calculates the CRC value for the entire frame using the standardized algorithm and parameters. This CRC value is appended to the end of the frame as the Frame Check Sequence (FCS).
4. **Receiving End:** When a device receives a frame, it extracts the CRC (FCS) value from the received frame along with the data.
5. **CRC Calculation:** The receiving device performs the same CRC calculation using the same standardized algorithm and parameters. It treats the received data and the FCS as input to the CRC algorithm.
6. **Comparison:** If the calculated CRC value at the receiving end matches the FCS value in the received frame, the data is considered valid and intact. If there is a mismatch, it indicates that the data was corrupted during transmission, and the frame is discarded.