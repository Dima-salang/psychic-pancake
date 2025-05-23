  

  

**IP Addresses and Their Purpose:**

- In LANs, devices communicate using unique MAC addresses, suitable for small-scale networks.
- MAC addressing doesn't scale well for large networks and global communication.

**Introduction to IP and IP Addresses:**

- IP addresses are numerical labels for devices in IP networks.
- They serve for network and host identification.
- IP operates at the network layer, abstracting the physical network.
- IPv4 (32-bit) and IPv6 (128-bit) are the two main IP address versions.

  

**IP Address Basics:**

- IP addresses are 32-bit numbers divided into four octets.
- Each octet is represented in decimal form and can range from 0 to 255.
- The format is known as dotted decimal notation (e.g., 12.34.56.78).
- Dotted decimal notation simplifies IP address representation.
- IP addresses are hierarchical and assigned to organizations, making routing more efficient.

**Ownership and Assignment:**

- IP addresses are owned by networks, not individual devices.
- Devices on a network are assigned IP addresses by the network's infrastructure.
- Dynamic Host Configuration Protocol (DHCP) is commonly used to assign dynamic IP addresses.
- Static IP addresses are manually configured and often reserved for servers and network devices.

  

  

## IP Datagram Dissection

  

![[/Untitled 35.png|Untitled 35.png]]

- **Version:** The version field indicates the version of the IP protocol being used. The current version is 4, also known as IPv4. IPv6 is the newer version of IP, but it is not yet as widely used as IPv4.
- **Header length:** The header length field indicates the length of the IP header in 32-bit words. The minimum length is 5 words (20 bytes) and the maximum length is 15 words (60 bytes). The header length is variable because the IP options field is optional and can be used to specify special characteristics for the datagram.
- **Service type:** The service type field specifies the quality of service (QoS) for the datagram. This field can be used to prioritize datagrams or to indicate that they should be handled in a certain way. For example, a datagram with a high priority service type might be given preference over a datagram with a low priority service type.
- **Total length:** The total length field indicates the total length of the datagram, including the header and the payload. The maximum length is 65,535 bytes. This field is used to ensure that the datagram does not exceed the maximum size that a router or other network device can handle.
- **Identification:** The identification field is used to identify a group of datagrams that are part of the same transmission. This field is used by routers to reassemble datagrams that have been fragmented.
- **Flags:** The flags field indicates the fragmentation status of the datagram. The flags can be used to indicate whether the datagram can be fragmented, whether it has already been fragmented, and where the current fragment fits into the original datagram.
- **Fragment offset:** The fragment offset field indicates the offset of the current fragment from the beginning of the datagram. This field is used by routers to reassemble datagrams that have been fragmented.
- **Time to live (TTL):** The time to live (TTL) field specifies the number of routers that the datagram can pass through before it is discarded. This field is used to prevent datagrams from looping endlessly through the network.
- **Protocol:** The protocol field specifies the transport layer protocol that is being used. The most common transport layer protocols are TCP and UDP.
- **Header checksum:** The header checksum is used to verify the integrity of the IP header. The checksum is calculated over the contents of the header and is used to ensure that the header has not been corrupted during transmission.
- **Source IP address:** The source IP address field specifies the IP address of the sender of the datagram.
- **Destination IP address:** The destination IP address field specifies the IP address of the recipient of the datagram.
- **IP options:** The IP options field is an optional field that can be used to specify special characteristics for the datagram. For example, the IP options field can be used to specify the maximum fragment size or to request that the datagram be routed along a specific path.
- **Padding:** The padding field is an optional field that is used to pad the header to the correct length. The padding is usually filled with zeros.

  

The maximum size of an IP datagram is 65,535 bytes. This is the maximum size that an IP datagram can be, including the header and the payload. However, the maximum size of an IP datagram may be smaller than this on some networks. The maximum size of an IP datagram is limited by the size of the network's MTU (Maximum Transmission Unit). The MTU is the largest size of a packet that can be sent on a network without being fragmented.

If the size of an IP datagram is greater than the MTU of the network, the datagram will be fragmented into smaller datagrams that are each smaller than the MTU. The fragmentation process is handled by the IP layer. The IP layer adds fragmentation information to the header of each fragment. This information allows the receiving host to reassemble the fragments into the original datagram.

- Basically, the network layer knows the maximum size of the payload for the ethernet frame so that it knows how small the IP datagrams should be for fragmentation.

  

The payload is the data that is being transported by the IP datagram. It is located after the IP header and before the padding field. The payload can be any type of data, such as text, images, or video.

  

## Encapsulation

![[/Untitled 1 20.png|Untitled 1 20.png]]

  

  

## Address Class System

is a method used to categorize and manage IP addresses within the IPv4 addressing scheme.

![[/Untitled 2 15.png|Untitled 2 15.png]]

This system is now mainly replaced by CIDR due to inefficient IP allocation.

- Class A uses the first octet as the network ID and the rest are host IDs
- Class B uses the first two octets as the network ID and the rest are host IDs
- Class C uses the first three octets as the network ID and the rest are host IDs
- Class D is used for multicasting
- Class E is used for research purposes.

  

Usually your network will cover 192.168.1.0-255 for your hosts

- The total number of usable IP addresses are 254 since: So it’s always 2^8 - 2
    
    - 192.168.1.0 is the network address
    - 192.168.1.255 is the broadcast address
    
      
    
    - And the address taken up by your default gateway.

  

127.0.0.0 is the loopback address and lives virtually in our devices. So we can ping 127.0.0.1 to test our network. But the thing is, this loopback address can accommodate 16 million hosts but are just wasted because if you ping anything within the range of 127.255.255.255, you will still ping the loopback address.

  

We now usually use a classless address system using CIDR which allows flexible subnetting and efficient IP allocation. Because, you cannot waste 16 million IP addresses for a single organization when they are not going to use all of them. It’s better to subnet them.

  

NAT and Private IP addresses are basically the band-aid solution for running out of IPv4 addresses.

  

  

## Address Resolution Protocol

1. **ARP Purpose:** ARP is used to find the hardware (MAC) address of a device when you know its IP address. This is crucial for sending data on a local network.
2. **ARP Table:** Devices maintain an ARP table, which is like a local directory that pairs IP addresses with corresponding MAC addresses. It's useful for quick lookups.
3. **ARP Process:** Let's say a computer wants to send data to IP address 10.20.30.40. It checks its ARP table first. If there's no entry for that IP, it sends a broadcast ARP request to all devices on the local network.
4. **ARP Response:** The device with IP 10.20.30.40 replies with an ARP response containing its MAC address. Now the sending computer knows where to send the data.
5. **Cache:** To avoid repeated ARP requests, the sender usually caches (stores) the ARP response in its table. Entries have a limited lifetime to adapt to network changes.

  

## Subnetting

  

- Subnetting is the process of dividing a large network into smaller subnetworks.
- It enhances network management by allowing efficient utilization of IP addresses and improving network security.
- Large networks often have too many devices like in Class A networks, 16 million hosts but many are unused and not to mention that a single router cannot manage them efficiently. Subnetting is a solution where each subnet operates independently with its gateway router.
- Core routers route the message to the gateway router responsible for the network using the network ID. Gateway routers serve as entry or exit points for a specific network.

![[/Untitled 3 14.png|Untitled 3 14.png]]

- A subnet mask also is a way for a computer to use AND operators to determine if an IP address exists on the same network.

What subnetting is, is just really changing the subnet mask to suit our needs. If we want more host IPs, we just steal some bits from the network bits from the subnet mask. So instead of 255.255.255.0, we get 255.255.254.0 or basically 1111111.1111111.1111110.00000000 meaning that we now have 2^9 -2 P addresses to work with instead of 2^8 - 2

  

![[/Untitled 4 11.png|Untitled 4 11.png]]

In calculating the how many host bits we need to hack, we use the powers of two tables but 2x. So it’s going to be 256, 128, 64, 32, 16, 8, 4, 2 and we base it off on that. If we need divide our network within 4 networks, we select 4 then count how many bits are needed to get there. In this case, two bits are needed, therefore, we need to hack 2 bits. So the subnet mask will look like this 11111111.11111111.11111111.11000000. We now have to find the increment. The increment is just the last bit. Which is 64. So your network’s 192.168.1.0 will then be divided into 4 through 192.168.1.0 - 192.168.1.63 for the first network then, 192.168.1.64 - 192.168.1.127 for the second network and so on with an increment of 64.

  

  

## Classless Inter-Domain Routing or CIDR

  

**II. Limitations of Traditional Subnetting and Address Classes:**

- In traditional subnetting, the network ID is fixed at eight bits for Class A, 16 bits for Class B, and 24 bits for Class C networks.
- This leads to limitations such as only 254 Class A networks and 2,097,152 potential Class C networks.
- Routing tables become overloaded with entries for many Class C networks, even if they are routed to the same destination.
- The predefined sizing of networks doesn't always align with the needs of businesses.

**III. Introduction to CIDR (Classless Inter-Domain Routing):**

- CIDR is a more flexible approach to IP address blocks.
- It builds upon subnetting by using subnet masks to delineate networks.
- The term "demarcation point" signifies the boundary where one network or system ends and another begins.
- CIDR simplifies IP addressing by combining the network ID and subnet ID into one, leading to CIDR notation.

**IV. CIDR Notation and Simplification:**

- CIDR notation, often referred to as slash notation (e.g., 9.100.100.100/24), abandons the concept of address classes.
- The network mask (e.g., 255.255.255.0) in CIDR notation tells us the network ID, while the host ID remains the same.
- This approach simplifies how routers and network devices interpret IP addresses.
- CIDR allows for more arbitrary network sizes compared to the rigid class-based structure.

**V. Flexibility in Network Sizes with CIDR:**

- Previously, companies requiring more addresses than a single Class C network provided had to obtain a whole second Class C network.
- With CIDR, address space can be combined into contiguous chunks with flexible net masks (e.g., /23 or 255.255.254.0).
- This reduces the number of entries in routing tables, as routers only need to know one entry for a range of addresses.
- CIDR also increases the available host IDs within a network.

**VI. Host IDs and Network Sizes:**

- In traditional subnetting, you lose two host IDs per network (network ID and broadcast address).
- For example, in a slash 24 network with 2^8 or 256 potential hosts, you have 256 - 2 = 254 available IPs.
- In a slash 23 network, which has 2^9 or 512 potential hosts, you have 512 - 2 = 510 available IPs.
- The use of CIDR allows for more efficient utilization of host IDs and address space.

  

# ROUTING

  

### Router

- A network device that forwards traffic depending on the destination address of that traffic.

![[/Untitled 5 11.png|Untitled 5 11.png]]

![[/Untitled 6 10.png|Untitled 6 10.png]]

  

**II. Basics of Routing and Routing Tables:**

- Routing involves forwarding data packets based on their destination addresses.
- Routers are network devices with at least two network interfaces, connecting multiple networks.
- The key steps in basic routing are:
    1. Receiving a data packet on one of its interfaces.
    2. Examining the destination IP address of the packet.
    3. Looking up the destination network in its routing table.
    4. Forwarding the packet through the interface closest to the remote network, as determined by the routing table.
- This process is repeated until the data packet reaches its destination.

  

**IV. Introduction of a Third Network:**

- Expanding on the example, a third network, Network C (172.16.1.0/23), is introduced.
- Another router connects Network B and Network C, with IPs 10.0.0.1 on Network B and 172.16.1.1 on Network C.
- When a computer at 192.168.1.100 wants to send data to 172.16.1.100, it sends the packet to its gateway.
- The first router knows the quickest path to Network C is via the second router (10.0.0.1).
- The second router forwards the packet to its final destination in Network C (172.16.1.100).

**V. Scaling Routing on the Internet:**

- On the internet, routing involves many more networks and routers than in the example.
- Traffic often traverses numerous routers, possibly crossing multiple paths.
- Core internet routers form a mesh to ensure redundancy and fault tolerance.
- The core concept remains the same: routers determine the best path based on destination IPs and routing tables

  

  

- Routing tables can vary in complexity, but they typically share common elements.
- A basic routing table often consists of four columns:
    
    1. **Destination Network:** This column defines each network the router knows about, including the network ID and netmask. It tells the router what IP addresses might exist on that network.
    2. **Next Hop:** This column specifies the IP address of the next router or gateway that should receive data packets intended for the destination network. In some cases, it may state that the network is directly connected.
    3. **Total Hops:** This column represents the number of hops (intermediate routers) required to reach the destination network. Routers aim to find the shortest path to a destination, and this information helps them make routing decisions. So this is continuously updated.
    4. **Interface:** This column indicates which of the router's interfaces should be used to forward traffic to match the destination network.
    
    ![[/Untitled 7 7.png|Untitled 7 7.png]]
    
      
    
      
    
    **IV. The Significance of "Total Hops":**
    
    - Understanding routing tables and how they work involves recognizing that there can be multiple paths to reach a destination network.
    - Routers strive to choose the shortest and fastest path for data transmission, considering factors like network availability, link quality, and traffic congestion.
    - "Total Hops" in the routing table keeps track of how many routers (hops) data packets must traverse to reach the destination.
    - Routers continuously update this information based on dynamic routing protocols and network changes.
    
    **V. Complexity of Internet Routing Tables:**
    
    - In complex networks like the internet, routing tables can become extremely large and dynamic.
    - Core internet routers may have millions of entries in their routing tables.
    - These tables must be consulted for every incoming packet to determine the best route to its final destination.
    - Routers use algorithms and protocols to efficiently handle routing decisions, ensuring timely and reliable data delivery.
    
      
    
      
    
    **Understanding Routing Protocols: Interior Gateway Protocols (IGPs)**
    
    In networking, routing protocols are critical for routers to communicate and share information about the best paths to reach destination networks. There are two main categories of routing protocols: Interior Gateway Protocols (IGPs) and Exterior Gateway Protocols (EGPs). In this explanation, we'll focus on Interior Gateway Protocols, which are used within a single autonomous system.
    
    **I. Routing Protocol Categories:**
    
    1. **Interior Gateway Protocols (IGPs):** Used within a single autonomous system (AS) to route traffic between networks and routers controlled by the same network operator. In networking, an Autonomous System is a collection of networks that all fall under the control of a single network operator.
    2. **Exterior Gateway Protocols (EGPs):** Used to exchange routing information between independent autonomous systems, typically on the internet.
    
    **II. Interior Gateway Protocols (IGPs):**
    
    - IGPs are further divided into two main types: Link State Routing Protocols and Distance Vector Routing Protocols.
    
    **III. Link State Routing Protocols:**
    
    - Link State Routing Protocols are advanced and modern routing protocols that focus on sharing detailed information about the state of each router's links and interfaces within the autonomous system.
    - These protocols aim to provide a comprehensive view of the entire network topology, including the status of links, routers, and network segments.
    - Key characteristics of Link State Routing Protocols:
        - Each router advertises the state of its interfaces (links) to every other router within the same AS.
        - All routers within the AS have a complete and up-to-date picture of the network's state.
        - Complex algorithms run on routers to calculate the best paths to reach destination networks based on the complete network information.
        - Examples of Link State Routing Protocols include OSPF (Open Shortest Path First) and IS-IS (Intermediate System to Intermediate System).
    
    ![[/Untitled 8 6.png|Untitled 8 6.png]]
    
    **IV. Distance Vector Routing Protocols:**
    
    - Distance Vector Routing Protocols are older and simpler routing protocols that focus on sharing routing information in the form of distance vectors.
    - A distance vector is essentially a list of network destinations and the number of hops required to reach each destination.
    - Key characteristics of Distance Vector Routing Protocols:
        - Routers share their entire routing table (a list of known networks and associated hop counts) with their neighboring routers.
        - Routers update their routing tables based on received distance vectors from neighboring routers.
        - These protocols may not provide a complete and accurate view of the entire network topology, leading to slower convergence times and potential routing loops.
        - Examples of Distance Vector Routing Protocols include RIP (Routing Information Protocol) and EIGRP (Enhanced Interior Gateway Routing Protocol).
    
    **V. Advantages of Link State Protocols over Distance Vector Protocols:**
    
    - Link State Protocols provide a more comprehensive and accurate view of the network, which leads to faster convergence and reduced risk of routing loops.
    - They adapt more quickly to network changes and offer better scalability.
    - As hardware has become more powerful and affordable, Link State Protocols have largely replaced Distance Vector Protocols in modern networks.
    
      
    
    **Understanding Exterior Gateway Protocols and Autonomous Systems:**
    
    In the world of computer networking and the internet, Exterior Gateway Protocols (EGPs) play a crucial role in routing data between routers representing the edges of different Autonomous Systems (ASes). Here's a breakdown of the key concepts:
    
    **1. Exterior Gateway Protocols (EGPs):**
    
    - Exterior Gateway Protocols are routing protocols used to exchange routing information between routers that belong to different Autonomous Systems (ASes).
    - EGPs are essential for routers to communicate and determine optimal paths for data traffic across AS boundaries.
    
    **2. Autonomous Systems (ASes):**
    
    - An Autonomous System (AS) is a collection of networks and routers under the control of a single organization or network operator.
    - ASes are a fundamental unit in internet routing and are typically assigned a unique Autonomous System Number (ASN) by the Internet Assigned Numbers Authority (IANA).
    
    **3. Internet Assigned Numbers Authority (IANA):**
    
    - IANA is a nonprofit organization responsible for managing various critical internet resources, including IP address allocation, domain name system (DNS) management, and ASN allocation.
    - IANA allocates 32-bit ASNs to organizations, ensuring uniqueness and coordination of AS numbers.
    
    **4. ASN Format and Use:**
    
    - An Autonomous System Number (ASN) is a 32-bit numerical identifier assigned to an AS.
    - ASNs are used to identify and represent entire autonomous systems on the internet.
    - Unlike IP addresses, which are split into network and host portions, ASNs are typically referred to as single decimal numbers (e.g., AS19604).
    - ASNs serve as a way to identify and differentiate ASes when routers need to exchange routing information.
    
    **5. Core Internet Routing and ASes:**
    
    - Core Internet routers, often operated by Internet Service Providers (ISPs), play a critical role in routing data across the global internet.
    - These routers need to know about the existence and details of various autonomous systems to efficiently route data traffic to its destination.
    - Routing data to the edge router (border router) of the appropriate autonomous system is a key objective of core internet routers.
    
      
    

### The most common **distance vector protocols** are [RIP, or Routing Information Protocol](https://en.wikipedia.org/wiki/Routing_Information_Protocol) ([IETF RFC2453](https://tools.ietf.org/html/rfc2453)), and [EIGRP, or Enhanced Interior Gateway Routing Protocol](https://en.wikipedia.org/wiki/Enhanced_Interior_Gateway_Routing_Protocol) ([Cisco documentation](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/16406-eigrp-toc.html)). The most common link state protocol is [OSPF, or Open Shortest Path First](https://en.wikipedia.org/wiki/Open_Shortest_Path_First) ([IETF RFC2328](https://tools.ietf.org/html/rfc2328)).

### In terms of **exterior gateway protocols**, there is only one in use today. The entire Internet needs to agree on how to exchange this sort of information, so a single standard has emerged. This standard is known as [BGP, or Border Gateway Protocol](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) ([IETF RFC4271](https://tools.ietf.org/html/rfc4271)).

  

  

## Non-Routable Address Spaces or Private IP Addresses

  

**1. IPv4 Address Exhaustion:**

- When the Internet Protocol version 4 (IPv4) was first defined, it used a 32-bit addressing scheme, allowing for approximately 4.3 billion unique IP addresses.
- While this seemed like a vast number, it became evident over time that this pool of addresses wouldn't be sufficient to accommodate the growing number of internet-connected devices.

**2. RFC 1918 - Non-Routable Address Space:**

- In 1996, RFC 1918 was published as part of the Request for Comments (RFC) series, which establishes standards and protocols for the internet.
- RFC 1918 defined the concept of non-routable address space, which consists of IP address ranges that are reserved for private, internal use within organizations and cannot be routed on the public internet.

**3. Purpose of Non-Routable Address Space:**

- Non-routable address space is designed to address the limitations of IPv4 by allowing organizations to use private IP addresses for their internal networks.
- These addresses are not globally unique and cannot be used to communicate directly with devices on the public internet.

**4. Benefits of Non-Routable Address Space:**

- Non-routable address space helps conserve the limited pool of publicly routable IP addresses. Organizations can use private IP addresses internally, reducing the demand for unique public addresses.
- It enhances security by isolating internal network traffic from the public internet, reducing exposure to potential threats.

**5. Key Ranges of Non-Routable Address Space:**

- RFC 1918 defined three primary address ranges for non-routable address space:
    - 10.0.0.0/8: This range allows for 16,777,216 unique IP addresses and is often used by larger organizations.
    - 172.16.0.0/12: This range provides 1,048,576 unique IP addresses and is commonly used for medium-sized networks.
    - 192.168.0.0/16: This range offers 65,536 unique IP addresses and is frequently used for smaller networks, including home networks.

**6. Use Within Autonomous Systems:**

- Non-routable address space can be used within autonomous systems (ASes) using interior gateway protocols. ASes are collections of networks managed by a single organization.
- Interior gateway protocols, such as OSPF or BGP within an AS, can route traffic to non-routable address space as needed for internal communication.
- Exterior gateway protocols used to exchange information between ASes do not route non-routable addresses on the public internet.

In summary, the introduction of non-routable address space thro

  

  

  

# Module 2 Glossary

  

### **New terms and their definitions: Course 2 Module 2**

**Address class system:** A system which defines how the global IP address space is split up

**Address Resolution Protocol (ARP):** A protocol used to discover the hardware address of a node with a certain IP address

**ARP table:** A list of IP addresses and the MAC addresses associated with them

**ASN:** Autonomous System Number is a number assigned to an individual autonomous system

**Demarcate:** To set the boundaries of something

**Demarcation point:** Where one network or system ends and another one begins

**Destination network:** The column in a routing table that contains a row for each network that the router knows about

**DHCP:** A technology that assigns an IP address automatically to a new device. It is an application layer protocol that automates the configuration process of hosts on a network

**Dotted decimal notation:** A format of using dots to separate numbers in a string, such as in an IP address

**Dynamic IP address:** An IP address assigned automatically to a new device through a technology known as Dynamic Host Configuration Protocol

**Exterior gateway:** Protocols that are used for the exchange of information between independent autonomous systems

**Flag field:** It is used to indicate if a datagram is allowed to be fragmented, or to indicate that the datagram has already been fragmented

**Fragmentation:** The process of taking a single IP datagram and splitting it up into several smaller datagrams

**Fragmentation offset field:** It contains values used by the receiving end to take all the parts of a fragmented packet and put them back together in the correct order

**Header checksum field:** A checksum of the contents of the entire IP datagram header

**Header length field:** A four bit field that declares how long the entire header is. It is almost always 20 bytes in length when dealing with IPv4

**IANA:** The Internet Assigned Numbers Authority, is a non-profit organization that helps manage things like IP address allocation

**Identification field:** It is a 16-bit number that's used to group messages together

**Interface:** For a router, the port where a router connects to a network. A router gives and receives data through its interfaces. These are also used as part of the routing table

**Interior gateway:** Interior gateway protocols are used by routers to share information within a single autonomous system

**IP datagram:** A highly structured series of fields that are strictly defined

**IP options field:** An optional field and is used to set special characteristics for datagrams primarily used for testing purposes

**Network Address Translation (NAT):** A mitigation tool that lets organizations use one public IP address and many private IP addresses within the network

**Next hop:** The IP address of the next router that should receive data intended for the destination networking question or this could just state the network is directly connected and that there aren't any additional hops needed. Defined as part of the routing table

**Non-routable address space:** They are ranges of IPs set aside for use by anyone that cannot be routed to

**Padding field:** A series of zeros used to ensure the header is the correct total size

**Protocol field:** A protocol field is an 8-bit field that contains data about what transport layer protocol is being used

**Routing protocols:** Special protocols the routers use to speak to each other in order to share what information they might have

**Service type field:** A eight bit field that can be used to specify details about quality of service or QoS technologies

**Static IP address:** An IP address that must be manually configured on a node

**Subnet mask:** 32-bit numbers that are normally written as four octets of decimal numbers

**Subnetting:** The process of taking a large network and splitting it up into many individual smaller sub networks or subnets

**Time-To-Live field (TTL):** An 8-bit field that indicates how many router hops a datagram can traverse before it's thrown away

**Total hops:** The total number of devices data passes through to get from its source to its destination. Routers try to choose the shortest path, so fewest hops possible. The routing table is used to keep track of this

**Total length field:** A 16-bit field that indicates the total length of the IP datagram it's attached to

### **Terms and their definitions from previous modules**

B

**Bit:** The smallest representation of data that a computer can understand

**Border Gateway Protocol (BGP):** A protocol by which routers share data with each other

**Broadcast:** A type of Ethernet transmission, sent to every single device on a LAN

**Broadcast address:** A special destination used by an Ethernet broadcast composed by all Fs

C

**Cable categories:** Groups of cables that are made with the same material. Most network cables used today can be split into two categories, copper and fiber

**Cables:** Insulated wires that connect different devices to each other allowing data to be transmitted over them

**Carrier-Sense Multiple Access with Collision Detection (CSMA/CD):** CSMA/CD is used to determine when the communications channels are clear and when the device is free to transmit data

**Client:** A device that receives data from a server

**Collision domain:** A network segment where only one device can communicate at a time

**Computer networking:** The full scope of how computers communicate with each other

**Copper cable categories :** These categories have different physical characteristics like the number of twists in the pair of copper wires. These are defined as names like category (or cat) 5, 5e, or 6, and how quickly data can be sent across them and how resistant they are to outside interference are all related to the way the twisted pairs inside are arranged

**Crosstalk:** Crosstalk is when an electrical pulse on one wire is accidentally detected on another wire

**Cyclical Redundancy Check (CRC):** A mathematical transformation that uses polynomial division to create a number that represents a larger set of data. It is an important concept for data integrity and is used all over computing, not just network transmissions

D

**Data packet:** An all-encompassing term that represents any single set of binary data being sent across a network link

**Datalink layer:** The layer in which the first protocols are introduced. This layer is responsible for defining a common way of interpreting signals, so network devices can communicate

**Destination MAC address**: The hardware address of the intended recipient that immediately follows the start frame delimiter

**Duplex communication:** A form of communication where information can flow in both directions across a cable

E

**Ethernet:** The protocol most widely used to send data across individual links

**Ethernet frame:** A highly structured collection of information presented in a specific order

**EtherType field:** It follows the Source MAC Address in a dataframe. It's 16 bits long and used to describe the protocol of the contents of the frame

F

**Fiber cable:** Fiber optic cables contain individual optical fibers which are tiny tubes made of glass about the width of a human hair. Unlike copper, which uses electrical voltages, fiber cables use pulses of light to represent the ones and zeros of the underlying data

**Five layer model**: A model used to explain how network devices communicate. This model has five layers that stack on top of each other: Physical, Data Link, Network, Transport, and Application

**Frame check sequence:** It is a 4-byte or 32-bit number that represents a checksum value for the entire frame

**Full duplex:** The capacity of devices on either side of a networking link to communicate with each other at the exact same time

H

**Half-duplex:** It means that, while communication is possible in each direction, only one device can be communicating at a time

**Hexadecimal:** A way to represent numbers using a numerical base of 16

**Hub:** It is a physical layer device that broadcasts data to everything computer connected to it

I

**Internet Protocol (IP):** The most common protocol used in the network layer

**Internet Service Provider (ISP):** A company that provides a consumer an internet connection

**Internetwork:** A collection of networks connected together through routers - the most famous of these being the Internet

L

**Line coding:** Modulation used for computer networks

**Local Area Network (LAN):** A single network in which multiple devices are connected

M

**MAC(Media Access Control) address:** A globally unique identifier attached to an individual network interface. It's a 48-bit number normally represented by six groupings of two hexadecimal numbers

**Modulation:** A way of varying the voltage of a constant electrical charge moving across a standard copper network cable

**Multicast frame:** If the least significant bit in the first octet of a destination address is set to one, it means you're dealing with a multicast frame. A multicast frame is similarly set to all devices on the local network signal, and it will be accepted or discarded by each device depending on criteria aside from their own hardware MAC address

N

**Network layer:** It's the layer that allows different networks to communicate with each other through devices known as routers. It is responsible for getting data delivered across a collection of networks

**Network port:** The physical connector to be able to connect a device to the network. This may be attached directly to a device on a computer network, or could also be located on a wall or on a patch panel

**Network switch:** It is a level 2 or data link device that can connect to many devices so they can communicate. It can inspect the contents of the Ethernet protocol data being sent around the network, determine which system the data is intended for and then only send that data to that one system

**Node:** Any device connected to a network. On most networks, each node will typically act as a server or a client

O

**Octet:** Any number that can be represented by 8 bits

**Organizationally Unique Identifier (OUI):** The first three octets of a MAC address

**OSI model:** A model used to define how network devices communicate. This model has seven layers that stack on top of each other: Physical, Data Link, Network, Transport, Session, Presentation, and Application

P

**Patch panel:** A device containing many physical network ports

**Payload:** The actual data being transported, which is everything that isn't a header

**Physical layer:** It represents the physical devices that interconnect computers

**Preamble:** The first part of an Ethernet frame, it is 8 bytes or 64 bits long and can itself be split into two sections

**Protocol:** A defined set of standards that computers must follow in order to communicate properly is called a protocol

R

**Router:** A device that knows how to forward data between independent networks

S

**Server:** A device that provides data to another device that is requesting that data, also known as a client

**Simplex communication:** A form of data communication that only goes in one direction across a cable

**Source MAC address:** The hardware address of the device that sent the ethernet frame or data packet. In the data packet it follows the destination MAC address

**Start Frame Delimiter (SFD):** The last byte in the preamble, that signals to a receiving device that the preamble is over and that the actual frame contents will now follow

T

**Transmission Control Protocol (TCP):** The data transfer protocol most commonly used in the fourth layer. This protocol requires an established connection between the client and server

**Transport layer:** The network layer that sorts out which client and server programs are supposed to get the data

**Twisted pair cable:** The most common type of cabling used for connecting computing devices. It features pairs of copper wires that are twisted together

U

**Unicast transmission:** A unicast transmission is always meant for just one receiving address

**User Datagram Protocol (UDP):** A transfer protocol that does not rely on connections. This protocol does not support the concept of an acknowledgement. With UDP, you just set a destination port and send the data packet

V

**Virtual LAN (VLAN):** It is a technique that lets you have multiple logical LANs operating on the same physical equipment

**VLAN header:** A piece of data that indicates what the frame itself is. In a data packet it is followed by the EtherType