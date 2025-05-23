---
Created: 2024-02-15T18:10
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
> [!important] **Computer Network**
> 
> s - a collection of autonomous computers interconnected by a single technology.

Two computers are said to be interconnected if they are able to **exchange information.**

![[/Untitled 19.png|Untitled 19.png]]

  

**End systems** are connected together by a network of communication links and packet switches.

- Communication links are like the physical medium to transmit information.
    - Cable
    - copper
    - wire
    - optical fiber
    - radio

  

Different communication links can transmit data at different rates, with the transmission rate of a link measured in bits/second.

  

When one end system has data to send to another end system, the sender segments the data and adds header bytes to each segment. The resulting packages of information, known as **packets** are sent through the network to the recipient end system where they are reassembled into the original data.

  

A **packet switch** takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links. There are usually two types of packet switches:

- Routers
    - typically used in the network core
- Link-layer switches
    - typically used in access networks.

The sequence of communication links and packet switches traversed by a packet from the sending end system to the receiving end system is known as a **route or path** through the network.

  

End systems access the Internet through ISPs, including residential ISPs, such as local cable or telephone companies; corporate ISPs; university ISPs; ISPs that provide WiFi access in public places; and cellular data ISPs, providing mobile access to smartphones. Each ISP is in itself a network of packet switches and communication links.

  

The Internet is all about connecting end systems to each other, so the ISPs that provide access to end systems **must also be interconnected**. These lower-tier ISPs are thus interconnected through national and international upper-tier ISPs and these upper-tier ISPs are connected directly to each other.

  

End systems, packet switches, and other pieces of the Internet run **protocols** that control the sending and receiving of information within the Internet. **TCP and IP** are two of the most important.

- The IP protocol specifies the format of the packets
- The Internet’s principal protocols are collectively known as TCP/IP.

  

Internet standards are developed by the Internet Engineering Task Force (IETF). The IETF standards documents are called requests for comments or RFCs.

  

  

# A Services Description

- we can also see the Internet as an **infrastructure that provides services to applications.** The applications are said to be distributed applications since they involve multiple end systems that exchange data with each other.
- But how does one program running on one end system instruct the Internet to deliver data to another program running on another end system?

  

End systems attached to the Internet provide a socket interface that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program runnin on another end system. This Internet socket in interface is a set of rules the the sender must follow.

  

  

# What is a Protocol?

![[/Untitled 1 8.png|Untitled 1 8.png]]

  

All activity in the Internet that involves two or more communicating remote entities is governed by a protocol.

  

> [!important] _A_
> 
> **_protocol_** _defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event._

  

  

# The Network Edge

- the computers and other devices connected to the Internet are often referred to as en systems as they sit at the edge of the Internet. Increasing number of non-traditional “things” are also being attached to the internet as end systems.
- End systems are also referred to as hosts because they host or run application programs. Hosts are then further divided into two:
    - Clients
    - Servers
        - tend to be more powerful machines that store and distribute data and content. Today, most data reside in large data centers.

## Access Networks

- the network that physically connects an end system to the first router (aka the “edge router”) on a path from the end system to any other distant end system.

  

![[/Untitled 2 5.png|Untitled 2 5.png]]

  

Today, the two most prevalent types of broadband residential access are digital subscriber line or DSL and cable.

  

### DSL

- A residence typically obtains DSL internet access from the same local telephone company or telco that provides its wired local phone access. Thus, when DSL is used, a customer’s telco is also its ISP.
- Each customer’s DSL modem uses the existing telephone line exchange data with a digital subscribe line access multiplexer or DSLAM located in the telco’s central office or CO.
- The home’s DSL modem takes digital data and translates it to high-frequency tones for transmission over telephone wires to the CO; the analog signals from many such houses are translated back into digital format at the DSLAM. The residential telephone line carries both data and traditional telephone signals simultaneously, which are encoded at different frequencies.
    - a high-speed downstream channel, in the 50 kHz to 1 MHz band
    - a medium-speed upstream channel, in the 4 kHz to 50 kHz band
    - An ordinary two-way telephone channel, in the 0 to 4 kHz band
- On the customer side, a splitter separates the data and telephone signals arriving to the home and forwards the data to the DSL modem. On the telco side, in the CO, the DSLAM separates the data and phone signals and sends the data into the internet. Hundreds or even thousands of households connect to a single DSLAM.
- The DSL standards define multiple transmission rates, including downstream transmission rates of 24 Mbs and 52 Mbs, and upstream rates of 3.5 Mbps and 16 Mbps, the newest standard provides for aggregate upstream plus downstream rates of 1 Gbps.
    
    - Because the down and upstream rates are different, the access is said to be asymmetric.
    
      
    
      
    
    ### Cable Internet Access
    
    - makes use of the cable television company’s existing cable television infrastructure
    
      
    
      
    

  

# The Network Core

  

## Packet Switching

- in a network application, end systems exchange messages with each other. Messages can contain anything.
- To sen a message form a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as packets. Packet switches transmits them through communication links.
- Packets are transmitted over each communication link at a rate equal to the full transmission rate of the link.

### Store-and-Forward Transmission

- most packet switches use store-and-forward transmission at the inputs to the links.
- Store-and-forward transmission means that the packet switch must receive the entire packet fully before it can begin to transmit the first bit of the packet onto the outbound link. Routers need to receive, store, and process the entire packet before forwarding.

  

### Queueing Delays and Packet Loss

- each packet switch has multiple links attached to it. For each attached link, the packet switch has an output buffer or an output queue. If an arriving packet needs to be transmissted onto a link but finds the link busy with the transmission of another packet the arriving packet must wait in the output buffer.
- Thus, packets can suffer output buffer queueing delays.

![[/Untitled 3 5.png|Untitled 3 5.png]]

- these delays depend on the congestion of the network. Since the amount of buffer space is finite, if the buffer is full and another packet arrives, packet loss will occurr—either the arriving packet or one of the already-queued packets will be dropped.

  

### Forwarding Tables and Routing Protocols

- how does the router determine which link it should forward the packet onto?
- This is done using IP addresses. When a source end system wants to send a packet to a destination end system, the source includes the destination’s IP address in the packet’s header. The router will then examine this packet and forward it onto an adjacent router.
- Thus, each router has a forwarding table that maps destination addresses or portions of the destination addresses to that router’s outbound links.
- The Internet has a number of special routing protocols that are used to automatically set the forwarding tables.

  

### Circuit Switching

- There are two fundamental approaches to moving data through a network of links and switches: circuit switching and packet switching.
- in circuit-switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems. In packet-switched networks, these resources are not reserved; a session’s messages use the resources on demand and, as a consequence, may have to wait (that is, queue) for access to a communication link.
    
    - being on a telephone line is like being in a circuit-switched network, there is a direct and reserved connection for that specific call session that guarantees a constant transmission rate. This connection is called a circuit.
    - When the network establishes the circuit, it also reserves a constant transmission rate in the network’s links (representing a fraction of each link’s transmission capacity) for the duration of the connection. Since a given transmission rate has been reserved for this sender-receiver connection, the sender can transfer the data to the receiver at the guaranteed constant rate.
    - For example, four circuit switches are interconnected by four links. Each of these links has four circuits so that each link can support four simultaneous connections.
    - When two hosts want to communicate, the network establishes a dedicated end-to-end connection between the two hosts. Because, each link has four circuits, for each link used by the end-to-end connection, the connection gets 1/4 of the link’s total transmission capacity for the duration of the connection.
    
    ![[/Untitled 4 4.png|Untitled 4 4.png]]
    

  

Proponents of packet switching have always argued that circuit switching can be wasteful because the dedicated circuits are idle during silent periods. Another is that establishing end-to-end circuits and reserving end-to-end transmission capacity is complicated and requires complex signaling software to coordinate the operation of the switches along the end-to-end path.

  

  

### A Network of Networks

- end systems connect into the Internet via an access ISP. But this is only a part of the puzzle, the access ISPs themselves need to be interconnected.

![[/Untitled 5 4.png|Untitled 5 4.png]]

  

  

### Overview of Delay

- a packet starts in a host, passes through a series of routers and ends its journey in another host or the destination. As a packet travels the path, it suffers delay at each node.
- The most important delays are the nodal processing delay, queueing delay, transmission delay, and propagation delay; together these delays accumulate to give a total nodal delay.

![[/Untitled 6 4.png|Untitled 6 4.png]]

  

## Types of Delays

  

### Processing Delay

- the time required to examine the packet’s header and determine where to direct the packet is part of the processing delay. The processing delay can also include other factors, such as the time needed to check for bit-level errors in the packet that occurred in transmitting the packet’s bits from the upstream node.
- Processing delays in high-speed routers are typically on the order of microseconds.

  

### Queueing Delay

- at the queue, the packet experiences a delay as it waits to be transmitted onto the link. the length of the queueing delay of a specific packet will depend on the number of earlier-arriving packets. if the queue is empty, the queueing delay is zero, but if the traffic is heavy, the queueing delay will be long.
- Queueing delays can be on the order of microseconds to milliseconds in practice.

  

### Transmission Delay

- packets can only be transmitted only after all the packets that have arrived before it have been transmitted. Basically, you need to sequentially transmit the packets in the queue to the outbound link.
- The transmission delay is L/R where L is the number of bits and R is the rate of transmission by bits/sec.
- It is the amount of time required to push (transmit) all of the packet’s bits into the link.
- Transmission delays are typically on the order of microseconds to milliseconds in practice

  

### Propagation Delay

- once a bit is pushed into the link, it needs to propagate to router B. The time to propagate from the beginning of the link to router B is the propagation delay. the bit propagates at the propagation speed of the link which depends on the physical medium of the link and is in the range that is equal to or little less than the speed of light.
- The propagation delay is the distance between two routers divided by the propagation speed: d/s.
- In WANs, propagation delays are on the order of milliseconds.

  

## Queueing Delay and Packet Loss

  

  

  

# Application Layer

[[Application Layer]]