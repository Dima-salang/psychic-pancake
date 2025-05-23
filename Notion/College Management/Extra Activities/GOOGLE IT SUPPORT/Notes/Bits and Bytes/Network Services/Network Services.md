  

  

## Domain Name System

**2. The Role of DNS:**

- DNS serves as a global and distributed network service that translates human-readable domain names (e.g., [**www.weather.com**](http://www.weather.com/)) into numeric IP addresses (e.g., 184.29.131.121).
- Instead of remembering numeric IP addresses for every website or service, users can simply type domain names into their web browsers.
- DNS ensures that users can access websites and services without having to deal with the complexities of IP addresses.

**3. Dynamic IP Addresses:**

- IP addresses associated with domain names can change over time for various reasons, such as server migrations or load balancing.
- DNS allows organizations to update the IP address associated with a domain name, ensuring that users can still access their services without disruption.
- Users don't need to know or update IP addresses manually; DNS handles this behind the scenes.

**4. Geographical Distribution:**

- Organizations with a global presence often distribute their web servers across multiple data centers worldwide.
- DNS can be configured to resolve domain names to different IP addresses based on the user's geographic location.
- This ensures that users are served by the nearest web server, reducing latency and improving website performance.

  

  

## **Understanding DNS (Domain Name System) Name Resolution**

DNS, or Domain Name System, is a critical component of the Internet that translates user-friendly domain names into numeric IP addresses, allowing humans to easily access websites and services. DNS name resolution involves multiple types of DNS servers and a hierarchical structure for efficient and accurate resolution. Here's a detailed look at how DNS name resolution works:

**1. DNS Configuration:**

- DNS servers are one of the essential configurations required for a device to operate on a network.
- A typical modern network configuration includes configuring the following four aspects for a host: IP address, subnet mask, gateway, and DNS server.
- While a computer can function without DNS or without a configured DNS server, it becomes challenging for users to access websites and services without proper DNS resolution.

**2. Types of DNS Servers:**

- DNS servers serve various roles in the DNS resolution process. A single DNS server can perform multiple roles simultaneously.
- The primary types of DNS servers include:
    - Caching Name Servers: These servers store domain name lookups for a certain period, preventing repetitive full DNS resolutions.
    - Recursive Name Servers: They perform full DNS resolution requests and are often combined with caching servers in local networks.
    - Root Name Servers: These servers are responsible for directing queries toward the appropriate TLD (Top-Level Domain) name servers.
    - TLD Name Servers: Each TLD (e.g., .com, .org) has its own name server responsible for its domain names.
    - Authoritative Name Servers: Responsible for specific domain names, handling DNS lookups for the organization's domain.

**3. Caching and Recursive Name Servers:**

- Caching and recursive name servers are typically provided by ISPs or local network administrators.
- These servers store recently resolved domain names and their corresponding IP addresses for a certain duration.
- When a user requests a domain name, the local name server checks its cache first before performing a full DNS resolution.
- Cached data has a Time to Live (TTL) value that specifies how long the server can store the data before discarding it and performing a new resolution.

**4. Hierarchical DNS Structure:**

- DNS operates in a hierarchical structure with multiple levels, ensuring the accuracy and trustworthiness of DNS responses.
- The resolution process involves contacting various types of DNS servers in a specific order:
    
    - Root Name Servers: There are 13 global root servers (anycast), which direct queries to the appropriate TLD name server.
    - TLD Name Servers: These servers handle top-level domain queries (e.g., .com, .org) and redirect queries to authoritative name servers.
    - Authoritative Name Servers: Responsible for specific domains, providing the actual IP address associated with a domain name.
    
      
    

## **TCP vs. UDP Overview**

- **UDP (User Datagram Protocol):**
    - Connectionless: No setup or teardown required for a connection.
    - Ideal for simple, low-overhead communication.
    - No inherent error recovery mechanism.
- **TCP (Transmission Control Protocol):**
    - Connection-oriented: Requires a three-way handshake to establish a connection and a four-way handshake to terminate it.
    - Provides reliable, error-checked data transmission.
    - Suitable for more complex and robust data exchanges.

## **DNS via TCP**

When a full DNS lookup occurs over TCP, it involves multiple packet exchanges:

1. Initiating the Connection:
    - Host sends a SYN packet to the local DNS server on port 53.
    - DNS server responds with a SYN-ACK packet.
    - Host acknowledges with an ACK, establishing the connection (3 packets).
2. Requesting Data:
    - Host sends the actual DNS request (e.g., "I'd like the IP address for food.com, please").
    - DNS server responds with an ACK, confirming receipt (2 packets).
3. Querying Root Server:
    - If the local DNS server doesn't have cached data, it queries a root name server.
    - This requires a three-way handshake, request, response, and ACK (4 packets).
4. Querying TLD Server:
    - The recursive DNS server communicates with the top-level domain (TLD) server.
    - Another three-way handshake, request, response, and ACK (4 packets).
5. Querying Authority Server:
    - The recursive DNS server contacts the authoritative name server for the domain.
    - Yet another three-way handshake, request, response, and ACK (4 packets).
6. Final Response:
    - The local DNS server responds to the initial DNS resolver's request.
    - The resolver sends an ACK to confirm receipt (2 packets).
7. Closing the TCP Connection:
    - Requires a four-way handshake (4 packets).

**Total Packets for a DNS Lookup via TCP: 44**

While 44 packets might not be a significant burden on modern networks, the overhead increases significantly in comparison to UDP, especially considering that DNS traffic is a precursor to actual data transfer.

## **DNS via UDP**

DNS over UDP is simpler and more efficient due to its connectionless nature:

1. The original computer sends a UDP packet to the local DNS server on port 53, requesting the IP for food.com (1 packet).
2. The local DNS server, acting as a recursive server, communicates with the root server and TLD server, with responses included (3 packets).
3. The recursive server communicates with the authoritative name server and receives the IP address (2 packets).
4. The local DNS server responds to the DNS resolver with the IP for foo.com (1 packet).

**Total Packets for a DNS Lookup via UDP: 8**

DNS over UDP significantly reduces overhead, making it ideal for simple queries. Error recovery in UDP is minimal, and if a response is lost, the DNS resolver can re-issue the request, providing basic functionality akin to TCP.

  

## **DNS over TCP Considerations**

While DNS predominantly uses UDP, DNS over TCP exists and is used when responses are too large for a single UDP datagram. In such cases, a DNS server will signal that a TCP connection is required to retrieve the data, demonstrating DNS's flexibility.

  

  

## DNS Resource Record Types

  

## **1. A Record (Address Record)**

- **Purpose:** Maps a domain name to an IPv4 address.
- **Usage:** Resolving domain names to their corresponding IPv4 addresses.
- **Note:** Multiple A records for a single domain name can be configured to support DNS round-robin for load balancing traffic across multiple IPs.
    - Round-robin is like a DNS server returning multiple IPs for a domain. If one user connects to 10.1.1.1, then another user will get connected to 10.1.1.2 for the next DNS resolution. But, all users will get returned all four IP addresses in case of failure.

## **2. Quad A Record (AAAA Record)**

- **Purpose:** Similar to A record but returns an IPv6 address instead of IPv4.
- **Usage:** Resolving domain names to their corresponding IPv6 addresses.

## **3. CNAME Record (Canonical Name Record)**

- **Purpose:** Redirects traffic from one domain to another.
- **Usage:** Ensuring that different domain names point to the same server without duplicating A records.
- **Note:** CNAME stands for "canonical name" and simplifies server IP updates by centralizing changes to a single record.
    - This is more simple as users entering [microsoft.com](http://microsoft.com) will be redirected [www.microsoft.com](http://www.microsoft.com) automatically. You could also put in two A records but if a change in the original domain name occurs, you would also configure both A records.

## **4. MX Record (Mail Exchange Record)**

- **Purpose:** Specifies the mail server responsible for receiving email messages for a domain.
- **Usage:** Ensuring proper email delivery to the designated mail server, separate from other services.

## **5. SRV Record (Service Record)**

- **Purpose:** Defines the location of various specific services, not limited to email.
- **Usage:** Identifying the servers providing specific services, such as CalDAV for calendars and scheduling.

## **6. TXT Record (Text Record)**

- **Purpose:** Originally intended for human-readable text associated with a domain name.
- **Usage:** Over time, TXT records have evolved to convey additional data, including configuration preferences for network services.
- **Note:** TXT records are flexible and used creatively to communicate data not initially intended for DNS.

## **7. NS Record (Name Server Record) and SOA Record (Start of Authority Record)**

- **Purpose:** NS records specify authoritative name servers for a domain, while SOA records contain authoritative zone information.
- **Usage:** Defining authoritative information about DNS zones, including the name servers responsible for the domain.

  

## **Three Primary Parts of a Domain Name**

1. **Top-Level Domain (TLD):**
    - The last part of a domain name, such as ".com" in "[**www.google.com**](http://www.google.com/)."
    - Represents the highest level in the DNS hierarchy.
    - Examples include ".com," ".net," ".edu," as well as country-specific TLDs like ".de" (Germany) and ".cn" (China).
    - The number of TLDs has expanded significantly in recent years, including vanity TLDs.
2. **Domain:**
    - The middle part of a domain name, such as "google" in "[**www.google.com**](http://www.google.com/)."
    - Used to denote where control transitions from a TLD name server to an authoritative name server.
    - Registered and chosen by individuals or organizations but must end with a predefined TLD.
3. **Subdomain:**
    - The leftmost part of a domain name, such as "www" in "[**www.google.com**](http://www.google.com/)."
    - Also referred to as a host name if it is assigned to a single host.
    - Subdomains allow further subdivision of a domain and can be freely chosen by the domain owner.

## **Fully Qualified Domain Name (FQDN)**

- An FQDN is the complete domain name that includes all three parts: subdomain, domain, and TLD.
- For example, "[**www.google.com**](http://www.google.com/)" is a fully qualified domain name.
- Combining these parts yields a unique address for identifying resources on the internet.

## **Domain Registration and Subdomains**

- While registering a domain with a registrar incurs a cost, subdomains can be freely assigned by those in control of the registered domain.
- Registrars are companies authorized by ICANN (Internet Corporation for Assigned Names and Numbers) to sell unregistered domain names.
- Subdomains can be creatively crafted, but the fully qualified domain name's total length should not exceed 255 characters, with individual sections limited to 63 characters.
- DNS can theoretically support up to 127 levels of domains in a single FQDN, though this level of complexity is uncommon.

## **ICANN's Role**

- ICANN, the Internet Corporation for Assigned Names and Numbers, is a non-profit organization responsible for the administration and definition of TLDs.
- ICANN, in collaboration with IANA (Internet Assigned Numbers Authority), oversees global IP address allocations and DNS management.
- The growth of the internet has led to the introduction of numerous TLDs beyond the traditional ones, expanding domain naming possibilities.

  

  

## Subdomains

Subdomains are subdivisions of a larger domain within the DNS (Domain Name System) hierarchy. They are used to organize and categorize resources or services on the internet under a specific domain. Subdomains are created by adding a prefix to the main domain, separating each level with a period (dot). Here's a detailed explanation of subdomains and their use:

**1. Structure of a Subdomain:**

- A subdomain is placed to the left of the main domain, forming a hierarchical structure.
- It typically consists of one or more words or phrases separated by periods (dots). For example, in "blog.example.com," "blog" is the subdomain.
- Subdomains can be further divided into sub-subdomains, creating a deeper hierarchy. For instance, "support.blog.example.com" introduces another level.

**2. Purpose of Subdomains:**

- **Organization:** Subdomains are primarily used to organize content or services within a domain. They help categorize and compartmentalize resources, making it easier to manage and navigate a website or network.
- **Functionality:** Subdomains can represent distinct functions or services of a website or application. For example, "mail.example.com" might point to the email server, while "store.example.com" could be the online store.
- **Localization:** Subdomains can be used for regional or language-specific content. "fr.example.com" might contain content in French, while "es.example.com" serves Spanish-speaking users.
- **Load Balancing:** In some cases, subdomains are employed for load balancing and redundancy. Multiple subdomains can point to different servers or server clusters, distributing traffic for improved performance and fault tolerance.

  

Subdomains are typically free to create and use once you have registered the main domain. When you purchase a domain from a domain registrar, you gain the ability to create and manage subdomains associated with that main domain. Registrars often provide domain management tools that allow you to create subdomains and specify where they should point (e.g., to a specific server or IP address).

Here's how it works:

1. **Purchase a Domain:** You need to register a main domain (e.g., example.com) through a domain registrar. Registering a domain usually involves an annual fee.
2. **Create Subdomains:** After registering the main domain, you can create subdomains using the registrar's control panel or DNS management interface. This process is typically free and allows you to define subdomains like blog.example.com, store.example.com, etc.
3. **Configure DNS:** For each subdomain, you configure DNS records (e.g., A records, CNAME records) to specify where the subdomain should point. These DNS records are used to direct traffic to the appropriate server or IP address.
4. **Utilize Subdomains:** Once you've set up subdomains and configured DNS records, you can use them for various purposes, such as hosting a website, setting up specific services, or organizing content.

  

  

## **DNS Zones Defined**

- **Hierarchy:** DNS zones represent hierarchical divisions of the DNS namespace. They allow for the organization and management of resource records within a specific section of the DNS hierarchy.
- **Responsibility:** Each DNS zone is associated with an authoritative name server responsible for responding to queries related to that zone. This server holds the authoritative information for the zone.
- **Non-overlapping:** Zones are non-overlapping, meaning the administrative authority of one zone does not encompass another. For example, the TLD name server for ".com" does not manage the "google.com" domain; it ends at the authoritative server for "google.com."

## **Purpose of DNS Zones**

- **Simplified Management:** DNS zones are used to simplify the management of resource records. As the number of resource records within a domain grows, managing them as a single zone can become complex. Zones allow administrators to partition and manage specific sections of a domain independently.
- **Control:** Zones offer greater control and flexibility, especially for organizations with complex network structures. They provide a structured way to organize resources, such as subdomains, and apply settings like TTL values.

  

## **Configuration Through Zone Files**

- **Zone Files:** DNS zones are configured through zone files, which are simple text files that declare all resource records for a particular zone.
- **SOA Record:** Every zone file contains a Start of Authority (SOA) resource record declaration, which defines the zone and specifies the authoritative name server for it.
- **NS Records:** Zone files often include NS records indicating other name servers that may also be responsible for the zone.
- **Other Resource Records:** Zone files can include various other resource record types (e.g., A, Quad A, CNAME) to define mappings and configurations for the zone.
- **TTL Values:** Default TTL (Time to Live) values for the records served by the zone may also be specified in zone files.

## **Multiple Servers for a Zone**

- Multiple physical servers, each with its Fully Qualified Domain Name (FQDN) and IP address, can be involved in serving DNS traffic for a single zone.
- This redundancy ensures availability and fault tolerance. If one server encounters issues or hardware failures, others can continue to serve DNS queries.

  

## **Reverse Lookup Zones**

- Reverse lookup zones allow DNS resolvers to request an IP address and receive the associated Fully Qualified Domain Name (FQDN) in return.
- These zones are configured similarly to standard zones but primarily contain Pointer (PTR) resource record declarations that resolve IP addresses to domain names.

  

## **DNS Zones and Subdomains:**

1. **Zone Hierarchies:**
    - DNS zones are a hierarchical concept that allows for the organization of DNS data within a domain. Zones can be thought of as individual segments of a domain.
2. **Authority Levels:**
    - Each DNS zone is served by an authoritative name server responsible for responding to DNS queries related to that zone.
    - A DNS zone can encompass a domain and its subdomains, provided they are within the same administrative control and authoritative name server.
3. **Zone Boundaries:**
    - Zones do not overlap; they have distinct boundaries. The authoritative name server for one zone does not manage resources in another zone.
    - The organization and structure of zones depend on the DNS administrator's preferences and needs.

## **Example: Google.com and Subdomains:**

Let's use "google.com" and "support.google.com" as examples to illustrate how DNS zones and subdomains work:

- **google.com Zone:**
    - The "google.com" domain is a zone by itself, with its authoritative name server(s). All resource records for "google.com" and its immediate subdomains are typically managed within this zone.
    - This zone contains records like A (IPv4), Quad A (IPv6), CNAME, MX (Mail Exchanger), etc., for resources directly under "google.com."
- **support.google.com Subdomain:**
    - "support.google.com" is a subdomain of "google.com." It does not have a separate zone with its authoritative name server.
    - Instead, the authoritative name server(s) responsible for the "google.com" zone manage records for "support.google.com" along with other subdomains directly under "google.com."
- **Administrative Control:**
    - The key factor is administrative control. If the same entity administers both "google.com" and its subdomains like "support.google.com," they can be managed within the same zone.
    - Administrative control allows for the efficient management of related resources.
- **Separate Zones for Different Subdomains:**
    
    - In some cases, larger organizations may choose to create separate zones for specific subdomains, especially if those subdomains have distinct administrators or DNS management requirements.
    - For instance, "mail.google.com" and "store.google.com" might have their own zones if they have separate DNS administrators or distinct DNS configurations.
    
    # **DHCP (Dynamic Host Configuration Protocol) Essentials**
    
    **DHCP Overview:**
    
    - DHCP (Dynamic Host Configuration Protocol) is a critical networking protocol used to automate the configuration of hosts (computers/devices) on a TCP/IP-based network.
    - DHCP simplifies and streamlines the configuration process, reducing administrative overhead.
    
    **Four Key Configuration Parameters:**
    
    - Every computer on a modern TCP/IP network requires at least four specific configurations:
        1. **IP Address**: A unique identifier for the host on the network.
        2. **Subnet Mask**: Defines the local network's boundaries.
        3. **Primary Gateway**: The router's IP address that connects the local network to other networks.
        4. **DNS Server**: Resolves domain names to IP addresses.
    
    **Challenges Without DHCP:**
    
    - Manually configuring these parameters on numerous devices can be time-consuming and prone to errors.
    - Assigning unique IP addresses to each device can be challenging, especially in large networks.
    
    **Role of DHCP:**
    
    - DHCP is an application layer protocol that automates the configuration process of hosts on a network.
    - When a computer connects to the network, it can query a DHCP server to obtain all networking configurations simultaneously.
    
    **Benefits of DHCP:**
    
    - Reduces administrative overhead by automating configuration tasks.
    - Solves the challenge of choosing and assigning specific IPs to individual machines.
    - Ideal for client devices (e.g., desktops, laptops, mobile phones) where the exact IP is less important as long as it's within the correct network range.
    
    **Standard DHCP Modes:**
    
    1. **Dynamic Allocation:**
        - Common mode.
        - A range of IP addresses is set aside for client devices.
        - Each device receives an available IP when it connects.
        - IPs can change almost every time a device connects.
    2. **Automatic Allocation:**
        - Similar to dynamic allocation.
        - The DHCP server keeps track of previously assigned IPs for devices.
        - Tries to assign the same IP to the same device if possible.
    3. **Fixed Allocation (Static Allocation):**
        - Requires a manually specified list of MAC addresses and corresponding IPs.
        - DHCP server assigns IPs based on this predefined list.
        - Offers control and security; only devices with configured MAC addresses can obtain an IP.
    
      
    
    **DHCP as an Application Layer Protocol:**
    
    - DHCP operates as an application layer protocol in the OSI model.
    - It relies on lower-layer protocols (transport, network, data link, and physical) to function.
    - DHCP's primary role is to configure the network layer, making it somewhat paradoxical.
    
    **DHCP Discovery Process:**
    
    - DHCP discovery is the process by which a client configured for DHCP obtains network configuration information.
    - The DHCP discovery process consists of four steps:
    
    **1. Server Discovery:**
    
    - The DHCP client, lacking an IP address and knowledge of the DHCP server's IP, sends a DHCP discover message as a specially crafted broadcast onto the network.
    - DHCP listens on UDP port 67, and DHCP discovery messages are sent from UDP port 68.
    - The DHCP discover message is encapsulated in a UDP datagram with destination port 67, source port 68, and further encapsulated in an IP datagram with a destination IP of 255.255.255.255 and a source IP of 0.0.0.0.
    - This broadcast message reaches every node on the local area network, including potential DHCP servers.
    
    **2. DHCP Server Response:**
    
    - The DHCP server, upon receiving the DHCP discover message, examines its own configuration.
    - Based on its configuration (dynamic, automatic, fixed address allocation), the DHCP server decides whether to offer an IP address to the client.
    - The DHCP server responds with a DHCP offer message (DHCPOFFER), using a destination port of 68, source port of 67, destination broadcast IP of 255.255.255.255, and its own IP as the source.
    - The DHCP offer message reaches every machine on the network, but the client recognizes it based on its MAC address included in the message.
    
    **3. Client Request:**
    
    - The DHCP client processes the DHCP offer to review the offered IP address.
    - The client then sends a DHCP request (DHCPREQUEST)message, essentially accepting the offered IP.
    - This message is sent from an IP of 0.0.0.0 to the broadcast IP of 255.255.255.255.
    
    **4. DHCP Acknowledgement:**
    
    - The DHCP server receives the DHCP request and responds with a DHCP Acknowledgement (DHCPACK) message.
    - This message is again sent to the broadcast IP of 255.255.255.255 and includes the source IP of the DHCP server.
    - The client identifies the message as intended for itself based on its MAC address in the message fields.
    
    **DHCP Lease:**
    
    - The configuration information provided by the DHCP server to the client is known as a DHCP lease.
    - A DHCP lease includes an expiration time, which can range from days to a short duration.
    - Once a lease expires, the DHCP client initiates the DHCP discovery process again to negotiate a new lease.
    - Clients can also release their leases to return assigned IPs to the DHCP server when disconnecting from the network.
    
      
    
    # **Network Address Translation (NAT)**
    
    **Introduction to NAT:**
    
    - NAT, or Network Address Translation, is a technique rather than a defined standard.
    - It's implemented differently by various operating systems and network hardware vendors, but the core concept remains constant.
    - NAT is used to translate one IP address into another for various purposes, including security and IPv4 address space preservation.
    
    **Basic Function of NAT:**
    
    - NAT allows a gateway, typically a router or firewall, to rewrite the source IP address of an outgoing IP datagram while preserving the original IP for the response.
    - It helps manage network traffic and provides additional security measures.
    
    **Example of NAT:**
    
    - Consider two networks: Network A (10.1.1.0/24) and Network B (192.168.1.0/24), with a router having interfaces on both networks (10.1.1.1 and 192.168.1.1).
    - Two computers, Computer 1 (IP: 10.1.1.100) on Network A and Computer 2 (IP: 192.168.1.100) on Network B, want to communicate.
    - Computer 1 sends a packet to the router (gateway) for forwarding.
    - With NAT enabled, the router rewrites the source IP of Computer 1's packet to its own IP (192.168.1.1) before forwarding it to Computer 2.
    - Computer 2 receives the packet from the router, thinking it originated from the router itself.
    - Computer 2 responds, and the router rewrites the destination IP back to Computer 1's IP before forwarding the response.
    
    **IP Masquerading and Security:**
    
    - NAT, in this example, hides Computer 1's IP from Computer 2, a concept known as IP masquerading.
    - IP masquerading enhances security because it prevents external entities from directly connecting to Computer 1.
    - To establish a connection, external entities need to know the router's IP, which acts as a shield for the entire address space of Network A.
    - This configuration is referred to as one-to-many NAT, commonly used in LANs to protect internal network addresses.
    
      
    
    **Imagine Your Home Network:**
    
    - You have multiple devices at home like phones, tablets, and laptops.
    - Your internet service provider (ISP) gives you one public IP address to access the internet.
    
    **Here's where NAT comes in:**
    
    - Your home router acts as a NAT device.
    - When a device in your home (let's say your laptop) wants to visit a website, it sends a request with its private IP address to your router.
    - Your router, before sending the request to the website, changes the sender's address to its own public IP address.
    - The website receives the request and sends the response back to your router's public IP.
    
    **Key Points:**
    
    - NAT hides your devices' private IP addresses from the internet.
    - Your router uses its public IP for all your devices when they talk to the internet.
    - When the website replies, your router knows which device to send the response to based on the original request.
    
    **Why NAT is Useful:**
    
    - Security: It keeps your devices' IPs private, making it harder for someone on the internet to directly access them.
    - IP Conservation: It lets many devices in your home share a single public IP address.
    
      
    
      
    

**Port Preservation:**

- In one-to-many NAT, when multiple computers share a single external IP, port preservation is crucial.
- Outbound connections from devices use a random source port (ephemeral port) in the range of 49,152 to 65,535.
- The NAT router remembers the source port used by each device and the corresponding internal IP.
- When responses come back from the internet to the router's external IP, it uses the source port information to send the response to the correct internal device.

**Port Conflicts:**

- Sometimes, two devices might choose the same source port simultaneously.
- To resolve this, the router selects a different available source port at random for one of the devices.

**Port Forwarding:**

- Port forwarding allows specific destination ports to be mapped to specific internal devices.
- It's commonly used when a network has services (e.g., web server, mail server) running on different devices internally.
- External users only need to know the router's external IP.
- Incoming traffic to specific ports on the router gets forwarded to the appropriate internal device based on port numbers.
- For instance, traffic to port 80 could go to a web server (e.g., 10.1.1.5), while traffic to port 25 could go to a mail server (e.g., 10.1.1.6).

**Benefits of Port Forwarding:**

- Simplifies external access to multiple services within a network.
- Allows for IP masquerading, where internal IPs are hidden from external users.
- Enhances security by controlling which services are accessible from the outside.

  

## Further Examples

**Port Preservation:**

- **Purpose:** Port preservation is primarily used for maintaining outbound connections initiated by devices within the network.
- **How It Works:** When a device initiates an outbound connection to an external server, the NAT router keeps track of the source port used by that device.
- **Scenario:** It ensures that when responses come back from the external server, they are correctly mapped to the originating device based on the source port.
- **Example:** If Computer A uses port 51,300 for an outbound request, the NAT router will remember this port and correctly forward the incoming response to Computer A when it arrives.

**Port Forwarding:**

- **Purpose:** Port forwarding, also known as port mapping, is used to direct inbound traffic from external sources to specific devices within the internal network.
- **How It Works:** The NAT router is configured to listen for traffic on specific external ports, and when it receives traffic on those ports, it forwards it to a predefined internal IP address and port.
- **Scenario:** It's used when you want to make services within your internal network (e.g., web server, mail server) accessible to external users.
- **Example:** If you configure port forwarding to forward external port 80 to the internal IP of your web server (e.g., 192.168.1.10), all incoming HTTP requests to the router's external IP will be directed to the web server.

**Key Differences:**

- Port preservation deals with maintaining the consistency of source ports for outbound connections and ensures that responses are correctly directed back to the originating devices.
- Port forwarding focuses on routing incoming traffic from external sources to specific devices or services within the internal network, allowing external users to access internal resources.

  

  

## Virtual Private Networks or VPNs

  

![[/Untitled 37.png|Untitled 37.png]]

**What is a VPN (Virtual Private Network)?**

- VPNs are a technology that extends a private or local network's reach to a remote location or device that is not physically connected to that network.
- They create an encrypted communication tunnel, often referred to as a VPN tunnel, between the user's device and a VPN server.

**Common Use Case: Remote Access for Employees**

- One of the most common uses of VPNs is to allow employees to securely access their organization's network resources when they are not in the office.
- Employees use VPN clients to establish a connection to their company's VPN server.
- The VPN server assigns the user's device a virtual interface with an IP address that matches the company's internal network.
- The user's device can now send and receive data over the VPN tunnel, accessing internal resources as if it were physically connected to the office network.

**How VPNs Work:**

- VPNs operate at the transport layer (Layer 4) or higher layers of the OSI model.
- They use encryption to protect the data transmitted through the tunnel.
- Most VPNs encapsulate the data into an encrypted payload within the transport layer (Layer 4) of the original packet.
- At the VPN server endpoint, the layers of the packet are stripped away, and the payload is decrypted, leaving the top three layers (network, transport, and application) of a new packet.
- This new packet is then encapsulated with the appropriate data link layer information and sent across the network.

**Authentication and Security:**

- VPNs require strict authentication to ensure that only authorized users and devices can establish a connection.
- Two-factor authentication is commonly used in VPN setups, adding an extra layer of security beyond a username and password.
- VPNs use encryption to protect data privacy, making it difficult for unauthorized parties to intercept or read the transmitted data.

**Site-to-Site VPNs:**

- In addition to remote access for users, VPNs can also be used to establish site-to-site connectivity.
- This allows two physically separated office locations to act as a single network and access each other's resources securely.
- VPN routers or specialized VPN devices establish the VPN tunnel between the two office networks.

**Diverse VPN Implementations:**

- VPN is a general concept, not a single protocol or technology.
- There are numerous VPN implementations with varying details of how they work.
- It's essential to understand that VPNs provide encrypted tunnels for remote devices or networks to securely access resources on another network, even if they are not physically connected.

  

  

**Proxy Services Overview:**

- A proxy service is a server that acts as an intermediary between a client and another server.
- It is something that communicates on behalf of something else.
- Proxies provide various benefits, including anonymity, security, content filtering, and improved performance.
- A gateway router is a type of proxy.

**Proxy Types and Use Cases:**

1. **Web Proxies:**
    - Historically used for improved performance by caching web content.
    - Modern usage includes content filtering and access control, preventing users from accessing specific websites during work hours.
2. **Reverse Proxies:**
    - Used by popular websites to distribute traffic among multiple backend servers, ensuring high availability and load balancing.
    - Can handle decryption and encryption of encrypted web traffic, offloading this resource-intensive task from backend servers.

There are many more proxies that are used nowadays.

  

**Reverse Proxy Overview:**

- A reverse proxy is a server that acts as an intermediary between client requests and multiple backend servers.
- Unlike a forward proxy (commonly known as a web proxy), which serves clients accessing the internet, a reverse proxy serves as a front-end for servers providing services, often within an organization's network.

**Key Functions of a Reverse Proxy:**

1. **Load Balancing:**
    - One of the primary roles of a reverse proxy is load balancing.
    - When a client sends a request to a website, the reverse proxy distributes that request to one of the many backend servers capable of handling it.
    - This ensures even distribution of traffic across multiple servers, preventing overload on a single server and improving overall performance and reliability.
2. **Security:**
    - Reverse proxies can act as an added layer of security by shielding backend servers from direct exposure to the internet.
    - They can filter incoming requests, blocking malicious traffic and mitigating Distributed Denial of Service (DDoS) attacks.
    - Additionally, they can perform SSL/TLS termination, handling the encryption and decryption of web traffic, thus relieving backend servers from this resource-intensive task.
3. **Caching:**
    - Some reverse proxies can cache frequently accessed content, reducing the load on backend servers and improving response times.
    - Cached content can be served to clients directly from the reverse proxy without the need to access the backend servers.

**Use Cases for Reverse Proxies:**

1. **High-Traffic Websites:**
    - Popular websites with heavy traffic often employ reverse proxies to distribute requests among multiple web servers.
    - This ensures fast response times and high availability.
2. **Application Delivery Controllers (ADCs):**
    - In enterprise environments, reverse proxies, often called Application Delivery Controllers (ADCs), manage traffic to various internal applications.
    - They can provide load balancing, security, and traffic optimization for these applications.
3. **SSL/TLS Termination:**
    - Reverse proxies can handle the encryption and decryption of HTTPS traffic (SSL/TLS termination), allowing backend servers to focus on processing requests.
4. **Content Delivery Networks (CDNs):**
    
    - CDNs use reverse proxies to cache and distribute content (e.g., images, videos, and static web assets) to users from servers geographically closer to them, reducing latency.
    
      
    
      
    

Forward proxies, often simply referred to as "proxies," serve a different purpose than reverse proxies. While reverse proxies sit in front of backend servers and manage incoming client requests, forward proxies sit between client devices and external servers on the internet. Here's a detailed look at forward proxies:

**Forward Proxy Overview:**

- A forward proxy acts as an intermediary server between client devices and external servers on the internet.
- Clients, such as individual computers or devices within a local network, configure their applications or network settings to use the forward proxy for internet access.
- The forward proxy forwards client requests to external servers and returns the responses to the clients.

**Key Functions of a Forward Proxy:**

1. **Anonymity and Privacy:**
    - Forward proxies provide anonymity to clients by hiding their IP addresses from external servers.
    - When clients access websites through a forward proxy, the external servers see the IP address of the proxy server rather than the client's IP.
    - This anonymity is useful for privacy and security purposes, as it helps protect the identity and location of clients.
2. **Content Filtering and Access Control:**
    - Organizations often use forward proxies to enforce content filtering policies.
    - They can block access to specific websites, categories of websites (e.g., social media or adult content), or particular types of content (e.g., malware or malicious websites).
    - Access control lists (ACLs) can be configured on the forward proxy to define which websites or content are accessible and which are restricted.
3. **Caching:**
    - Similar to reverse proxies, some forward proxies can cache web content.
    - Caching frequently accessed web pages, images, or files can reduce bandwidth usage and speed up access times for clients.
4. **Access to Restricted Content:**
    - In some cases, users in countries with internet censorship or restrictions may use forward proxies to access blocked content and bypass geo-restrictions.
5. **Logging and Monitoring:**
    
    - Forward proxies can log all client requests and internet activity.
    - This logging can help organizations monitor and analyze web traffic for security and compliance purposes.
    
      
    
    **A record:** The most common resource record, used to point a certain domain name at a certain IPv4 IP address
    
    **Anycast:** A technique that's used to route traffic to different destinations depending on factors like location, congestion, or link health
    
    **Automatic allocation:** A range of IP addresses is set aside for assignment purposes
    
    **Caching and recursive name servers:** They are generally provided by an ISP or your local network, and their purpose is to store domain name lookups for a certain amount of time
    
    **CNAME:** A resource record used to map one domain to another
    
    **DHCP discovery:** The process by which a client configured to use DHCP attempts to get network configuration information
    
    **Domain Name System (DNS):** A global and highly distributed network service that resolves strings of letters, such as a website name, into an IP address
    
    **DNS zones:** A portion of space in the Domain Name System (DNS) that is controlled by an authoritative name server
    
    **Domain:** Used to demarcate where control moves from a top-level domain name server to an authoritative name server
    
    **Domain name:** A website name; the part of the URL following www.
    
    **Dynamic allocation:** A range of IP addresses is set aside for client devices and one of these IPs is issued to these devices when they request one
    
    **Fixed allocation:** Requires a manually specified list of MAC address and the corresponding IPs
    
    **Fully qualified domain name:** When you combine all the parts of a domain together
    
    **IP masquerading:** The NAT obscures the sender's IP address from the receiver
    
    **MX record:** It stands for mail exchange and this resource record is used in order to deliver email to the correct server
    
    **Name resolution:** This process of using DNS to turn a domain name into an IP address
    
    **Network Address Translation (NAT):** A mitigation tool that lets organizations use one public IP address and many private IP addresses within the network
    
    **NS record:** It indicates other name servers that may also be responsible for a particular zone
    
    **NTP servers**: Used to keep all computers on a network synchronized in time
    
    **Pointer resource record:** It resolves an IP to a name
    
    **Port forwarding:** A technique where specific destination ports can be configured to always be delivered to specific nodes
    
    **Port preservation**: A technique where the source port chosen by a client, is the same port used by the router
    
    **Proxy service**: A server that acts on behalf of a client in order to access another service
    
    **Quad A (AAAA) record:** It is very similar to an A record except that it returns in IPv6 address instead of an IPv4 address
    
    **Recursive name servers:** Servers that perform full DNS resolution requests
    
    **Reverse lookup zone files:** They let DNS resolvers ask for an IP, and get the FQDN associated with it returned
    
    **Reverse proxy:** A service that might appear to be a single server to external clients, but actually represents many servers living behind it
    
    **Round robin:** It is a concept that involves iterating over a list of items one by one in an orderly fashion
    
    **SRV record:** A service record used to define the location of various specific services
    
    **Start of authority:** A declaration of the zone and the name of the name server that is authoritative for it
    
    **Top Level Domain (TLD):** The top level of the DNS or the last part of a domain name. For example, the “com” in www.weather.com
    
    **Time-To-Live field (TTL):** An 8-bit field that indicates how many router hops a datagram can traverse before it's thrown away
    
    **Two-factor authentication:** A technique where more than just a username and password are required to authenticate. Usually, a short-lived numerical token is generated by the user through a specialized piece of hardware or software
    
    **TXT record:** It stands for text and was originally intended to be used only for associating some descriptive text with a domain name for human consumption
    
    **Types of DNS servers:** There are five primary types of DNS servers; caching name servers, recursive name servers, root name servers, TLD name servers, and authoritative name servers
    
    **Virtual Private Network (VPN):** A technology that allows for the extension of a private or local network, to a host that might not work on that same local network
    
    **Zone Files:** Simple configuration files that declare all resource records for a particular zone
    
    ### **Terms and their definitions from previous modules**
    
    A
    
    **ACK flag:** One of the TCP control flags. ACK is short for acknowledge. A value of one in this field means that the acknowledgment number field should be examined
    
    **Acknowledgement number:** The number of the next expected segment in a TCP sequence
    
    **Address class system:** A system which defines how the global IP address space is split up
    
    **Address Resolution Protocol (ARP):** A protocol used to discover the hardware address of a node with a certain IP address
    
    **Application layer payload:** The entire contents of whatever data applications want to send to each other
    
    **Application layer:** The layer that allows network applications to communicate in a way they understand
    
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
    
    **CLOSE_WAIT:** A connection state that indicates that the connection has been closed at the TCP layer, but that the application that opened the socket hasn't released its hold on the socket yet
    
    **CLOSE:** A connection state that indicates that the connection has been fully terminated, and that no further communication is possible
    
    **Collision domain:** A network segment where only one device can communicate at a time
    
    **Computer networking:** The full scope of how computers communicate with each other
    
    **Connection-oriented protocol:** A data-transmission protocol that establishes a connection at the transport layer, and uses this to ensure that all data has been properly transmitted
    
    **Connectionless protocol:** A data-transmission protocol that allows data to be exchanged without an established connection at the transport layer. The most common of these is known as UDP, or User Datagram Protocol
    
    **Copper cable categories:** These categories have different physical characteristics like the number of twists in the pair of copper wires. These are defined as names like category (or cat) 5, 5e, or 6, and how quickly data can be sent across them and how resistant they are to outside interference are all related to the way the twisted pairs inside are arranged
    
    **Crosstalk:** Crosstalk is when an electrical pulse on one wire is accidentally detected on another wire
    
    **Cyclical Redundancy Check (CRC):** A mathematical transformation that uses polynomial division to create a number that represents a larger set of data. It is an important concept for data integrity and is used all over computing, not just network transmissions
    
    D
    
    **Data offset field:** The number of the next expected segment in a TCP packet/datagram
    
    **Data packet:** An all-encompassing term that represents any single set of binary data being sent across a network link
    
    **Datalink layer:** The layer in which the first protocols are introduced. This layer is responsible for defining a common way of interpreting signals, so network devices can communicate
    
    **Demarcate:** To set the boundaries of something
    
    **Demarcation point:** Where one network or system ends and another one begins
    
    **Demultiplexing:** Taking traffic that's all aimed at the same node and delivering it to the proper receiving service
    
    **Destination MAC address:** The hardware address of the intended recipient that immediately follows the start frame delimiter
    
    **Destination network**: The column in a routing table that contains a row for each network that the router knows about
    
    **Destination port:** The port of the service the TCP packet is intended for
    
    **DHCP:** A technology that assigns an IP address automatically to a new device. It is an application layer protocol that automates the configuration process of hosts on a network
    
    **Dotted decimal notation:** A format of using dots to separate numbers in a string, such as in an IP address
    
    **Duplex communication:** A form of communication where information can flow in both directions across a cable
    
    **Dynamic IP address:** An IP address assigned automatically to a new device through a technology known as Dynamic Host Configuration Protocol
    
    E
    
    **ESTABLISHED:** Status indicating that the TCP connection is in working order, and both sides are free to send each other data
    
    **Ethernet frame:** A highly structured collection of information presented in a specific order
    
    **Ethernet:** The protocol most widely used to send data across individual links
    
    **EtherType field:** It follows the Source MAC Address in a dataframe. It's 16 bits long and used to describe the protocol of the contents of the frame
    
    **Exterior gateway:** Protocols that are used for the exchange of information between independent autonomous systems
    
    F
    
    **Fiber cable:** Fiber optic cables contain individual optical fibers which are tiny tubes made of glass about the width of a human hair. Unlike copper, which uses electrical voltages, fiber cables use pulses of light to represent the ones and zeros of the underlying data
    
    **FIN_WAIT:** A TCP socket state indicating that a FIN has been sent, but the corresponding ACK from the other end hasn't been received yet
    
    **FIN:** One of the TCP control flags. FIN is short for finish. When this flag is set to one, it means the transmitting computer doesn't have any more data to send and the connection can be closed
    
    **Firewall:** It is a device that blocks or allows traffic based on established rules
    
    **Five layer model:** A model used to explain how network devices communicate. This model has five layers that stack on top of each other: Physical, Data Link, Network, Transport, and Application
    
    **Flag field:** It is used to indicate if a datagram is allowed to be fragmented, or to indicate that the datagram has already been fragmented
    
    **Fragmentation offset field:** It contains values used by the receiving end to take all the parts of a fragmented packet and put them back together in the correct order
    
    **Fragmentation:** The process of taking a single IP datagram and splitting it up into several smaller datagrams
    
    **Frame check sequence:** It is a 4-byte or 32-bit number that represents a checksum value for the entire frame
    
    **FTP:** An older method used for transferring files from one computer to another, but you still see it in use today
    
    **Full duplex:** The capacity of devices on either side of a networking link to communicate with each other at the exact same time
    
    H
    
    **Half-duplex:** It means that, while communication is possible in each direction, only one device can be communicating at a time
    
    **Handshake:** A way for two devices to ensure that they're speaking the same protocol and will be able to understand each other
    
    **Header checksum field:** A checksum of the contents of the entire IP datagram header
    
    **Header length field:** A four bit field that declares how long the entire header is. It is almost always 20 bytes in length when dealing with IPv4
    
    **Hexadecimal:** A way to represent numbers using a numerical base of 16
    
    **Hub:** It is a physical layer device that broadcasts data to everything computer connected to it
    
    I
    
    **IANA:** The Internet Assigned Numbers Authority, is a non-profit organization that helps manage things like IP address allocation
    
    **Identification field:** It is a 16-bit number that's used to group messages together
    
    **Instantiation:** The actual implementation of something defined elsewhere
    
    **Interface:** For a router, the port where a router connects to a network. A router gives and receives data through its interfaces. These are also used as part of the routing table
    
    **Interior gateway:** Interior gateway protocols are used by routers to share information within a single autonomous system
    
    **Internet Protocol (IP):** The most common protocol used in the network layer
    
    **Internet Service Provider (ISP):** A company that provides a consumer an internet connection
    
    **Internetwork:** A collection of networks connected together through routers - the most famous of these being the Internet
    
    **IP datagram:** A highly structured series of fields that are strictly defined
    
    **IP options field:** An optional field and is used to set special characteristics for datagrams primarily used for testing purposes
    
    L
    
    **Line coding:** Modulation used for computer networks
    
    **Listen:** It means that a TCP socket is ready and listening for incoming connections
    
    **Local Area Network (LAN):** A single network in which multiple devices are connected
    
    M
    
    **MAC(Media Access Control) address:** A globally unique identifier attached to an individual network interface. It's a 48-bit number normally represented by six groupings of two hexadecimal numbers
    
    **Modulation:** A way of varying the voltage of a constant electrical charge moving across a standard copper network cable
    
    **Multicast frame:** If the least significant bit in the first octet of a destination address is set to one, it means you're dealing with a multicast frame. A multicast frame is similarly set to all devices on the local network signal, and it will be accepted or discarded by each device depending on criteria aside from their own hardware MAC address
    
    **Multiplexing:** It means that nodes on the network have the ability to direct traffic toward many different receiving services
    
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
    
    **Options field:** It is sometimes used for more complicated flow control protocols
    
    **Organizationally Unique Identifier (OUI):** The first three octets of a MAC address
    
    **OSI model:** A model used to define how network devices communicate. This model has seven layers that stack on top of each other: Physical, Data Link, Network, Transport, Session, Presentation, and Application
    
    P
    
    **Padding field:** A series of zeros used to ensure the header is the correct total size
    
    **Patch panel:** A device containing many physical network ports
    
    **Payload:** The actual data being transported, which is everything that isn't a header
    
    **Physical layer:** It represents the physical devices that interconnect computers
    
    **Port:** It is a 16-bit number that's used to direct traffic to specific services running on a networked computer
    
    **Preamble:** The first part of an Ethernet frame, it is 8 bytes or 64 bits long and can itself be split into two sections
    
    **Presentation layer:** It is responsible for making sure that the unencapsulated application layer data is actually able to be understood by the application in question
    
    **Protocol field:** A protocol field is an 8-bit field that contains data about what transport layer protocol is being used
    
    **Protocol:** A defined set of standards that computers must follow in order to communicate properly is called a protocol
    
    **PSH flag:** One of the TCP control flags. PSH is short for push. This flag means that the transmitting device wants the receiving device to push currently- buffered data to the application on the receiving end as soon as possible
    
    R
    
    **Router:** A device that knows how to forward data between independent networks
    
    **Routing protocols:** Special protocols the routers use to speak to each other in order to share what information they might have
    
    **RST flag:** One of the TCP control flags. RST is short for reset. This flag means that one of the sides in a TCP connection hasn't been able to properly recover from a series of missing or malformed segments
    
    S
    
    **Sequence number:** A 32-bit number that's used to keep track of where in a sequence of TCP segments this one is expected to be
    
    **Server or Service:** A program running on a computer waiting to be asked for data
    
    **Server:** A device that provides data to another device that is requesting that data, also known as a client
    
    **Service type field:** A eight bit field that can be used to specify details about quality of service or QoS technologies
    
    **Session layer:** The network layer responsible for facilitating the communication between actual applications and the transport layer
    
    **Simplex communication:** A form of data communication that only goes in one direction across a cable
    
    **Socket:** The instantiation of an endpoint in a potential TCP connection
    
    **Source MAC address:** The hardware address of the device that sent the ethernet frame or data packet. In the data packet it follows the destination MAC address
    
    **Source port:** A high numbered port chosen from a special section of ports known as ephemeral ports
    
    **Start Frame Delimiter (SFD):** The last byte in the preamble, that signals to a receiving device that the preamble is over and that the actual frame contents will now follow
    
    **Static IP address:** An IP address that must be manually configured on a node
    
    **Subnet mask:** 32-bit numbers that are normally written as four octets of decimal numbers
    
    **Subnetting:** The process of taking a large network and splitting it up into many individual smaller sub networks or subnets
    
    **SYN flag:** One of the TCP flags. SYN stands for synchronize. This flag is used when first establishing a TCP connection and make sure the receiving end knows to examine the sequence number field
    
    **SYN_RECEIVED:** A TCP socket state that means that a socket previously in a listener state, has received a synchronization request and sent a SYN_ACK back
    
    **SYN_SENT:** A TCP socket state that means that a synchronization request has been sent, but the connection hasn't been established yet
    
    T
    
    **TCP checksum:** A mechanism that makes sure that no data is lost or corrupted during a transfer
    
    **TCP segment:** A payload section of an IP datagram made up of a TCP header and a data section
    
    **TCP window:** The range of sequence numbers that might be sent before an acknowledgement is required
    
    **Time-To-Live field (TTL):** An 8-bit field that indicates how many router hops a datagram can traverse before it's thrown away
    
    **Total hops:** The total number of devices data passes through to get from its source to its destination. Routers try to choose the shortest path, so fewest hops possible. The routing table is used to keep track of this
    
    **Total length field:** A 16-bit field that indicates the total length of the IP datagram it's attached to
    
    **Transmission Control Protocol (TCP):** The data transfer protocol most commonly used in the fourth layer. This protocol requires an established connection between the client and server
    
    **Transport layer:** The network layer that sorts out which client and server programs are supposed to get the data
    
    **Twisted pair cable:** The most common type of cabling used for connecting computing devices. It features pairs of copper wires that are twisted together
    
    U
    
    **Unicast transmission:** A unicast transmission is always meant for just one receiving address
    
    **URG flag:** One of the TCP control flags. URG is short for urgent. A value of one here indicates that the segment is considered urgent and that the urgent pointer field has more data about this
    
    **Urgent pointer field:** A field used in conjunction with one of the TCP control flags to point out particular segments that might be more important than others
    
    **User Datagram Protocol (UDP):** A transfer protocol that does not rely on connections. This protocol does not support the concept of an acknowledgement. With UDP, you just set a destination port and send the data packet
    
    V
    
    **Virtual LAN (VLAN):** It is a technique that lets you have multiple logical LANs operating on the same physical equipment
    
    **VLAN header:** A piece of data that indicates what the frame itself is. In a data packet it is followed by the EtherType