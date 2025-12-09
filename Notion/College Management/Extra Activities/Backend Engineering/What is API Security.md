Below are **clear, concise, professor-level lecture notes** covering everything in your provided text. They are structured so you can **study, revise, or teach** this topic easily.

---

# 📘 **Lecture Notes — What Is an API & API Security (Sections 1.2–1.3)**

*By your senior researcher/professor with 50+ years of experience*

---

## **1.2 What Is an API?**

### **Definition**

* An **API (Application Programming Interface)** is a *boundary* between software components.
* It defines **operations**, **data structures**, and **rules** that allow one component or system to interact with another.

### **Traditional APIs**

* Historically delivered as **software libraries**:

  * Linked **statically** or **dynamically** at runtime.
  * Examples:

    * **OpenGL** → 3D graphics
    * **TCP/IP libraries** → networking

### **Modern APIs**

* Increasingly provided as **remote services** over the internet, especially:

  * **RESTful Web Services** over HTTP.

### **Purpose**

* Enable functionality sharing across applications.
* Example: A **photo archive** API:

  * list albums
  * view photos
  * add comments
* Used by:

  * Web apps
  * Mobile apps
  * Other APIs
  * SaaS tools (e.g., a word processor embedding images)

### **APIs vs UIs**

| **API**                                           | **UI**                                         |
| ------------------------------------------------- | ---------------------------------------------- |
| Designed for **software-to-software** interaction | Designed for **human-to-software** interaction |
| Structured, predictable, machine-parsable         | Friendly, visual, human-oriented               |
| Regular, minimalistic data formats (JSON/XML)     | Rich layouts, visuals                          |

---

## **1.2.1 API Styles**

### 🎯 **1. RPC (Remote Procedure Call)**

* Exposes functions/procedures remotely.
* Mimics local function calls.
* Often uses **binary formats** (efficient but tightly coupled).
* Requires **client stubs**.
* Examples:

  * **gRPC** (modern, efficient)
  * **SOAP** (XML-based, older but still common)

### 🎯 **2. RMI (Remote Method Invocation)**

* RPC for object-oriented systems.
* Clients call **methods on remote objects**.
* Historical technologies:

  * CORBA
  * EJB (Enterprise Java Beans)
* Declined due to complexity.

### 🎯 **3. REST (Representational State Transfer)**

* Originated from **Roy Fielding’s dissertation**.
* Emphasizes:

  * Standard HTTP verbs (GET, POST, PUT, DELETE)
  * Standard message formats (JSON, XML)
  * **Low coupling**
  * Hyperlinks for navigation (HATEOAS)
* Most common style in modern APIs.

### 🎯 **4. Query-Focused APIs**

* Designed for **flexible data retrieval**.
* Heavy on querying, light on endpoints.
* Examples:

  * **SQL databases**
  * **GraphQL**
* Client controls the shape of returned data.

### **Choosing the Right Style**

* **Internal microservices** → RPC (fast, controllable environment)
* **Public APIs** → REST (universally compatible)
* **Complex data retrieval** → GraphQL or SQL

### 📌 Note on Microservices

* Application is a **collection of loosely coupled services**.
* Each exposes its own API.
* Security becomes distributed → covered later in microservices chapters.

---

## **1.3 API Security in Context**

API security sits at the intersection of:

### **1. Information Security**

* Protect information across:

  * creation
  * storage
  * transmission
  * backup
  * destruction
* Involves:

  * **Threat modeling**
  * **Access control**
  * **Cryptography**

**Cryptography definition:** techniques to prevent unauthorized reading or tampering.

---

### **2. Network Security**

Focuses on secure **data flow** and **network access protections**.

Learn about:

* Firewalls
* Load balancers
* Reverse proxies
* HTTPS / TLS

**HTTPS definition:**
HTTP over **TLS encryption** → hides data in transit.

---

### **3. Application Security**

Ensuring code itself is secure.

Includes:

* Secure coding practices
* Avoiding common vulnerabilities (SQLi, XSS, CSRF, etc.)
* Managing secrets and credentials

---

## **1.3.1 A Typical API Deployment**

APIs are seldom exposed directly. Requests typically pass through a chain of infrastructure:

### **1. Firewalls**

* Operate at the **network (TCP/IP) level**
* Allow only specific ports (e.g., 80 & 443)
* Block unexpected traffic

### **2. Load Balancers**

* Distribute traffic among servers
* Prevent overload on a single machine
* Can also perform:

  * health checks
  * connection pooling

### **3. Reverse Proxies**

* Sits in front of application servers
* Performs:

  * TLS termination (SSL offloading)
  * Authentication
  * Caching
  * Routing
  * Rate limiting

**SSL Termination Definition:**
TLS is terminated at proxy → data forwarded to backend (encrypted or not).

---

### **Additional Infrastructure**

#### **API Gateway**

* Advanced reverse proxy
* Aggregates many microservice APIs into one unified API
* Can handle:

  * authentication
  * rate limiting
  * request transformations
  * monitoring

#### **Web Application Firewall (WAF)**

* Understands HTTP/HTTPS semantics
* Blocks:

  * SQL injection
  * XSS
  * Bot traffic
  * API abuse patterns

#### **IDS / IPS**

* Inside internal networks
* Detects suspicious behavior
* IPS may actively block attacks

---

## 🔑 Key Takeaways

* APIs form **software boundaries** enabling modular, reusable, distributed systems.
* Several API styles exist; REST is the most common today.
* API security blends **InfoSec + Network Security + AppSec**.
* Real-world API deployments involve:

  * firewalls
  * load balancers
  * reverse proxies
  * possibly WAFs, gateways, IDS/IPS
* Security is multi-layered: infrastructure protects the edges, but **secure API design** is still essential.

---

If you'd like, I can also create:

✅ **Flashcards**
✅ **Diagrams**
✅ **A summarized cheat sheet**
✅ **Practice questions or exam-style problems**

Just tell me what you prefer!



Below is a full, structured lecture based on **Section 1.4 — Elements of API Security**, written as if being delivered in a university-level software security course. It includes explanations, examples, diagrams (text-based), and instructor-style commentary.

---

# **📘 Lecture: Elements of API Security**

## **Introduction**

Welcome to today’s lecture on the *foundational elements of API security*. APIs are now the backbone of modern applications—from mobile apps to microservices—and understanding how to secure them is essential.

Today we'll answer the critical question:

> **“If an API already defines what operations users can perform, why do we need additional security?”**

As you’ll see, API security is far more than limiting what endpoints exist. It’s about knowing *who* can call them, *how* those calls can interact, and *what threats emerge in real-world environments*.

---

# **1. Why API Security Matters**

Even if your API exposes only “safe” operations, three major realities force us to think about security:

---

## **1.1 Different Types of Users**

Many APIs are used by groups with different permissions:

* Admins vs. regular users
* Internal services vs. public users
* Authorized clients vs. bots

Without proper access controls:

> **Any user could perform any action.**

This is unacceptable in real systems handling financial data, personal info, or administrative functionality.

---

## **1.2 Operation Combinations Can Create Vulnerabilities**

Even if *each API endpoint* is safe individually, **combinations can create insecurities**.

### **Example: Insecure Banking API**

* `/withdraw` checks if a user has enough money
* `/deposit` checks if deposit is allowed
* But neither verifies *where the money came from*

An attacker could:

1. Withdraw from Account A
2. Deposit into Account B

No guarantee of consistency.

A **secure design** would expose a single atomic `/transfer` operation.

> Key lesson: **API security must consider system behavior as a whole, not individual endpoints.**

---

## **1.3 Implementation Vulnerabilities**

Even a well-designed API can be insecure if implemented poorly.

Example:
If you don’t validate input length, attackers may send huge payloads → your server runs out of memory → **Denial of Service (DoS)**.

### **Definition: Denial of Service (DoS)**

A DoS attack prevents legitimate users from accessing a service by overwhelming or crashing it.

---

# **2. Designing Security Early**

It is far cheaper—financially and technically—to design security *before* coding, not after production bugs.

You’ll see throughout this course that secure design is proactive, not reactive.

---

# **3. Understanding Security: Assets, Goals, and Threats**

API security requires understanding four core components:

1. **Assets**
2. **Security goals**
3. **Security mechanisms**
4. **Environment & threat models**

Let's break those down.

---

# **3.1 Assets**

Assets = *anything that has value*.

### **Examples of API assets**

* User data (names, emails, credit cards)
* Sensitive attributes (sexual orientation, political views)
* Database contents
* API keys, credentials, tokens
* Hardware resources (CPU, storage, bandwidth)

Even **reputation** is an asset:
If user passwords leak, users suffer harm and your company loses trust.

> Rule of thumb:
> **If someone could be harmed by its compromise, it’s an asset.**

---

# **3.2 Security Goals**

Security goals define what it *means* to protect assets.

The classic framework is the **CIA Triad**:

### **1. Confidentiality**

Only authorized parties can read the data.

### **2. Integrity**

Data cannot be altered without authorization.

### **3. Availability**

Authorized users should always be able to access the system.

These are essential for almost every API.

Other goals may include:

* **Accountability** – Knowing who performed which action
* **Non-repudiation** – A user cannot deny having done something

---

## **3.2.1 How do we make goals testable?**

Security goals are broad. To work with them, we refine them into **concrete constraints**.

### **Example: Instant Messaging API**

Functional requirement:

> “Users can read their messages.”

Security constraints now refine confidentiality:

* Users can only read **their own** messages
* A user must be **logged in**
* Logged-out users cannot read any messages

From this, we can write test cases:

* User A cannot read User B’s messages
* Unauthenticated users cannot read any message

---

# **3.3 Iterative Security Process**

Security is not one-and-done. It evolves as the system evolves.

```
Identify assets
     ↓
Define security goals
     ↓
Refine into concrete constraints
     ↓
Implement & test
     ↓
Discover new assets → return to top
```

Example:
After implementing login, you generate **session cookies** → these are new assets to protect.

---

# **3.4 Environment & Threat Models**

Security depends on context.

A cycling club's timing API doesn’t need to defend against nation-state attackers.
A hospital system definitely does.

We use **threat modeling** to decide what threats matter.

### **Definition: Threat**

A way a security goal could be violated.

### **Definition: Threat Model**

The set of threats we consider relevant to our system.

### **Definition: Threat Modeling**

The process of identifying, recording, and analyzing threats.

Threat modeling makes us ask:

* Who are the attackers?
* What might they want?
* What assets matter?
* Which threats are realistic?
* Where are trust boundaries?

---

# **3.4.1 Dataflow Diagram Example**

Below is a text-based version of the pizza delivery API diagram:

```
┌──────────────┐      ┌──────────────────────┐
│  Web Browser │ ---> │ Pizza Ordering API   │
└──────────────┘      └──────────────────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ DBMS (Database Engine) │
                └──────────────────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ Database Files       │
                └──────────────────────┘

Also used by:
┌──────────────┐
│ Mobile App   │
└──────────────┘
```

Trust boundaries help us see where data crosses between:

* client → server
* server → database
* internal → external systems

These boundaries often indicate where threats concentrate.

---

# **3.4.2 STRIDE: A Systematic Threat Model**

To systematically identify threats, we use **STRIDE**:

| Category | Meaning                | Example                          |
| -------- | ---------------------- | -------------------------------- |
| **S**    | Spoofing               | Pretending to be another user    |
| **T**    | Tampering              | Modifying data or requests       |
| **R**    | Repudiation            | Denying performing an action     |
| **I**    | Information Disclosure | Leaking sensitive info           |
| **D**    | Denial of Service      | Making the API unavailable       |
| **E**    | Elevation of Privilege | Gaining unauthorized permissions |

Each category maps to general countermeasures:

* Spoofing → authentication
* Tampering → integrity checks, signatures
* Information Disclosure → encryption, access control
* DoS → rate limiting, quotas
* etc.

STRIDE ensures you don’t forget important threat types.

---

# **Conclusion**

API security is a holistic practice involving:

* Understanding what you’re protecting
* Knowing what "secure" means for your system
* Anticipating the environment and threats
* Iteratively refining the design as you build

Next lectures will cover:

* Authentication & Authorization
* HTTPS/TLS
* Secure design patterns
* Common vulnerabilities
* API gateways & infrastructure security

---

If you'd like, I can also create:

✅ Exam-style questions
✅ A study summary / cheat sheet
✅ Slides for a presentation
✅ A diagram-based printable handout

Just tell me!


Below is a **professor-level but simple, concise, and fully coherent lecture** on **Section 1.5 — Security Mechanisms**. Everything is rewritten for clarity, pedagogy, and practical understanding.

---

# **📘 Lecture: Security Mechanisms in API Security (Section 1.5)**

Now that you understand assets, threats, and security goals, we can examine the **five major security mechanisms** that every professionally engineered API uses:

1. **Encryption**
2. **Authentication**
3. **Access Control (Authorization)**
4. **Audit Logging**
5. **Rate-Limiting**

These mechanisms together form a *security pipeline* that every request must pass through before reaching your application logic.

---

# **1. The Security Pipeline: How an API Defends Itself**

You should imagine a request traveling through several “filters,” each responsible for protecting a different security goal:

```
Client → HTTPS Encryption → Rate-Limiting → Authentication → Audit Logging → Access Control → Application Logic
```

* **Encryption** → protects confidentiality + integrity *in transit*
* **Rate-limiting** → protects availability
* **Authentication** → identifies users & clients
* **Audit logs** → ensure accountability
* **Access control** → enforces confidentiality & integrity

Many production systems offload some of these stages to components like **API gateways**, but understanding them yourself is essential before outsourcing.

Let’s go through each mechanism in depth.

---

# **2. Encryption**

Encryption protects data **outside your API**, where you have no control over the environment.

Two situations require encryption:

### **2.1 Data in transit**

Data traveling over networks (especially the internet) must be encrypted.

* We use **TLS (Transport Layer Security)** for HTTPS.
* TLS prevents attackers from **reading** or **modifying** requests/responses.

This defends against:

* eavesdropping
* session hijacking
* tampering (Man-in-the-Middle attacks)

### **2.2 Data at rest**

Data stored in:

* databases
* disks
* backups
  may be accessed by someone with physical access or stolen hardware.

Encrypting at rest protects confidentiality even if storage is compromised.

---

# **3. Identification and Authentication**

Authentication verifies the **identity claim** a user makes.

We never authenticate “the person”—we authenticate **claims** such as:

* “I am [alice@example.com](mailto:alice@example.com)”
* “I am service-client-42”

Authentication proves whether that claim is **true**, using **credentials**.

### **Why identify users at all?**

Because it allows:

1. **Accountability** → “who did what?”
2. **Authorization decisions** → what can this user do?
3. **Protection against anonymous abuse** → especially DoS

---

## **3.1 Authentication Factors**

Security uses three categories of authentication:

1. **Something you know**

   * Password, PIN, recovery phrase

2. **Something you have**

   * Smart card
   * Hardware token
   * Authenticator app (TOTP)

3. **Something you are**

   * Fingerprint
   * Iris scan
   * Face recognition

### **Multi-Factor Authentication (MFA)**

Using **two or more** *different* factors provides strong security.

Examples:

* Password (know) + SMS code (have)
* Password (know) + TOTP app (have)

Note:

* Two passwords ≠ 2FA
* Password + device-generated code = real 2FA

---

# **4. Access Control (Authorization)**

After you identify the user, you must decide **what they are allowed to do**.

The purpose: preserve **confidentiality** and **integrity**.

### Example:

* A user may read only *their own* messages
* They may send messages only to friends
* Admin-only endpoints must remain restricted

There are **two major approaches**.

---

## **4.1 Identity-Based Access Control**

This is the most common model.

Process:

1. Identify the user (e.g., via JWT or session cookie)
2. Check rules such as:

   * “Admins can delete users”
   * “User123 can read messages belonging to User123”

Pros:

* Familiar
* Maps well to business rules

Cons:

* If identity is compromised, permissions follow

---

## **4.2 Capability-Based Access Control**

Less common but extremely powerful.

A *capability* is an **unforgeable token that grants permission** to a specific resource.

Example:

* A URL with a secret, unguessable token that gives access to a shared file.
* Whoever possesses the token can access the resource.

Like a physical key:

* It opens only what it’s made for
* If you don’t have it, you can’t access the resource

Capabilities combine:

* **Who can act**
* **What they can do**
  into a single object.

Covered deeply in Chapters 8–9.

---

# **5. Audit Logging**

Audit logs record **every operation** performed through your API.

Their goal:

* **Accountability**
* Post-incident forensic analysis
* Real-time threat detection
* Compliance (GDPR, financial regulations, etc.)

A good audit entry includes:

* Who performed the request (user ID, client ID)
* When it happened
* The request details (type, target resource)
* Whether the request succeeded or failed
* Context: nearby requests by same actor

### Requirements:

* Logs must be **tamper-resistant**
* Logs often contain **PII**, so they must be protected and have reason-based retention policies

---

# **6. Rate-Limiting**

Rate-limiting protects **availability** by preventing:

* DoS attacks
* Clients consuming more resources than allowed
* Accidental overload
* Billing abuse

This mechanism ensures no single client monopolizes CPU, memory, or connections.

### **How it works**

The system tracks requests per:

* IP address
* user ID
* API key
* application

If limits are exceeded:

* The system rejects or delays requests

This is called **throttling**.

---

## **6.1 Throttling**

Instead of rejecting outright, the server:

* Queues the request or
* Responds with “slow down” status (HTTP 429)

If ignored, further requests are rejected.

---

## **6.2 Important Principle**

Rate-limiting must use **less** resources than normal request handling.

That’s why it’s commonly implemented in:

* load balancers
* API gateways
* reverse proxies
* specialized DoS-protection services

---

# **Final Summary**

A secure API relies on **multiple layers** of security, each addressing a different goal:

| Mechanism      | Protects                    | Example Threat Prevented |
| -------------- | --------------------------- | ------------------------ |
| Encryption     | Confidentiality + integrity | MITM attacks             |
| Authentication | Accountability              | Impersonation            |
| Authorization  | Confidentiality + integrity | Data leakage             |
| Audit Logging  | Accountability + forensics  | Insider abuse            |
| Rate-Limiting  | Availability                | DoS attacks              |

Together, they form a **robust, layered defense** ensuring the API is safe to expose to the real world.

---

If you'd like, I can also deliver:

✅ A simplified cheat sheet
✅ Diagrams/flowcharts
✅ Practice exam questions
✅ Case studies
✅ Real-world examples (e.g., Stripe, Google, AWS)

Just tell me!
