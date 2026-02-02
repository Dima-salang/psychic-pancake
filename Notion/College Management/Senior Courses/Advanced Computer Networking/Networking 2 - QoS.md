Good day. Please settle in.

In our previous discussions, we constructed the **WAN** to connect our sites and utilized **VPNs** to secure the tunnels. We have built the highway. Now, in **Module 9: QoS Concepts**, we must manage the traffic flows upon it.

As a systems architect, you must accept a harsh reality: **Bandwidth is finite, but demand is infinite.**

If you allow a file transfer (FTP) to compete equally with a Voice over IP (VoIP) call during a period of congestion, the physics of packet switching dictates that both will suffer. However, the FTP user will merely experience a delay of a few seconds; the VoIP user will experience a dropped call. This is unacceptable in a production environment.

**Quality of Service (QoS)** is the art of managing unfairness. It is the mechanism by which we prioritize mission-critical, delay-sensitive traffic at the expense of resilient, bulk traffic.

---

### Part 1: The Physics of Congestion

Before we configure routers, we must understand the problem. A router interface is a choke point. When the volume of ingress traffic exceeds the speed of the egress interface, we have **congestion**.

#### 1.1 Where Congestion Occurs

Congestion is not random; it occurs at structural bottlenecks:

1. **Aggregation:** Multiple input links merging into a single output.
2. **Speed Mismatch:** A 1000 Mbps LAN port feeding a 100 Mbps WAN port.
3. **LAN to WAN:** Moving from a high-speed local network to a slower service provider link.

#### 1.2 The Metrics of Quality

When congestion occurs, buffers fill up. This leads to four distinct phenomena you must measure:

1. **Bandwidth:** The raw capacity (bits per second).
2. **Packet Loss:** When buffers are full, new packets are discarded (Tail Drop).
    - _Real-world impact:_ For Voice/Video, this causes skips or artifacts. For Data, it triggers TCP retransmissions, which slows throughput.
3. **Delay (Latency):** The time from source to destination. This is not a single number; it is the sum of:
    - _Code Delay:_ Compression time.
    - _Serialization Delay:_ Time to put bits on the wire (fixed).
    - _Queuing Delay:_ Time waiting in a buffer (variable).
    - _Propagation Delay:_ Physics (speed of light over fibre).
4. **Jitter:** The _variation_ in delay. If packet A takes 20ms and packet B takes 50ms, the jitter is high.
    - _The Mechanism:_ Digital Signal Processors (DSPs) use a **playout delay buffer** to smooth this out. If jitter exceeds the buffer size, the packet is discarded just as if it were lost.

---

### Part 2: Traffic Characteristics

To solve congestion, you must know your traffic profiles. We classify traffic into three distinct archetypes.

#### 2.1 Voice (The Fragile)

Voice traffic is smooth and benign (low bandwidth), but it is incredibly sensitive.

- **The Profile:** UDP-based. It does not retransmit dropped packets.
- **The Thresholds:** You must engineer the network to meet these strict limits:
    - Latency: $\le$ 150 ms
    - Jitter: $\le$ 30 ms
    - Loss: $\le$ 1%
- **Bandwidth:** Low (30–128 Kbps).

#### 2.2 Video (The Greedy and Fragile)

Video is the most difficult to manage. It is bursty and greedy (high bandwidth) but shares the sensitivity of voice.

- **The Profile:** UDP-based. The packet size varies significantly every 33ms based on content (e.g., a static scene vs. an explosion).
- **The Thresholds:**
    - Latency: $\le$ 400 ms.
    - Jitter: $\le$ 50 ms.
    - Loss: $\le$ 1%.

#### 2.3 Data (The Resilient)

Data is robust. It uses **TCP**, which ensures reliability through retransmission.

- **The Profile:** Can be smooth or bursty. It is generally insensitive to drop and delay.
- **The Strategy:** We segregate data into "Mission Critical" (interactive apps) vs. "Best Effort" (email/web). We can afford to drop data packets to save a voice call, because TCP will simply resend the data.

---

### Part 3: Queuing Algorithms

When the buffer fills, the router must decide which packet to transmit next. This is the **Queuing Algorithm**.

1. **FIFO (First-In, First-Out):** The default. No priority. If a large file download fills the buffer, voice packets stuck behind it will die. This is the absence of QoS.
2. **WFQ (Weighted Fair Queuing):** An automated algorithm. It identifies "flows" and ensures fair bandwidth allocation. It prevents high-bandwidth flows (FTP) from starving low-bandwidth flows (Telnet). _Note: It does not support tunneling._
3. **CBWFQ (Class-Based Weighted Fair Queuing):** The engineer takes control. We define classes based on ACLs or protocols and assign a guaranteed bandwidth to each. However, within each class, it is still FIFO.
4. **LLQ (Low Latency Queuing):** This is the industry standard for VoIP. It combines CBWFQ with a **Strict Priority Queue (PQ)**. Traffic in the PQ is sent _before_ anything else.
    - _Critical Design Rule:_ You must police the PQ. If the priority queue is flooded, it will starve all other queues completely.

---

### Part 4: QoS Models

How do we architect this across the entire WAN?

#### 4.1 Best Effort

No QoS. The internet standard. Scalable, but offers no guarantees.

#### 4.2 IntServ (Integrated Services)

The "Reservation" Model. Applications signal the network (using **RSVP** - Resource Reservation Protocol) to reserve bandwidth _before_ sending data.

- _Analogy:_ Calling a restaurant to reserve a table. If no table is available, you are denied entry (Admission Control).
- _Drawback:_ It is not scalable. Core routers cannot maintain state for thousands of individual flows.

#### 4.3 DiffServ (Differentiated Services)

The "Classification" Model. The current standard.

- _Mechanism:_ We mark packets at the edge. Core routers simply look at the mark and service the packet accordingly (Hop-by-Hop).
- _Analogy:_ Priority boarding on an airline. The airline doesn't know _who_ you are, just that your ticket says "First Class".

---

### Part 5: Implementation Tools

To implement DiffServ, we use a three-step toolbox: **Classification/Marking**, **Congestion Management**, and **Congestion Avoidance**.

#### 5.1 Classification and Marking (The Labeling)

We must identify traffic and "mark" it so downstream routers know how to treat it.

**Layer 2 Marking (Ethernet 802.1Q/p):**

- Uses the **CoS (Class of Service)** field.
- 3 bits = 8 values (0–7).
- _Limitation:_ Does not survive if the frame crosses a router (Layer 3 boundary).

**Layer 3 Marking (IP Headers):**

- Original Standard: **IP Precedence (IPP)** (3 bits).
- Current Standard: **DSCP (Differentiated Services Code Point)**.
- Uses 6 bits of the IPv4 ToS field or IPv6 Traffic Class field, providing 64 possible classes.

**The DSCP Values You Must Know:**

1. **EF (Expedited Forwarding):** Decimal 46. Used for **Voice**. Low latency, low loss.
2. **AF (Assured Forwarding):** Used for data. It uses a matrix (AFxy).
    - _x (Class):_ 1 (Worst) to 4 (Best).
    - _y (Drop Probability):_ 1 (Low) to 3 (High).
    - _Example:_ AF41 is Class 4, Low Drop (High Priority Data). AF13 is Class 1, High Drop (Bulk Data).
3. **BE (Best Effort):** DSCP 0. The default.

**Trust Boundaries:** Mark traffic as close to the source as possible. Ideally, trust the IP phone. If you cannot trust the endpoint (e.g., a PC), mark traffic at the ingress switch port.

#### 5.2 Congestion Avoidance (WRED)

While queuing manages congestion _after_ it happens, **WRED (Weighted Random Early Detection)** prevents it.

- _Mechanism:_ It monitors the average queue depth. If the queue gets too full, WRED randomly drops TCP packets _before_ the buffer is full.
- _The Logic:_ Dropping a few TCP packets causes the sender to throttle back (slow down window size), preventing the queue from filling up and tail-dropping all traffic.

#### 5.3 Shaping vs. Policing

These tools control the rate of traffic.

- **Shaping (Buffer & Delay):** "You are speaking too fast; I will hold your words and release them slowly." Smooths traffic. Used on egress.
- **Policing (Drop & Remark):** "You are speaking too fast; I will silence you." Chops traffic. Used on ingress.

---

### Practical Takeaway

When you deploy QoS in the field:

1. **Identify:** Use NBAR or ACLs to identify Voice and Video.
2. **Mark:** Set Voice to **EF** and critical Video to **AF41**.
3. **Queuing:** Configure **LLQ**. Give Voice the Priority Queue. Give Data a guaranteed bandwidth share using CBWFQ.
4. **Avoidance:** Enable **WRED** on the data queues to prevent TCP synchronization/starvation.

QoS does not create bandwidth; it merely dictates who suffers when bandwidth is scarce. As engineers, it is our job to ensure that the suffering falls on the applications that can tolerate it.

Are there specific questions regarding the DSCP bit-mapping or the configuration of LLQ?