Alright, let's slow down and really look at this. You want the whole picture, not just the view from the window. You want to know where every screw goes and why.

I've gone back to the bench, opened the book to the Table of Contents for Module 15, and I’m reading the blueprint. The module is titled **Information Security Management**, and it’s broken down into five distinct sections. It's a hierarchy of needs, really. You can't manage risk if you don't know your assets. You can't manage change if you don't have a process.

Let's disassemble the module, component by component, and see how the machine works.

***

### 1. Asset Protection: Knowing What You Have
The module starts with **Asset Management**. In physics, if you don't know the mass of an object, you can't calculate its momentum. In security, if you don't know what you have, you can't protect it.

*   **Asset Inventory:** This is the most basic step. You have to make a list. Every laptop, server, smartphone, and piece of software. But it's not just a list; it's an accounting of value. We use **Asset Value** to determine how much security we need to apply.
*   **Asset Classification:** Once you have the inventory, you classify it. Just like we classify elements into gases, liquids, and solids, we classify data. A public webpage is like a rock on the ground—it needs no protection. A list of customer credit cards is like a diamond—it goes in the safe. Classification levels typically include:
    *   **Public:** No harm if disclosed.
    *   **Internal:** Only for employees.
    *   **Confidential:** Could cause harm if disclosed.
    *   **Restricted/Highly Confidential:** Severe damage if disclosed (like government secrets).
*   **Data Sovereignty:** The module also touches on legal issues. Where does the data live? If your "cloud" is in Ireland but your company is in the US, whose laws apply? This is **Data Sovereignty**—the concept that digital information is subject to the laws of the nation where it is located. It's like a piece of luggage on a plane; it has to follow the laws of the country it's flying over.

### 2. Change Management: The Controlled Experiment
The second section is **Change Management**. This is the process of controlling the life cycle of changes to IT infrastructure. It's about preventing chaos.

*   **The "Why":** Why do we need this? Because systems are fragile. You change one configuration setting on a server, and suddenly the email system goes down. Change Management is the discipline to prevent that. It ensures standardized methods and procedures are used for all changes.
*   **The Process:** The book outlines a clear, step-by-step process:
    1.  **Request for Change (RFC):** Someone says, "I need to update this software."
    2.  **Assess the Impact:** Analyze the risk. What happens if this fails? Who does it affect?
    3.  **Build and Test:** Create the change in a sandbox environment (a lab). Don't experiment on the live system!
    4.  **Deployment:** Roll out the change to the real world.
    5.  **Document:** Record everything. If the change breaks something later, you need to know exactly what you did.
*   **The CAB (Change Advisory Board):** This is a brilliant concept. You don't let one person decide to change the reactor. You bring together a committee—a board of experts from different departments—to review major changes. They provide the checks and balances.

### 3. Risk Management: The Probability of Disaster
This is the heart of the module. **Risk Management** is the process of identifying, assessing, and controlling threats to an organization's capital and earnings.

*   **Defining Risk:** The book gives us a formula for risk. It’s a multiplication problem!
    $$ \text{Risk} = \text{Threat} \times \text{Vulnerability} \times \text{Asset Value} $$
    *   **Threat:** A potential cause of an unwanted incident (a hacker, a fire).
    *   **Vulnerability:** A weakness in the system (a software bug, a door without a lock).
    *   **Asset Value:** The worth of the asset being targeted.
    *   If any of these three variables is zero, the Risk is zero. If there's no threat, there's no risk. If there's no vulnerability, there's no risk.

*   **Risk Analysis:** How do we measure risk?
    *   **Quantitative Risk Analysis:** This is the math. We calculate **ALE (Annualized Loss Expectancy)**.
        *   **AV (Asset Value):** How much is the server worth? \$100,000.
        *   **EF (Exposure Factor):** If a fire happens, how much will we lose? 50%?
        *   **SLE (Single Loss Expectancy):** AV × EF = \$50,000 loss per fire.
        *   **ARO (Annualized Rate of Occurrence):** How often do fires happen? Once every 10 years? (0.1)
        *   **ALE:** SLE × ARO = \$5,000. This tells us we shouldn't spend \$10,000 on a fire suppression system for that specific risk. It's pure economics.
    *   **Qualitative Risk Analysis:** This is the "gut feeling" or subjective approach. We use scales like Low, Medium, and High. We create a **Risk Matrix**. It's less precise but faster and often sufficient for complex scenarios where you can't put a dollar sign on reputation.

*   **Risk Treatments:** Once you know the risk, what do you do?
    1.  **Mitigation:** You reduce the risk. (Install a firewall, patch the software).
    2.  **Transference:** You hand the risk to someone else. (Buy cyber insurance).
    3.  **Acceptance:** You accept the risk because the cost of fixing it is too high. You build a "rainy day fund" to pay for the loss if it happens.
    4.  **Avoidance:** You stop the activity that causes the risk. (Don't store credit card numbers if you don't want to be hacked for them).

*   **Risk Appetite:** This is the amount of risk an organization is willing to tolerate. A bank has a low appetite for risk. A tech startup might have a high appetite, taking big risks to get big rewards.

### 4. Oversight and Operations: The Human Factor
The next section deals with **Oversight and Operations**. You can have the best technology in the world, but it fails if the humans mess it up.

*   **Governance:** This is the framework. It's the policies, standards, and procedures.
    *   **Policies:** The high-level rules ("We shall protect data").
    *   **Standards:** The specific requirements ("Passwords must be 12 characters").
    *   **Procedures:** The step-by-step instructions ("Here is how you reset a password").
*   **Compliance:** This is checking if you are following the rules. Are you compliant with GDPR? Are you compliant with HIPAA? Compliance is binary: you are, or you aren't.
*   **Roles and Responsibilities:** Who does what?
    *   **Data Owner:** The person who is ultimately responsible for the data (usually a senior executive).
    *   **Data Steward:** The person who handles the data day-to-day (the database admin).
    *   **Data Custodian:** The person who implements the technical controls (the IT guy).
    *   **Auditor:** The person who checks if everyone else did their job.

### 5. Third-Party Risk Management: The Supply Chain
Finally, the module covers **Third-Party Risk Management (TPRM)**. No organization is an island. We all rely on vendors.

*   **The Problem:** You might have a secure network, but if your HVAC vendor has a weak password and they have remote access to your building, you are vulnerable. This is the **Supply Chain** attack surface.
*   **The Solution:** You must assess the risk of your vendors.
    *   **Questionnaires:** Send them a survey. "Do you have a firewall?"
    *   **On-Site Assessments:** Go visit their office. Check their physical security.
    *   **SOC Reports (Service Organization Control):** Ask for their audit report. A **SOC 2 Type II** report tells you that an auditor checked their security controls over a period of time. It's like a report card.
*   **Contractual Agreements:** Put it in writing.
    *   **SLA (Service Level Agreement):** "If the server goes down, you must fix it in 4 hours."
    *   **MOU (Memorandum of Understanding) / ISA (Interconnection Security Agreement):** Documents that define how two organizations will share data and connect securely.

***

### Summary
So, you see, Module 15 is about the **Scientific Method applied to Security**.
1.  **Observe** (Asset Management).
2.  **Hypothesize** (Risk Management).
3.  **Experiment Safely** (Change Management).
4.  **Analyze the System** (Governance and Operations).
5.  **Test the Boundaries** (Third-Party Risk).

It's not just "management"; it's the physics of keeping an organization alive. It's calculating the friction, managing the momentum, and ensuring the whole structure doesn't collapse under its own weight.


*adjusts chair and picks up the chalk*

Alright, let's dive into this. You want Module 15 - Information Security Management. This is where we step back from the technical bits and bytes and look at the *system* of security. The governance, the processes, the human element. This is what separates a technician from a security professional who can actually run a program.

Let me walk you through what I see in this textbook, but more importantly, what it *means* in practice.

---

## **Module 15: Information Security Management - A Deep Dive**

### **The Big Picture: Why This Module Matters**

*writes on the board*

Look, you can be the best cryptographer in the world. You can know every CVE, every exploit, every packet structure. But if you don't know how to *manage* security as a business function, you're just a very expensive liability. Information Security Management is about making security *sustainable*, *measurable*, and *aligned with business objectives*.

This module covers five key processes:
1. **Asset Management**
2. **Risk Management** 
3. **Third-Party Risk Management**
4. **Change Management**
5. **Awareness Management**

Let me break each of these down with the detail they deserve.

---

## **1. Asset Management (Page 504)**

*leans on the desk*

You cannot protect what you do not know you have. This sounds obvious, but I cannot tell you how many breaches occur because someone forgot about a server, a database, or a cloud instance running in an abandoned AWS account.

### **Asset Management Fundamentals**

**What is an asset?** In security terms, an asset is anything of value to the organization that requires protection. This includes:

- **Physical assets**: Servers, workstations, networking equipment, mobile devices
- **Data assets**: Customer databases, intellectual property, financial records, employee PII
- **Software assets**: Operating systems, applications, licenses
- **Human assets**: People with specialized knowledge or access
- **Intangible assets**: Reputation, brand value, regulatory compliance status

### **The Asset Management Lifecycle**

*draws a cycle on the board*

```
IDENTIFY → CLASSIFY → PROTECT → MONITOR → DISPOSE
    ↑___________________________________________↓
```

**Identification**: This is inventory. Not just "we have 500 laptops" but *which* laptops, *where* they are, *who* has them, *what's installed on them*, *what data they access*. Automated discovery tools are essential here - you cannot rely on spreadsheets maintained by humans.

**Classification**: Not all assets are equal. We classify based on:
- **Criticality**: How important is this to business operations?
- **Sensitivity**: What would happen if this were disclosed?
- **Value**: Replacement cost, revenue generation, regulatory penalties if lost

The textbook mentions data classifications (from Module 2) - this applies to assets too. A server holding public marketing materials is not the same as a server holding cryptographic keys.

**Protection**: Once classified, we apply appropriate controls. This is where your technical knowledge comes in - encryption, access controls, monitoring, backups. But the *decision* of what protection to apply comes from asset management.

**Monitoring**: Assets change. New software installed, configurations drift, ownership changes. Continuous monitoring ensures your inventory stays accurate.

**Disposal**: When assets reach end-of-life, secure disposal is critical. I don't just mean "throw it away" - I mean cryptographic wiping, certificate revocation, access removal, and chain-of-custody documentation.

### **Asset Management Tools and Techniques**

- **CMDB (Configuration Management Database)**: The single source of truth
- **Network discovery tools**: Nmap, asset scanners
- **Agent-based inventory**: Software installed on endpoints reporting back
- **Cloud asset management**: AWS Config, Azure Resource Manager, GCP Asset Inventory
- **Barcode/RFID tracking**: For physical assets

*points at the board*

The key insight here: **Asset management is the foundation for every other security process.** You cannot do vulnerability management on systems you don't know exist. You cannot do risk assessment on assets you haven't inventoried. You cannot protect data you don't know you have.

---

## **2. Risk Management (Page 512)**

*paces slowly*

Now we get to what I consider the *core* of security management. Risk management is the process of understanding what could go wrong, how likely it is, how bad it would be, and what we should do about it.

### **Defining Risk**

*writes*

```
RISK = THREAT × VULNERABILITY × IMPACT
```

Or more formally: The likelihood that a threat will exploit a vulnerability to cause harm to an asset, and the resulting business impact.

Let me unpack each component:

**Threat**: Any circumstance or event with the potential to adversely impact organizational operations. From Module 1, we know these include:
- Natural disasters (floods, earthquakes)
- Malicious actors (hackers, nation-states, insiders)
- Accidental actions (employee mistakes)
- System failures (hardware, software)

**Vulnerability**: A weakness in an information system, system security procedures, internal controls, or implementation that could be exploited by a threat source. From Module 12, we know how to find these through vulnerability scanning.

**Impact**: The magnitude of harm that could result from a security incident. This includes:
- Financial losses (direct theft, fines, legal costs)
- Operational disruption (downtime, lost productivity)
- Reputational damage (lost customers, brand harm)
- Regulatory consequences (compliance violations)
- Safety impacts (if critical infrastructure is involved)

### **The Risk Management Process**

*draws a detailed flowchart*

```
1. RISK IDENTIFICATION
        ↓
2. RISK ANALYSIS (Qualitative & Quantitative)
        ↓
3. RISK EVALUATION (Risk Matrix)
        ↓
4. RISK TREATMENT (Accept, Mitigate, Transfer, Avoid)
        ↓
5. RISK MONITORING & REVIEW
        ↓
    (back to 1)
```

**Step 1: Risk Identification**

We identify risks through:
- **Threat intelligence**: What are attackers doing right now? (Module 12)
- **Vulnerability assessments**: What weaknesses do we have? (Module 12)
- **Business impact analysis**: What matters most to the organization?
- **Historical data**: What has happened to us or our peers?
- **Scenario analysis**: "What if" exercises

**Step 2: Risk Analysis**

Two approaches:

*Qualitative Analysis*: Using subjective ratings (Low/Medium/High, or 1-5 scales). Faster but less precise. Good for initial triage.

*Quantitative Analysis*: Using actual numbers. 
- Annualized Rate of Occurrence (ARO): How many times per year?
- Single Loss Expectancy (SLE): Cost of one incident
- Annualized Loss Expectancy (ALE): ARO × SLE

Example: If a data breach costs $4M (SLE) and happens on average every 4 years (ARO = 0.25), then ALE = $1M per year.

**Step 3: Risk Evaluation**

We map risks on a matrix:

```
        IMPACT
        Low   Med   High
      +------+------+------+
T   H |  M   |  H   |  H   |
H   i |  M   |  M   |  H   |
R   g |      |      |      |
E   h |  L   |  M   |  H   |
A     |      |      |      |
T   L |  L   |  L   |  M   |
      +------+------+------+
```

L = Low risk (monitor)
M = Medium risk (plan treatment)
H = High risk (immediate action)

**Step 4: Risk Treatment**

Four options:

1. **Risk Avoidance**: Eliminate the risk entirely. Stop the activity, remove the asset, discontinue the service. Most effective but often impractical.

2. **Risk Mitigation**: Implement controls to reduce likelihood or impact. This is where most security spending goes. Firewalls, encryption, training, monitoring.

3. **Risk Transfer**: Move the risk to someone else. Cyber insurance, outsourcing, cloud providers. Note: You cannot transfer *all* risk - regulatory and reputational risk usually stay with you.

4. **Risk Acceptance**: Consciously decide to accept the risk. This is valid for low risks, or when mitigation cost exceeds potential loss. **Must be documented and approved.**

**Step 5: Risk Monitoring**

Risks change. New threats emerge, vulnerabilities are discovered, business context shifts. Continuous monitoring ensures risk assessments remain current.

### **Risk Management Frameworks**

The textbook mentions frameworks in Module 1. For risk specifically:
- **NIST SP 800-30**: Guide for Conducting Risk Assessments
- **ISO 27005**: Information Security Risk Management
- **FAIR (Factor Analysis of Information Risk)**: Quantitative risk analysis model
- **OCTAVE**: Operationally Critical Threat, Asset, and Vulnerability Evaluation

---

## **3. Third-Party Risk Management**

*leans forward*

This is increasingly critical. Your security is only as strong as your weakest vendor.

### **The Third-Party Risk Problem**

Modern organizations rely on hundreds or thousands of third parties:
- Cloud providers (IaaS, PaaS, SaaS)
- Managed security services
- Payment processors
- Data analytics firms
- Cleaning services (physical access!)
- Supply chain vendors

Each of these has:
- Access to your systems or data
- Their own security practices (unknown to you)
- Their own third parties (fourth-party risk!)

*draws on board*

```
YOU ←→ VENDOR A ←→ VENDOR B (fourth party)
  ↓
VENDOR C ←→ VENDOR D
```

### **Third-Party Risk Management Process**

**Due Diligence (Before Engagement)**:
- Security questionnaires
- SOC 2 reports, ISO 27001 certificates
- Penetration test results
- Financial stability checks
- Reference checks
- On-site assessments (for critical vendors)

**Contractual Controls**:
- Security requirements in contracts
- Right to audit clauses
- Incident notification requirements
- Data handling and destruction requirements
- Liability and indemnification

**Ongoing Monitoring**:
- Continuous security ratings (BitSight, SecurityScorecard)
- Annual reassessments
- Monitoring for breaches at vendors
- Review of vendor's third-party list (fourth-party risk)

**Incident Response Planning**:
- What happens when your vendor is breached?
- Communication protocols
- Data recovery procedures
- Contract termination procedures

### **Special Cases**

**Cloud Service Providers**: Shared responsibility model. AWS secures the *cloud*; you secure *in* the cloud. Many breaches occur from customer misunderstanding of this boundary.

**Open Source Software**: Third-party code in your applications. Supply chain attacks (like the SolarWinds incident) demonstrate the danger. Software composition analysis (SCA) tools are essential.

---

## **4. Change Management (Page 509)**

*sits on the edge of the desk*

Change is the enemy of security. Every change is an opportunity to introduce vulnerabilities, break controls, or create misconfigurations. But change is also necessary. The goal is *controlled* change.

### **What is Change Management?**

A formal process for managing all changes to IT systems, ensuring:
- Changes are authorized
- Risks are assessed
- Testing is performed
- Rollback plans exist
- Documentation is updated

### **Types of Changes**

**Standard Changes**: Pre-approved, low-risk, well-understood. Example: Adding a new user to an existing role. These follow a standard procedure.

**Normal Changes**: Require assessment and authorization. Most changes fall here. Example: Deploying a new application version.

**Emergency Changes**: Urgent changes needed to resolve an incident. Faster process but still requires documentation and retrospective review. Example: Blocking an IP address during an active attack.

### **The Change Management Process**

*draws*

```
1. CHANGE REQUEST
        ↓
2. CHANGE ASSESSMENT
   - Technical review
   - Security review
   - Business impact
        ↓
3. CHANGE APPROVAL
   (Change Advisory Board)
        ↓
4. CHANGE IMPLEMENTATION
   - Testing
   - Deployment
   - Verification
        ↓
5. POST-IMPLEMENTATION REVIEW
        ↓
6. DOCUMENTATION UPDATE
```

**Security's Role in Change Management**:

Security must review all changes for:
- New vulnerabilities introduced
- Control implications (does this bypass our DLP?)
- Compliance implications (does this affect our PCI scope?)
- Privilege changes
- Data flow changes

*emphatically*

The number of breaches caused by "we didn't think about security when we made that change" is staggering. Shadow IT (Module 1) is essentially uncontrolled change.

### **Change Management and Security Operations**

- **Patch Management**: A specialized change process (Module 12)
- **Configuration Management**: Ensuring changes don't drift from secure baselines
- **Release Management**: Coordinating application changes
- **Infrastructure as Code**: Changes managed through version control (Module 11)

---

## **5. Awareness Management**

*stands up*

Technology alone cannot secure an organization. People are both the weakest link and the strongest defense. Awareness management is about making security *cultural*, not just procedural.

### **The Human Element**

From Module 2 (Social Engineering), we know that attackers target people. From Module 5, we know that users click malicious links. From Module 6, we know that mobile users bypass security controls for convenience.

Security awareness aims to:
- Reduce human error
- Enable employees to recognize and report threats
- Create a security-conscious culture
- Ensure compliance with policies

### **Awareness Program Components**

**Training**:
- **New hire training**: Security basics before system access
- **Role-based training**: Different risks for executives, developers, finance staff, IT admins
- **Annual refresher**: Reinforcement and updates
- **Specialized training**: Phishing simulations, secure coding (Module 6), incident response

**Communications**:
- Security newsletters
- Threat briefings
- Posters and reminders
- "Security tip of the week"

**Measurement**:
- Phishing simulation click rates
- Training completion rates
- Quiz scores
- Incident reports from employees (desired behavior!)
- Security metrics in performance reviews

### **Effective Awareness Techniques**

*writes*

```
WHAT DOESN'T WORK          WHAT WORKS
─────────────────          ──────────
Annual PowerPoint          Interactive, scenario-based training
                          
Generic "don't click"      Specific, current threat examples
                          
Blame and punishment       Positive reinforcement for reporting
                          
One-size-fits-all          Targeted, role-based content
                          
Checking the box           Continuous, embedded in workflow
```

**Behavioral Science in Security Awareness**:
- **Nudges**: Default settings, visual cues that encourage secure behavior
- **Social proof**: "90% of your colleagues reported phishing emails last month"
- **Loss aversion**: Framing risks in terms of what could be lost
- **Immediate feedback**: Instant response to training quiz answers

### **Security Culture**

The ultimate goal is a culture where:
- Security is everyone's responsibility, not just IT's
- Reporting suspicious activity is normal and rewarded
- Security policies are understood as enabling business, not blocking it
- Employees feel comfortable asking security questions

*pauses*

This is hard. It takes years. But organizations with strong security cultures are dramatically more resilient.

---

## **Integration: How These Five Processes Work Together**

*draws a large diagram*

```
┌─────────────────────────────────────────┐
│         INFORMATION SECURITY            │
│              MANAGEMENT                 │
│                                         │
│  ┌─────────┐    ┌─────────┐            │
│  │  ASSET  │←──→│  RISK   │            │
│  │MANAGEMENT│    │MANAGEMENT│           │
│  └────┬────┘    └────┬────┘            │
│       │              │                  │
│       └──────┬───────┘                  │
│              ↓                          │
│       ┌─────────────┐                   │
│       │  THIRD-PARTY │                  │
│       │RISK MANAGEMENT│                 │
│       └──────┬──────┘                   │
│              ↓                          │
│  ┌─────────┐    ┌─────────┐            │
│  │ CHANGE  │←──→│AWARENESS│            │
│  │MANAGEMENT│    │MANAGEMENT│           │
│  └─────────┘    └─────────┘            │
│                                         │
└─────────────────────────────────────────┘
```

- **Asset Management** feeds **Risk Management** (you can't assess risk on unknown assets)
- **Risk Management** informs **Third-Party Risk Management** (vendors are assets with special risks)
- **Change Management** affects all assets and risks (changes create new risks)
- **Awareness Management** enables all processes (people execute all of these)

---

## **Governance, Risk, and Compliance (GRC)**

*writes "GRC" in large letters*

Module 14 covers Oversight and Operations, including governance and compliance. Module 15's risk management fits in the middle. Together, these form **GRC**:

- **Governance**: The framework of rules, practices, and processes by which an organization is directed and controlled (Module 14)
- **Risk Management**: Identifying and addressing risks (Module 15)
- **Compliance**: Ensuring adherence to laws, regulations, and standards (Module 14)

These are not separate silos. Effective security management integrates all three.

---

## **Key Metrics and KPIs**

How do you know if your information security management is working?

**Asset Management**:
- Percentage of assets inventoried
- Time to identify new assets on network
- Percentage of assets with assigned owners

**Risk Management**:
- Number of high risks open > 90 days
- Risk reduction year-over-year
- Percentage of risks with treatment plans

**Third-Party Risk**:
- Percentage of vendors assessed
- Average vendor risk score
- Time to assess new vendors

**Change Management**:
- Percentage of changes with security review
- Security incidents caused by changes
- Emergency change rate (indicator of planning quality)

**Awareness**:
- Phishing simulation click rate trend
- Security training completion
- Time to report suspicious emails

---

## **Common Pitfalls and Lessons Learned**

*leans back*

Let me share what I've seen go wrong:

1. **"We have a policy for that"**: Policies without enforcement are just paper. I've seen organizations with 200-page security policies that nobody follows.

2. **Risk assessment theater**: Checking boxes without actually understanding risks. "We rated everything Medium because that's safe."

3. **Vendor trust without verification**: "They're a big company, they must be secure." No. Verify.

4. **Change management as bureaucracy**: When the process is so slow that people work around it, you have shadow IT and uncontrolled risk.

5. **Awareness as annual punishment**: Making employees sit through boring training they immediately forget. Waste of time and money.

6. **Siloed security**: When asset management, risk management, and operations don't talk to each other. The left hand doesn't know what the right hand is protecting.

---

## **Connection to CompTIA Security+ Exam**

*points to the certification objectives*

From Appendix A and the module objectives:

- **5.1**: Explain the importance of risk management processes and concepts
- **5.2**: Explain the importance of business continuity and disaster recovery concepts (Module 13)
- **5.3**: Explain the importance of security policies, standards, and procedures
- **5.4**: Explain privacy and sensitive data concepts in relation to security
- **5.5**: Explain the importance of security compliance and regulations

Module 15 directly supports these objectives. The exam tests whether you understand:
- Risk terminology and calculations
- Risk treatment options
- The purpose of change management
- Third-party risk concepts
- Security awareness training effectiveness

---

## **Final Thoughts**

*puts down the chalk*

Module 15 is where cybersecurity becomes information security management. This is the difference between being able to configure a firewall and being able to run a security program.

The technical skills from Modules 1-14 are necessary but not sufficient. Without management processes, you have point solutions without strategy. You have tools without understanding of what matters. You have activity without progress.

Effective information security management means:
- Knowing what you have (Asset Management)
- Understanding what could go wrong (Risk Management)
- Managing your extended enterprise (Third-Party Risk)
- Controlling change (Change Management)
- Enabling your people (Awareness Management)

*looks at the board covered in diagrams*

This is hard work. It's not as exciting as exploit development or incident response. But it's what makes security *sustainable*. Organizations that do this well can weather attacks that would destroy others.

Any questions? Let's dig deeper into any of these areas.

---

*waits, chalk in hand*