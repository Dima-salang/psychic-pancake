Hello. We have arrived at **Module 12: Information Security Maintenance**.

In my years of engineering, I have seen many organizations celebrate the "go-live" date of a security architecture as if it were the finish line. This is a fatal error. In security, **implementation is merely the starting line**. The moment a system is deployed, it begins to degrade. Software ages, configurations drift, employees churn, and the threat landscape shifts beneath your feet.

This lecture focuses on the discipline of **maintenance**. We will explore the rigorous management models required to keep a program alive, the specific domains of monitoring, and the critical, often-overlooked realm of physical security.

---

# Lecture: Information Security Maintenance

## I. The Philosophy of Maintenance

Management models are frameworks that structure the tasks of managing business functions. In information security, the core concept is **continuous improvement**. We do not simply "maintain" the status quo; we cycle through assessment, evaluation, and change to close the gap between where we are and where we need to be.

We adhere to the guidance of **NIST Special Publication (SP) 800-100**, "Information Security Handbook: A Guide for Managers." This document outlines 13 specific areas of information security management that require ongoing monitoring.

### The 13 NIST Maintenance Areas

To maintain a program, you must actively monitor these areas:

1. **Governance:** Reviewing policies and ensuring alignment with organizational mission.
2. **SDLC:** Ensuring security is integrated into every phase of system development (Initiation to Disposal).
3. **Awareness and Training:** Tracking compliance and effectiveness of training.
4. **Capital Planning:** Integrating security into the investment budget (CPIC).
5. **Interconnecting Systems:** Managing the risks of shared data and connections (Interconnection Security Agreements - ISA).
6. **Performance Measures:** Using metrics to verify effectiveness (not just activity).
7. **Security Planning:** Updating strategic and tactical plans.
8. **Contingency Planning:** Periodically reviewing and testing IR, DR, and BC plans.
9. **Risk Management:** Continuously identifying and analyzing risks.
10. **Certification and Accreditation:** Continuously monitoring controls to ensure systems remain authorized to operate.
11. **Acquisition:** Managing security during the purchase of products and services.
12. **Incident Response:** Analyzing trends in incidents to improve detection and reaction.
13. **Configuration and Change Management (CCM):** Managing the effects of changes to hardware, software, and firmware.

---

## II. The Security Maintenance Model

Beyond the NIST list, we utilize a structured **Maintenance Model** consisting of five domains. This is the operational engine of your security program.

### Domain 1: External Monitoring

You cannot defend against what you do not see coming. The goal here is **early awareness** of new threats, vulnerabilities, and attacks.

- **Data Sources:** We gather intelligence from vendors, CERT organizations (like US-CERT), public networks (mailing lists like Bugtraq), and membership sites,.
- **The Process:** We collect raw intelligence, filter it for relevance (e.g., "Do we use the software mentioned in this alert?"), assign a risk impact, and communicate it to decision-makers.

### Domain 2: Internal Monitoring

This provides an informed awareness of the state of your _own_ network.

- **Characterization:** Maintaining a precise inventory of every device, channel, and application. If you don't know you have it, you cannot patch it.
- **IDPS Monitoring:** Using Intrusion Detection/Prevention Systems not just for alerts, but for **traffic analysis**. Analyzing unsuccessful attacks can reveal which of your device signatures are exposed.
- **Difference Analysis:** This is a crucial engineering concept. We capture a baseline of a system, and then periodically compare the current state to that baseline. Any unexplained difference (e.g., a new open port, a changed file size) indicates trouble.

### Domain 3: Planning and Risk Assessment

We must constantly look at the "big picture."

- **Operational Risk Assessment:** As new projects arise, we conduct Risk Assessments (RAs). These include Network Connectivity RAs, Business Partner RAs, and Application RAs.
- **Documentation:** Every RA must document the background, planned controls, and a formal **opinion of risk**.

### Domain 4: Vulnerability Assessment and Remediation

This is the process of identifying specific flaws and fixing them. We use four types of assessments:

1. **Internet VA:** Finding vulnerabilities in the public-facing network. This involves planning, target selection, scanning, and analysis.
2. **Intranet VA:** Scanning the internal network.
3. **Platform Security Validation (PSV):** Verifying that systems are configured according to policy (e.g., ensuring passwords expire).
4. **Wireless VA:** Scanning for insecure Wi-Fi access points.

**Penetration Testing:** Distinct from vulnerability assessment, this involves a security professional acting as a hacker to exploit vulnerabilities. It can be Black Box (no prior knowledge) or White Box (full knowledge).

**Remediation:** Once a flaw is found, we must repair it (patching), accept the risk, or remove the asset.

### Domain 5: Readiness and Review

We must keep the program itself healthy.

- **Policy Review:** Policies must be reviewed periodically. If the business changes, the policy must change.
- **Rehearsals:** Conducting war games and drills to ensure the team is ready.

---

## III. Physical Security

We often focus on digital packets, but physical access bypasses digital security. If I can steal your server, your firewall is irrelevant.

### 1. The Seven Major Sources of Physical Loss

Donn B. Parker identified these physical threats:

1. **Temperature:** Extreme heat or cold.
2. **Gases:** Humid/dry air or suspended particles.
3. **Liquids:** Water and chemicals.
4. **Living Organisms:** Viruses, bacteria, insects, rodents.
5. **Projectiles:** Objects in motion.
6. **Movement:** Collapse, shearing, vibration.
7. **Energy Anomalies:** Surges, static electricity, radiation.

### 2. Physical Controls

- **Perimeter:** We use walls, fences, and gates.
- **Locks:** Mechanical locks (keys) and electromechanical locks (smart cards, biometrics).
- **Mantraps:** A small enclosure with separate entry and exit points. One door must close before the other opens, preventing "tailgating".
- **Computer Rooms:** Must be secured with higher-level controls.

### 3. Environmental Controls (HVAC)

Heating, Ventilation, and Air Conditioning (HVAC) is a security system.

- **Temperature:** Computing equipment operates best between 70°F and 74°F. Heat above 100°F can damage media; 175°F damages hardware.
- **Humidity:** This is a delicate balance. High humidity causes condensation (short circuits). Low humidity causes **static electricity** (ESD). Optimal humidity is between 40% and 60%.
- **Static Electricity:** A human can generate 12,000 volts walking across a carpet. It takes only **10 volts** to damage a microchip. This represents a "catastrophic failure" (immediate) or a "latent failure" (delayed),.

### 4. Fire Security

Fire requires three elements: Temperature (ignition), Fuel, and Oxygen. Fire suppression systems work by removing one of these.

- **Water:** Sprinklers (wet pipe vs. dry pipe). Effective but damages equipment.
- **Gas:** Halon (banned due to ozone depletion) or replacements like FM-200. These remove oxygen or interrupt the chemical reaction without damaging electronics.

### 5. Power Management

- **UPS (Uninterruptible Power Supply):** Provides short-term backup power.
- **Energy Anomalies:** We must protect against:
    - **Fault:** Short-term interruption.
    - **Blackout:** Long-term interruption.
    - **Sag:** Short-term decrease in voltage.
    - **Brownout:** Long-term decrease in voltage.
    - **Spike:** Short-term increase (surge).
    - **Surge:** Long-term increase,.

### 6. Interception of Data

Data can be stolen physically via:

- **Direct Observation:** Shoulder surfing.
- **Interception of Data Transmission:** Tapping wires.
- **Electromagnetic Interception:** Known as **TEMPEST**. Equipment emits radiation that can be intercepted and reconstructed. The U.S. government has a program (TEMPEST) to shield devices from this monitoring.

---

## IV. Summary

Module 12 teaches us that security is not a product; it is a process.

1. **Monitor continuously:** Use the 5-domain maintenance model.
2. **Manage configuration:** Use CCM to control changes.
3. **Physical Security is critical:** Manage temperature, humidity, and power just as rigorously as you manage firewalls.

This concludes the lecture on Module 12. You now possess the framework to maintain security over the long term.