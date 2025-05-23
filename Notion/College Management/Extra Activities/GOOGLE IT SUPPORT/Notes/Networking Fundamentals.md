---
Created: 2023-09-10T20:38
Reviewed: false
---
  

# Internet

- **Definition:** The Internet is a global network of interconnected computers, creating a vast web of communication. Networking refers to the practice of connecting these computers to facilitate data exchange.
- **Networks Everywhere:** Networks can range from simple home setups connecting devices like smartphones and laptops to large-scale corporate networks or the worldwide Internet.
- **The Internet vs. World Wide Web (WWW):** While the Internet is the physical infrastructure connecting computers globally, the World Wide Web is a collection of web pages, websites, and web services accessed through web browsers.

  

**How Networking Works:**

- **Communication Between Computers:** Computers in a network communicate by sending and receiving data, enabling various applications and services.
- **IP Addresses:** Each device on a network has an IP address (Internet Protocol address), serving as a unique identifier. IPv4 (32-bit) and IPv6 (128-bit) are the two IP address standards.
- **MAC Addresses:** Devices also have Media Access Control (MAC) addresses, which are hardware-based identifiers embedded in network interface cards (NICs).
- **Data Packetization:** Data transferred over a network is broken down into packets, small units of information. Each packet includes the destination IP address, source IP address, and other control information.
- **Routing:** Data packets are routed through a series of network devices, such as routers and switches, based on destination IP addresses. These devices determine the optimal path for data transmission.
- **Layered Communication:** The OSI (Open Systems Interconnection) model defines seven layers, from physical transmission to application. Each layer has specific functions and interacts with adjacent layers.

**Ways to Connect Computers to a Network:**

1. **Ethernet Cable (Wired):**
    - **Physical Connection:** Ethernet cables provide a physical, wired connection to the network.
    - **Network Ports:** Computers have Ethernet network ports (commonly RJ-45 connectors) where you plug in the Ethernet cable.
    - **Advantages:** Wired connections offer stability, security, and often higher speeds, making them suitable for desktop computers and fixed installations.
2. **Wi-Fi (Wireless):**
    - **Wireless Networking:** Wi-Fi, or wireless fidelity, enables devices to connect to a network using radio waves and antennas.
    - **Devices with Wi-Fi:** Most modern devices, such as laptops, smartphones, and smart TVs, come equipped with Wi-Fi capabilities.
    - **Advantages:** Wireless connections offer mobility and convenience, making them ideal for portable devices and scenarios where wired connections are impractical.
3. **Fiber Optic Cable (High-Speed):**
    
    - **Glass Fiber Technology:** Fiber optic cables use glass fibers to transmit data using light signals, offering unparalleled speed and data capacity.
    - **Speed and Efficiency:** Data is transmitted as pulses of light, providing faster and more efficient data transfer compared to electrical cables.
    - **Cost:** Fiber optic installations are more expensive than other methods but are critical for high-demand applications, data centers, and long-distance connections.
    
      
    
    **Network Devices and Their Functions:**
    
    1. **Router:**
        - **Connection Hub:** A router connects multiple devices within a network and manages the routing of data packets between them.
        - **Traffic Routing:** Routers use network protocols to determine the best path for data packets to reach their destination.
        - **Network Address Translation (NAT):** Routers perform NAT to map private IP addresses to a single public IP address when connecting to the Internet.
    2. **Switch:**
        - **Data Switching:** Switches operate at the data link layer (Layer 2) of the OSI model and are used to forward data frames within a local area network (LAN).
        - **Efficient Data Delivery:** Unlike hubs, which broadcast data to all connected devices, switches intelligently forward data only to the intended recipient, improving network efficiency.
    3. **Hub:**
        
        - **Broadcasting:** Hubs operate at the physical layer (Layer 1) and send incoming data packets to all connected devices.
        - **Limited Efficiency:** Hubs are less efficient in handling network traffic, as all devices receive every packet, which can lead to congestion.
        
          
        
    
    **Network Protocols:**
    
    - **Definition:** Network protocols are like sets of rules that govern how data is transmitted, received, and managed in computer networks. They ensure that data is routed correctly, securely, and efficiently to its intended destination.
    
      
    
    **Transmission Control Protocol (TCP):**
    
    - **Role:** TCP is one of the core protocols in the TCP/IP suite and plays a vital role in reliable data transmission between computers over networks.
    - **Reliability:** TCP ensures the reliable delivery of data by establishing a connection between sender and receiver, breaking data into packets, and numbering them for reassembly. It manages acknowledgment of received data and retransmits lost packets.
    - **Usage:** TCP is commonly used for applications that require accurate data delivery, such as web browsing, email, and file transfers.
    
    **Internet Protocol (IP):**
    
    - **Role:** IP is responsible for addressing and routing packets of data so that they can travel across networks and arrive at the correct destination.
    - **IP Addresses:** IP assigns unique numerical addresses (IP addresses) to devices on a network. These addresses are essential for identifying the source and destination of data packets.
    - **Routing:** IP helps routers make decisions about how to forward data packets through networks, ensuring they reach their intended destination.
    - **Versions:** There are two main versions of IP in use today: IPv4 (Internet Protocol version 4) and IPv6 (Internet Protocol version 6). IPv6 was introduced to address the limited availability of IPv4 addresses due to the rapid growth of the internet.
    
    **TCP/IP Protocol Suite:**
    
    - **Definition:** TCP/IP refers to the combination of the Transmission Control Protocol (TCP) and the Internet Protocol (IP). Together, these protocols form the foundation of the internet and modern network communication.
    - **Importance:** TCP/IP has become the dominant protocol suite for internet communication, enabling devices to connect, communicate, and share data seamlessly across the global network.
    - **Compatibility:** TCP/IP is used by virtually all internet-connected devices, regardless of their operating systems or hardware. This universality makes it a crucial protocol for global network connectivity.
    
      
    
    **Accessing the Internet Through the Web:**
    
    - **Web Pages:** Websites consist of text documents formatted using HTML (HyperText Markup Language). They can contain various multimedia content, including text, images, audio, and video.
    - **URL:** URL stands for Uniform Resource Locator and serves as a web address. It's how users navigate to websites. A URL typically includes the "www" (World Wide Web) prefix, a domain name (e.g., reddit.com), and a domain ending (e.g., .com).
    - **Domain Names:** Domain names are human-readable website names, and anyone can register them. They are administered by ICANN (Internet Corporation for Assigned Names and Numbers). Once registered, a domain name becomes unique and unavailable for others to use unless it becomes available again.
    - **Domain Endings:** Different domain endings (e.g., .edu, .net, .org) indicate the type or purpose of a website. For instance, .edu is primarily used for educational institutions.
    - **IP Addresses:** Computers on the internet use IP addresses to locate each other. When you enter a domain name into your browser, it performs a DNS lookup to find the corresponding IP address.
    
      
    
    **A Brief History of the Internet:**
    
    - The Internet's origins trace back to the 1950s when computers were massive and required direct interaction. DARPA (Defense Advanced Research Projects Agency) initiated the ARPANET project in the late 1960s, allowing remote computer access.
    - However, a significant challenge remained: networks couldn't communicate with each other. This issue was resolved in the 1970s with the development of TCP/IP (Transmission Control Protocol and Internet Protocol) by Vinton Cerf and Bob Kahn.
    - TCP/IP facilitated the sharing of data between computers and laid the foundation for the modern Internet. Its adoption grew from a few institutions to billions of computers worldwide.
    
    **The Birth of the World Wide Web:**
    
    - In the 1990s, Tim Berners-Lee, a computer scientist, invented the World Wide Web (WWW). It introduced new protocols for displaying information in web pages, transforming the Internet into a multimedia-rich platform.
    - The World Wide Web democratized access to information, enabling anyone with an Internet connection to explore a vast array of content.
    - Over the past 30 years, the Internet has evolved significantly, from basic email and web pages to video chats, instant news, online shopping, and even online education platforms like the one being used in this course.
    - The development of the Internet involved the collective efforts of brilliant scientists and organizations, resulting in the global network that we rely on today.
    
      
    
    **Internet Protocol (IP) Addresses:**
    
    - Internet Protocol version 4 (IPv4) is the current protocol widely used for IP addresses. IPv4 addresses consist of 32 bits separated into four groups, often expressed as four decimal numbers (e.g., 73.55.242.3).
    - IPv4 addresses are finite, with fewer than 4.3 billion unique addresses available. Due to the rapid proliferation of devices connected to the Internet, this number has become insufficient.
    
    **IPv6 Addresses:**
    
    - Internet Protocol version 6 (IPv6) addresses were introduced to address the limitations of IPv4. IPv6 addresses consist of 128 bits, which provides a significantly larger address space.
    - IPv6 adoption has been gradually increasing to accommodate the growing number of devices. IPv6 addresses are represented as a series of hexadecimal characters separated by colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334).
    - The sheer number of IPv6 addresses is immense, making it practically impossible to exhaust this address space. There are 2^128 (approximately 340 undecillion) possible IPv6 addresses.
    
      
    
    - Network Address Translation (NAT) is a mitigation technique used to alleviate the IPv4 address shortage. It allows organizations to use one public IP address for many private IP addresses within their network.
    - NAT acts like a receptionist for a company. The receptionist knows the company's public phone number (IP address) and forwards incoming calls (data packets) to the appropriate private extensions (devices) within the organization.