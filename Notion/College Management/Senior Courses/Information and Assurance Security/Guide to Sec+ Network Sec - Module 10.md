*cracks knuckles and bounces on toes*

Ah, Module 10! **Wireless Network Attacks and Defenses**—the invisible battlefield. This is where security gets *physical* in a way that cables don't. Radio waves flying through walls, through windows, through your neighbor's apartment. You can't see them, you can't touch them, but oh boy can you attack them.

*draws a building with waves emanating*

The fundamental problem: **wired networks have geography as a defense**. To tap a cable, I need physical access. To intercept Wi-Fi, I just need to be *close enough*. And "close enough" might mean a parking lot, a nearby building, or a drone with a directional antenna.

Let's dive into the radio.

---

## SECTION 1: WIRELESS ATTACKS

*writes "The Air is Hostile"*

---

### 1.1 Cellular Networks

*draws cell towers*

Your phone talks to towers using protocols like LTE, 5G. These *should* be secure—strong encryption, mutual authentication. But...

**IMSI Catchers ("Stingrays")**:

*draws a fake tower*

IMSI = International Mobile Subscriber Identity. Your phone's unique identifier.

The attack:
1. Attacker deploys fake cell tower, stronger signal than real towers
2. Phones automatically connect to strongest signal
3. Fake tower tells phones: "Downgrade to 2G/3G" (weaker or no encryption)
4. Attacker intercepts calls, texts, location data
5. Real tower impersonation: Forward to actual network, victim never knows

**SS7 Attacks**:

SS7 = Signaling System 7. The *backend* protocol connecting cellular networks worldwide. Designed in 1975 with *no security*.

Attacks via SS7:
- Location tracking: Query any phone's location
- Call/text interception: Redirect to attacker
- Fraud: Steal authentication codes sent via SMS

*shakes head*

The protocol trusts *any* network on the system. Compromise one small carrier in one country, access the global network.

---

### 1.2 Bluetooth Attacks

*draws short-range waves*

Bluetooth: 10-100 meters, depending on class. Seems limited, but dangerous.

**Bluejacking**: Send unsolicited messages to Bluetooth devices. Annoying, mostly harmless.

**Bluesnarfing**: Unauthorized access to device data—contacts, calendars, SMS. Old vulnerabilities, mostly patched.

**Bluebugging**: Take control of device—make calls, send messages, eavesdrop via microphone. The phone becomes a remote bug.

**BLE (Bluetooth Low Energy) vulnerabilities**:

Modern IoT devices use BLE for "beacons"—constant advertising of presence. Attackers can:
- Track devices (and thus people) by their unique BLE identifiers
- Spoof beacons to confuse apps
- Exploit parsing vulnerabilities in BLE stacks

**KNOB and KBD attacks**:
- Key Negotiation of Bluetooth: Force weak encryption keys
- Key Derivation Function: Weakness in how keys are generated

---

### 1.3 Near Field Communication (NFC) Attacks

*holds hands very close*

NFC: Extremely short range, ~4 centimeters. *Contactless* payments, access cards.

**The range myth**: "Only works when touching." Not quite. Specialized antennas can reach 1-2 meters. In a crowded subway, that's plenty.

**Eavesdropping**: Intercept NFC communication. Hard due to short range, but possible.

**Relay attacks**: The clever one.

*draws two attackers*

Attacker A stands near victim with card/phone. Attacker B stands near payment terminal. They communicate via fast radio link. Victim's card *thinks* it's at the terminal. Terminal *thinks* card is present. Transaction completes. Card never left victim's pocket.

**Skimming**: Malicious NFC reader harvests card data. Contactless cards reveal: card number, expiry date, *enough to make online purchases* in many cases.

---

### 1.4 Radio Frequency Identification (RFID) Attacks

*draws tiny chips*

RFID: Passive chips powered by reader's radio signal. Inventory tracking, access badges, passports.

**Types**:
- **Low frequency (125-134 kHz)**: Access cards, animal chips. Short range, no encryption typically.
- **High frequency (13.56 MHz)**: NFC, smart cards, passports. More capable.
- **Ultra-high frequency**: Supply chain, toll passes. Longer range.

**Attacks**:

**Cloning**: Read the ID, write to blank card. Many systems *only check the ID*, not cryptographic authentication.

**Skimming**: Covertly read RFID from distance. UHF tags readable at 10+ meters. Passport in your back pocket? Scannable.

**Replay attacks**: Capture legitimate authentication, replay later.

**Power analysis**: Measure power consumption during cryptographic operations to extract keys.

---

### 1.5 Wireless Local Area Network (WLAN) Attacks

*writes "Wi-Fi: The Big One"*

This is where most wireless security focus lies. 802.11 standards—b, g, n, ac, ax, now Wi-Fi 6/6E/7.

**The fundamental problem**: Wi-Fi broadcasts. The packets are in the air. Anyone with an antenna can *see* them. Encryption is the *only* protection.

**Attack: Rogue Access Points ("Evil Twin")**

*draws two identical networks*

1. Attacker creates Wi-Fi network with same SSID as legitimate network
2. Stronger signal, closer proximity, or jamming legitimate network
3. Victims connect to evil twin
4. Attacker provides internet (maintaining illusion) while intercepting all traffic
5. Credential harvesting, malware injection, traffic analysis

**Attack: Wardriving**

*draws a car with antenna*

Drive around, map Wi-Fi networks. GPS + network discovery. Not an attack itself, but reconnaissance. Find:
- Networks with weak encryption
- Default SSIDs (suggest default passwords)
- Hidden networks (still broadcast, just without SSID in beacon)

**Attack: Warchalking**

Old school: Physical symbols drawn near buildings indicating Wi-Fi status. Modern equivalent: Online databases of mapped networks.

**Attack: Packet Injection and Fuzzing**

Send malformed 802.11 frames to:
- Crash access points (DoS)
- Exploit driver vulnerabilities
- Manipulate client connections

**Attack: WPA/WPA2 Cracking**

*gets excited about the math*

WPA2-Personal uses **PSK** (Pre-Shared Key). The weakness: **4-way handshake**.

When client connects, there's a 4-message exchange establishing session keys. This handshake contains enough information to *verify* password guesses offline.

**The attack**:
1. Capture 4-way handshake (or just the first two messages)
2. Take candidate password, compute what the handshake *would* look like
3. Compare to captured handshake
4. Match? Password found. No match? Try next password.

*draws the handshake*

Modern GPUs can test *billions* of passwords per second. Weak PSKs fall in hours or minutes.

**WPA3 improvements**: Simultaneous Authentication of Equals (SAE) prevents offline dictionary attacks. Each password guess requires interaction with the access point—rate limited, detectable.

---

## SECTION 2: VULNERABILITIES OF WLAN SECURITY

*writes "Why Old Wi-Fi is Broken"*

---

### 2.1 Wired Equivalent Privacy (WEP)

*draws a broken lock*

WEP: 1997, "wired equivalent" security. **Completely broken. Do not use.**

**The flaws**:

**RC4 stream cipher weaknesses**:
- WEP uses RC4 with 24-bit IV (Initialization Vector)
- IV is *supposed* to be random, but 24 bits = only 16 million possible values
- On busy networks, IVs repeat ("IV collision")
- RC4 + key reuse = statistical attacks recover key

**Key recovery**: Tools like Aircrack-ng can recover WEP keys in *minutes* by collecting enough packets with weak IVs.

*shakes head sadly*

WEP was designed when export cryptography was restricted. Deliberately weak. Obsolete since 2004, yet still seen occasionally.

---

### 2.2 Wi-Fi Protected Setup (WPS)

*draws a "easy setup" button*

WPS: Push button or PIN to connect without typing long passwords. Convenience over security.

**PIN method flaw**:

8-digit PIN, but checked in *two halves*. First 4 digits, then last 4 digits.

*does the math on board*

Total combinations: 10,000 (first half) + 1,000 (second half, last digit is checksum) = **11,000 possibilities**.

Not 100 million. 11 thousand. Brute force in *hours*.

**Pixie Dust attack**: Some WPS implementations leak enough information in the protocol exchange to recover the PIN without full brute force. Minutes.

**Defense**: Disable WPS entirely. The convenience isn't worth the vulnerability.

---

## SECTION 3: WIRELESS SECURITY SOLUTIONS

*writes "How to Do It Right"*

---

### 3.1 MAC Address Filtering

*draws a bouncer with a list*

**The idea**: Only allow specific device MAC addresses to connect.

**The reality**: MAC addresses are **easily spoofed**. Attacker sniffs valid MAC from network traffic, changes their MAC to match. Bypassed in seconds.

**Verdict**: Security theater. Don't rely on it.

---

### 3.2 WPA, WPA2, WPA3

*draws evolution*

**WPA (2003)**: Temporary fix for WEP. TKIP encryption, still RC4-based. Deprecated, insecure.

**WPA2 (2004)**: Current standard for years. AES-CCMP encryption. Two modes:
- **Personal (PSK)**: Single password for all users. Vulnerable to offline cracking if password weak.
- **Enterprise (802.1X)**: Individual credentials, RADIUS authentication. Much stronger.

**WPA3 (2018)**: Modern security.

| Feature | WPA2 | WPA3 |
|--------|------|------|
| Encryption | AES-CCMP | AES-GCMP-256 |
| Key exchange | 4-way handshake | SAE (Dragonfly) |
| Forward secrecy | No | Yes |
| Offline cracking | Possible | Prevented |
| Open networks | No encryption | OWE (Opportunistic Wireless Encryption) |

**SAE (Simultaneous Authentication of Equals)**:
- Password-based, but *interactive*
- Each guess requires protocol exchange with AP
- Rate limiting prevents brute force
- Mathematical properties prevent offline attack

**Forward secrecy**: Session keys derived independently. Compromise long-term password? Past sessions still secure.

**OWE**: Even "open" WPA3 networks encrypt traffic between client and AP. No authentication, but *privacy* from eavesdroppers.

---

### 3.3 Additional Wireless Security Protections

**SSID Hiding ("Closed Network")**:

Don't broadcast network name. **Worthless for security**. SSID still visible in probe requests, association frames. Trivial to discover. Adds inconvenience, not protection.

**Enterprise Authentication (802.1X/EAP)**:

*draws RADIUS server*

Each user has unique credentials. Authentication via:
- **PEAP**: Protected EAP, username/password, server certificate validates identity
- **EAP-TLS**: Certificate-based, mutual authentication, strongest option

RADIUS server centralizes policy: valid user, but on wrong VLAN? Reject. Device not compliant? Quarantine.

**Wireless Intrusion Detection/Prevention Systems (WIDS/WIPS)**:

Dedicated sensors or access points in monitor mode, watching for:
- Rogue access points
- Evil twins
- Unauthorized clients
- Attack signatures (deauth floods, etc.)

Automatic response: Block rogue APs by sending deauthentication frames (controversial—may be illegal in some jurisdictions).

**Site Surveys and RF Planning**:

*draws coverage patterns*

- Measure signal strength, adjust power levels
- Minimize signal bleeding outside physical perimeter
- Identify rogue devices
- Channel planning to avoid interference

**Physical Security**:

*grins*

The forgotten layer. Directional antennas on buildings reduce leakage. Shielding in walls (Faraday cage principles) contains signals. Sometimes the best wireless security is... a wire.

---

## THE COMPLETE WIRELESS SECURITY PICTURE

*draws final architecture*

```
[Internet]
    ↓
[Firewall/Router]
    ↓
[WPA3-Enterprise Access Points]
    ├─ 802.1X authentication to RADIUS
    ├─ Certificate validation
    ├─ Strong encryption (AES-GCMP)
    ├─ Forward secrecy
    └─ Network segmentation by user role
         ├─ [Employee VLAN] ← Full access
         ├─ [Guest VLAN] ← Internet only, isolated
         └─ [IoT VLAN] ← Restricted, monitored
    
[WIDS sensors] ← Monitor for attacks, rogues
    
[Policy] ← Strong passwords, certificate deployment, 
           regular auditing, user education
```

---

## KEY TAKEAWAYS

*writes rapidly*

| Threat | Defense |
|--------|---------|
| Evil twin / Rogue AP | WIDS, certificate pinning, user education |
| WPA2 cracking | WPA3, strong passwords, Enterprise auth |
| WPS vulnerability | Disable WPS |
| WEP | Never use. Replace immediately. |
| Bluetooth attacks | Disable when not needed, patch devices |
| NFC/RFID skimming | Shielded wallets, contactless limits, transaction alerts |
| Cellular interception | Encrypted apps (Signal, etc.), not SMS for 2FA |

---

*leans back and grins*

Wireless security is physics meeting cryptography meeting human behavior. The signals are invisible, the attacks are real, and the defenses require both technical rigor and operational discipline.

The beautiful thing? WPA3, properly deployed, is genuinely strong. The tragedy? Most networks still run WPA2-Personal with passwords like "Company2024!" and wonder why they get breached.

*points at the board*

Questions? Shall I show you how the 4-way handshake math actually works? Or explore the fascinating world of software-defined radio, where a $20 USB dongle becomes a universal wireless attack tool?

The air is hostile, my friend. But it doesn't have to be defenseless.