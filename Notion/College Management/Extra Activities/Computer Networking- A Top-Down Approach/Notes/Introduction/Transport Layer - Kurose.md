[[TCP - Connection-Oriented]]




Excellent — now we’re diving into the **Transport Layer**, the critical glue between the **application layer** (what users and software actually use) and the **network layer** (which physically delivers packets between hosts).


I’ll give you a structured, in-depth **lecture-style explanation** of **Section 3.1: Introduction and Transport-Layer Services**, elaborating on the definitions, theory, and practical significance.

---

# 🚀 Lecture: Introduction and Transport-Layer Services

### 1. **The Role of the Transport Layer**

The **transport layer** is responsible for providing **logical communication** between **application processes** running on different hosts.

- **Logical communication** means: from the perspective of the application, it looks as though two processes are directly talking to each other, even though physically, the data is passing through multiple routers, switches, and links that the application never sees.
- The **illusion** is similar to a telephone call — you and your friend just “talk” even though your voice is being chopped into signals, transmitted through cables, satellites, and switching equipment.

👉 **Key point:**

The transport layer **hides the network’s complexity** and provides a clean, process-to-process communication channel for applications.

---

### 2. **Where the Transport Layer Lives**

- Transport protocols exist **only at the end systems (hosts)**.
- They do **not** operate inside the routers of the network core.

**Sending side:**

- Takes the application-layer message.
- Breaks it into smaller chunks if necessary.
- Adds a **transport-layer header** → creating a **segment**.
- Passes the segment down to the network layer, which wraps it into a datagram and forwards it.

**Receiving side:**

- Network layer delivers the datagram to the transport layer.
- Transport layer extracts the segment, removes the header, and delivers the data to the correct application process.

👉 **Routers’ role:** They **ignore transport-layer information**. They only examine network-layer fields (IP headers). This is why **transport-layer services must be end-to-end**.

---

### 3. **Multiple Transport Protocols**

The Internet supports more than one transport protocol:

- **TCP (Transmission Control Protocol)**: Reliable, connection-oriented, congestion-controlled.
- **UDP (User Datagram Protocol)**: Lightweight, connectionless, best-effort.

Each application chooses the protocol that fits its needs:

- Streaming video may prefer **UDP** (low latency, can tolerate loss).
- File transfer needs **TCP** (reliability is crucial).

---

### 4. **Transport vs. Network Layer**

Here’s the subtle but critical distinction:

- **Network Layer (IP):** Provides **host-to-host logical communication**.
    - It doesn’t know about individual processes.
    - Example: It can deliver a datagram to Host B, but it doesn’t know *which* application on Host B should receive it.
- **Transport Layer:** Provides **process-to-process logical communication**.
    - Adds **port numbers** to differentiate between applications running on the same host.
    - Example: On Host B, it ensures the datagram goes to the right application (e.g., web browser vs. email client).

---

### 5. **Household Analogy (Important Conceptual Aid)**

Imagine two houses (hosts) on opposite coasts, each with a dozen kids (application processes).

- The **postal service (network layer)** delivers letters between houses (host-to-host). It doesn’t care which kid inside the house the letter is meant for.
- Inside each house, one child (Ann on West Coast, Bill on East Coast) is responsible for **collecting and distributing letters** (transport layer).
    - Ann collects letters from her siblings, gives them to the postal carrier, and distributes incoming letters back to the right sibling.
    - From the kids’ perspective, Ann *is* the mail system.

👉 Mapping the analogy:

- Application messages = letters
- Processes = kids
- Hosts = houses
- Transport layer (Ann & Bill) = process-to-process communication
- Network layer (postal service) = host-to-host communication

---

### 6. **Service Models and Constraints**

- The transport layer can offer **different service models** depending on the protocol.
    - Reliable vs. unreliable delivery
    - With or without congestion control
    - With or without security/encryption
- BUT the transport layer’s capabilities are **constrained by the underlying network layer**.
    - If the network doesn’t guarantee bounded delay or bandwidth, the transport layer cannot magically create those guarantees.
    - However, the transport layer can **add features not provided by the network layer**:
        - Reliability (retransmissions, acknowledgments).
        - Error checking (checksums).
        - Multiplexing/demultiplexing using ports.
        - Security (TLS/SSL over TCP, or transport-layer encryption).

---

### 7. **Example of Added Services**

Even if the network layer (IP) is **unreliable** (it may drop, duplicate, or corrupt packets), the transport layer (e.g., TCP) can provide **reliable data transfer** to applications.

- This is done via **sequence numbers, acknowledgments, and retransmissions**.

Similarly, while IP offers no encryption, the transport layer (or above it) can use cryptographic mechanisms to ensure **confidentiality**.

- Example: HTTPS = HTTP over TLS over TCP.

---

# 📌 Key Takeaways

1. The **transport layer provides logical, process-to-process communication**, while the network layer only provides host-to-host delivery.
2. Transport-layer protocols live only in **end systems**, not routers.
3. Applications choose between multiple transport protocols (TCP vs. UDP), each with a different **service model**.
4. Transport-layer services are **constrained by network-layer capabilities** but can **enhance them** (reliability, security, etc.).
5. The **household analogy** illustrates this layered responsibility well.

---

💡 **Side Note (real-world relevance):**

When you connect to a website (say, *google.com*):

- **IP (network layer):** Ensures your packet gets to Google’s servers.
- **TCP (transport layer):** Ensures your browser process and Google’s web server process can talk reliably.
- **TLS (security at transport/application boundary):** Ensures no one else can read or tamper with that communication.

---

Would you like me to **draw and explain the full “encapsulation/decapsulation” process (application → transport → network → link → physical)**, showing exactly how a message is wrapped/unwrapped at each layer? That would make this relationship crystal clear.

Alright, let’s dive deep into **Section 3.1.2 – Overview of the Transport Layer in the Internet**.

This is a critical section because it lays the foundation for understanding how applications *actually communicate* over the Internet.

---

# 📖 Lecture: Overview of the Transport Layer in the Internet

## 1. The Role of the Transport Layer

The transport layer sits directly above the **network layer** (IP).

- **Network layer (IP)**: Provides **host-to-host** communication. It delivers data from one machine to another.
- **Transport layer (TCP/UDP)**: Extends this to **process-to-process** communication. It ensures that the message reaches the *right application* (process) on the destination host.

Think of it this way:

- IP = gets the package to the correct building.
- Transport layer = delivers the package to the correct apartment inside that building.

---

## 2. The Two Transport-Layer Protocols in the Internet

The Internet provides **two major transport protocols**:

### a) **UDP – User Datagram Protocol**

- **Service model**: Unreliable, connectionless.
- **Features**:
    - No connection setup.
    - No guarantee of delivery.
    - No guarantee of order.
    - No guarantee of data integrity (beyond a checksum).
- **Key services it *does* provide**:
    - **Process-to-process delivery** (via port numbers).
    - **Error detection** (via checksum).
- **Practical use**: Applications that value **speed over reliability**, e.g. DNS queries, video streaming, VoIP, online gaming.

> ⚡ Side Note: Because UDP is "unregulated," applications can send at any rate and for any duration. This makes UDP highly flexible, but also potentially dangerous (e.g., can be used in DDoS attacks).
> 

---

### b) **TCP – Transmission Control Protocol**

- **Service model**: Reliable, connection-oriented.
- **Features**:
    - Establishes a connection before data transfer.
    - Guarantees reliable delivery of data.
    - Preserves order of bytes (in-sequence delivery).
    - Performs **error detection** and retransmission.
    - Provides **flow control** (prevents sender from overwhelming receiver).
    - Provides **congestion control** (prevents sender from overwhelming the network).
- **Practical use**: Applications where reliability is critical, e.g. HTTP/HTTPS (web traffic), email (SMTP), file transfers (FTP).

> ⚡ Side Note: TCP is complex because it implements reliability on top of IP, which itself is unreliable. We’ll later study mechanisms like sequence numbers, acknowledgments, sliding windows, and congestion control algorithms (AIMD, Slow Start, etc.).
> 

---

## 3. Relationship Between IP and the Transport Layer

- **IP (Internet Protocol)** provides only a **best-effort delivery service**:
    - No guarantee of delivery.
    - No guarantee of order.
    - No guarantee of integrity.
- This makes IP **unreliable**.

The **transport layer protocols (UDP & TCP)** sit on top of IP and extend its services to applications:

- **UDP**: Adds just port numbers + error detection.
- **TCP**: Adds reliability, ordering, flow control, and congestion control.

---

## 4. Terminology – Segments vs Datagrams

- **Transport-layer packet** = **segment** (both TCP & UDP).
- **Network-layer packet** = **datagram**.
- Some RFCs say:
    - TCP packet → *segment*.
    - UDP packet → *datagram*.
    - Network packet → *datagram*.
        
        → This causes confusion, so textbooks simplify by using:
        
    - Transport-layer = *segment*.
    - Network-layer = *datagram*.

> ⚡ Important exam note: Always clarify whether the term “datagram” refers to the network-layer unit or specifically to a UDP packet.
> 

---

## 5. Transport Layer Services Summary

Here’s a quick service model comparison:

| Layer | Service Provided |
| --- | --- |
| **IP** | Host-to-host delivery (unreliable, best-effort) |
| **UDP** | Process-to-process delivery + error detection (unreliable, connectionless) |
| **TCP** | Reliable, ordered, process-to-process delivery + flow control + congestion control |

---

## 6. Why UDP vs TCP?

- When designing an application, developers **must choose** between UDP and TCP.
- **Factors to consider**:
    - **Reliability needs**: Email requires TCP; video streaming may prefer UDP.
    - **Latency sensitivity**: UDP avoids handshake/ack overhead.
    - **Network friendliness**: TCP plays nicely with congestion control; UDP does not.

> ⚡ Side Note: Some modern protocols (like QUIC, used in HTTP/3) build reliability and security on top of UDP to bypass TCP’s limitations while still ensuring ordered, reliable delivery.
> 

---

# 🎯 Key Takeaways

1. **IP is unreliable** → Transport layer (UDP/TCP) extends services to processes.
2. **UDP** = lightweight, unreliable, connectionless → fast but no guarantees.
3. **TCP** = heavyweight, reliable, connection-oriented → guarantees delivery & fairness.
4. **Transport layer provides process-to-process delivery** (multiplexing/demultiplexing).
5. Application developers must choose between UDP and TCP depending on requirements.

---

✅ Now that we’ve established this foundation, the next step (Section 3.2) is to dive into **multiplexing and demultiplexing**, i.e., *how exactly does the transport layer deliver data to the correct application process?*

Would you like me to continue the lecture into **3.2 Multiplexing and Demultiplexing**, or pause here so you can digest this part first?

Perfect — now we are moving into **Section 3.2: Multiplexing and Demultiplexing**, which is one of the *most fundamental* concepts in the transport layer. Without this, you wouldn’t be able to run multiple applications on your computer at the same time over the Internet. Let’s carefully unpack this.

---

# 📖 Lecture: Transport-Layer Multiplexing and Demultiplexing

---

## 1. Motivation and Definition

The **network layer (IP)** provides **host-to-host delivery**:

- It ensures that a packet (IP datagram) goes from Host A → Host B.
- But remember: Host B might be running **many different applications simultaneously**.

Example: You are downloading a webpage (HTTP), uploading a file (FTP), and running two terminal sessions (Telnet). That’s **four active processes**.

So the question is: *When an IP datagram arrives at Host B, how does the transport layer know which application process should receive the data inside it?*

👉 This is the job of **transport-layer multiplexing and demultiplexing**.

- **Multiplexing**: At the sender, the transport layer takes data chunks from multiple sockets (processes), adds transport-layer headers (including addressing info like port numbers), and hands segments to the network layer.
- **Demultiplexing**: At the receiver, the transport layer examines header fields (source/destination port numbers, IP addresses), identifies the correct socket, and delivers the data to the appropriate process.

---

## 2. Sockets as the Interface

- A **socket** is the API between the transport layer and the application process.
- A process doesn’t directly receive data from the transport layer — instead, data flows **through a socket**.
- Since a host can have multiple sockets simultaneously, each socket needs a **unique identifier**.

👉 This identifier depends on whether the socket is UDP or TCP:

- UDP socket: identified by **(Destination IP, Destination Port)**.
- TCP socket: identified by **(Source IP, Source Port, Destination IP, Destination Port)**.

---

## 3. How Multiplexing/Demultiplexing Works

Each **transport-layer segment** includes **two key fields**:

- **Source Port Number (16 bits)**
- **Destination Port Number (16 bits)**

Port numbers range from **0 to 65535**:

- **0–1023** → *Well-known ports*, reserved for standard applications.
    - e.g., HTTP (80), HTTPS (443), FTP (21), SMTP (25), DNS (53).
- **1024–65535** → *Ephemeral (dynamic) ports*, typically assigned to clients automatically.

> ⚡ Side Note: The governing body for port numbers is IANA (Internet Assigned Numbers Authority), and the official registry is continuously updated.
> 

**Mechanism:**

- At the sender:
    - The application gives data to the transport layer.
    - The transport layer adds **source and destination ports** → creates a segment.
    - The network layer encapsulates it into an IP datagram.
- At the receiver:
    - The IP layer delivers the datagram to the transport layer.
    - The transport layer looks at the **destination port**.
    - It chooses the corresponding socket → passes data to the correct application process.

---

## 4. Connectionless Multiplexing/Demultiplexing (UDP)

UDP provides a **very simple scheme**:

- A **UDP socket** is identified by only **(Destination IP, Destination Port)**.
- This means:
    - If two segments arrive at Host B with the same **destination IP and port**, they both go to the **same UDP socket**, even if their source differs.
    - Example: DNS server at port 53 will receive queries from *any* client, all mapped to the same socket.

> ⚡ Practical Note: UDP is often used in stateless protocols like DNS, SNMP, DHCP. These servers just respond to queries — they don’t maintain per-client state.
> 

### Example:

- Host A sends UDP data from port **19157** to Host B’s port **46428**.
- Transport layer at A → fills in **src port = 19157, dest port = 46428**.
- Host B receives the datagram → transport layer looks at dest port 46428 → delivers to the socket bound to port 46428.

**Source Port Number role**:

- Acts as part of the *return address*.
- If Host B wants to reply, it uses the source port from the incoming segment as the destination port of the reply.

---

## 5. Connection-Oriented Multiplexing/Demultiplexing (TCP)

TCP is more **sophisticated** than UDP, because it maintains **separate connections**.

- A **TCP socket** is identified by a **four-tuple**:
    1. Source IP address
    2. Source Port number
    3. Destination IP address
    4. Destination Port number

This allows multiple simultaneous connections to the same server process.

### Example:

- Suppose multiple clients connect to the same web server (port 80).
- Even if all connections go to **dest port 80**, the **source IP + source port** make each connection unique.
- This enables the server to maintain **per-client TCP connections**.

> ⚡ Side Note: This is why your browser can open many tabs to the same website — each tab establishes a distinct TCP connection, identified by a different ephemeral source port.
> 

---

## 6. TCP Server Socket Workflow

- A **server** typically has a **welcoming socket** (bound to a well-known port, e.g., 80).
- When a connection request arrives, the server:
    - Accepts it.
    - Creates a **new socket** bound to the client’s **(src IP, src port)** and server’s own **(dest IP, dest port)**.
- Now, all subsequent communication on this connection uses this **new dedicated socket**.

This allows a server to handle **many clients simultaneously** while listening on a single well-known port.

---

## 7. Security Note: Port Scanning

- Since servers wait on known ports, attackers can probe them using **port scanning tools** (like `nmap`).
- Port scanning identifies which services are running (e.g., port 21 open → FTP server running).
- This is useful for administrators but also dangerous if services have vulnerabilities.
    - Example: Slammer worm exploited Microsoft SQL server on port 1434.

> ⚡ Practical note: This is why firewalls block unused ports and intrusion detection systems monitor unusual scanning activity.
> 

---

## 8. Multiplexing Example (HTTP Sessions)

Consider:

- Host C opens two HTTP sessions to server B.
- Host A opens one HTTP session to server B.

All three connections go to **server B, port 80**.

- Host C uses two different source ports: 26145, 7532.
- Host A uses source port 26145 (same number as Host C’s first session).

Not a problem! Because:

- Connection is identified by **(src IP, src port, dest IP, dest port)**.
- Even if the **source port number overlaps**, the **source IP differs**, so the connections remain unique.

---

# 🎯 Key Takeaways

1. **Multiplexing/Demultiplexing** = delivering transport-layer segments to the correct process using **port numbers**.
2. **UDP sockets** are identified by **(Dest IP, Dest Port)** → all clients to the same server port share one socket.
3. **TCP sockets** are identified by **(Src IP, Src Port, Dest IP, Dest Port)** → each connection is unique.
4. **Well-known ports** (0–1023) are reserved for standard applications (HTTP 80, FTP 21, etc.).
5. **Ephemeral ports** (1024–65535) are dynamically assigned for client processes.
6. Port scanning is both a useful admin tool and a common attack method.

---

✅ With this understanding, you now see how the transport layer enables **multiple simultaneous communications** on a host — and how TCP and UDP differ in their multiplexing strategy.

Would you like me to continue into **Section 3.3 – UDP** (where we examine UDP in detail, including checksum and segment structure), or pause here so you can review Multiplexing/Demultiplexing?

Great set of questions — this dives right into the **transport layer (UDP/TCP), socket addressing, and how browsers handle concurrency**. Let’s go step by step.

---

### 🔹 1. Source Port in UDP

You are correct — **the source port alone cannot identify the sender**.

- A **port number** is only meaningful **in the context of an IP address**.
- To uniquely identify a communication endpoint, we need both:
    - **Source IP address** (which device sent the packet)
    - **Source port number** (which application/process on that device sent it)

👉 Together with the **Destination IP + Destination Port**, this forms the **4-tuple**:

```
(Source IP, Source Port, Destination IP, Destination Port)

```

This **4-tuple uniquely identifies a UDP or TCP session**.

- Example:
    
    ```
    (192.168.1.10, 51532, 93.184.216.34, 80)
    
    ```
    
    - Source IP = your laptop
    - Source port = 51532 (ephemeral port chosen by OS)
    - Destination IP = example.com
    - Destination port = 80 (HTTP server)

So:

- **Source port alone is not enough**
- But in combination with the source IP, it pinpoints the sender.

---

### 🔹 2. Why Do We Even Need Source Ports?

Because **many apps on one device share the same IP**.

- Example: your phone’s IP = `10.0.0.5`
- WhatsApp, YouTube, Spotify all send traffic from that one IP.
- The **port number** tells the OS/network stack which process should receive incoming replies.

So when the server replies, it sends the response back to:

```
(Destination IP = your device IP, Destination Port = your ephemeral port)

```

Your OS uses the destination port to forward the packet to the right application socket.

---

### 🔹 3. Browsers and Multiple Tabs

Here’s where it gets interesting:

- A **browser** can open **many simultaneous TCP connections** (or UDP for QUIC).
- Each **tab** may represent a different session, possibly to different servers.
- Even if multiple tabs connect to the same server, **they don’t always reuse the same TCP connection**.

### How it works:

- Your OS chooses an **ephemeral source port** for each new connection.
- Example with Chrome and 3 tabs to `example.com`:
    
    ```
    Tab 1: (192.168.1.10, 51532) → (93.184.216.34, 443)
    Tab 2: (192.168.1.10, 51533) → (93.184.216.34, 443)
    Tab 3: (192.168.1.10, 51534) → (93.184.216.34, 443)
    
    ```
    
- All use the same destination IP:port (server’s HTTPS listener), but different source ports.

### Important Notes:

- Modern browsers use **connection pooling and multiplexing**:
    - **HTTP/1.1** → multiple TCP connections per domain, limited by browser rules (often 6).
    - **HTTP/2** → one TCP connection can serve multiple streams (so tabs might share one).
    - **HTTP/3 (QUIC)** → multiplexed streams over one UDP connection.

So:

- **Yes, each tab can use its own source port** if needed.
- But depending on protocol (HTTP/2, HTTP/3), multiple tabs **may share a single connection**.

---

### 🔹 4. Putting It All Together

- **Source port alone** is meaningless → need source IP + source port.
- Browsers manage multiple tabs by opening **multiple connections with different ephemeral ports** (for HTTP/1.1), or by **multiplexing streams** (for HTTP/2/3).
- The OS socket layer ensures responses come back to the right tab/process.

---

✅ Quick analogy:

Think of **your IP address** as your street address, and **ports** as apartment numbers inside the building.

- Without the apartment number (port), the mail (packets) can’t reach the right resident (application).
- Without the street address (IP), the postman (network) doesn’t even know which building to go to.

---

Would you like me to **draw out a step-by-step example flow** showing how opening two browser tabs to the same site looks in terms of ports and IPs, so you can visualize it better?

Excellent question — you’re spotting a subtle but very important point about **multiplexing vs. connections** in TCP. Let’s carefully go through this.

---

## 🔑 First: The Basics

- TCP is a **connection-oriented protocol**:
    
    Each TCP connection is uniquely identified by a **4-tuple**:
    
    ```
    (Source IP, Source Port, Destination IP, Destination Port)
    
    ```
    
- Normally, every time your browser connects to a new server, it **must** perform a TCP 3-way handshake to establish a new connection.

---

## 🌍 Case 1: Multiple Servers

If you open 3 tabs and each loads a site from a **different server** (say google.com, openai.com, youtube.com):

- You absolutely need **3 separate TCP connections** because each server has a different (IP, Port).
- There’s no way around it — each handshake is required.

✅ You’re correct: you can’t reuse one TCP connection across different servers.

---

## 🌐 Case 2: Multiple Requests to the **Same Server**

This is where **demultiplexing over one connection** comes into play.

### **HTTP/1.0**

- Each request (e.g., an image, a CSS file) → required a new TCP connection.
- Very inefficient.

### **HTTP/1.1**

- Introduced **persistent connections**: multiple requests can reuse the *same TCP connection*.
- But requests had to go **one after the other** (head-of-line blocking).
- Browsers often opened several parallel TCP connections (e.g., 6 per domain) to speed things up.

### **HTTP/2**

- A game changer.
- Still uses a **single TCP connection** per server, but it introduces **streams**:
    - Each HTTP request/response pair is assigned a **stream ID**.
    - These streams are multiplexed over one TCP connection.
    - The TCP layer just sees “bytes”; the HTTP/2 framing layer demultiplexes the streams.

### **HTTP/3**

- Similar idea but uses **QUIC** (built on UDP instead of TCP).
- Provides multiplexing at the transport layer itself, avoiding TCP’s head-of-line blocking.

---

## 🎯 So, to answer your points:

> Wouldn’t we need to do a TCP handshake for every server?
> 
- ✅ Yes. Every distinct `(IP, Port)` pair = new TCP handshake. No way around it.

> How can multiple tabs to the same server work?
> 
- If they connect to **different servers** → separate TCP connections.
- If they connect to the **same server**:
    - Old HTTP/1.1: likely multiple TCP connections (limited number).
    - Modern HTTP/2/3: often just **one TCP connection** reused across all tabs. Inside it, HTTP/2 multiplexing keeps streams separate.

> How does demultiplexing work?
> 
- TCP delivers a raw byte stream.
- The application layer protocol (HTTP/2 or QUIC) defines **framing**: special headers/IDs that tell which bytes belong to which stream.
- So from the server’s perspective, it’s like having dozens of parallel requests over a single socket.

---

👉 Imagine it like this:

- TCP gives you **a pipe full of bytes**.
- HTTP/2 or QUIC says: “Okay, I’ll mark which bytes belong to tab A, which belong to tab B, etc.”
- The browser demultiplexes them before delivering to each tab.

---

Would you like me to **draw a side-by-side timeline of how HTTP/1.1 vs HTTP/2 vs HTTP/3 handle multiple tabs**? That makes the difference much clearer.

Perfect — this is a great point in the course to dive into how **web servers and TCP/UDP** actually work in practice, tying in both the theoretical definitions and the real-world engineering considerations. Let’s treat this as a lecture.

---

# 📖 Lecture: Web Servers and Transport Protocols (TCP vs UDP)

---

## 1. **Web Servers and Port Numbers**

- A **web server** (like Apache or Nginx) typically runs on **port 80** (HTTP) or **port 443** (HTTPS).
- When a client (e.g., your browser) sends an HTTP request, the **destination port** is always 80 (or 443 for HTTPS).
- The **source port** is chosen dynamically by the client OS (an ephemeral port).

Thus, many different clients can all connect to the same web server at the same time because the server distinguishes them by their unique **(source IP, source port)** pair.

👉 **Important definition**:

A TCP connection is uniquely identified by a **4-tuple**:

```
(Source IP, Source Port, Destination IP, Destination Port)

```

- This means that even though *all* requests are sent to port 80 on the server, the **source information** ensures connections don’t get mixed up.

---

## 2. **How the Server Handles Connections**

### Old Model (Process per Connection)

- Traditionally, servers like Apache would **spawn a new process** for each connection.
- Each process had its **own socket**, handling requests from a single client.
- This works but is heavy — processes consume memory and CPU.

### Modern Model (Threads / Event-driven)

- Today, high-performance servers use **one main process** with:
    - **Threads**: lightweight subprocesses, each with its own socket.
    - **Event-driven architectures** (like Nginx, Node.js): a single process handles thousands of sockets asynchronously.

👉 **Key detail**: There is not always a 1-to-1 mapping between connections and processes. Instead, connections are mapped to **sockets** and managed efficiently by the OS and server software.

---

## 3. **Persistent vs Non-Persistent HTTP**

### Non-Persistent HTTP (HTTP/1.0 style)

- Each request/response pair requires a **new TCP connection**.
- Example: a webpage with 10 images → 11 TCP handshakes.
- Each connection = create socket, do 3-way handshake, transmit, close socket.
- **Problem**: overhead is huge, servers waste resources constantly opening/closing sockets.

### Persistent HTTP (HTTP/1.1)

- A **single TCP connection** can be reused for multiple HTTP requests/responses.
- This dramatically reduces overhead.
- Still, HTTP/1.1 was limited — requests often had to be processed one after the other (head-of-line blocking).

### HTTP/2 and HTTP/3 (Modern)

- **HTTP/2**: Multiplexes multiple requests/responses over **one TCP connection** using *stream IDs*.
- **HTTP/3**: Uses **QUIC** (built on UDP), avoiding TCP’s limitations while still providing reliability at a higher layer.

👉 **Takeaway**: Persistent connections (and multiplexing in HTTP/2/3) solve the inefficiency of non-persistent connections.

---

## 4. **Why Use UDP at All?**

UDP is a very **minimal transport protocol** (defined in RFC 768). It only provides:

1. Multiplexing/demultiplexing (via ports).
2. Lightweight error detection (via checksum).

**No guarantees**: No reliability, no ordering, no congestion control. UDP is called **connectionless** because there’s no handshake, no state, no connection setup.

### Applications that Prefer UDP

Some applications actually benefit from UDP’s simplicity:

- **DNS**: speed is more important than reliability (queries are tiny and can be retried).
- **VoIP (Internet telephony)**: can tolerate some packet loss but not delay.
- **Streaming video, real-time conferencing**: prefer timely delivery over retransmission.
- **QUIC (used in HTTP/3)**: runs over UDP but re-implements reliability and congestion control in user space, avoiding TCP’s handshake and head-of-line blocking.

### Advantages of UDP

- ✅ Finer control over data timing (send immediately, no congestion delay).
- ✅ No connection establishment (faster startup).
- ✅ No connection state (lightweight, scalable).
- ✅ Small header (8 bytes vs TCP’s 20 bytes).

---

## 5. **Reliability Over UDP**

Although UDP itself is unreliable, applications can build their **own reliability mechanisms**:

- **Acknowledgments** and **retransmissions** at the application layer.
- QUIC is the best real-world example: it builds a reliable, secure transport protocol on top of UDP.

---

## 6. **Risks of UDP**

- Because UDP lacks **congestion control**, too much UDP traffic can flood the network and starve TCP flows.
- Example: If everyone streamed high-bitrate video over raw UDP, routers would drop packets excessively, crippling both UDP and TCP applications.
- Many ISPs and enterprises therefore throttle or block UDP traffic for security and stability reasons.

---

## 7. **Summary Table (Applications and Protocols)**

| Application | Typical Transport |
| --- | --- |
| Email (SMTP) | TCP |
| Remote login (SSH, Telnet) | TCP |
| Web (HTTP/HTTPS) | TCP (HTTP/1.1, 2), UDP (HTTP/3/QUIC) |
| File transfer (FTP) | TCP |
| DNS (name translation) | UDP |
| Internet telephony | UDP (sometimes TCP) |
| Streaming video/audio | UDP or TCP |
| Network management (SNMP) | UDP |

---

# 🎓 Key Takeaways

1. **Web servers use port numbers (80/443)** and distinguish clients by source IP and source port.
2. **Sockets** are the key abstraction — each TCP connection gets its own socket. Modern servers use threads or event-driven I/O rather than processes.
3. **Persistent HTTP** and later **HTTP/2/3 multiplexing** solve the inefficiency of non-persistent connections.
4. **UDP is connectionless** and minimal — no reliability, no congestion control.
5. **Some applications prefer UDP** (DNS, VoIP, streaming, QUIC) because speed and timing are more important than guaranteed delivery.
6. **Reliability can be built on top of UDP** (e.g., QUIC).
7. **Uncontrolled UDP** can overwhelm networks, which is why congestion control (even if app-level) is critical.

---

📌 Side Note for Students:

Think of TCP as “heavyweight, safe, reliable courier service” and UDP as “lightweight, fast, but risky postcard delivery.” Sometimes you need the guaranteed courier, sometimes you just want the postcard delivered as fast as possible, even if a few get lost.

---

Would you like me to now **draw the TCP vs UDP packet structure** (header fields) and show how minimal UDP really is compared to TCP? That’s often a great way to visualize their fundamental differences.





Excellent — let’s dive deep into this and treat it like a proper lecture. We are now in the section where **reliable data transfer (rdt) protocols** are introduced. This is one of the most fundamental building blocks of networking, because everything from TCP to higher-level applications ultimately relies on the principles we’ll explore here.

I’ll structure this lecture as follows:

1. **Context & Motivation** – why we need reliable protocols.
2. **rdt1.0 – Reliable transfer over a perfect channel**
3. **rdt2.0 – Handling channels with bit errors**
4. **The big problem with corrupted ACK/NAK**
5. **Introducing sequence numbers and the way forward**

---

## 1. Context & Motivation

The job of the **transport layer** is to deliver data **reliably** from one process to another, even though the **underlying network is unreliable**.

* IP itself is *best-effort*: it does not guarantee delivery, order, or integrity.
* Thus, if we want reliable communication, the transport layer must build mechanisms on top of this unreliable foundation.

To study this systematically, computer networking textbooks (like Kurose & Ross) present a sequence of **simplified reliable data transfer protocols (rdt)**. Each one addresses a new challenge:

* rdt1.0: No errors, no losses → trivial protocol.
* rdt2.0: Errors can occur → add error detection, ACKs, NAKs.
* rdt2.1 / rdt2.2: Handle corrupted ACK/NAK packets.
* rdt3.0: Handle packet *losses* in addition to errors.

This progressive approach shows you how **TCP itself evolved** conceptually.

---

## 2. rdt1.0 – Reliable Transfer over a Perfect Channel

This is the trivial case:

* The channel is assumed to be **perfectly reliable**.
* No packet gets lost, no corruption occurs, and the receiver is always fast enough to process data.

### Sender FSM (Finite State Machine)

* One state: “wait for data from above.”
* Event: `rdt_send(data)` → the sender creates a packet with `make_pkt(data)` and calls `udt_send(packet)` to send it into the channel.
* Then loops back to the same state.

### Receiver FSM

* One state: “wait for packet from below.”
* Event: `rdt_rcv(packet)` → extract data with `extract(packet, data)` and deliver it up using `deliver_data(data)`.

**Key Points:**

* There’s no acknowledgment mechanism, no retransmissions, no sequence numbers.
* This is only a thought experiment — in the real world, physical channels are *never* this perfect.

---

## 3. rdt2.0 – Reliable Data Transfer over a Channel with Bit Errors

Now we move to a more realistic channel model:

* Packets are always delivered in order.
* But **bits inside a packet may be corrupted** during transmission.

Think of this as when you’re talking over a noisy phone line — words get garbled.

### The solution: **Automatic Repeat reQuest (ARQ) protocols**

The class of protocols designed to handle this problem are called **ARQ protocols**. They rely on three key mechanisms:

1. **Error detection**

   * Using techniques like checksums, CRCs, or parity.
   * Extra bits are added to the packet so the receiver can check whether corruption occurred.
   * Example: UDP uses an Internet checksum field.

2. **Receiver feedback**

   * Receiver must tell the sender what it saw:

     * **ACK (Acknowledgment)** → packet was received correctly.
     * **NAK (Negative Acknowledgment)** → packet was corrupted, please resend.
   * These control messages are themselves small packets.

3. **Retransmission**

   * If the receiver signals that a packet was corrupted (NAK), the sender resends it.

### rdt2.0 Sender FSM

* Two states:

  1. **Wait for call from above**: Get data, make a packet with checksum, send it. Transition → waiting for ACK/NAK.
  2. **Wait for ACK/NAK**:

     * If ACK received: Go back to waiting for new data.
     * If NAK received: Retransmit the same packet, stay in this state until ACK arrives.

This is a **stop-and-wait protocol** → the sender cannot send a new packet until the current one is ACKed.

### rdt2.0 Receiver FSM

* Single state: wait for packet.
* On receiving a packet:

  * If corrupted → send NAK.
  * If correct → extract data, deliver it to the upper layer, and send ACK.

---

## 4. The Fatal Flaw in rdt2.0 – What If ACK/NAK is Corrupted?

Here’s the catch:

* We assumed ACK and NAK packets themselves are always reliable.
* But what if the **feedback** is corrupted?

Example:

* Sender transmits packet → receiver gets it correctly, sends back an ACK.
* ACK gets corrupted in transit → sender thinks it was a NAK and retransmits.
* Now the receiver gets a duplicate packet!

This introduces **ambiguity**:

* The receiver cannot tell if the new packet is a duplicate retransmission or new data.
* The sender cannot tell if the receiver actually got the packet correctly.

This is a serious issue — if left unresolved, it breaks correctness.

---

## 5. The Fix: Sequence Numbers

The elegant solution is to add a **sequence number field** to each data packet.

* Every packet is tagged with a **sequence number** (e.g., 0, 1, 2 …).
* The receiver uses the sequence number to distinguish:

  * A **new packet** vs.
  * A **retransmission of the previous packet**.

For a simple stop-and-wait protocol:

* A **1-bit sequence number** is sufficient (0 and 1 alternating).
* Why? Because the sender only has one outstanding unacknowledged packet at a time.

With this, even if the ACK/NAK is corrupted, the receiver can safely tell whether a retransmitted packet is a duplicate or truly new data.

---

## 🔑 Key Takeaways So Far

* **rdt1.0**: trivial, reliable channel, no feedback needed.
* **rdt2.0**: introduces **error detection, ACKs/NAKs, retransmission** → but flawed if ACK/NAK itself is corrupted.
* **Sequence numbers** are the critical next step to resolve the ambiguity of duplicates.

---

👉 In the next stage (rdt2.1 and rdt2.2), we’ll see how **sequence numbers are incorporated** and how the protocol evolves into something that actually works even when ACKs/NAKs are corrupted. Then later in rdt3.0, we’ll tackle the hardest case: **packet loss**.

---

Would you like me to **continue the lecture into rdt2.1 and rdt2.2 right now**, so we can fully flesh out the solution to the corrupted ACK/NAK problem?



Excellent — now we’re moving into the **real heart of reliable data transfer protocols**.
Let’s treat this as a lecture, as if I were walking you through this material in class.

---

# **Lecture: Reliable Data Transfer (rdt2.1 → rdt3.0)**

Up to this point, we’ve looked at simple versions of reliable data transfer:

* **rdt1.0** → Assumes a perfectly reliable channel. No errors, no losses. Trivial sender and receiver, each with one state.
* **rdt2.0** → Introduces *bit errors*. We need **checksums, ACKs, NAKs**, and retransmission. But it fails if the ACK/NAK themselves are corrupted.
* **rdt2.1** and **rdt2.2** → Fix the problems of rdt2.0 using **sequence numbers** and refined ACK handling.
* **rdt3.0** → Extends reliability to channels that can **both corrupt and lose packets**.

Now let’s go step by step through **rdt2.1 → rdt2.2 → rdt3.0**.

---

## **1. rdt2.1: Handling Corrupted ACKs/NAKs**

The problem with rdt2.0 was that **control packets (ACKs/NAKs) themselves could be corrupted**.
If that happens, the sender doesn’t know what to do.

### **The Fix: Sequence Numbers**

We solve this by **adding sequence numbers** to each data packet.

* The **sender** keeps track of which sequence number (0 or 1) it is sending.
* The **receiver** expects a specific sequence number next.
* If it gets the wrong one, it knows it’s a duplicate.

Why just 1 bit? Because this is still a **stop-and-wait protocol**:

* At most **one packet is outstanding** (either waiting for ACK or retransmission).
* So only two sequence numbers are needed: `0` and `1`, alternating. (Hence the name **alternating-bit protocol** later.)

### **Sender FSM (Figure 3.11)**

* Two states: *waiting to send seq=0*, *waiting to send seq=1*.
* Each state describes the packet currently being sent.
* If ACK received with matching sequence → move to next state.
* If NAK (or corrupted ACK/NAK) → resend packet.

### **Receiver FSM (Figure 3.12)**

* Two states: *waiting for seq=0* and *waiting for seq=1*.
* If expected packet arrives correctly → deliver data, send ACK.
* If corrupted packet arrives → send NAK.
* If out-of-order packet arrives (duplicate) → resend ACK for last correctly received packet.

⚡ **Key Takeaway**: rdt2.1 introduces **sequence numbers** and can now handle corrupted ACKs/NAKs without confusion.

---

## **2. rdt2.2: NAK-Free Protocol**

NAKs are not strictly necessary. Instead of saying “that was wrong” (NAK), the receiver can just say:

* “Last one I got correctly was seq=0.” (So implicitly: “I didn’t get seq=1.”)
* “Last one I got correctly was seq=1.” (So implicitly: “I didn’t get seq=0.”)

This eliminates the need for NAK packets.

### **Changes in rdt2.2**

* ACKs now **include the sequence number** of the packet being acknowledged.
* The sender checks the ACK’s sequence number to know whether the right packet was received.

So:

* **If ACK matches current packet’s seq** → move forward, send next packet.
* **If ACK repeats the previous packet’s seq** → that means receiver didn’t get the new one, so retransmit.

⚡ **Key Takeaway**: rdt2.2 uses **only ACKs** (with sequence numbers). Duplicate ACKs act as implicit NAKs.

This is very similar to how **modern TCP works**: TCP does not use NAKs, it relies on **duplicate ACKs** to infer loss.

---

## **3. rdt3.0: Dealing with Packet Loss**

Now we add another layer of realism: in real networks, not only can packets be corrupted, they can also be **lost entirely**.

### **The Problem**

If a data packet or its ACK gets lost, the sender might wait forever.

### **The Fix: Timers**

The sender starts a **countdown timer** whenever it sends a packet.

* If an ACK arrives in time → stop the timer.
* If the timer expires before an ACK is received → retransmit the packet.

This is the **classic retransmission timeout (RTO) mechanism** used in TCP.

### **Sender FSM (Figure 3.15)**

* On `rdt_send(data)`: send packet, start timer.
* On `rdt_rcv(ACK)` with correct seq: stop timer, advance state.
* On corrupted ACK or wrong ACK: resend packet.
* On `timeout`: resend packet, restart timer.

### **Receiver FSM** (not shown in the book, but straightforward):

* Same as rdt2.2 — expect seq=0 or seq=1.
* Deliver data only if expected packet arrives correctly.
* Send ACK with sequence number of received packet.

---

## **4. Practical Example**

Imagine sending data over Wi-Fi (which often drops packets):

1. **Sender (seq=0)** sends a packet. Starts timer.
2. **Receiver** gets it correctly. Sends `ACK0`.
3. Sender gets `ACK0` → stops timer, moves to seq=1.
4. **Next packet (seq=1)** is sent. But this time, packet is **lost**.
5. Sender waits… timer expires. Retransmits packet with seq=1.
6. Receiver gets it (seq=1), delivers data, sends `ACK1`.
7. Sender gets `ACK1` → moves to seq=0 again.

---

## **5. Putting It All Together**

At this point, we have developed the **essential components of reliable data transfer**:

* **Checksums** → Detect bit errors.
* **Sequence Numbers** → Detect duplicates, distinguish new vs retransmitted data.
* **ACKs (with sequence numbers)** → Provide receiver feedback.
* **Timers** → Recover from lost packets or ACKs.

This combination is exactly the foundation of **TCP’s reliable transport mechanism** — though TCP adds more complexity (sliding windows, cumulative ACKs, fast retransmit, congestion control, etc.).

⚡ **Key Takeaway**: rdt3.0 = the alternating-bit protocol with timers.
It is the **simplest complete reliable transport protocol**.

---

✅ **Summary Table**

| Protocol   | Channel Type                   | Features                                |
| ---------- | ------------------------------ | --------------------------------------- |
| **rdt1.0** | Perfectly reliable             | Just send/receive, trivial              |
| **rdt2.0** | Bit errors                     | Checksum, ACK/NAK, retransmit           |
| **rdt2.1** | Bit errors + corrupted ACK/NAK | Sequence numbers, ACK+NAK               |
| **rdt2.2** | Bit errors + corrupted ACK/NAK | Sequence numbers, **ACK only (no NAK)** |
| **rdt3.0** | Bit errors + packet loss       | Sequence numbers, ACK only, **timers**  |

---

Now, here’s a question for you (like I’d ask in class):
👉 If rdt3.0 already has **sequence numbers and timers**, why does TCP need **more complex mechanisms** like sliding windows and congestion control?

Would you like me to continue the lecture into that direction, so you see how this simple rdt foundation grows into **TCP’s full reliability model**?


Alright, let’s go into full **professor mode** here and break this down carefully. We’re now moving into one of the *most important* leaps in the design of reliable transport protocols: moving from **stop-and-wait (rdt3.0)** to **pipelined protocols** like **Go-Back-N (GBN)** and later **Selective Repeat (SR)**.

This is crucial because *protocol efficiency* is no longer about just being correct (rdt2.x, rdt3.0 gave us correctness) but about **performance** in real networks where round-trip times (RTTs) can dwarf transmission times.

---

# 🚦 Lecture: Pipelined Reliable Data Transfer Protocols & Go-Back-N (GBN)

---

## 1. The Problem with Stop-and-Wait (rdt3.0)

Recall **rdt3.0**:

* Sender transmits *one* packet.
* Waits for an ACK or times out.
* If ACK is good → send next.
* If ACK lost or packet lost → timeout → retransmit.
* **Sequence numbers alternate (0,1) → "alternating-bit protocol".**

✅ Functionally correct.
❌ Performance: *horrible* in high-speed, high-delay networks.

### Example:

* East coast ↔ West coast:

  * RTT = 30 ms
  * Transmission rate R = 1 Gbps
  * Packet size L = 1000 bytes (8000 bits)
  * Transmission delay: `d_trans = L/R = 8 µs` (tiny compared to RTT)

So:

* It takes **30 ms** to deliver one packet (waiting for ACK),
* During which the sender only spent **0.008 ms actually transmitting**.
* Utilization:

  $$
  U = \frac{L/R}{RTT + L/R} \approx 0.00027
  $$

  → **0.027% channel utilization!**

👉 That means with a **1 Gbps link**, we’re effectively only getting \~**267 kbps throughput**.

> **Takeaway:** Stop-and-wait wastes nearly all the bandwidth. It’s like sending a single truck across the country, then waiting for it to come back empty before sending the next one.

---

## 2. The Solution: Pipelining

**Pipelining** means:

* Don’t wait for an ACK before sending the next packet.
* Allow multiple *unacknowledged* packets to be "in flight" at once.
* This "pipeline" of packets fills the bandwidth-delay product of the network.

Visual (stop-and-wait vs pipelining):

* Stop-and-wait: channel is mostly empty.
* Pipelined: channel is full, ACKs and data flow continuously.

---

## 3. Consequences of Pipelining

When we allow multiple packets outstanding, new requirements appear:

1. **Sequence Numbers Must Expand**

   * Stop-and-wait used only {0,1}.
   * Pipelined: must support a range of numbers large enough to uniquely identify all *in-flight packets*.
   * With window size N, we need more than log₂(N) bits.

2. **Sender Buffers Packets**

   * Because packets may need retransmission (if ACK lost).
   * Sender must store unacknowledged packets.

3. **Receiver May Need Buffers**

   * If packets arrive out of order (depends on protocol, e.g., Selective Repeat vs GBN).
   * Must decide whether to **discard** or **buffer** them.

4. **Error Recovery Becomes More Complex**

   * With stop-and-wait: lost → retransmit the single packet.
   * With pipelining: which packets do we retransmit? All outstanding ones (GBN)? Or just the missing one (SR)?

---

## 4. Go-Back-N (GBN) Protocol

### Key Idea:

* Sender maintains a **window of size N**.
* Can transmit up to N *unacknowledged* packets.
* If a packet is lost or delayed, the sender **goes back and retransmits all packets from that one onwards**.

---

### 4.1 Sender’s View of the Window

Define:

* **base** = seq # of the oldest unacknowledged packet.
* **nextseqnum** = seq # of the next packet to send.

Then:

* `[0 … base-1]`: already acknowledged.
* `[base … nextseqnum-1]`: sent, but not ACK’d.
* `[nextseqnum … base+N-1]`: free slots, can send immediately.
* `[base+N … ]`: not usable yet (must wait).

This is called a **sliding window** protocol.

---

### 4.2 Sender Events

1. **rdt\_send(data)** (application passes data to send):

   * If window not full: create packet, send it, add to buffer.
   * If `base == nextseqnum`, start timer (for oldest unACK’d).
   * If window full: refuse or buffer data until space frees up.

2. **ACK Received**:

   * ACK is **cumulative** (important!).
   * ACK(n) → means receiver has received all packets up to n.
   * Slide window forward, free space.
   * If no unACK’d packets remain, stop timer. Otherwise, restart.

3. **Timeout**:

   * Sender retransmits **all packets in window**.
   * This is why it’s called **Go-Back-N**: we go back and retransmit from base onwards.

---

### 4.3 Receiver Events

Receiver is very simple in GBN:

* If packet is correct **and in order** (seq == expected): deliver data, send ACK, increment expectedseqnum.
* Else: discard packet, resend ACK for the *last in-order packet received*.

👉 Receiver does NOT buffer out-of-order packets.

* Simplicity: only one variable (expectedseqnum).
* Downside: wasted bandwidth if we discard valid but out-of-order packets.

---

### 4.4 Example (Window Size = 4)

1. Sender sends pkt0, pkt1, pkt2, pkt3.
2. Suppose pkt2 is lost.

   * Receiver gets pkt0, pkt1 → sends ACK1.
   * Receiver gets pkt3 → sees it’s out of order → discards, resends ACK1.
   * Same for pkt4, pkt5 → discards, resends ACK1.
3. Sender eventually times out (on pkt2), retransmits pkt2..pkt5.
4. Receiver accepts pkt2 (delivers), then pkt3, pkt4, pkt5 in order.

---

## 5. Analysis of GBN

### Advantages:

* Efficient compared to stop-and-wait.
* Simple receiver design (no buffering).
* Works well if loss probability is low.

### Disadvantages:

* **Inefficient under high loss**:

  * One lost packet causes retransmission of *all subsequent packets in the window*, even if they were received correctly.
  * This wastes bandwidth.

This inefficiency motivates **Selective Repeat (SR)**, where receiver buffers out-of-order packets and sender retransmits only missing ones.

---

## 6. Real-World Note

* TCP (which we’ll study soon) is essentially a **sliding window protocol**, borrowing heavily from GBN and SR.
* It uses **cumulative ACKs** like GBN but also has features (fast retransmit, selective acknowledgment options) that move it closer to SR.
* TCP also dynamically adapts its window size based on **congestion control**, which we’ll discuss later.

---

# 📌 Summary of Key Terms to Remember

* **Stop-and-wait**: one outstanding packet, terrible utilization.
* **Pipelining**: multiple in-flight packets to fill bandwidth-delay product.
* **Window size (N)**: max # of outstanding, unACK’d packets.
* **Sliding window**: sender view moves as ACKs arrive.
* **Cumulative ACKs**: one ACK acknowledges all previous packets.
* **GBN**: on timeout, retransmit *all packets from base onwards*.
* **Receiver in GBN**: simple, discards out-of-order packets.

---

👉 Next, we’ll cover **Selective Repeat (SR)**, which improves efficiency under loss by letting the receiver buffer out-of-order packets.

---

Would you like me to continue this lecture directly into **Selective Repeat (SR)** (3.4.4), so you see the contrast with GBN immediately?



Perfect — now we’re moving into **Selective Repeat (SR)**, which is the natural evolution after Go-Back-N (GBN). I’ll treat this as a formal lecture, tying the book’s content (your provided text) with my deeper commentary, side notes, and practical networking insights.

---

# 📖 Lecture: Selective Repeat (SR) – Principles of Reliable Data Transfer

---

## 1. The Motivation: Why Selective Repeat?

We saw earlier that **GBN (Go-Back-N)** solves the utilization problem of stop-and-wait by allowing multiple packets in flight. But GBN has an inefficiency:

* With **window size N** and a **large bandwidth-delay product**, the sender may have hundreds or thousands of packets “in flight.”
* If **one packet is lost**, GBN forces the sender to **retransmit that lost packet plus *all subsequent packets in the window***, even if the receiver had already received them correctly.

📌 **Analogy**:
Imagine dictating 1,000 words to a scribe. If the scribe mishears *one* word, you repeat **all 1,000 words again**. Highly redundant, highly inefficient.

This inefficiency **grows worse as error probability increases** — the pipeline gets clogged with **unnecessary retransmissions**.

👉 Enter **Selective Repeat (SR):**
Instead of retransmitting entire chunks, retransmit **only the packets actually lost or corrupted**.

---

## 2. Core Idea of Selective Repeat (SR)

* The **sender retransmits selectively** (only lost packets).
* The **receiver acknowledges packets individually**, not cumulatively as in GBN.
* The **receiver buffers out-of-order packets** until missing ones arrive, then delivers them in order to the upper layer.

This is more complex but far more efficient in error-prone environments.

---

## 3. Sender and Receiver Windows in SR

Like GBN, SR uses a **sliding window of size N**, but:

* The **sender’s window** allows up to N unACK’d packets in flight.
* The **receiver’s window** also spans N sequence numbers (not just one expected packet, like GBN).

### Figure 3.23 (Sender & Receiver View of Sequence Numbers)

* **Sender view**:

  * Left edge = `send_base` (oldest unACK’d).
  * Right edge = `send_base + N - 1`.
  * ACKs slide the window forward selectively.

* **Receiver view**:

  * Left edge = `rcv_base` (lowest not-yet-delivered).
  * Receiver accepts any packet in `[rcv_base, rcv_base+N-1]`.
  * Out-of-order packets are buffered until the gap is filled.

📌 **Key Difference from GBN:**
GBN receiver discards out-of-order packets.
SR receiver **buffers them**.

---

## 4. SR Sender Behavior (Figure 3.24)

Sender actions:

1. **Data received from above**:

   * If sequence number within window → send, buffer for possible retransmit.
   * If outside window → either buffer for later or refuse until window slides.

2. **Timeout**:

   * Unlike GBN, **each packet has its own timer**.
   * Only the timed-out packet is retransmitted.
   * ⚡ Implementation trick: one hardware timer can simulate many logical timers \[Varghese 1997].

3. **ACK received**:

   * Mark packet as received if in window.
   * If ACK = `send_base`, slide window forward to the next unACK’d packet.
   * If window slides, new packets (previously outside the window) can now be sent.

---

## 5. SR Receiver Behavior (Figure 3.25)

Receiver actions:

1. **Packet in window `[rcv_base, rcv_base+N-1]` and correct**:

   * Send ACK for it.
   * If not previously received → buffer it.
   * If it equals `rcv_base`, deliver it and any consecutive buffered packets upwards, then slide window forward.

2. **Packet in `[rcv_base-N, rcv_base-1]` (already ACK’d)**:

   * Still reACK it!
   * Why? Because ACKs can be lost in the channel, and without this reACK the sender may stall forever.

3. **Otherwise (outside of window)**: ignore.

📌 **Note**: This reACKing rule is essential. If the receiver didn’t reACK old packets, the sender might never advance its window if an ACK was lost.

---

## 6. Example Walkthrough (Figure 3.26)

Scenario:

* Sender sends pkt0..pkt5.
* pkt2 is lost.

Receiver:

* Gets pkt0, pkt1 → delivers them, ACK0, ACK1.
* Gets pkt3, pkt4, pkt5 → buffers them, sends ACK3, ACK4, ACK5.
* Waits for pkt2.

Sender:

* Times out on pkt2 → retransmits pkt2.
* Receiver gets pkt2 → now has pkt2..pkt5 in buffer.
* Delivers pkt2, pkt3, pkt4, pkt5 in order to upper layer.

✅ Only pkt2 was retransmitted (vs. GBN, which would’ve retransmitted pkt2–pkt5).

---

## 7. Subtle Challenge: Window Size vs Sequence Number Space

In practice, sequence numbers are **finite** (e.g., 0–3 if only 2 bits).

Problem: sender and receiver may lose sync about whether a packet is a *new transmission* or a *retransmission of an old packet*.

### Example (Figure 3.27):

* Sequence numbers = {0,1,2,3}.
* Window size = 3.

Case A: ACKs lost → sender retransmits pkt0 (old data).
Case B: ACKs arrive → sender sends pkt4 (new data, seq#0).

Receiver sees a packet with seq#0. But is it the old pkt0 retransmission, or the new pkt4?
→ Receiver can’t distinguish.

📌 **Rule**: For SR to work correctly:

$$
\text{Window size } N \leq \frac{1}{2} \times \text{Sequence Number Space}
$$

This guarantees sender/receiver don’t confuse old and new packets.

---

## 8. Practical Implications

* **SR is more efficient** than GBN under high loss, but more **complex**:

  * Requires per-packet timers.
  * Receiver must buffer out-of-order packets.
  * Sequence number management is trickier.

* **Real networks (TCP)**:

  * TCP is somewhere between GBN and SR.
  * Uses **cumulative ACKs** (like GBN).
  * But with **Selective Acknowledgment (SACK)** extensions, it can behave more like SR.

---

## 9. Key Terms to Remember

* **SR (Selective Repeat)**: retransmit *only* lost/corrupted packets.
* **Sender window**: maintains N outstanding packets, per-packet timers.
* **Receiver window**: buffers out-of-order packets, delivers in order when gaps are filled.
* **ACK rules**: individual ACKs, reACK previously acknowledged packets.
* **Window constraint**: $N \leq \frac{1}{2} \times \text{SeqNumSpace}$.

---

✅ With SR, we have completed the family of **reliable data transfer (rdt) protocols**:

* rdt1.0–rdt3.0 (stop-and-wait, correctness).
* GBN (efficiency with cumulative ACKs).
* SR (efficiency with selective retransmissions).

Now you see the *big picture*: every mechanism we’ve added was to address real-world constraints of **delay, loss, duplication, and reordering**.

---

Would you like me to next build a **comparison table (Stop-and-Wait vs GBN vs SR)** so you can clearly see their trade-offs side by side — the kind of thing examiners love to ask?

