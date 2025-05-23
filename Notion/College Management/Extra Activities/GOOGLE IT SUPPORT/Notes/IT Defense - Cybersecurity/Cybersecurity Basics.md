  

  

## **CIA Triad: Foundation of Information Security**

_Confidentiality:_

- **Concealing data from unauthorized access is the essence of confidentiality.**
- Daily practices like password protection exemplify confidentiality by restricting access to sensitive information.
- **Key to confidentiality is limiting data access to individuals who genuinely require it, ensuring robust protection against unauthorized eyes.**

_Integrity:_

- **Integrity ensures the accuracy and unaltered state of data throughout its lifecycle.**
- Downloading a file with a stated size of 3 megabytes but finding it to be 30 megabytes raises integrity concerns. Any alteration during download poses potential risks.
- Maintaining data consistency and reliability is integral to upholding integrity principles in information security.

_Availability:_

- **Availability emphasizes that information should be accessible to authorized users when needed.**
- Preparedness for data loss or system downtime aligns with availability principles.
- Security attacks often aim to disrupt availability, holding systems hostage or causing downtime. The goal is to ensure that information remains accessible to rightful users.

**Importance of CIA Triad:**

- The CIA triad serves as a guiding model for designing information security policies.
- Confidentiality, integrity, and availability are fundamental principles, forming the cornerstone of security practices in workplaces and personal environments.
- Security attacks target various aspects, including time, material possessions, and personal dignity. Understanding and implementing the CIA triad is crucial for safeguarding against these threats.

  

## **Key Security Terminology**

_Risk:_

- Definition: The possibility of experiencing a loss due to a system attack.
- Example: Not setting up a screen lock on your phone poses the risk of unauthorized access and data theft.

_Vulnerability:_

- Definition: A flaw in the system that could be exploited to compromise it.
- Example: Forgetting to disable a debug account in a web app before launch creates a vulnerability that attackers may exploit.

_Zero-Day Vulnerability:_

- Definition: A vulnerability unknown to the software developer or vendor but known to an attacker.
- Example: An undiscovered flaw in a software system that attackers exploit before the vendor can react.

_Exploit:_

- Definition: Software used to take advantage of a security bug or vulnerability.
- Example: Attackers write exploits for vulnerabilities to cause harm to systems by targeting and taking advantage of security flaws.

_Threat:_

- Definition: The possibility of danger that could exploit a vulnerability.
- Example: Potential attackers who might exploit vulnerabilities in a system, analogous to burglars considering breaking into a home.

_Hacker:_

- Definition: Someone attempting to break into or exploit a system.
- Example: Two common types are black hat hackers (malicious intent) and white hat hackers (find weaknesses to help fix them).

_Attack:_

- Definition: An actual attempt at causing harm to a system.
- Importance: Understanding threats and vulnerabilities helps in preparing for and mitigating attacks on a system.

  

  

**Overview of Common Malware Types**

_1. Viruses:_

- Function: Replicates and spreads by attaching to executable code, infecting files, and executing malicious actions.
- Analogy: Similar to biological viruses that attach to healthy cells and spread throughout the body.
- Example: Spreads by attaching to programs, making files susceptible to infection.

_2. Worms:_

- Function: Independent and self-replicating malware that spreads through networks.
- Example: ILoveYou worm, which spread via email, causing widespread damage and data loss.

_3. Adware:_

- Function: Displays advertisements and collects user data.
- Variants: Legitimate adware (with user consent) and malicious adware that may get installed without user consent.
- Visibility: Often encountered in daily internet usage.

_4. Trojan:_

- Function: Disguises itself as one thing but performs different, often malicious, actions.
- Analogy: Named after the historical Trojan Horse, as it requires user acceptance to execute.
- Example: Entices users to install by masquerading as legitimate software.

_5. Spyware:_

- Function: Spies on users by monitoring screens, key presses, webcams, and relaying information to a third party.
- Example: Keylogger, recording every keystroke, capturing passwords and confidential information.

_6. Ransomware:_

- Function: Holds data or systems hostage until a ransom is paid.
- Example: WannaCry ransomware attack in 2017, exploiting a vulnerability in older Windows systems, causing widespread disruptions.

7.Keyloggers

- Registers keystrokes.

  

_Conclusion:_

- Malware encompasses various types, each with unique characteristics and functionalities.
- Understanding these malware types is essential for recognizing potential threats and implementing effective security measures.
- Real-life examples, such as the WannaCry attack, emphasize the importance of proactive cybersecurity measures in combating evolving threats.

  

_7. Botnets:_

- Function: Malware that utilizes compromised machines (bots) to perform tasks controlled by the attacker.
- Network: Collection of bots forms a botnet, capable of executing distributed functions, e.g., Bitcoin mining.

_8. Backdoor:_

- Function: A secret entryway into a system, allowing unauthorized access.
- Purpose: Installed by attackers to maintain access even if the system owner discovers the compromise.
- Detection: Challenging to identify; requires thorough system analysis to locate and close.

_9. Rootkit:_

- Function: A collection of software/tools providing admin-level access to an operating system.
- Stealth: Hides its presence by manipulating the system, making detection difficult.
- Admin-level Control: Enables unauthorized modification of the operating system.

_10. Logic Bomb:_

- Function: Malware triggered by a specific event or time to execute a malicious program.
- Example: 2006 case involving an unhappy systems administrator triggering a logic bomb to disrupt company services, resulting in legal consequences.
- Purpose: Often used for malicious intent, such as disrupting services or manipulating stock prices.

  

**Antimalware Protection and Malware Removal**

**Detection and Verification:**

- Gather information from the user, noting when symptoms started and any unusual file downloads.
- Common symptoms include slow performance, spontaneous restarts, and abnormal memory usage.
- Verify issues by monitoring computer activity, checking resource manager for suspicious programs.

**Quarantine Malware:**

- Disconnect infected device from the internet by turning off WiFi and unplugging the ethernet cable.
- Disable automatic system backup to prevent malware reinfection through backups.
- Quarantining prevents the spread of malware and communication with other bad actors.

**Malware Removal:**

- Run an offline malware scan to identify, quarantine, and remove malware while disconnected.
- Anti-virus/anti-malware programs rely on updated threat definition files; ensure they are up to date.
- After removal, set threat definitions to update automatically and schedule regular scans.
- Turn on automatic backup and create a safe restore point for future system recovery.

  

  

# Network Attacks

**DNS Cache Poisoning Attack:**

- Concept: Trick DNS server into accepting a fake DNS record, leading to compromised DNS server.
- Result: Provides fake DNS addresses for legitimate websites, potentially spreading to other networks.
- Real-world Example: Large-scale attack in Brazil involved poisoning DNS cache of local ISPs, leading to users unknowingly installing a malicious banking trojan.

**Man-in-the-Middle (MitM) Attack:**

- Definition: Places attacker between two hosts believing they communicate directly.
- Session Hijacking: Common MitM attack involving stealing session tokens to impersonate a user on a website.
- Integrity Concerns: Emphasizes the importance of data integrity to prevent tampering.
- Rogue Access Point Attack: Unauthorized installation of an access point on a network, posing security risks.
- Evil Twin Attack: Similar to rogue AP, creates an identical network to monitor and intercept traffic.

  

  

**Denial-of-Service (DoS) Attack:**

- Objective: Prevent access to a service for legitimate users by overwhelming the network or server.
- Illustration: Imagine a small-capacity website that can only serve ten users; a DoS attacker would occupy all spots, denying access to legitimate users.
- Real-world Impact: DoS attacks aim to consume service resources, making it inaccessible for genuine users on major platforms like Google or Facebook.

**Ping of Death (PoD) Attack:**

- Method: Sends a malformed ping larger than the Internet Protocol can handle, causing a buffer overflow and potential system crash.
- Risk: Allows execution of malicious code and disrupts the target system.

**Ping Flood and SYN Flood Attacks:**

- Ping Flood: Overwhelms a system by sending numerous ping packets (ICMP echo requests), causing it to be prone to being overwhelmed and taken down.
- SYN Flood: Exploits the three-step TCP connection process by bombarding the server with SYN packets, keeping connections half open and consuming resources.

**Distributed Denial-of-Service (DDoS) Attack:**

- Nature: Utilizes multiple systems to carry out an attack, often orchestrated by a botnet.
- Amplification: Enables attackers to take down services at greater volumes and quicker rates.
- Real-world Example: October 2016 DDoS attack on DNS service provider DYN, causing a service outage for major websites like Reddit, GitHub, and Twitter.

**Implications:**

- DoS attacks disrupt service availability for genuine users, impacting major online platforms.
- Ping of Death and related attacks exploit vulnerabilities in network protocols, leading to potential system crashes.
- DDoS attacks, orchestrated by botnets, pose a significant threat to service providers and major online platforms.
- Mitigation strategies, such as enhanced network security and traffic filtering, are essential to defend against DoS and DDoS attacks.

  

**Injection Attacks in Software Development:**

- **Concept:**
    - Injection attacks involve injecting malicious code into software to exploit vulnerabilities.
    - Analogous to injecting a non-compatible substance into a system (e.g., a strawberry banana milkshake into a gas tank).
- **Mitigation Strategies:**
    - Employ mechanisms to validate and sanitize input data.
    - Implement strict input validation to prevent unauthorized code injection.

**Cross-Site Scripting (XSS) Attacks:**

- **Nature:**
    - Type of injection attack targeting users of a service.
    - Allows attackers to insert malicious scripts into websites, affecting users.
- **Objectives:**
    - Achieve session hijacking by embedding a script in a website that, when executed by a user, steals cookies or performs malicious actions.

**SQL Injection Attacks:**

- **Target:**
    - Entire website if it utilizes a SQL database.
- **Method:**
    - Attackers inject SQL commands through input fields, potentially allowing them to manipulate, delete, or copy data from the database.
- **Significance:**
    - Puts the entire website at risk by compromising the underlying database.

**Mitigation and Prevention:**

- **Software Development Principles:**
    - Validate input data to ensure it adheres to expected formats.
    - Sanitize user inputs to remove any potentially malicious code.
    - Implement secure coding practices to prevent vulnerabilities.

**Analogy:**

- **Car Analogy:**
    
    - Similar to ensuring a car only accepts the right type of fuel, software should only accept valid and sanitized input to prevent injection attacks.
    
      
    

# Password Attacks

**Brute Force Attacks:**

- **Nature:**
    - Systematically tries different character combinations until the correct password is found.
    - Effective but time-consuming due to the vast number of possible combinations.
- **Prevention:**
    - Captchas act as gatekeepers, distinguishing between humans and automated systems.
    - They act as a barrier, preventing automated attempts to log in endlessly.

**Dictionary Attacks:**

- **Approach:**
    - Targets commonly used words as passwords rather than attempting all possible combinations.
    - Focuses on words like "password," "monkey," or "football."
- **Defense:**
    - Strong passwords with a mix of upper and lower case letters, symbols, and avoiding common dictionary words.

  

Use strong passwords!

  

  

## **Social Engineering: A Deceptive Art**

Social engineering, akin to a con game, thrives on manipulative techniques to extract personal information and coerce victims into unwittingly assisting attackers. In this human-driven attack, the weakest link is not in the code but in the people behind the screens.

**Phishing: A Lure in Digital Waters**

- **Methodology:**
    - Attackers send malicious emails masquerading as legitimate entities.
    - Common scenario: A fake bank email claims an account compromise, redirecting victims to a fraudulent site.
- **Variation: Spear Phishing:**
    - Targets specific individuals or groups.
    - Incorporates personal information to enhance trustworthiness.
- **Whaling:** When a cybercriminal wants to spear phish a big target or “whale,” they will spend more time and effort deceiving the victim. A whale target is typically someone in a position of power, such as a wealthy and/or famous person, an executive of a company, or a high-level government employee. The whale is targeted because of the likelihood that they have the ability to pay high ransomware fees, trade valuable information or confidential data, or may be vulnerable to blackmail.
- **Vishing:** Cybercriminals use Voice over IP (VoIP) to make phone calls or leave voice messages pretending to be from reputable companies in order to trick victims into revealing personal information, such as banking details and credit card numbers. Although telephone scams have been running for decades, vishing with VoIP makes it easier for cybercriminals to hide their true identity. VoIP calls are significantly more difficult to trace than landline calls.

**Email Spoofing: Disguised Messages**

- **Nature:**
    - Manipulates sender addresses to deceive recipients.
    - Appearances can be deceiving, like receiving an email seemingly from a trusted friend.

**Beyond the Digital Realm: Physical Social Engineering**

- **Baiting: Luring with a Trap:**
    - Attackers leave USB drives in public places.
    - Leveraging curiosity, victims unknowingly install malware upon accessing the drive.
- **Tailgating: Gaining Physical Access:**
    - Deceptive tactics to enter restricted areas.
    - Exploits trust, posing as a legitimate visitor or service provider.
- **Shoulder surfing:** This malicious attack might have a specific victim or organization as their target. Shoulder surfing happens when a person looks over a victim’s shoulder to watch them enter login credentials, credit card numbers, or other sensitive information. For example, a temporary contractor for an organization may look over the shoulder of an employee to watch the employee enter their login info. The temporary employee’s goal might be to steal credentials in order to illegally obtain confidential company data or plant ransomware.
- **Impersonation:** This attack might happen over email, text messaging, or a phone call. The attacker impersonates someone who should have access to an organization’s computer network. For example, the attacker might call the IT Support team to request help with a password reset. Alternatively, the attacker might pretend to be a member of an organization’s IT Support team. They may call an employee to ask them to change some settings on their computer to fix a fake problem. These changes are intended to open a door for the cybercriminals to gain access to the organization’s network.
- **Dumpster Diving:** This in-person attack involves the attacker literally digging through the trash of an individual or organization to hunt for confidential information, like financial or customer information. Shredding all confidential documents is an easy way to prevent this type of attack.
- **Evil twin:** This type of attack involves the cybercriminal installing Wi-Fi routers that appear to belong to an organization's network. These Wi-Fi access points may not require a password and might appear to offer a stronger signal than the real Wi-Fi router. When victims connect to the fake Wi-Fi access point, the cybercriminal gains access to the victim’s wireless transmissions, which can include login credentials and other sensitive information.

  

# Physical security measures

Physical security measures make it harder for intruders to gain access, steal, or damage IT equipment and data. Here are some common methods:

- **A. Guards** monitor controlled access points throughout a facility to prevent unauthorized access.
- **B. Door locks** allow an area to be restricted. Only people with an authorized unlocking mechanism, like a key or security badge, can gain access to the restricted area.
- **C. Equipment locks** can restrict the movement of sensitive equipment, like servers, storage media, or terminals, by anchoring them to a less mobile structure. Only people with an authorized unlocking mechanism, like a key or security badge, can release the controlled equipment from its anchored location.
- **D. Video surveillance**: Video cameras allow continuous observation and recorded activity playback within controlled areas. Video surveillance can document who accesses a controlled area, how they access it, and what they do there.
- **E. Alarm systems** notify security by sounding an alarm or sending a message when a controlled area is accessed.
- **F. Motion sensors** are devices that detect movement within a controlled area. Motion sensors can trigger alarm systems or video surveillance.

**Protecting the entry points of a building**

- **G. Access control vestibules** create a space between two sets of interlocking doors or gateways to prevent unauthorized individuals from following authorized individuals into controlled facilities.
- **H. Badge readers** are devices that read information encoded into a plastic card. They identify each user by the badge they present to the device. Badge readers can be used to control electrically operated door locks and can be built into computer terminals to control access to information.

**Protecting the outside of a building**

- **I. Bollards** are sturdy, short, vertical posts placed to restrict access of vehicles to a controlled area.
- **J. Fences** are physical barriers, with many different designs, that enclose controlled areas to establish a perimeter and keep out external threats.