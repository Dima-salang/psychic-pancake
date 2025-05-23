  

## Network Application Architectures

  

An application developer will likely choose between two architectural paradigms:

  

### Client-Server Architecture

- there is an always-on host, called the server which services requests from many other hosts, called clients.
- Often, a single-server host is incapable and so, data centers are often used to create a powerful virtual server.

### P2P Architecture

- there is minimal (or no) reliance on dedicated servers in data centers. instead, the application exploits direct communication between pairs of intermittently connected hosts called peers.
- one of the compelling features of P2P is ti its self-scalability. they are also cost effective. but they also face challenges of security, performance, and reliability due to their highly decentralized structure.

  

## Processes Communicating

- A process is a program that is running within an end system. Processes on two different end systems communicate with each other by exchanging **messages** across the computer network.

  

### Client and Server Processes

- A network application consists of pairs of processes that send messages to each other over a network.
    - e.g. in a Web application a client browser process exchanges messages with a Web server process.
    - in a p2p file-sharing system, a file is transferred from a process in one peer to a process in another peer.

> [!important] _**In the context of a communication session between a pair of processes, the process that initiates the communication (that is, initially contacts the other process at the beginning of the session) is labeled as the client. The process that waits to be contacted to begin the session is the server.**_

  

### The Interface Between the Process and the Computer Network

- a process sends messages into, and receives messages from, the network through a software interface called a socket.
    - A process is analogous to a house and the socket to its door. When the process wants to send a message, it sends it through its door. This sending process assumes that there is a transportation infrastructure on the other side of its door that will transport the message to the door of the destination process.
- A socket is the interface between the application layer and the transport layer within a host. it is also referred to as the API between the application and the network, since the socket is the programming interface with which network application are built.
- The application developer has control of everything on the application-layer side of the socket but has little control of the transport-layer side of the socket.

![[/image 18.png|image 18.png]]

  

  

### Addressing Processes

- To identify the receiving process, we need
    - the address of the host; its IP address.
    - an identifier that specifies the receiving process in the destination host; its port number
        - In addition to knowing the address of the host, the sending process must also identify the receiving process (more specifically, the receiving socket) running in the host. This is needed because in general a host could be running many network applications.

  

## Transport Services Available to Applications

- the application at the sending side pushes messages through the socket. at the other side of the socket, the transport-layer protocol has the responsibility of getting the messages to the socket of the receiving process.
- When you develop applications, you must choose the available transport-layer protocols.
- We can broadly classify the possible services along four dimensions:

  

### Reliable Data Transfer

- something has to be done to guarantee that the data sent by one end of the application is delivered correctly and completely to the other end of the application. it is said to provide reliable data transfer.
- when a transport-layer protocol does not provide reliable data transfer, some of the data set may never arrive at the receiving process. this may be acceptable for loss-tolerant applications.

  

### Throughput

- throughput is the rate at which the sending process can deliver bits to the receiving process.
- because other sessions will be sharing the bandwidth along the network path, and that these other sessions will be coming and going, the available throughput can fluctuate with time. a transport-layer protocol could provide a guaranteed available throughput at some specified rate. with such a service, the application could request a guaranteed throughput or bits/sec, and the transport protocol would then ensure that the available throughput is always is always at least r bits/sec.
- applications that have throughput requirements are said to be **bandwidth-sensitive applications.** meanwhile, elastic applications can make use of as much, or as little, throughput as happens to be available.

  

### Timing

- transport-layer protocols can also provide timing guarantees. can be used by interactive real-time applications such as multiplayer games or internet telephony.
- an example guarantee might be that every bit that the sender pumps into the socket arrives at the receiver’s socket no more than 100 msec later.

  

### Security

- e.g. encryption before delivering data to the receiving process.

  

## Transport Services Provided by the Internet

- The Internet makes two transport protocols available to applications: UDP and TCP.

  

![[/image 1 13.png|image 1 13.png]]

  

  

### TCP Services

- _Connection-oriented service_. TCP has the client and server exchange transport-layer control information with each other before the application-level messages begin to flow. After the handshaking phase, a TCP connection is said to exist between the sockets of the two processes. The connection is a full-duplex connection. When the application finishes sending the messages, it must tear down the connection.
- Reliable data transfer service. the communicating processes can rely on TCP to deliver all data set without error and in the proper order. TCP also includes a congestion-control mechanism for the general welfare of the Internet rather than for the communicating processes. The TCP congestion control mechanism throttles a sending process when the network is congested between sender and receiver.

  

### UDP Services

- no-frills, lightweight transport protocol, providing minimal services.
- It is connectionless, so there is no handshaking before the two processes start to communicate. UDP provides an unreliable data transfer service—that is, UDP provides no guarantee that the message will ever reach the receiving process and that they may arrive out of order.

  

## Services Not Provided By Internet Transport Protocols

- There are no throughput or timing guarantees in TCP and UDP.
- Modern time-sensitive applications work fairly well because they have been designed to cope, to the greatest extent possible with these lack of guarantees.

  

  

## Application-Layer Protocols

- An application-layer procotol defines how an application’s processes, running on different end systems pass messages to each other.
    - The types of messages exchanged
    - The syntax of the message types
    - The semantics of the fields
    - Rules for determining when and how a process sends messages and responds to messages
- Some application-layer protocols are specified in RFCs and are therefore in the public domain. However, there are many proprietary application-layer protocols such as Skype’s/

  

## The Web and HTTP

  

HTTP is at the heart of the Internet because it is the protocol that runs and facilitates communication in the Web. Hyper Text Transfer Protocol or HTTP is a protocol that dictates how messages are structured, sent, and received between end systems in the Web. Specifically, these end systems are the clients and the servers. We can see HTTP in action in our everyday activities such as surfing the Web, scrolling through Facebook, or just visiting websites.

  

### **Introduction to HTTP**

HTTP, short for **Hypertext Transfer Protocol**, is the backbone of data communication on the web. It is an **application-layer protocol** in the OSI and TCP/IP models, specifically designed for transferring hypertext (documents, images, videos, etc.) over the internet. It operates on a **request-response model**, primarily between clients (like browsers) and servers (hosting websites or APIs).

HTTP was first introduced in 1991 as part of the World Wide Web by Tim Berners-Lee. Since then, it has undergone multiple updates:

1. **HTTP/0.9 (1991)**: A simple protocol for transferring raw text files.
2. **HTTP/1.0 (1996)**: Added support for metadata (headers), status codes, and content types.
3. **HTTP/1.1 (1997)**: Introduced persistent connections, pipelining, and caching improvements.
4. **HTTP/2 (2015)**: Focused on performance with multiplexing and binary framing.
5. **HTTP/3 (2022)**: Runs over QUIC (instead of TCP), reducing latency for faster and more secure communication.

---

### **Key Features of HTTP**

1. **Statelessness**:
    
    HTTP is inherently **stateless**, meaning each request-response pair is independent. The server doesn’t retain any information about previous interactions. This simplifies server design but necessitates additional tools like cookies or sessions for maintaining state (e.g., for user authentication).
    
2. **Human-Readable**:
    
    HTTP messages are text-based (at least up to HTTP/1.1), making them easy to debug and interpret.
    
3. **Extensibility**:
    
    HTTP can handle various content types (HTML, JSON, XML, images, etc.) and support numerous extensions via headers.
    

---

### **HTTP’s Position in the OSI and TCP/IP Models**

HTTP operates at the **application layer**, but it relies on lower layers for actual data transfer:

- **Transport Layer**: Uses **TCP** for reliable delivery or **QUIC** (in HTTP/3).
- **Network Layer**: Uses IP to route packets.
- **Link Layer**: Handles the physical transmission of data over cables, Wi-Fi, etc.

---

### **The Request-Response Cycle**

HTTP is built around a simple yet powerful **request-response cycle**. Let’s break it down:

### **1. The HTTP Request**

The client sends an HTTP request to a server. A typical HTTP request has the following components:

1. **Request Line**:
    
    ```Plain
    pgsql
    CopyEdit
    METHOD PATH HTTP-VERSION
    
    ```
    
    - **METHOD**: The HTTP method (e.g., GET, POST, PUT, DELETE) specifies the action to be performed.
    - **PATH**: The resource's path (e.g., `/index.html` or `/api/users`).
    - **HTTP-VERSION**: The protocol version (e.g., HTTP/1.1, HTTP/2).
    
    Example:
    
    ```Plain
    pgsql
    CopyEdit
    GET /index.html HTTP/1.1
    
    ```
    
2. **Headers**:
    
    Provide metadata about the request. Common headers include:
    
    - `Host`: Specifies the target domain (e.g., `example.com`).
    - `User-Agent`: Describes the client (e.g., browser type).
    - `Accept`: Specifies accepted response formats (e.g., `text/html`, `application/json`).
    
    Example:
    
    ```Plain
    makefile
    CopyEdit
    Host: example.com
    User-Agent: Mozilla/5.0
    Accept: text/html
    
    ```
    
3. **Body** (Optional):
    
    The body contains data sent to the server (e.g., form data or JSON payload in a POST request).
    

---

### **2. The HTTP Response**

The server processes the request and sends back a response with the following structure:

1. **Status Line**:
    
    ```Plain
    css
    CopyEdit
    HTTP-VERSION STATUS-CODE REASON-PHRASE
    
    ```
    
    - **STATUS-CODE**: Indicates the result of the request. Common codes include:
        - `200 OK`: Request succeeded.
        - `404 Not Found`: Resource not found.
        - `500 Internal Server Error`: Server-side error.
    - **REASON-PHRASE**: A textual explanation of the status code.
    
    Example:
    
    ```Plain
    CopyEdit
    HTTP/1.1 200 OK
    
    ```
    
2. **Headers**:
    
    Metadata about the response. Common headers include:
    
    - `Content-Type`: Specifies the media type (e.g., `text/html`, `application/json`).
    - `Content-Length`: Length of the response body in bytes.
    - `Set-Cookie`: Sets a cookie for the client.
    
    Example:
    
    ```Plain
    yaml
    CopyEdit
    Content-Type: text/html
    Content-Length: 2048
    
    ```
    
3. **Body** (Optional):
    
    Contains the actual resource (e.g., HTML, JSON, image data).
    

---

### **Common HTTP Methods**

HTTP defines several methods for different purposes. Let’s explore the most common ones:

1. **GET**:
    
    - Retrieves data from the server.
    - No request body; parameters are sent in the URL (query string).
    - Idempotent (repeated requests yield the same result).
    
    Example:
    
    ```Plain
    bash
    CopyEdit
    GET /api/users?id=123 HTTP/1.1
    
    ```
    
2. **POST**:
    
    - Submits data to the server (e.g., creating a resource).
    - Includes a request body with the data.
    
    Example:
    
    ```Plain
    bash
    CopyEdit
    POST /api/users HTTP/1.1
    Content-Type: application/json
    {
      "name": "Luis",
      "age": 21
    }
    
    ```
    
3. **PUT**:
    - Updates or replaces an existing resource.
    - Includes the entire updated resource in the request body.
4. **DELETE**:
    - Deletes a resource on the server.
5. **HEAD**:
    - Similar to GET, but only fetches headers, not the body. Useful for checking if a resource exists.

---

### **How HTTP Works: Step-by-Step**

1. **DNS Resolution**:
    
    The client resolves the domain (e.g., `example.com`) to an IP address via DNS.
    
2. **TCP/QUIC Connection**:
    - In HTTP/1.1 and HTTP/2, the client establishes a TCP connection to the server on port 80 (HTTP) or 443 (HTTPS).
    - HTTP/3 uses QUIC, which is faster and built on UDP.
3. **TLS Handshake (for HTTPS)**:
    
    If HTTPS is used, the client and server establish a secure channel via the TLS protocol.
    
4. **Request Transmission**:
    
    The client sends the HTTP request to the server.
    
5. **Server Processing**:
    
    The server processes the request (e.g., querying a database, running code).
    
6. **Response Transmission**:
    
    The server sends the response back to the client.
    
7. **Client Rendering**:
    
    The client (e.g., browser) processes the response, displaying the content to the user.
    

---

### **HTTP Versions**

### **HTTP/1.1**

- Default for most modern applications.
- **Persistent connections**: The same TCP connection can be reused for multiple requests, reducing overhead.
- **Chunked transfer encoding**: Allows data to be sent in chunks, useful for streaming.

### **HTTP/2**

- Introduced **multiplexing**, allowing multiple requests and responses to be sent over a single connection simultaneously.
- Uses binary framing for better performance.

### **HTTP/3**

- Runs over QUIC (built on UDP), reducing latency and improving reliability, especially in lossy networks.

---

### **Challenges and Solutions**

1. **Latency**:
    
    HTTP/2 and HTTP/3 mitigate latency using multiplexing and QUIC.
    
2. **Security**:
    
    HTTPS (HTTP + TLS) ensures encrypted communication to protect data from eavesdropping and tampering.
    
3. **Caching**:
    
    HTTP caching headers like `Cache-Control`, `ETag`, and `Expires` reduce unnecessary requests to the server.
    
4. **Compression**:
    
    HTTP/2 and HTTP/3 support header compression (HPACK or QPACK).