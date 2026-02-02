Hello. We have spent considerable time building the managerial and administrative frameworks of security—policy, risk management, and personnel. Now, we descend from the abstract into the concrete. We enter the domain of **Security Technology**.

In **Module 8**, we explore the enforcement mechanisms of information security. If policy is the law, technology is the officer on the beat ensuring that law is obeyed. We will focus on three pillars: **Access Control** (who gets in), **Firewalls** (what traffic gets in), and **VPNs** (how we connect securely from the outside).

As a systems engineer, I must warn you: Technology cannot solve a problem you do not understand. If you deploy a firewall without a policy, you have simply purchased an expensive space heater. Let us begin.

---

# Lecture: Access Controls, Firewalls, and VPNs

## I. Access Control: The Fundamental Logic

Access control is the selective method by which systems specify who may use a particular resource and how they may use it. It is the translation of human policy into machine logic.

### 1. The Four Fundamental Functions

Every access control system, whether a lock on a door or a Kerberos server, performs four sequential functions:

1. **Identification:** The entity (user or system) claims an identity (e.g., "I am User X").
2. **Authentication:** The entity proves that identity (e.g., "Here is User X's password").
3. **Authorization:** The system determines what actions the authenticated entity can perform (e.g., "User X can read File A but not write to it").
4. **Accountability:** The system records the actions for later review (e.g., "User X read File A at 10:00 AM").

### 2. Approaches to Control

How do we decide _who_ gets access? We use specific models:

- **Discretionary Access Control (DAC):** The data owner controls access. If I create a file, I decide if you can read it. This is the default in most consumer operating systems (Windows, macOS).
- **Mandatory Access Control (MAC):** The system controls access based on classification. Users have clearances; objects have classification labels. Even if I create a file, I cannot grant you access unless your clearance matches the file's label. This is "lattice-based" control, common in military systems.
- **Role-Based Access Control (RBAC):** Access is tied to a job function, not an individual. If you are a "Manager," you inherit manager privileges. If you leave, we simply assign the new person the "Manager" role. This is highly efficient for large organizations.
- **Attribute-Based Access Control (ABAC):** The most modern and granular approach. Access is based on attributes of the user, the resource, and the _environment_ (e.g., "User can access Data X, but only from the HQ office during business hours"). NIST considers this the evolution of RBAC and MAC.

### 3. Authentication Factors

To authenticate (prove identity), we rely on three factors, often combined for **Multifactor Authentication (MFA)**:

1. **Something you know:** Passwords, passphrases, PINs. (Weakness: susceptible to brute force and social engineering).
2. **Something you have:** Smart cards, hardware tokens (RSA SecurID), smartphones. (Weakness: can be stolen).
3. **Something you are:** Biometrics.

**The Physics of Biometrics:** Biometrics (fingerprints, iris scans) function by converting physical characteristics into digital code. They are not perfect. We measure their effectiveness using the **Crossover Error Rate (CER)**.

- **False Reject Rate (Type I Error):** The system denies an authorized user. (Annoying).
- **False Accept Rate (Type II Error):** The system admits an imposter. (Fatal to security).
- **CER:** The point where False Reject and False Accept rates are equal. The lower the CER, the more accurate the device.

---

## II. Formal Architecture Models

Before we build software, we build mathematical models to prove security properties. You must know these two foundational models:

### 1. Bell-LaPadula (Confidentiality)

Designed to preserve secrecy (military model). It enforces two rules:

- **Simple Security Property:** "No Read Up." A subject at Secret clearance cannot read Top Secret data.
- **Star (*) Property:** "No Write Down." A subject at Top Secret cannot write data to a Secret file (preventing accidental leakage).

### 2. Biba (Integrity)

Designed to prevent data corruption. It inverts Bell-LaPadula:

- **Simple Integrity Property:** "No Read Down." A subject cannot read data from a lower integrity level (to prevent contamination).
- **Integrity Star (*) Property:** "No Write Up." A subject cannot write data to a higher integrity level (preventing a lower-trust entity from corrupting high-trust data).

---

## III. Firewalls: The Network Perimeter

A firewall is a device that prevents specific types of information from moving between the outside (untrusted) network and the inside (trusted) network.

### 1. Processing Modes (How they think)

- **Packet-Filtering (Static):** The simplest form. It looks at the packet header (IP source/destination, port number) and accepts or denies based on a rule set. It acts like a club bouncer checking IDs but ignoring what is happening inside the club.
- **Application Layer Proxy:** It acts as a middleman. If you want to view a webpage, you ask the proxy; the proxy asks the web server, reconstructs the page, scans it for malware, and hands it to you. Highly secure, but slow.
- **Stateful Packet Inspection (SPI):** The modern standard. It tracks the _state_ of a network connection (e.g., "This incoming packet is a response to a request we sent 200ms ago"). It maintains a **state table**. If a packet arrives that is not part of an established conversation, it is dropped.

### 2. Architectures (How they are built)

- **Bastion Host:** A device exposed directly to the untrusted network. It must be highly secured (hardened) because it is the primary target.
- **Screened Subnet (DMZ):** The most common secure architecture. We use two firewalls.
    - **External Firewall:** Allows traffic from the Internet into the DMZ (Demilitarized Zone).
    - **Internal Firewall:** Allows traffic from the DMZ into the internal network.
    - **The Strategy:** Public-facing servers (Web, Email) live in the DMZ. If they are compromised, the hacker is still trapped in the DMZ, unable to breach the internal firewall to get to the core database.

### 3. Best Practices for Configuration

Firewall rules are processed in order. The logic matters:

1. **Implicit Deny:** The last rule in any firewall must be "Deny All." If traffic is not explicitly allowed, it is forbidden.
2. **Specificity:** Place specific rules (e.g., "Allow IP 10.0.0.5 to Port 80") at the top.
3. **Outbound Filtering:** Do not just filter incoming traffic; filter outgoing traffic to prevent malware from "phoning home" to a command server.

---

## IV. Remote Access and VPNs

When employees work outside the perimeter, we must extend the boundary of trust.

### 1. Authentication Protocols

- **RADIUS (Remote Authentication Dial-In User Service):** A centralized system. When you dial in (or connect via VPN), the access server passes your credentials to the RADIUS server, which checks them and authorizes access.
- **Kerberos:** Used heavily in Windows Active Directory. It uses "tickets." A user authenticates once to a Key Distribution Center (KDC) and receives a Ticket-Granting Ticket (TGT), which is used to request access to specific services without re-entering a password.

### 2. Virtual Private Networks (VPNs)

A VPN creates a private, encrypted tunnel through a public network (the Internet). We implement them in two modes using protocols like IPSec:

- **Transport Mode:** Encrypts the _data_ (payload) but leaves the header unencrypted. Efficient, but an observer can see who is talking to whom.
- **Tunnel Mode:** Encrypts the _entire packet_ (header and payload) and wraps it in a new header. This masks the destination of the traffic, providing higher security. This is the standard for gateway-to-gateway VPNs (e.g., connecting a branch office to HQ).

---

## V. Summary and Practical Takeaways

As you design security systems, remember these engineering principles:

1. **Defense in Depth:** Never rely on a single firewall. Use a DMZ, internal filtering, and host-based controls.
2. **Least Privilege:** Configure access controls so users have only the permissions necessary for their job, and no more.
3. **Default Deny:** Configure firewalls to block everything by default, then open only what is strictly required.

Technology is the enforcement arm of your security strategy. If your policy is weak, your firewall rules will be weak, and your network will be compromised.