Hello. We have spent the previous modules building the fortress: policies, firewalls, and access controls. These are your walls and gates. But as any seasoned engineer will tell you, walls are static. They cannot see a thief climbing over them in the dark, nor can they detect a traitor opening the gate from the inside.

For that, we need **visibility**.

In **Module 9**, we explore the technologies that serve as the eyes and ears of information security: **Intrusion Detection and Prevention Systems (IDPS)**, **Security Information and Event Management (SIEM)**, **Deception Technologies**, and **Scanning Tools**. We are moving from passive defense to active monitoring and analysis.

---

# Lecture: Intrusion Detection, Prevention, and Security Assessment Tools

## I. The Philosophy of Intrusion Detection

Why do we need intrusion detection if we have firewalls? Because prevention systems eventually fail. A firewall blocks traffic based on rules (IP address, port). However, if an attacker tunnels malicious traffic through a distinct, allowed port (like port 80 for Web traffic), the firewall will let it through.

We need a system that looks _inside_ the packet or monitors the _behavior_ of the system.

### 1. Terminology and Metrics

To understand these systems, you must master the vocabulary of accuracy. In security engineering, precision is paramount.

- **Alert/Alarm:** An indication that a system has been attacked or is under attack.
- **False Positive:** An alarm sounds, but there is no attack. This is a "Type I" error. _Consequence:_ It creates noise and desensitizes administrators to real threats (alert fatigue).
- **False Negative:** An attack occurs, but the system stays silent. This is a "Type II" error. _Consequence:_ This is the most grievous failure; the system failed its primary job.
- **Confidence Value:** A fuzzy logic score indicating how likely it is that an alert represents a real attack.

### 2. IDPS: Detection vs. Prevention

Historically, we had **Intrusion Detection Systems (IDS)**. They were passive; they saw the burglar and called the police (sent an alert). Today, we use **Intrusion Detection and Prevention Systems (IDPS)**. These are active. They see the burglar, call the police, _and_ lock the doors automatically. An IDPS can modify its environment—for example, by reconfiguring a firewall to block the offender immediately.

---

## II. Classifying IDPS by Location

We categorize these systems based on _where_ they sit and _what_ they watch.

### 1. Network-Based IDPS (NIDPS)

This is your perimeter guard. It sits on a network segment (often behind the firewall or in the DMZ) and monitors traffic for the whole segment.

- **How it works:** It inspects packet headers and payloads looking for patterns. It requires a specific connection called a **Monitoring Port** (also known as a SPAN port or mirror port) on a switch, which copies all traffic passing through the switch to the IDPS.
- **Advantages:** One device can monitor a large network; it is passive and usually invisible to the attacker.
- **Disadvantages:** It struggles with encrypted traffic (it cannot read the payload of an HTTPS packet). It can be overwhelmed by high traffic volumes.

### 2. Wireless IDPS (WIDPS)

This monitors the radio spectrum (Layers 2 and 3). It looks for unauthorized access points (rogue APs), protocol anomalies, and man-in-the-middle attacks on the Wi-Fi network. It cannot inspect higher-level TCP/UDP protocols efficiently; it focuses on the wireless transport itself.

### 3. Network Behavior Analysis (NBA)

While NIDPS looks at specific packets, NBA looks at **flow**. It examines traffic patterns to detect anomalies, such as a massive spike in bandwidth usage (indicating a Denial of Service attack) or a sudden flow of data from an internal database to an external IP (indicating malware exfiltration). NBA sensors are often deployed passively to monitor key network segments.

### 4. Host-Based IDPS (HIDPS)

This is the bodyguard. It resides on a specific server or computer. It does not watch the network; it watches the **files and memory** of that specific machine.

- **How it works:** It monitors system logs, configuration files, and critical system files. It flags unauthorized file modifications (integrity monitoring).
- **Advantage:** It can see the outcome of an attack (e.g., a file being deleted). It can analyze encrypted traffic _after_ the host decrypts it.
- **Disadvantage:** It consumes the host's computing resources (CPU/RAM). If the host is compromised, the HIDPS may be disabled by the attacker.

---

## III. Detection Methodologies: How the IDPS "Thinks"

Once the system sees the data, how does it decide if it is malicious? There are three primary mathematical approaches.

### 1. Signature-Based (Knowledge-Based)

This acts like a virus scanner. It compares observed traffic against a database of known attack signatures (fingerprints).

- **Pros:** Very low false positives for known attacks. Fast.
- **Cons:** It is blind to **Zero-Day attacks** (new attacks with no signature). If the database is not updated, the system is useless.

### 2. Anomaly-Based (Behavior-Based)

This system learns. It establishes a **baseline** of normal traffic over time (training phase). Once trained, any deviation from the baseline (e.g., a user logging in at 3:00 AM, or a printer sending gigabytes of data) triggers an alarm.

- **Pros:** Can detect Zero-Day attacks because they behave differently than normal traffic.
- **Cons:** High false positive rate. Traffic varies naturally; the system might flag a legitimate high-traffic event as an attack. Attackers can also "train" the system by slowly increasing malicious activity over time to shift the baseline.

### 3. Stateful Protocol Analysis (SPA)

This is deep inspection. It understands how protocols (like TCP or HTTP) are _supposed_ to work. It tracks the state of the network connection. If a packet violates the rules of the protocol—for example, sending a data payload in the middle of a handshake sequence—it flags it. This is sometimes called "Deep Packet Inspection".

---

## IV. The Brain: SIEM (Security Information and Event Management)

In a large enterprise, you have firewalls, NIDPS, HIDPS, and servers all generating thousands of logs per second. No human can read them all.

We use **SIEM** systems to aggregate this data.

- **Function:** SIEM collects logs from all devices, normalizes the data (puts it in a standard format), and performs **event correlation**.
- **The Goal:** To turn data into intelligence. A failed login on a server is just a log entry. But a failed login on a server, simultaneous with a firewall alert from a Russian IP address, and a spike in database traffic is an **Incident**. SIEM connects these dots.

---

## V. Deception Technologies: Honeypots and Honeynets

Sometimes, we want the attackers to win—but only against a target we control.

- **Honeypot:** A decoy system filled with fabricated data designed to look valuable. It serves three purposes:
    1. **Diversion:** Keeps the attacker away from real assets.
    2. **Intelligence:** Allows researchers to study the attacker's methods (Zero-Day exploits).
    3. **Delay:** Wastes the attacker's time.
- **Honeynet:** A network of honeypots simulated to look like a production network.
- **Padded Cell:** A hardened honeypot. When an IDPS detects an attacker, it seamlessly transfers them to a padded cell (a simulated environment) where they can do no harm, but we can continue to monitor them.

**Legal Warning:** You must distinguish between **Enticement** and **Entrapment**.

- _Enticement:_ Placing a jar of honey on the porch (legal). You leave a system open, but you do not force the attacker in.
- _Entrapment:_ Pushing someone toward a crime they wouldn't have committed otherwise (illegal). If you actively lure the attacker in a way that creates the crime, your evidence may be inadmissible.

---

## VI. The Offensive Toolkit: Scanning and Analysis

To defend a network, you must see it through the eyes of an attacker. We use the same tools hackers use to find vulnerabilities before they do.

### 1. Reconnaissance Tools (Footprinting & Fingerprinting)

- **Footprinting:** Collecting public data about the target (IP ranges, DNS records). Tools like _Sam Spade_ are used here.
- **Fingerprinting:** Scanning a specific system to determine its operating system and open ports. For example, if a server responds to a ping in a specific way, we know it is running Windows Server 2019.

### 2. Port Scanners

- **Nmap:** The industry standard. It scans IP addresses to see which ports are open (e.g., Port 80 for Web, Port 22 for SSH). An open port is a potential entry door.

### 3. Packet Sniffers

- **Wireshark:** A protocol analyzer. It captures traffic "off the wire" and allows you to read the exact contents of the packets. It is essential for troubleshooting and for forensic analysis of what an attacker actually sent.

### 4. Vulnerability Scanners

- **Active Scanners (e.g., Nessus):** These probe the network, attempting to identify known vulnerabilities (e.g., "This server is missing Security Patch KB1234"). They generate high traffic and can sometimes crash older systems.
- **Passive Scanners:** These listen to network traffic without sending probes. They infer vulnerabilities based on the versions of software they see communicating.

---

## VII. Summary and Engineering Takeaway

We have covered a vast array of technologies in Module 9. Here is how they fit into your security architecture:

1. **Scanning Tools (Nessus/Nmap)** tell you where you are weak _before_ the attack.
2. **NIDPS** watches the wire for attacks in transit.
3. **HIDPS** watches the servers for attacks that slipped past the network.
4. **SIEM** aggregates all this data to give the human analyst a "single pane of glass" view of the threat landscape.
5. **Honeypots** distract the enemy and provide intelligence.

**Final Thought:** Do not fall into the trap of thinking a tool solves the problem. A tool only provides data. It requires a skilled human analyst (you) to interpret that data and make the decisions that protect the organization. Tools without policy and human oversight are merely noise generators.