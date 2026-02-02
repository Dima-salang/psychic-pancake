Hello. We have spent the previous modules designing the architecture of security. We have built blueprints, defined policies, analyzed risks, and selected technologies. Now, we face the most difficult phase of engineering: **Implementation**.

In my experience, a mediocre design well-implemented is often superior to a perfect design poorly executed. **Module 11** focuses on transforming the abstract "Information Security Blueprint" into a concrete, functioning reality. This requires a fusion of rigorous systems engineering, disciplined project management, and a deep understanding of organizational psychology.

This lecture covers **Implementing Information Security**. We will explore the Security Systems Development Life Cycle (SecSDLC), the mechanics of Project Management, technical implementation strategies like the Bull’s-Eye Model, and the critical human element of managing change.

---

# Lecture: Implementing Information Security

## I. Securing the Systems Development Life Cycle (SDLC)

Implementation is not simply "installing software." It is the process of changing the way an organization operates. To do this securely, we must integrate security into the very lifecycle of system creation.

### 1. The Philosophy: Software Assurance (SwA)

Historically, security was "bolted on" at the end of the development process. This is a catastrophic engineering failure. Security must be "baked in." This concept is **Software Assurance (SwA)**: the methodological approach to building security into the development life cycle.

- **The Goal:** To ensure that software functions as intended and _only_ as intended, free of vulnerabilities.
- **Common Body of Knowledge (CBK):** The DHS and DoD have defined a SwA CBK which dictates that security requirements must be defined alongside functional requirements.

### 2. The NIST SecSDLC Approach

The National Institute of Standards and Technology (NIST) outlines a specific five-phase approach to integrating security into the SDLC (NIST SP 800-64, Rev. 2). You must master these phases:

1. **Initiation:** Security planning begins here. We categorize information, assess business impact, and determine privacy requirements. If you wait until later, you are already behind.
2. **Development/Acquisition:** Whether building or buying, we perform risk assessments, analyze security requirements, and design the security architecture. We conduct functional and security testing _before_ the system is finalized.
3. **Implementation/Assessment:** The system is installed. We integrate it into the environment and plan/conduct certification and accreditation activities (verifying controls work).
4. **Operations/Maintenance:** This is the longest phase. We perform configuration management, continuous monitoring of controls, and operational readiness reviews.
5. **Disposal:** Often neglected. We must ensure information preservation (archiving) and media sanitization (wiping/destroying drives) before hardware is discarded.

### 3. Case Study: Microsoft's Security Development Lifecycle (SDL)

In the early 2000s, Microsoft paused development to retrain its engineers in security. The result was the **SDL**, a seven-phase methodology.

- **Key takeaway:** It culminates in an **Incident Response Plan**. You do not release software without knowing how you will respond when it breaks.

---

## II. Information Security Project Management

Implementing a blueprint is a complex project. It requires **Project Management (PM)**—the application of knowledge, skills, tools, and techniques to project activities to meet requirements.

### 1. Developing the Project Plan

The plan is the instruction manual for implementation.

- **Work Breakdown Structure (WBS):** This is the engineer's roadmap. We break the project into major tasks, then subtasks, then **action steps**.
    - _Rule of Thumb:_ A task becomes an action step when it can be completed by one person/skill set and has a single deliverable.
    - _Attributes:_ Each task must have a description, assigned resource (skill set), start/end dates, estimated effort, and dependencies.

### 2. Project Planning Considerations

When building the plan, you must navigate several constraints:

- **Financial:** You must have a Cost-Benefit Analysis (CBA) validated prior to the plan.
- **Priority:** Capital constraints often force prioritization. We prioritize controls based on the value of the asset and the severity of the threat.
- **Scope:** Avoid "scope creep." Implement large projects in stages (phased implementation).
- **Staffing:** You need qualified personnel. The project manager acts as the interface between the business and the technology.

### 3. Execution: The Feedback Loop

Once the project is running, we manage it using **Gap Analysis** (also called a cybernetic loop).

1. Set the goal (Project Plan).
2. Measure current progress.
3. Identify the gap between current progress and the goal.
4. Apply corrective action to close the gap.

**Warning: Projectitis** This is a phenomenon where the project manager spends more time manipulating the project management software (cosmetics) than managing the actual project. Avoid this. Focus on the work, not the chart.

---

## III. Technical Implementation Strategies

How do we physically introduce the new security system? We use specific conversion strategies and architectural models.

### 1. Conversion Strategies

Moving from the old system to the new one carries risk. We generally use one of four strategies:

1. **Direct Changeover:** Stop the old, start the new. High risk. If the new system fails, you have no backup.
2. **Phased Implementation:** Implement parts of the system over time. Lower risk, but takes longer.
3. **Pilot Implementation:** Implement the full system for a single group or location. If it fails, only that group is affected.
4. **Parallel Operations:** Run both the old and new systems simultaneously. Lowest risk (you have a safety net), but highest cost (you are paying for two systems) and highest workload (users may have to enter data twice).

### 2. The Bull’s-Eye Model

When prioritizing _what_ to fix, we work from the outside in. This is the **Bull’s-Eye Model**:

- **Layer 1 (Outer Ring): Policies.** You cannot implement technology without the policy to define its use. Fix policy first.
- **Layer 2: Networks.** Secure the perimeter and internal infrastructure. If the network is hostile, the systems cannot be safe.
- **Layer 3: Systems.** Secure the servers and desktops (OS hardening).
- **Layer 4 (Center): Applications.** Secure the specific software.
- _Engineering Logic:_ Implementing application security (Layer 4) on an insecure network (Layer 2) allows an attacker to bypass your application controls via the network. You must build the foundation first.

### 3. Technology Governance and Change Control

Complex environments require **Change Control**. You cannot simply "patch a server."

- **Process:** A change is requested -> Estimated impact is analyzed -> Approval is granted (or denied) -> Change is implemented -> Change is verified.
- **Governance:** This ensures that technological changes align with organizational goals and do not introduce new vulnerabilities.

---

## IV. The Non-Technical Aspect: Managing Change

The greatest barrier to implementation is often not technology, but **culture**. People resist change.

### 1. Lewin’s Change Model

To successfully implement security, we follow the psychological model developed by Kurt Lewin:

1. **Unfreezing:** Preparing the organization. Dismantling existing habits and convincing personnel that the change is necessary.
2. **Moving:** The transition period. Implementing the new security controls. This is often chaotic and difficult.
3. **Refreezing:** Establishing the new status quo. Making the new security habits permanent and accepted.

### 2. Overcoming Resistance

Employees resist change due to fear of the unknown, loss of status, or perceived complexity.

- **The Solution:** Resilience. We must cultivate a culture where change is viewed as necessary for survival. This requires a strong executive **champion** to validate the project and clear communication to explain _why_ the change is happening.

---

## V. Certification and Accreditation (C&A)

In high-stakes environments (like the federal government), systems must undergo C&A before deployment.

- **Certification:** The comprehensive technical evaluation of the security controls. (Does it work?)
- **Accreditation:** The official management decision to authorize the operation of the system, accepting the risk associated with it. (Are we allowed to use it?).
- _Note:_ In federal systems, this has largely been evolved into the **Risk Management Framework (RMF)**, but the concepts of technical verification (certification) and executive authorization (accreditation) remain fundamental to engineering rigor.

---

## Summary

To summarize Module 11:

1. **Design Security In:** Use the SecSDLC to integrate security from the Initiation phase through Disposal.
2. **Plan the Work:** Use Work Breakdown Structures (WBS) to detail every task, but avoid "Projectitis."
3. **Prioritize Correctly:** Use the Bull's-Eye model—Policy first, then Networks, Systems, and finally Applications.
4. **Manage the Humans:** Use Lewin’s model (Unfreeze, Move, Refreeze) to handle the inevitable resistance to change.

Implementation is where theory meets reality. It is messy, expensive, and difficult, but it is the only way to actually secure an organization.