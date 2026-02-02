Good day. It is a pleasure to welcome you to this lecture on **WAN Concepts**.

As we approach this topic, I ask you to shift your mindset from that of a mere consumer of the internet to that of a systems architect. In my fifty years working with systems—from the early days of circuit switching to modern MPLS clouds—the fundamental challenge has remained unchanged: **How do we connect disparate islands of data across vast distances reliably, securely, and cost-effectively?**

Local Area Networks (LANs) are easy; you own the copper, the fibre, and the switches. Wide Area Networks (WANs) are difficult because you must rely on third-party infrastructure that you do not control.

We will structure this lecture by first establishing the architectural intuition, then moving into the physical "plumbing" and standards, and finally analysing the evolution from legacy systems to the modern connectivity options you will deploy in the field.

---

### Part 1: The Intuition and Architecture of WANs

#### 1.1 The Fundamental Difference: Ownership vs. Service

A LAN connects computers within a small geographic area (a home, an office building). You own the equipment, and you pay no fee to transmit data between rooms.

A **WAN (Wide Area Network)** allows you to connect beyond the boundary of your LAN. The defining characteristic of a WAN is not just distance; it is **telecommunications**. Because you cannot run your own cable across a continent (or even across a city), you must lease infrastructure from a Service Provider (SP) or a carrier.

**The Trade-off Matrix:**

- **LAN:** High bandwidth, low cost (after installation), full control.
- **WAN:** Bandwidth is expensive (monthly fees), latency is higher, and you are subject to the carrier's constraints.

#### 1.2 Private vs. Public WANs

When you lease a connection, you generally have two choices:

1. **Private WAN:** You pay for a dedicated path. It offers guaranteed security, consistent bandwidth, and a specific Service Level Agreement (SLA).
2. **Public WAN:** You use the global internet to connect sites. This is cheaper and flexible, but security is not guaranteed (without encryption), and bandwidth fluctuates based on local traffic.

#### 1.3 WAN Topologies

How we connect our sites determines the network's cost and resilience. Do not memorise these as shapes; understand them as traffic flow strategies.

- **Point-to-Point:** A direct line between two sites (Site A $\leftrightarrow$ Site B). It acts as a transparent Layer 2 bridge. It is simple but scales poorly. If you have 10 sites, connecting them all point-to-point becomes incredibly expensive.
- **Hub-and-Spoke:** A central site (Hub) connects to multiple remote sites (Spokes). Spoke A cannot talk to Spoke B directly; they must go through the Hub.
    - _Advantage:_ Efficient use of resources.
    - _Risk:_ The Hub is a **single point of failure**.
- **Full Mesh:** Every site connects to every other site using virtual circuits. This is the most fault-tolerant design but is the most expensive to implement.
- **Dual-Homed:** A site connects to two different routers or ISPs for redundancy. This increases complexity but provides load balancing and distributed computing capabilities.

---

### Part 2: WAN Operations and Standards

Now, let us look under the hood. Unlike LANs, which are almost exclusively Ethernet today, WANs operate on a complex mix of Layer 1 (Physical) and Layer 2 (Data Link) protocols defined by authorities like TIA/EIA, ISO, and IEEE.

#### 2.1 The Vocabulary of the "Edge"

To operate in this field, you must speak the language of the service provider. Refer to the connection point where your network meets the provider's network:

1. **CPE (Customer Premises Equipment):** The devices physically located at your facility (the subscriber's site). This includes your routers and modems.
2. **DCE vs. DTE:**
    - **DTE (Data Terminal Equipment):** Your device (e.g., a router) that serves as the source or destination of data.
    - **DCE (Data Communications Equipment):** The device (e.g., a modem or CSU/DSU) that puts data onto the local loop. The DTE connects to the DCE.
3. **The Demarcation Point:** This is critical for troubleshooting. It is the physical location (often a junction box) that separates your liability from the provider's liability. If a cable breaks on the street side, it is their problem; on the building side, it is yours.
4. **The Local Loop:** Also called the "last mile." It is the copper or fibre cable connecting your CPE to the provider’s **Central Office (CO)**.

#### 2.2 Switching Mechanisms

How does data traverse the provider's core?

- **Circuit-Switched (Legacy):** Think of a phone call. A dedicated physical path is established _before_ communication starts. If you and I are connected, no one else can use that capacity, even if we are silent. Examples: **PSTN** (Public Switched Telephone Network) and **ISDN**.
- **Packet-Switched (Modern):** Data is segmented into packets and routed over a shared network. Many users share the same infrastructure (like cars on a highway). This is far more efficient and cheaper. Examples: **MPLS, Metro Ethernet**.

#### 2.3 Optical Standards

For long-distance transport, we use fibre optics. You will encounter **SDH** (Synchronous Digital Hierarchy) globally and **SONET** in North America. These standards define how we define light pulses on fibre. To increase capacity, we use **DWDM** (Dense Wavelength Division Multiplexing), which sends multiple data streams simultaneously using different colours (wavelengths) of light.

---

### Part 3: Traditional vs. Modern Connectivity

The industry is in a transition period. You will likely inherit networks containing both legacy and modern technologies.

#### 3.1 Traditional Connectivity (The Legacy)

Historically, if you needed a WAN, you had three main options:

1. **Dedicated Leased Lines:** We used T-carrier (T1/T3 in North America) or E-carrier (E1/E3 in Europe) systems.
    - _Pros:_ Secure, constant availability.
    - _Cons:_ The most expensive option. Limited flexibility—you pay for fixed capacity whether you use it or not.
2. **Circuit-Switched:** Dial-up (PSTN) or ISDN. These are largely obsolete due to low speeds (ISDN maxing out around 2 Mbps).
3. **Packet-Switched (Legacy):**
    - **Frame Relay:** A Layer 2 technology using "virtual circuits" identified by DLCIs.
    - **ATM (Asynchronous Transfer Mode):** Used fixed-length 53-byte cells. It was designed for video/voice but was complex.

#### 3.2 Modern Connectivity (The Standard)

Today, we demand flexibility and speed. The market has shifted toward two dominant paradigms:

1. **Ethernet WAN (Metro Ethernet):** Instead of converting your LAN packets to Frame Relay or ATM, providers now offer Ethernet directly over fibre. This allows your WAN to look and behave exactly like your LAN. It reduces administrative costs and integrates easily.
    
2. **MPLS (Multiprotocol Label Switching):** This is the heavy lifter of enterprise WANs. MPLS is a routing technique that directs data from one node to the next based on short path labels rather than long network addresses.
    
    - **Why it matters:** It is "multiprotocol"—it can carry IPv4, IPv6, Ethernet, or even legacy Frame Relay traffic.
    - **The Mechanism:** Routers at the edge (PE routers) attach labels. Internal routers (P routers) switch packets quickly based solely on these labels.

---

### Part 4: Internet-Based Connectivity

For many businesses, dedicated private WANs are too expensive. The alternative is to use the public internet as the transport medium.

#### 4.1 Wired Options

- **DSL (Digital Subscriber Line):** Uses existing twisted-pair telephone copper. It is an "always-on" connection. Note the distinction between **ADSL** (Asymmetric - fast download, slow upload) and **SDSL** (Symmetric). Performance degrades significantly as distance from the Central Office increases.
- **Cable:** Uses coaxial cable (TV lines). High bandwidth (DOCSIS standard), but bandwidth is **shared** among neighbours. If your entire street downloads data, your speed drops.
- **Fibre (FTTx):** The gold standard. Fibre to the Home (FTTH) or Building (FTTB) delivers the highest bandwidth.

#### 4.2 Wireless Options

Useful for remote locations or backups:

- **Cellular (4G/5G/LTE):** Uses radio waves. Coverage and signal interference are key constraints.
- **Satellite:** Essential for rural areas lacking infrastructure. Requires line-of-sight to a satellite in geosynchronous orbit. _Caveat:_ Weather (rain fade) and high latency are significant issues.

#### 4.3 Virtual Private Networks (VPNs)

If we use the public internet (DSL/Cable/Satellite) for business, we lack security. The solution is the **VPN**. A VPN creates an encrypted tunnel over the public network.

- **Site-to-Site VPN:** Connects entire networks (Branch Office to HQ). It is transparent to the end-users.
- **Remote Access VPN:** Allows individual employees (telecommuters) to securely connect to the corporate network via software.

#### 4.4 ISP Redundancy Strategies

Finally, how do you connect to the Internet Service Provider?

- **Single-homed:** One link, one ISP. (Lowest cost, no redundancy).
- **Dual-homed:** Two links, same ISP. (Protects against cable cuts, but not ISP failure).
- **Multihomed:** Connections to _two different_ ISPs. (Protects against ISP failure).
- **Dual-multihomed:** Redundant links to multiple ISPs. (Maximum resilience, highest cost).

---

### Summary and Practical Takeaway

To summarise Module 7: A WAN is defined by its scope and its ownership. You are moving data over infrastructure you do not control.

1. **Design Phase:** Choose your topology based on failure tolerance versus budget (Hub-and-Spoke vs. Mesh).
2. **Selection Phase:** Move away from legacy T1/E1 lines. Use **Metro Ethernet** for high-bandwidth site-to-site needs or **MPLS** if you need to carry diverse protocols with Quality of Service.
3. **Access Phase:** For branch offices, **Internet-based VPNs** over Fiber or Cable are cost-effective alternatives to private lines, provided you manage the security correctly.

Mastering these concepts allows you to build networks that can grow from a single small office to a globally distributed enterprise.

Are there specific aspects of MPLS or the physical layer standards you would like me to elaborate on further?