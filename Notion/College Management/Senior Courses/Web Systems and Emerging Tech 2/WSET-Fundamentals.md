-----

## 💻 Fundamentals: Introduction to the Internet

### Learning Objectives

Before we begin, let's look at what you will be able to master by the end of this session.

> **Original Text:** After completing this chapter, you will be able to:
>
>   * Define Internet
>   * Explain how the internet works
>   * Describe and explain AJAX, XML, and JSON

**Deep Dive:** These three points are the pillars of modern web development. You'll not only be able to *define* the Internet, but you'll understand its history, its core protocols, and the architecture that allows it to function globally. We will then spend significant time dissecting **AJAX (Asynchronous JavaScript and XML/JSON)**, **XML (Extensible Markup Language)**, and **JSON (JavaScript Object Notation)**, which are crucial for dynamic, responsive web applications.

-----

## 📜 A Brief Introduction to the Internet

The Internet wasn't born overnight. It was the result of decades of research and interconnected projects.

### ARPANet (1 of 3)

> **Original Text:** ARPANet. Network constructed by the US Department of Defense’s **Advanced Research Project Agency** that connected a dozen ARPA-funded research laboratories and universities. Used **packet switching** to allow multiple computers to communicate on a single network. The first node of the network was established at **UCLA in 1969**. Primary early use was simple text-based communications through electronic mail.

**Deep Dive:** The **ARPANet** is considered the direct ancestor of the Internet. The key innovation here is **packet switching**. Prior to this, communication networks used **circuit switching** (like an old telephone system), which dedicated a physical line for the entire duration of a connection, even during silences.

**Packet switching** breaks data into small, labeled blocks (**packets**) that can travel along different, optimized paths and be reassembled at the destination. This made the network far more efficient, robust, and capable of handling failures. The establishment of the first node at **UCLA in 1969** marks the theoretical start of the Internet.

### BITNET and CSNET (2 of 3)

> **Original Text:**
>
>   * **BITNET**: Acronym for **Because It’s Time Network** that began in City University of New York. Initially built to provide electronic mail and file transfers.
>   * **CSNET**: Acronym for **Computer Science Network**. Connected the University of Delaware, Purdue University, the University of Wisconsin, the RAND Corporation, and Bolt, Beranek, and Newman (Massachusetts-based research company). Initial purpose was to provide electronic mail.

**Deep Dive:** While ARPANet focused on defense and high-end research, networks like **BITNET** and **CSNET** arose in the late 70s and early 80s to expand communication to the wider academic community. They were critical for popularizing the use of **electronic mail (email)** and fostering the collaborative environment that would later define the global Internet. They demonstrated the sheer utility of network communication beyond its military roots.

### NSFNet (3 of 3)

> **Original Text:** **NSFNet**. New national network created in 1986 sponsored by the **National Science Foundation**. Initially connected the NSF-funded supercomputer centers that were at five universities. By 1990, replaced ARPANet for most nonmilitary uses. By 1992, connected more than one million computers globally. In 1995, a small part of NSFNet returned to being a research network, the rest became known as the **Internet**.

**Deep Dive:** The **NSFNet** was the crucial step in transitioning the network from a government/military project to a public infrastructure. The NSF's decision to connect its five **supercomputer centers** and, crucially, to allow *traffic not related to supercomputing* (a policy called **acceptable use**), led to massive, explosive growth. When the NSF officially decommissioned its backbone in 1995, the network was fully privatized and commercialized—this is the point where the name **Internet** truly entered the global lexicon.

-----

## 🌐 What is the Internet?

> **Original Text:** The **Internet** is a huge collection of computers connected in a communications network. These computers are of every imaginable size, configuration, and manufacturer. **Transmission Control Protocol/Internet Protocol (TCP/IP)** allows diverse devices to communicate with each other. It became the standard for computer network communications in 1982.

**Deep Dive:** At its core, the Internet is a global network of interconnected devices—servers, routers, PCs, phones, and so on. The key to making all these disparate systems talk to each other is the **TCP/IP** suite.

  * **IP (Internet Protocol):** Handles the **addressing** and **routing** of the data packets, ensuring they get from the source to the destination. It's the postal service, handling the addressing.
  * **TCP (Transmission Control Protocol):** Handles the **reliable delivery** of data. It breaks the data into packets, reassembles them at the destination, and requests re-transmission of any lost packets. It's the verification system that ensures the letter *arrived* complete and in order.

**TCP/IP** is the fundamental language of the Internet, standardized since 1982.

-----

## 📍 Internet Protocol Addresses and Domain Names

To communicate, every device on the Internet needs a unique identifier.

### Internet Protocol Addresses

> **Original Text:** The **Internet Protocol (IP) address** of a machine connected to the Internet is a unique **32-bit number**. IP addresses are usually written as four 8-bit numbers, separated by periods. The four parts are separately used by Internet-routing computers to decide where a message must go next to get to its destination. ... In late 1998, a new IP standard **IPv6**, was approved which expanded the address size from **32 bits to 128 bits**.

**Deep Dive:**

1.  **IPv4 (32-bit):** A 32-bit number can represent approximately $4.3$ billion addresses ($2^{32}$). This format is familiar as **dotted-decimal notation**, e.g., `192.168.1.1`. As the number of devices exploded, we quickly ran out of available IPv4 addresses—this is known as **IPv4 address exhaustion**.
2.  **IPv6 (128-bit):** Approved in 1998, **IPv6** addresses the exhaustion problem. A 128-bit address allows for an astronomical number of addresses, about $3.4 \times 10^{38}$. IPv6 is now the preferred standard, though transition is ongoing.

### Domain Names and DNS

> **Original Text:** Due to the difficulty remembering numbers, machines on the internet also have **textual names**. These names begin with the name of the host machine, followed by progressively larger enclosing collections of machines called **domains**. ... **Fully qualified domain name example:** `movies.marxbros.comedy.com`

> **Original Text:** **Domain Name System**
> Domain name conversion


**Deep Dive:** While computers prefer the numerical IP address, humans prefer easy-to-remember names. This is where the **Domain Name System (DNS)** comes in. DNS is essentially the **phone book of the Internet**.

  * A **Fully Qualified Domain Name (FQDN)** like `www.google.com` is hierarchical, read from right-to-left:
      * `.com` is the **Top-Level Domain (TLD)**.
      * `google` is the **Second-Level Domain**.
      * `www` is the specific **host** or **subdomain**.
  * **The DNS Process:** When you type a domain name into your browser, a **DNS Resolver** on your network queries a hierarchy of **Name Servers** to find the correct IP address associated with that domain name. Once the IP address is returned, your browser can then connect directly to the server. This conversion is what the **DNS system** is dedicated to doing.

-----

## ⚙️ Different Protocols (Pre-Web)

The early Internet had a variety of distinct protocols, each for a single purpose.

> **Original Text:**
>
>   * **telnet** – protocol developed in 1969 to allow user on one computer on the Internet to log onto and use another computer on the Internet
>   * **File Transfer Protocol (ftp)** – developed to transfer files among computers on the Internet
>   * **Usenet** – developed to serve as an electronic bulletin board
>   * **mailto** – developed to allow messages to be sent from the user of one computer to other users of the computer on the Internet
>
> This variety of protocols, each having their own interface and useful only for which it was designed, restricted the growth of the Internet.

**Deep Dive:** These protocols were revolutionary for their time, but they were disjointed. If you wanted to check email, transfer a file, and log into a remote computer, you had to use three completely different applications with three different interfaces. This fragmentation was the barrier that needed to be overcome for the Internet to become a mass medium.

-----

## 🕸️ The World Wide Web (The Breakthrough)

> **Original Text:** In 1989, **Tim Berners-Lee** proposed a new protocol for the Internet, which suggested a way to let all users, but particularly scientists, browse each other’s papers on the Internet. He developed **HTML, URLs, and HTTP.**

**Deep Dive:** The creation of the **World Wide Web (WWW)** was the pivotal moment. Tim Berners-Lee, while working at **CERN**, solved the fragmentation problem by creating a universal system for **linking and accessing** documents.

  * **HTML (HyperText Markup Language):** The common document format for structuring content.
  * **HTTP (HyperText Transfer Protocol):** The unified protocol for transferring documents.
  * **URLs (Uniform Resource Locators):** The standardized addressing system for finding resources.

This was the combination that allowed a single application—the **Web Browser**—to access all the diverse information on the Internet through a consistent interface.

-----

## 🏛️ Basic Web Architecture

The Web operates on a fundamental **client-server model**.

> **Original Text:** The web is a **two-tiered architecture**. A **web browser** displays information content, A **web server** that transfers information to the client.

> **Original Text:**
>
>   * **Web Browser:** A software application for retrieving, presenting, and traversing information resources on the World Wide Web.
>   * **Web Server:** A computer program that accepts HTTP requests and return HTTP responses with optional data content, OR a computer that runs this program.

**Deep Dive:** The **Web Browser** (the **client**) initiates the communication, and the **Web Server** responds. This is a crucial concept. The client is typically your device, running software like Chrome or Firefox. The server is a powerful computer, often running software like Apache or Nginx, that stores the website's files. The communication between them is governed by **HTTP**.

### Universal Resource Identifier (URI)

> **Original Text:** **URIs** are used for two different purposes:
>
>   * To name a resource (**Uniform Resource Names** - URNs)
>   * To provide a path to, or location, of a resource (**Uniform Resource Locators** - URLs)

**Deep Dive:** A **URI** is the general term for an identifier of a resource. A **URL** is the specific type of URI that tells you *how* and *where* to find a resource.

> **Original Text (URL structure):** It contains four distinct parts: the **protocol type**, the **machine name**, the **directory path** and the **file name**.

**Example:** $[https://www.example.com/products/item.html$](https://www.google.com/search?q=https://www.example.com/products/item.html%24)

| Part | Example Component |
| :--- | :--- |
| **Protocol Type** | `https` (HTTP Secure) |
| **Machine Name** | `www.example.com` (Domain Name) |
| **Directory Path** | `/products/` |
| **File Name** | `item.html` |

-----

## 🗣️ The HyperText Transfer Protocol (HTTP)

HTTP is the lifeblood of the Web—it's a text-based protocol that governs how clients and servers exchange information.

> **Original Text:** HTTP is an application-level protocol for distributed, collaborative, hypermedia information systems. HTTP is a **request/response standard** of a client and a server.

### Request and Response Structure

> **Original Text (Request Message):** The request message consists of the following: **Request line**, **Headers** (Accept-Language, Accept, ….), **An empty line**, **An optional message body**.

A typical exchange looks like this:

1.  The **Client** sends an **HTTP Request Message** (e.g., "Give me the file at this URL").
2.  The **Server** processes the request.
3.  The **Server** sends an **HTTP Response Message** (e.g., the requested file, or an error).

### Request Methods (Verbs)

These methods indicate the **desired action** the client wants the server to perform.

| Method     | Original Text Description                                                                                          | **Deep Dive/Action**                                                                             |
| :--------- | :----------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------- |
| **GET**    | Requests a representation of the specified resource. Requests using GET should only retrieve data.                 | **READ/RETRIEVE** data from the server. (The most common request.)                               |
| **POST**   | Used to submit an entity to the specified resource, often causing a change in state or side effects on the server. | **CREATE** new data on the server. Used for submitting forms, uploading files.                   |
| **HEAD**   | Asks for a response identical to that of a GET request, but without the response body.                             | Gets only the **metadata** (headers) of the resource, useful for checking if a file has changed. |
| **PUT**    | Replaces all current representations of the target resource with the request payload.                              | **UPDATE/REPLACE** an existing resource entirely.                                                |
| **DELETE** | Deletes the specified resource.                                                                                    | **DELETE** a specified resource.                                                                 |
| **PATCH**  | Used to apply partial modifications to a resource.                                                                 | **PARTIALLY UPDATE** a resource. (More efficient than PUT for small changes.)                    |

> **Original Text (Safety):** **HEAD, GET, OPTIONS and TRACE are defined as safe (no side effects).** POST, PUT and DELETE are intended for actions which may cause side effects on the server.

**Deep Dive (Safety):** A **safe method** is one that is designed *not* to change the state of the server. You should be able to issue a **GET** request repeatedly without worry. **POST, PUT, and DELETE** are **unsafe** because they modify or create data, causing side effects.

### Status Lines (The Server's Reply)

The first line of the server's response is the **status line**, which contains a numerical code that tells the client the result of the request.

| Code Range | Category              | **Deep Dive Meaning**                                                                               |
| :--------- | :-------------------- | :-------------------------------------------------------------------------------------------------- |
| **1xx**    | Informational         | The request was received and the process is continuing.                                             |
| **2xx**    | **Success**           | The action was successfully received, understood, and accepted. (**200 OK** is the most common.)    |
| **3xx**    | Redirection           | The client must take immediate action to complete the request (e.g., the page moved to a new URL).  |
| **4xx**    | **Client-Side Error** | The request contains bad syntax or cannot be fulfilled (**404 Not Found** is the most famous).      |
| **5xx**    | **Server-Side Error** | The server failed to fulfill an apparently valid request (**500 Internal Server Error** is common). |

-----

## 🔒 HTTP Session State Management

> **Original Text:** **HTTP is a stateless protocol.** Hosts do not need to retain information about users between requests. Statelessness is a scalability property. For example, when a host needs to customize the content of a website for a user.

**Deep Dive:** **Statelessness** means every single request (GET, POST, etc.) is handled by the server as a completely new, independent transaction. The server forgets everything about the previous request. This is good for **scalability** (any server can handle any request), but it makes maintaining a "session" (like a user logging in or adding items to a shopping cart) impossible without adding mechanisms.

The solutions to this problem are: **Cookies, Sessions, Hidden variables in forms,** and **URL encoded parameters**.

### Cookies

> **Original Text:** A **cookie** is a small piece of text stored on a user's computer by a web browser. A cookie consists of one or more name-value pairs containing bits of information such as user preferences. It is sent as an HTTP header by a web server to a web browser and then sent back unchanged by the browser each time it accesses that server.

**Deep Dive:** A cookie is the server's way of saying, "Here's a unique ID. Hold onto this, and show it to me every time you come back." The browser stores it and sends it with every subsequent request. This is how the server can maintain state and remember a user's login status or shopping cart items.

### Sessions

> **Original Text:** A **session** is a reference to a certain time frame for communication between two devices... A session can temporarily store information related to the activities of the user while connected.

**Deep Dive:** The **session** is a concept for **server-side storage**. The server stores the *actual* user data (e.g., a list of items in a cart) in its memory or database, and then typically uses a **cookie** (often called a **session ID cookie**) to link the incoming request to the correct data stored on the server. This is generally more secure than storing all data directly in the cookie itself.

-----

## 🧑‍💻 From Static to Dynamic: Web Architecture Evolution

The basic two-tier architecture of a browser and server quickly evolved to handle more complex, data-driven applications.

> **Original Text:** This basic web architecture is fast evolving to serve a wider variety of needs beyond static document access and browsing. **CGI** extends the architecture to **three-tiers** by adding a **back-end server** that provides services to the Web server.

### Common Gateway Interface (CGI)

> **Original Text:** **CGI** is a standard protocol for interfacing external application software with a web server. CGI programs are executable programs that run on the Web server. The CGI program typically returns **HTML pages that it constructs on the fly**.

**Deep Dive (The Three-Tier Model):** CGI was the first popular method to create truly **dynamic web pages** (pages whose content changes based on user input or database data).

1.  **Client Tier (Browser):** Makes the request.
2.  **Web Server Tier (Web Server + CGI Program):** Receives the request and executes the external CGI program (e.g., a script written in Perl or Python).
3.  **Data Tier (Database Server):** The CGI program often connects here to retrieve/store data.

This **three-tier architecture** separates the presentation (browser), application logic (CGI/server-side code), and data storage (database), which is the standard model for complex, database-driven websites today.

### Client-side vs. Server-side Processing

In modern web systems, work is distributed between the client and the server.

| Aspect | **Server-side Processing** (e.g., PHP, Python, Java) | **Client-side Processing** (e.g., JavaScript) |
| :--- | :--- | :--- |
| **Location** | Executes on the Web Server. | Executes in the Web Browser. |
| **Purpose** | Creates the dynamic page content, accesses databases, handles sensitive business logic. | Handles user interaction, UI updates, input validation, and making asynchronous requests. |
| **Benefit** | Secure, powerful access to resources, universal compatibility across browsers. | Faster, more responsive user experience (no server round-trip needed for small tasks). |

-----

## 🔄 AJAX: The Modern Web Paradigm

> **Original Text:** **AJAX: Asynchronous JavaScript and XML**. Ajax isn’t a technology. It’s really several technologies, each flourishing in its own right, coming together in powerful new ways. Ajax incorporates: **XHTML and CSS, Document Object Model, XML and XLST, XMLHttpRequest, JavaScript.**

**Deep Dive:** AJAX is not a single language or technology; it's an **approach** or **technique** for building interactive web applications. It allows the browser to communicate with the server in the **background**—**asynchronously**—without interrupting or reloading the current page.

**The Key Component:** The **XMLHttpRequest** object (or the modern `fetch()` API) allows JavaScript to send HTTP requests and receive responses *in the background*.

**The AJAX Flow:**

1.  User clicks a button (e.g., "Like").
2.  **JavaScript** sends an AJAX request to the server.
3.  The server processes the request (e.g., records the "Like" in the database).
4.  The server sends back only the necessary data (e.g., "Success, new count is 101").
5.  **JavaScript** updates only the relevant part of the page (e.g., changing the like count to 101) without a full page reload.

### Drawbacks of AJAX

> **Original Text:** It breaks browser history engine (Back button). No bookmark. The same origin policy. Ajax opens up another attack vector for malicious code that web developers might not fully test for.

**Deep Dive:** Early AJAX applications had these issues, which modern frameworks (using the History API) have largely mitigated.

  * **Back/Bookmark Issues:** Since only a small part of the page changes, the browser's history entry might not update, leading to a confusing back button experience.
  * **Same Origin Policy (SOP):** A critical security measure that restricts a document or script loaded from one origin (e.g., `site.com`) from interacting with a resource from another origin (e.g., `othersite.com`). This prevents malicious scripts from easily reading data from other sites you are logged into.

-----

## 🧩 XML and JSON: Data Interchange Formats

Web services and AJAX need standardized formats to exchange data between the client and server. **XML** and **JSON** are the two dominant standards.

### XML (Extensible Markup Language)

> **Original Text:** **XML** is a universally agreed **markup meta-language** primarily used for information exchange. The two primary building blocks of XML are **elements** and **attributes**. Elements are tags and have values. Elements are structured as a **tree**.

**Deep Dive:** XML is *extensible* because you define your own tags (unlike HTML, which has fixed tags). It's designed to **describe data**, not display it.

> **Sample XML:**
>
> ```xml
> <?xml version="1.0" encoding="UTF-8"?>
> <person>
>   <id type="integer">1111</id>
>   <last_name>Smith</last_name>
>   <first_name>John</first_name>
>   > </person>
> ```

  * **Elements:** `<person>`, `<id>`, `<last_name>`
  * **Attribute:** `type="integer"` on the `<id>` element.

XML is verbose and human-readable, making it excellent for complex document structures, but its verbosity adds overhead (larger file size).

### JSON (JavaScript Object Notation)

> **Original Text:** **JSON** is a lightweight computer data interchange format. JSON is based on a subset of the **JavaScript programming language**. It is considered a language-independent data format. It serves as an alternative to the use of the XML format.

**Deep Dive:** JSON is derived from JavaScript's object literal syntax, but it is **language-independent**—meaning any modern programming language can easily read and write JSON.

> **Sample JSON:**
>
> ```json
> {
> "firstName": "John",
> "lastName": "Smith",
> "address": {
>   "street": "21 2nd Street",
>   "city": "New York",
>   ...
>   },
> "phoneNumbers":[ "212 555-1234", "646 555-4567" ]
> }
> ```

  * **Structure:** Data is stored in **key-value pairs** (like an object or associative array) and **ordered lists** (like an array).

JSON has largely replaced XML for use in AJAX and web services because it is **lighter**, **less verbose**, and a more natural fit for JavaScript and many other modern programming languages, leading to faster parsing and data transfer.

-----

I trust this comprehensive breakdown has given you a strong foundation in Web Systems and Technologies. We've covered the history, the core protocols, the architecture, and the crucial data formats that enable the dynamic web.

Do you have any questions about the difference between IPv4 and IPv6, or perhaps a particular HTTP method you'd like to explore in more detail?