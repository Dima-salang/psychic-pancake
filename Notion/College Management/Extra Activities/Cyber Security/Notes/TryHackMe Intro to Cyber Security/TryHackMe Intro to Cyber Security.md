# Offensive Security

- is the process of ==breaking into computer systems==, exploiting software bugs, and finding loopholes in applications to gain unauthorized access to them.
- To beat a hacker, you need to think like a hacker.
- Usually, we are engaged with Red Teams and Pen Testers.

  

# Defensive Security

- Concerned with two main tasks:
    - Preventing intrusions from occurring
    - Detecting intrusions when they occur and responding properly.
- Blue teams are part of defensive security.

Some of the tasks that are related to defensive security include:

- User cyber security awareness: Training users about cyber security helps protect against various attacks that target their systems.
- Documenting and managing assets: We need to know the types of systems and devices that we have to manage and protect properly.
- Updating and patching systems: Ensuring that computers, servers, and network devices are correctly updated and patched against any known vulnerability (weakness).
- Setting up preventative security devices: firewall and intrusion prevention systems (IPS) are critical components of preventative security. Firewalls control what network traffic can go inside and what can leave the system or network. IPS blocks any network traffic that matches present rules and attack signatures.
- Setting up logging and monitoring devices: Without proper logging and monitoring of the network, it won’t be possible to detect malicious activities and intrusions. If a new unauthorized device appears on our network, we should be able to know.

  

# Security Operations Center (SOC)

- is a team of cyber security professionals that monitor the network and its systems to detect malicious cybersecurity events.
    - Vulnerabilities: whenever a system vulnerability is discovered, it is essential to fix it by installing a proper update or patch. When a fix is not available, the necessary measures should be taken to prevent an attacker from exploiting it.
    - Policy violations: a security policy is a set of rules required for the protection of the network and systems.
    - Unauthorized activity
    - Network intrusion

# Threat Intelligence

- in this context, intelligence refers to information you gather about actual and potential enemies.
- A threat is any action that can disrupt or adversely affect a system.
- Threat intelligence aims to gather information to help the company better prepare against potential adversaries. The purpose would be to achieve a threat-informed defense.
- intelligence needs data. data has to be collected, processed, and analyzed.

  

  

# Digital Forensics and Incident Response (DFIR)

  

## Digital Forensics

- forensics is the application of science to investigate crimes and establish facts. With the rise of digital systems, digital forensics was born.
- In defensive security, the focus of digital forensics shifts to analyzing evidence of an attack and its perpetrators and other areas such as intellectual property theft, cyber espionage, and possession of unauthorized content.

Consequently, digital forensics will focus on different areas such as:

- File System: Analyzing a digital forensics image (low-level copy) of a system’s storage reveals much information, such as installed programs, created files, partially overwritten files, and deleted files.
- System memory: If the attacker is running their malicious program in memory without saving it to the disk, taking a forensic image (low-level copy) of the system memory is the best way to analyze its contents and learn about the attack.
- System logs: Each client and server computer maintains different log files about what is happening. Log files provide plenty of information about what happened on a system. Some traces will be left even if the attacker tries to clear their traces.
- Network logs: Logs of the network packets that have traversed a network would help answer more questions about whether an attack is occurring and what it entails.

  

## Incidence Response

- an incident usually refers to a data breach or a cyber attack; however, it can also be something less critical.
- How would you respond to a cyber attack? incident response specifies the methodology. The aim is to reduce damage and recover in the shortest time possible.

The four major phases of the incident response process are:

1. Preparation: This requires a team trained and ready to handle incidents. Ideally, various measures are put in place to prevent incidents from happening in the first place.
2. Detection and Analysis: The team has the necessary resources to detect any incident; moreover, it is essential to further analyze any detected incident to learn about its severity.
3. Containment, Eradication, and Recovery: Once an incident is detected, it is crucial to stop it from affecting other systems, eliminate it, and recover the affected systems. For instance, when we notice that a system is infected with a computer virus, we would like to stop (contain) the virus from spreading to other systems, clean (eradicate) the virus, and ensure proper system recovery.
4. Post-Incident Activity: After successful recovery, a report is produced, and the learned lesson is shared to prevent similar future incidents.

![[/Untitled 8.png|Untitled 8.png]]

  

  

## Malware Analysis

- malware stands for malicious software.

Malware analysis aims to learn about such malicious programs using various means:

1. Static analysis works by inspecting the malicious program without running it. Usually, this requires solid knowledge of assembly language (processor’s instruction set, i.e., computer’s fundamental instructions).
2. Dynamic analysis works by running the malware in a controlled environment and monitoring its activities. It lets you observe how the malware behaves when running.

  

  

There are also open-source sites in the internet to verify whether an IP address is malicious or not.

  

  

A flag can be seen as a string of text you receive once you accomplish a task.

  

A Security Information and Event Management System (SIEM) gathers security-related information and events from various sources and presents them via one system.

  

  

# Identification and Authentication Failure

- identification is the ability to identify a user uniquely while authentication refers to the ability to prove that the use is whom they claim to be.

  

  

# Broken Access Control

- access control ensures that each user can only access files related to their role or work

  

# Injection

- an injection attack refers to a vulnerability in the web application where the user can insert malicious code as part of their input. One cause of this vulnerability is the lack of proper validation and sanitization of the user’s input.

  

# Cryptographic Failures

- cryptography focuses on encryption and decryption.
    - Encryption scrambles cleartext into ciphertext which should be gibberish to anyone who does not know the key
    - Decryption converts the ciphertext back into the original cleartext using the key.
- Failures include
    - sending sensitive data in clear text. HTTP instead of HTTPS
    - relying on a weak cryptographic algorithm