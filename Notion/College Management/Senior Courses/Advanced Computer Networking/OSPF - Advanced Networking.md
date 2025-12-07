# 🧭 OSPF Features and Characteristics: Lecture Notes

These notes provide a comprehensive overview of the Open Shortest Path First (**OSPF**) routing protocol, covering its features, components, operations, and different implementations.

## 🌟 Introduction to OSPF

* **Definition:** OSPF is a **link-state routing protocol**.
* **Purpose:** Developed as an alternative to the distance-vector Routing Information Protocol (RIP).
    * RIP's sole reliance on **hop count** as a metric for the best route became problematic, as it doesn't scale well in large networks with paths of varying speeds.
* **Advantages over RIP:** Offers **faster convergence** and scales to much larger network implementations.
* **Versions:**
    * **OSPFv2:** Used for **IPv4** networks (Primary focus of the module is single-area OSPFv2).
    * **OSPFv3:** Used for **IPv6** networks.
* **Areas Concept:** OSPF uses the concept of **areas** to divide the routing domain, which helps control routing update traffic.
    * A **link** is an interface on a router or a network segment (connecting two routers or a stub network like an Ethernet LAN).
    * **Link State** is information about the state of the link, including the **network prefix**, **prefix length**, and **cost**.

---

## ⚙️ Components of OSPF

All routing protocols share similar components: **messages**, **data structures**, and an **algorithm**.

### 1. Routing Control Messages (Five Packet Types)

Routers exchange these messages to convey routing information:

| Packet Type | Purpose |
| :--- | :--- |
| **Hello** | Used to **discover neighboring routers** and establish neighbor adjacencies. |
| **Database Description (DBD)** | Used to describe the contents of the Link State Database. |
| **Link State Request (LSR)** | Used to request specific Link State Records (LSA) from a neighbor. |
| **Link State Update (LSU)** | Used to send the requested LSAs to a neighbor. |
| **Link State Acknowledgment (LSAck)** | Used to explicitly acknowledge the receipt of an LSU. |

### 2. Data Structures (Three Databases)

OSPF messages are used to create and maintain three databases (tables) kept in **RAM**:

| Database | Table Name | Command to View | Function/Content |
| :--- | :--- | :--- | :--- |
| **Adjacency Database** | **Neighbor Table** | `show ip ospf neighbor` | Lists all neighboring routers with which a router has established a bi-directional communication. |
| **Link State Database (LSDB)** | **Topology Table** | `show ip ospf database` | Lists information about **all routers** in the network. Represents the network topology; all routers within an area have an **identical LSDB**. |
| **Forwarding Database** | **Routing Table** | `show ip route` | Lists the **best routes** generated after the SPF algorithm runs on the LSDB. Each router's routing table is **unique**. |

### 3. Algorithm

* **Name:** **Dijkstra's Algorithm**, also known as the **Shortest Path First (SPF)** algorithm.
* **Basis:** The algorithm is based on the **cumulative cost** to reach the destination.
* **Process:**
    1.  It creates an **SPF Tree** by placing the calculating router at the root.
    2.  It calculates the shortest path to every other node (network).
    3.  The best routes from the SPF Tree are then placed into the **Forwarding Database** (which creates the Routing Table).

---

## 🔁 OSPF Link State Operations

OSPF routers complete a generic link-state routing process to reach a state of **convergence** (stability of the network). The process involves five steps:

1.  **Establish Neighbor Adjacencies:**
    * OSPF-enabled routers send **Hello packets** out all OSPF-enabled interfaces to discover neighbors.
    * If a neighbor is present and responds, the routers attempt to establish a **neighbor adjacency** (bi-directional communication).
    2.  **Exchange Link State Advertisements (LSAs):**
    * Once adjacencies are established, routers exchange **LSAs**.
    * LSAs contain the **state and cost** of each directly connected link.
    * Routers **flood** their LSAs to adjacent neighbors, who, in turn, flood them until all routers in the area have the LSAs.
    3.  **Build the Link State Database (LSDB):**
    * Upon receiving LSAs, OSPF-enabled routers build their **Topology Table (LSDB)**.
    * This database holds all the information about the topology of the area.
    4.  **Execute the SPF Algorithm:**
    * Routers run the CPU-intensive **SPF/Dijkstra's algorithm** on the LSDB.
    * This step creates the **SPF Tree**.
    5.  **Choose the Best Route:**
    * The best paths to each network from the SPF Tree are offered to the **IP Routing Table**.
    * A route is inserted unless a better route source (lower **Administrative Distance - AD**) already exists.
        * **Static Route AD:** 1
        * **OSPF AD:** 110
    * Routing decisions are made based on the entries in the routing table.
    * *Example:* If a cost from R1 to R2 is 20, and the cost from R2 to its LAN is 2, the total cost is 22.

---

## 🌍 Single-Area vs. Multi-Area OSPF

OSPF supports **hierarchical routing** using areas to make it more efficient and scalable.

### Single-Area OSPF

* All routers are in **one area**.
* **Best Practice:** Use **Area 0** (the backbone area). If no area is specified, it is considered Area 0.

### Multi-Area OSPF

* OSPF is implemented using **multiple areas** in a hierarchical fashion.
* **Requirement:** All areas must connect to the **backbone (Area 0)**.
* **Area Border Routers (ABRs):** Routers that interconnect the areas.
* **Inter-area routing:** Routing that occurs between areas.
* **Advantages of Multi-Area OSPF:**
    * **Smaller Routing Tables:** Leads to faster lookups.
    * **Reduced Link State Update Overhead:** Updates are localized.
    * **Reduced Frequency of SPF Calculations:** The SPF algorithm (which is CPU-intensive) is only run when a topology change occurs **within an area**. Routers in other areas only update their routing tables, not rerun the SPF algorithm.
    * Effectively partitions a potentially large database into smaller, more manageable ones.

---

## 🌐 OSPF Version 3 (OSPFv3)

* **Purpose:** The OSPF version equivalent for exchanging **IPv6 prefixes**.
    * *Recall:* In IPv6, the network address is the **prefix**, and the subnet mask is the **prefix length**.
* **Functionality:** Has the same functionality as OSPFv2, but uses **IPv6** as the network layer transport.
* **Algorithm:** Uses the **SPF/Dijkstra's algorithm** for path calculation.
* **Independence:** OSPFv3 runs as a **separate process** from OSPFv2 (IPv4 counterpart).
    * Each version has separate **adjacency tables**, **topology tables**, and **IP routing tables**.
* **Configuration:** OSPFv3 configuration and verification commands are **similar** to OSPFv2.
* **Address Families (Beyond Scope):** OSPFv3 includes support for both IPv4 and IPv6 routes using the Address Families feature.

| Data Structure | OSPFv2 (IPv4) | OSPFv3 (IPv6) |
| :--- | :--- | :--- |
| Neighbor Table | Yes | Yes |
| Topology Table | Yes | Yes |
| Routing Table | Yes | Yes |

***

Would you like a summary table comparing the key characteristics of OSPF and RIP?


## 📦 OSPF Packets and Link State Updates: Lecture Notes

This section details the five types of **Link State Packets (LSPs)** used by OSPF to establish neighbor adjacencies, maintain topology information, and exchange routing updates.

---

## 💾 OSPF Packet Types

OSPF uses five types of packets, each serving a specific role in the routing process:

| Type | Packet Name | Abbreviation | Primary Function |
| :--- | :--- | :--- | :--- |
| **Type 1** | **Hello** | N/A | Establish and maintain **neighbor adjacencies**; discover neighbors; elect the **DR/BDR** (on multi-access networks). |
| **Type 2** | **Database Description** | **DBD** | Check for **database synchronization**; contains an abbreviated list of the sending router's **LSDB**. |
| **Type 3** | **Link State Request** | **LSR** | Request **specific link state records** (LSAs) from a neighbor based on the DBD. |
| **Type 4** | **Link State Update** | **LSU** | Reply to an **LSR**; announce new information; contains **one or more LSAs**. |
| **Type 5** | **Link State Acknowledgment**| **LSAck**| Confirms receipt of an **LSU** (Type 4 packet). The data field is typically empty. |

---

## 🔄 Link State Updates (LSU and LSA)

The process of exchanging database information involves the DBD, LSR, LSU, and LSAck packets:

1.  **Synchronization Check (DBD):** Routers initially exchange **Type 2 DBD** packets, which contain an abbreviated list of the sending router's **LSDB**. The receiving router compares this against its own local LSDB.
    * The LSDB must be **identical** on all link-state routers within an area to construct an accurate SPF tree.
2.  **Information Request (LSR):** If the receiving router finds discrepancies or needs more detail, it sends a **Type 3 LSR** packet requesting specific link-state records.
3.  **Update Delivery (LSU):** The router that received the LSR replies with a **Type 4 LSU** packet, which contains the specifically requested link-state records.
    * LSUs are also used to forward general OSPF routing updates, such as link changes.
4.  **Confirmation (LSAck):** The receiving router sends a **Type 5 LSAck** packet to confirm the reliable receipt of the LSU.

### 📝 LSU vs. LSA

The terms **LSU** and **LSA** are often used interchangeably, but there is a distinct difference:

* **LSA (Link State Advertisement):** The **actual piece of link-state information** (e.g., a specific route, network segment, or router link). An LSU contains one or more LSAs.
* **LSU (Link State Update):** The **packet container** or "envelope" used to transport one or more LSAs.

**Note:** An LSU packet can contain up to 11 different types of OSPFv2 LSAs. OSPFv3 has renamed some LSAs and includes two additional types.

---

## 🖐️ Hello Packet (Type 1) Details

The **Hello packet** is the most crucial packet for initial neighbor discovery and ongoing maintenance.

* **Packet Type:** Type 1
* **Purpose:**
    * **Discover OSPF neighbors** and establish bi-directional communication (neighbor adjacencies).
    * **Advertise parameters** (like Hello/Dead intervals, subnet mask, etc.) that two routers must agree upon to become neighbors.
    * **Elect the Designated Router (DR) and Backup Designated Router (BDR)** on multi-access networks (e.g., Ethernet).
        * **Point-to-point links** do not require a DR or BDR election.

### OSPFv2 Hello Packet Fields

The Type 1 Hello packet contains several important fields, including:

* **Type:** Set to 1 (for Hello).
* **Router ID:** Unique 32-bit ID of the sending router.
* **Area ID:** The OSPF area the router belongs to.
* **Router Priority:** Used in the DR/BDR election process.
* **Hello Interval / Dead Interval:** Timers that must match between neighbors.
* **Designated Router (DR) / Backup Designated Router (BDR):** IP addresses of the current DR and BDR.
* **Neighbors:** List of all other OSPF neighbors from which a Hello packet has been recently seen.


Absolutely! It can be confusing at first, so let's break down the OSPF neighbor formation and database synchronization process into a straightforward conversation between two routers, R1 and R2, using the five packet types.

Imagine OSPF is a friendly protocol that makes routers talk to each other to build a complete map of the network.

---

## 🤝 Phase 1: Neighbor Discovery and Relationship Building (Hello)

This phase is all about **establishing a two-way connection** and deciding who gets to talk first.

1.  **"Hello, are you there?" (Hello Packet - Type 1)**
    * **R1** starts sending **Hello packets** out of its OSPF-enabled interfaces. These packets go to the multicast address **224.0.0.5** (a special address that all OSPF routers listen to).
    * The Hello packet includes R1's **Router ID** and certain parameters (like the **Hello** and **Dead Intervals**, **Area ID**, and **Subnet Mask**).
2.  **"I see you, and here's my ID." (Hello Packet - Type 1)**
    * **R2** receives the Hello packet. It checks if R1's parameters (timers, masks, Area ID) match its own. If they match, it lists R1 in its neighbor table.
    * R2 then sends its own Hello packet back to R1, and crucially, **R2 includes R1's Router ID** in the "Neighbors" field of its packet.
3.  **"Ah, a two-way street!" (Two-Way State)**
    * **R1** receives R2's Hello packet and sees its own Router ID listed. This confirms that R2 has also recognized R1.
    * At this point, R1 and R2 are confirmed **neighbors** (they have a two-way communication). On broadcast networks (like Ethernet), they might also elect a **Designated Router (DR)** and a **Backup Designated Router (BDR)** to manage the traffic.

---

## 🗺️ Phase 2: Database Synchronization (DBD, LSR, LSU, LSAck)

The goal now is to ensure both routers have an **identical Link State Database (LSDB)**, which is the complete map of the network.

### 1. Initial Comparison (DBD)

* **R1 and R2** negotiate a **Master/Slave** relationship (the router with the higher Router ID becomes the Master and controls the sequence of the exchange).
* **"Here's a summary of my map." (Database Description - DBD Packet - Type 2)**
    * The Master sends a **DBD packet** to the Slave. This packet is essentially a **summary** or a table of contents of its entire LSDB. It only contains the **headers** of the Link State Advertisements (**LSAs**), not the full LSA data.
    * The Slave checks this summary against its own LSDB.

### 2. Requesting Details (LSR)

* **"Wait, I'm missing some things on your list!" (Link State Request - LSR Packet - Type 3)**
    * If the Slave sees an LSA header in the DBD that it is missing or that is newer than its own copy, it sends an **LSR packet** back to the Master. The LSR specifically asks for the **full, detailed LSA** for that network segment.

### 3. Sending the Full Information (LSU)

* **"Here are the details you asked for." (Link State Update - LSU Packet - Type 4)**
    * The Master replies with an **LSU packet**. This packet acts as an **envelope** that contains the complete, detailed **Link State Advertisement (LSA)** data that the Slave requested.
    * The **LSA** is the actual piece of routing information (like "I am connected to network 192.168.1.0/24 with a cost of 10").

### 4. Confirmation (LSAck)

* **"Got it. Thanks!" (Link State Acknowledgment - LSAck Packet - Type 5)**
    * The Slave sends an **LSAck packet** to the Master to confirm that it successfully received and processed the LSU. This makes the OSPF update process **reliable**.

---

## ✅ Phase 3: Convergence (Full State)

* R1 and R2 repeat the DBD/LSR/LSU/LSAck exchange until their **LSDBs are identical and synchronized**. They are now in the **FULL** state (fully adjacent).
* Finally, both routers independently run the **Shortest Path First (SPF) Algorithm** on the identical LSDB to calculate the best, loop-free paths to all destinations.
* These best paths are then installed in the main **IP Routing Table**.



## 🌐 OSPF: Open Shortest Path First - Lecture Notes

These notes cover the core concepts, multi-step process, neighbor requirements, and metric calculation used by the Open Shortest Path First (**OSPF**) routing protocol.

---

## 🌟 OSPF Overview and Core Concepts

| Characteristic | Description |
| :--- | :--- |
| **Protocol Type** | **Link-State Protocol** (vs. Distance-Vector, like RIP). |
| **Standard** | **Open Standard**; widely supported by virtually any router vendor. |
| **Scope** | **Interior Gateway Protocol (IGP)**, designed for use *within* a single Autonomous System. |
| **Core Goal** | Learn about **every router and subnet** within the entire network. |
| **Result** | Every OSPF router in an area holds an **identical view (map)** of the network. |
| **Information** | Routing information is shared using **Link-State Advertisements (LSAs)**, which contain information about subnets, routers, and other network details. |
| **Database** | LSAs are stored in the **Link-State Database (LSDB)** (or Topology Table). |

---

## 🪜 The OSPF Three-Step Process

OSPF achieves full network knowledge through three main steps:

### 1. Becoming OSPF Neighbors
Routers running OSPF agree to form a relationship to begin exchanging information.

### 2. Exchanging Database Information
Neighbors exchange their **LSDB** contents (LSAs) to ensure they have the same complete map of the network topology.

### 3. Choosing the Best Routes
Once the LSDB is synchronized, each router runs a calculation to choose the best, lowest-cost routes to add to its main **Routing Table**.

---

## 🤝 Step 1: Forming Neighbor Relationships

### A. Router ID Selection (RID)

The **Router ID (RID)** is a unique number in the format of an IPv4 address used to identify an individual router (its OSPF name).

| Method | Priority | Selection Logic |
| :--- | :--- | :--- |
| **1st** | **Highest** | **Manually configured** using the `router-id` command. |
| **2nd** | **Medium** | The **highest up IP address** on any **loopback interface**. |
| **3rd** | **Lowest** | The **highest up IP address** on any **non-loopback interface**. |

### B. Hello Message Exchange

Routers reach out to potential neighbors using the friendly **Hello packet** (Type 1), which contains the unique RID and a list of already known neighbors.

| Required Matching Parameters (Strict Requirements) |
| :--- |
| **Area ID** (Used for scaling OSPF into multiple areas). |
| Connecting links must be on the **same subnet** (same network and mask). |
| **Hello and Dead Timers** must be identical. |
| **Authentication** (if used) must match. |
| **Stub Area Flag** must match. |
| Routers must have **unique Router IDs**. |

> 💡 **Default Timers:** The default **Hello Timer** is **10 seconds** for point-to-point and broadcast networks. The **Dead Timer** is how long the router will wait (usually 4x the Hello Timer, or 40 seconds) before assuming the neighbor is down.

### C. Neighbor States (Simplified)

1.  **Init State:** R1 sends a Hello; R2 receives it and checks the parameters.
2.  **Two-Way State:** R2 sends a Hello back, including R1's RID in its list of known neighbors. R1 sees its own ID, confirming two-way communication. They are now considered neighbors.

---

## 👑 Designated Router (DR) and Backup DR (BDR)

DRs and BDRs are only elected on **Multi-Access Networks** (like Ethernet LANs), **NOT** on point-to-point connections.

* **Problem without DR/BDR:** On a multi-access network with 6 routers, if one link goes down, the affected router sends an update to all 5 neighbors. Each neighbor then floods the update again, creating excessive, redundant traffic (flooding storm).
* **Solution:** A **DR** and a **BDR** are elected to centralize updates.
    * **DR Role:** The DR receives all updates and is responsible for re-flooding a single, official copy to all other routers.
    * **BDR Role:** The BDR passively monitors the DR and takes over if the DR fails.
    * **Neighbor Status:** On a multi-access network, routers will only become **full neighbors** with the DR and BDR. Other routers (**DROTHERS**) remain in the **Two-Way State**.

### DR/BDR Election Process

1.  **Highest OSPF Priority:** The router with the highest configured OSPF interface priority wins (default is 1; can be changed to 0-255 to influence the election).
2.  **Tie-breaker:** If priority ties, the router with the **highest Router ID (RID)** wins.

---

## 🔄 Step 2: Database Exchange (LSDB Synchronization)

This exchange occurs once the routers are in the Two-Way state (or fully adjacent with a DR/BDR).

| State | Packet Type | Router Role | Action |
| :--- | :--- | :--- | :--- |
| **Exstart** | N/A | **Master/Slave** determined (highest RID is Master). | Master controls the sequence numbers and initiates the exchange. |
| **Exchange** | **DBD (Type 2)** | Both | Routers send a **Database Description (DBD)**, a summary list (LSA headers) of their LSDB. |
| **Loading** | **LSR (Type 3)** | Router A (needs update) | Router A looks at the DBD and sends a **Link State Request (LSR)** for any missing or newer LSAs. |
| **Loading** | **LSU (Type 4)** | Router B (has update) | Router B replies with a **Link State Update (LSU)**, which is the container carrying the full, requested **LSA** (Link State Advertisement). |
| **Loading** | **LSAck (Type 5)** | Router A (received LSU) | Router A sends a **Link State Acknowledgment (LSAck)** to confirm receipt of the LSU. |
| **Full** | N/A | Both | Exchange complete. LSDBs are synchronized. |

---

## 🥇 Step 3: Choosing the Best Routes (Cost Metric)

OSPF uses a metric called **Cost** to determine the best path. The lowest total path cost wins.

* **Cost Definition:** OSPF Cost is a value inversely proportional to the bandwidth of the link. Higher bandwidth = lower cost.
* **Formula:** $\text{Cost} = \frac{\text{Reference Bandwidth (Default 100,000 Kbps)}}{\text{Interface Bandwidth}}$

| Interface Type | Bandwidth (Kbps) | Default Cost |
| :--- | :--- | :--- |
| **Fast Ethernet** | 100,000 | **1** |
| **Gigabit Ethernet** | 1,000,000 | **1** |
| **Serial (T1)** | 1,544 | **64** |
| **Ethernet** | 10,000 | **10** |

> ⚠️ **Note:** By default, OSPF assigns a cost of **1** to all links faster than 100 Mbps (like Gig-E). This is inaccurate. To fix this, the **Reference Bandwidth** in the OSPF calculation must be manually increased on the router.

### Cost Calculation Example

To find the best path, a router calculates the cumulative cost for each path:

$$\text{Total Path Cost} = \sum (\text{Cost of each Outgoing Interface on the Path})$$

The path with the **Lowest Total Cost** is chosen and installed into the routing table.


## 🌐 OSPF Operational States and Adjacency Formation

This section details the state machine that OSPF routers progress through to establish an adjacency and synchronize their databases, and explains the necessity of Designated Routers (DR) and Backup Designated Routers (BDR) in multi-access networks.

---

## 🚦 OSPF Operational States

When an OSPF router first connects to a network, it attempts to create adjacencies, exchange routing information, calculate routes, and reach **convergence** (the **Full State**).

| State | Action/Event | OSPF Process Transition |
| :--- | :--- | :--- |
| **Down** | No **Hello packets** received from a neighbor. Router sends Hello packets to begin discovery. | $\rightarrow$ **Init** |
| **Init (Initialization)** | **Hello packets** received from a neighbor, containing the sender's **Router ID (RID)**. | $\rightarrow$ **Two-Way** |
| **Two-Way** | Communication is **bi-directional**. The router sees its own RID in the neighbor's Hello packet. **DR/BDR election** occurs on multi-access links. | $\rightarrow$ **Exstart** |
| **Exstart** | Routers decide which one will be the **Master** (higher RID) to initiate the **DBD** exchange and set the initial sequence number. | $\rightarrow$ **Exchange** |
| **Exchange** | Routers exchange **Database Description (DBD)** packets (summaries of the LSDB). | $\rightarrow$ **Loading** (if more info needed) or **Full** (if synchronized). |
| **Loading** | Routers use **Link-State Request (LSR)** and receive **Link-State Update (LSU)** packets to gather missing or more current routing information. | $\rightarrow$ **Full** |
| **Full** | The **Link-State Database (LSDB)** is fully **synchronized**. The network is considered **converged**. |

---

## 🤝 Establishing Neighbor Adjacencies (Down, Init, Two-Way)

The process of finding and confirming a neighbor relationship is managed by the **Hello packet**.

### 1. Down State $\rightarrow$ Init State

* **Action:** The router starts sending **Hello packets** out of all OSPF-enabled interfaces.
* **Destination:** The Hello packet is sent to the reserved **multicast IPv4 address** for all OSPF routers: **$224.0.0.5$**.
* **Content:** The packet contains the router's unique **Router ID (RID)**.
* **Init Event:** When a neighboring OSPF router (**R2**) receives R1's Hello packet (containing R1's RID), R2 adds R1 to its neighbor list and transitions to the Init state.

### 2. Init State $\rightarrow$ Two-Way State

* **Action:** R2 sends its own Hello packet back to R1.
* **Content:** R2's Hello packet now includes **R1's RID** in its list of known neighbors.
* **Two-Way Event:** When R1 receives R2's Hello and sees **its own RID** listed, it confirms that R2 has also recognized it, and the communication is **bi-directional**. R1 transitions to the Two-Way state.

### 3. Transition from Two-Way

* **Point-to-Point Links (e.g., Serial Cable):** Routers immediately transition from the Two-Way state to the **Exstart** state.
* **Multi-Access Networks (e.g., Ethernet):** A **Designated Router (DR)** and **Backup Designated Router (BDR)** election must take place before transitioning to the Exstart state.

---

## 💾 Synchronizing OSPF Databases (Exstart, Exchange, Loading, Full)

After the Two-Way state, the routers use the remaining four OSPF packet types (DBD, LSR, LSU, LSAck) to synchronize their databases.

### 1. Exstart State: Master/Slave Negotiation

* The two adjacent routers decide which one will be the **Master** (initiator) and which will be the **Slave**.
* The router with the **higher Router ID** is elected the **Master** and controls the initial **sequence number** for the DBD packets.

### 2. Exchange State: Comparing Summaries (DBD)

* **Packet Used:** **Database Description (DBD - Type 2)**.
* The Master sends a DBD packet, which is a **summary** containing the **header** (or table of contents) of its LSDB.
* The Slave acknowledges the DBD using an **LSAck** packet, and then sends its own DBD.

### 3. Loading State: Requesting Details (LSR, LSU, LSAck)

* **Comparison:** Each router compares the DBD summaries against its local LSDB.
* **Request:** If a router sees a link-state entry in the DBD that is more current (newer sequence number) or entirely missing, it sends a **Link-State Request (LSR - Type 3)** for the specific LSA.
* **Update:** The neighbor holding the current information responds with a **Link-State Update (LSU - Type 4)** packet, which contains the full, requested **LSA**.
* **Acknowledge:** The requesting router sends a **Link-State Acknowledgment (LSAck - Type 5)** to confirm receipt of the LSU.
* **Process:** This cycle continues until all LSRs are satisfied.

### 4. Full State: Convergence

* Once all LSDBs are synchronized, the adjacent routers transition to the **Full State**.
* **Updates (LSUs)** are sent only when a **topological change is perceived (incremental updates)** or every **30 minutes** to refresh the information.

---

## 👑 DR and BDR Election in Multi-Access Networks

The DR/BDR election is necessary on multi-access links (e.g., Ethernet) to solve two main problems:

### 1. Excessive Adjacencies

* On a multi-access network with $N$ routers, the number of required adjacencies is $N \times (N-1) / 2$.
    * **Example:** A network with 5 routers would require $5 \times (4) / 2 = \mathbf{10}$ adjacencies.
    * **Problem:** Creating adjacencies with every router is unnecessary and leads to an excessive number of LSA exchanges.

### 2. Extensive LSA Flooding

* If every router had to flood and acknowledge every received LSA to every other router on the same network, the network traffic would become chaotic and inefficient.

### DR/BDR Solution

* **Designated Router (DR):** Elected to be the **collection and distribution point** for all LSAs on that segment. It receives updates from everyone and sends a single, official update out.
* **Backup Designated Router (BDR):** Elected to take over the DR's role if the DR fails.
* **DROTHERS:** All other routers on the segment.

| Router Type | Adjacency Status |
| :--- | :--- |
| **DR $\leftrightarrow$ BDR** | Full State |
| **DR $\leftrightarrow$ DROTHERS** | Full State |
| **BDR $\leftrightarrow$ DROTHERS** | Full State |
| **DROTHER $\leftrightarrow$ DROTHER** | Two-Way State (They do **NOT** become fully adjacent) |

### DR/BDR Election Rules

1.  **Highest OSPF Priority** (Default is 1).
2.  If priorities are tied, the **Highest Router ID (RID)** wins.

> 📝 **Note:** The DR is only used to disseminate LSAs. The router will still use the best next-hop router indicated in its routing table for the forwarding of all other data packets.