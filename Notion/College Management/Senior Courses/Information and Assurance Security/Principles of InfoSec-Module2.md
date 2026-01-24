Hello. Welcome to the second lecture in our series on the Principles of Information Security.

In our previous session, we established _what_ information security is—its history and its definitions. Today, we turn our attention to the _why_ and the _what against_. We are discussing **Module 2: The Need for Information Security**.

As engineers and managers, we do not implement security for the sake of security. We implement it to enable the business to survive and thrive in a hostile environment. To do this effectively, we must adopt the mindset of a military strategist. As Sun Tzu wrote in _The Art of War_: "If you know the enemy and know yourself, you need not fear the result of a hundred battles".

This lecture is structured into three critical pillars:

1. **Business Alignment:** Why we secure the organization.
2. **The Threat Landscape:** A detailed taxonomy of the 12 categories of threats.
3. **Software Assurance:** The structural failures in development that enable these threats.

---

### Part 1: Business Needs First

The most common failure mode for security professionals is becoming so enamored with technology that they forget the organizational mission. You must internalize this maxim: **When security needs and business needs collide, business wins**.

If our security measures are so draconian that the organization cannot generate revenue, we have failed. We must balance protection with availability.

**The Four Primary Functions of Information Security** An effective security program performs four specific functions for an organization:

1. **Protecting Functionality:** Management is responsible for ensuring the organization continues to function. This is not merely a technical issue; it is a management issue. If the payroll system goes down, the business stops. Security ensures resiliency,.
2. **Protecting Data:** Data is often an organization’s most valuable asset. We must protect it in three states: **in transmission**, **in processing**, and **at rest (storage)**. Without data, modern organizations lose their record of transactions and ability to deliver value.
3. **Enabling Safe Application Operation:** Modern organizations rely on complex, integrated applications (e-mail, ERP systems, instant messaging). We must create an environment where these applications can run without being compromised or hijacked.
4. **Safeguarding Technology Assets:** This refers to the infrastructure itself—the hardware, the networks, and the physical plant. We must employ secure infrastructure appropriate to the size of the organization.

---

### Part 2: The Threat Landscape

To defend a system, you must understand what threatens it. We distinguish between a **threat** (a category of objects or persons representing danger), a **threat agent** (the specific instance, e.g., a lightning strike or a specific hacker), and an **attack** (the active realization of a threat).

We classify these dangers into **12 Categories of Threats**. We will analyze each in depth.

#### 1. Compromises to Intellectual Property (IP)

IP includes trade secrets, copyrights, trademarks, and patents. The threat here is the unauthorized appropriation or use of this content.

- **Software Piracy:** The most common breach. Using software without a license is theft. Mechanisms like product activation and online registration are used to combat this, though they raise privacy concerns,.
- **Copyright Infringement:** Using protected content (music, code, text) without permission.

#### 2. Deviations in Quality of Service (QoS)

Our information systems depend on supporting utilities: power, telecommunications, and internet connectivity.

- **Availability Disruption:** If your internet connection is severed (e.g., a backhoe cuts the fiber), your data is safe, but your business stops.
- **Power Irregularities:** We worry about **spikes** (momentary increase), **surges** (prolonged increase), **sags** (momentary low voltage), **brownouts** (prolonged drop), and **blackouts** (total loss). Electronic equipment is highly sensitive to these fluctuations,.

#### 3. Espionage or Trespass

This involves unauthorized access to information.

- **Shoulder Surfing:** A low-tech attack where an attacker observes a user entering a password or viewing sensitive data.
- **Hackers:** We categorize hackers into **Experts** (who develop software scripts and codes) and **Novices** (often called "script kiddies" or "packet monkeys") who use tools written by experts without fully understanding them.
- **Password Attacks:**
    - **Brute Force:** Trying every possible combination of characters.
    - **Dictionary Attack:** Narrowing the field by using a dictionary of common passwords.
    - **Rainbow Tables:** A sophisticated attack using pre-computed hashes. If an attacker steals a database of hashed passwords, they can look up the hash in a rainbow table to find the plaintext password instantly, rather than cracking it. This emphasizes the need for "salting" passwords (adding random data before hashing),,.

#### 4. Forces of Nature

Also known as _force majeure_. Fire, flood, earthquake, lightning, and landslides. These cannot be prevented, only mitigated through contingency planning (disaster recovery and business continuity),.

#### 5. Human Error or Failure

This is **the greatest threat** to information security. It is not malicious; it is accidental.

- **The Weakest Link:** Employees closest to the data are most likely to compromise it through mistakes (e.g., sending a confidential file to the wrong email recipient).
- **Social Engineering:** Attackers exploit human nature (helpfulness, fear) to trick people into revealing information.
    - **Phishing:** Mass emails attempting to trick users into clicking malicious links.
    - **Spear Phishing:** Highly targeted phishing aimed at a specific individual or small group.
    - **Pretexting:** Creating a fake scenario (e.g., "I'm from IT support") to get credentials.
    - **Business E-mail Compromise (BEC):** Compromising legitimate business email accounts to conduct unauthorized transfers of funds,.

#### 6. Information Extortion

This is the theft of information followed by a demand for payment to prevent its release or return it. **Ransomware** is the most prevalent modern example, where data is encrypted by malware, and the key is withheld until a ransom is paid.

#### 7. Sabotage or Vandalism

This involves the deliberate destruction of systems or data.

- **Website Defacement:** A digital form of graffiti that damages reputation.
- **Cyberterrorism:** politically motivated hacking to cause panic or disruption.
- **Hacktivism:** Using hacking to interfere with operations as a form of protest.

#### 8. Software Attacks (Malware)

This involves software designed to damage, destroy, or deny service.

- **Viruses:** Code segments that attach to existing programs and replicate. They require user action (like opening a file) to spread.
- **Worms:** Self-replicating malware that spreads automatically over networks without user interaction (e.g., Code Red, Conficker).
- **Trojan Horses:** Malware disguised as helpful software.
- **Polymorphic Threats:** Malware that changes its own code/signature over time to evade antivirus detection.
- **Denial of Service (DoS):** Overwhelming a target with traffic to prevent legitimate access.
- **Distributed Denial of Service (DDoS):** Using a coordinated botnet (army of compromised "zombie" computers) to launch a massive DoS attack,.
- **Packet Sniffing:** Monitoring data traveling over a network. If data is unencrypted, the sniffer reads it in plaintext.
- **Spoofing:** Masquerading as a different IP address to gain access.
- **Man-in-the-Middle (MitM):** An attacker intercepts communications between two parties, relaying messages while eavesdropping or modifying them.
- **Pharming:** Redirecting legitimate Web traffic to an illegitimate site (often via DNS cache poisoning).

#### 9. Technical Hardware Failures

Equipment fails. We measure this using **MTBF** (Mean Time Between Failure) and **MTTF** (Mean Time To Failure). A classic example is the Intel Pentium II FDIV bug, where a hardware flaw caused calculation errors.

#### 10. Technical Software Failures

Software is written by humans; therefore, it contains errors (bugs).

- **Buffer Overflows:** Occur when an application writes more data to a block of memory (buffer) than the buffer is allocated to hold. This excess data can overwrite adjacent memory, potentially executing malicious code sent by an attacker.
- **OWASP Top 10:** A list of the most critical web application vulnerabilities, including SQL Injection (sending database commands through web inputs) and Cross-Site Scripting (XSS).

#### 11. Technological Obsolescence

Using outdated infrastructure creates risk. Older systems may no longer receive security patches, making them permanent vulnerabilities within the network.

#### 12. Theft

The illegal taking of property. This can be physical (stealing a laptop) or electronic (stealing data). Electronic theft is complex because the owner still has the original data; they may not realize a theft has occurred.

---

### Part 3: Software Assurance and Development

Why is software so vulnerable? Because security is often an afterthought. We must move toward **Software Assurance (SA)**—building security into the development life cycle (SDLC).

We must avoid the "Deadly Sins of Software Security",,:

1. **Buffer Overruns:** As discussed, failing to validate data length.
2. **Command Injection:** Failing to sanitize user input, allowing attackers to execute OS commands.
3. **Failure to Handle Errors:** When a system crashes, does it fail "open" (insecure) or "closed" (secure)? Poor error handling can reveal system details to attackers.
4. **Cross-Site Scripting (XSS):** Allowing application code to inject scripts into a user's browser.
5. **Improper Use of Cryptography:** Rolling your own crypto or hard-coding keys.
6. **SQL Injection:** Inserting database queries into input fields to bypass authentication or steal data.

**The Economic Balance** Finally, understand that perfect security is impossible. We are managing **Risk**—the probability of an unwanted occurrence. We must balance the cost of protection against the value of the asset. The goal is to bring residual risk into alignment with the organization's **risk appetite**.

### Summary

In this module, we have defined the battlefield. You now understand that security is a business enabler, not just a technical hurdle. You can categorize the 12 types of threats facing any organization, from the physical force of a flood to the subtle manipulation of a SQL injection attack.

In our next module, we will discuss how to organize and manage the defense against these threats through policy and governance.