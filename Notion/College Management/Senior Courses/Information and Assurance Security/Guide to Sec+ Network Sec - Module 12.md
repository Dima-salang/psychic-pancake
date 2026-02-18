*cracks knuckles and leans forward with intense focus*

Ah, Module 12! **Vulnerability Management**—the discipline of finding and fixing holes before the bad guys do. This is where security becomes *operational*, where we stop theorizing and start systematically hunting for weaknesses.

*draws a target with holes in it*

The fundamental truth: **all software has bugs**. Some bugs are vulnerabilities—holes that let attackers in. Your job? Find them, prioritize them, fix them, verify they're fixed. Repeat forever.

This is not glamorous work. This is *sweeping the floors* of security. But neglect it, and your house fills with dirt until someone slips.

Let's get systematic.

---

## SECTION 1: VULNERABILITY SCANNING

*writes "Find the Holes"*

---

### 1.1 Vulnerability Scan Basics

*draws a scanner emitting probes*

**What it is**: Automated tool that probes systems, identifies services, compares against database of known vulnerabilities.

**How it works**:

1. **Discovery**: Find live hosts on network (ping sweep, ARP scan)
2. **Port scanning**: Identify open ports (TCP/UDP connect, SYN stealth, FIN/NULL/Xmas scans)
3. **Service detection**: Banner grabbing, protocol analysis—"This is Apache 2.4.41 on Ubuntu"
4. **Vulnerability identification**: Match against CVE database, configuration checks, version comparisons
5. **Reporting**: Prioritized list with severity, description, remediation

*draws the CVE entry*

**CVE** = Common Vulnerabilities and Exposures. Standardized identifier: CVE-2021-44228 (Log4j). Includes CVSS score (severity), description, references.

---

### 1.2 Sources of Threat Intelligence

*writes "Know What to Look For"*

**Vulnerability databases**:
- **NVD** (National Vulnerability Database): US government, comprehensive
- **MITRE CVE**: The numbering authority
- **Vendor advisories**: Microsoft, Cisco, Oracle—often more accurate, more timely

**Exploit databases**:
- **Exploit-DB**: Public exploits, proof-of-concept code
- **Metasploit**: Framework with integrated exploits

**Threat feeds**:
- **CISA KEV** (Known Exploited Vulnerabilities): Actively exploited in the wild—patch these *now*
- **EPSS** (Exploit Prediction Scoring System): Probability of exploitation, helps prioritize

*draws priority matrix*

```
High severity + Actively exploited = PATCH IMMEDIATELY
High severity + No exploit yet = Patch soon
Low severity + Actively exploited = Patch immediately (severity wrong!)
Low severity + No exploit = Scheduled maintenance
```

**The CISA KEV insight**: CVSS severity ≠ actual risk. A "medium" vulnerability with active exploitation is more dangerous than a "critical" with no known exploit.

---

### 1.3 Scanning Decisions

*draws branching paths*

**Credentialed vs. Uncredentialed**:

| | Uncredentialed | Credentialed |
|---|---|---|
| **Access** | External attacker view | Internal/authenticated view |
| **Depth** | Network-visible only | Registry, file system, patch level, configuration |
| **Accuracy** | False positives possible | More accurate, comprehensive |
| **Risk** | Non-intrusive | Requires account, potential system impact |

**Authenticated scanning finds more**. Patch management status, registry settings, installed software versions. But requires credentials, trust, potential performance impact.

**Scanning scope**:
- **External**: From internet, attacker's perspective
- **Internal**: Inside network, lateral movement perspective
- **Agent-based**: Software on endpoint, continuous, no network scan needed
- **Agentless**: Network-based, no endpoint installation

**Scan frequency**:
- Continuous: Agent-based, real-time visibility
- Periodic: Weekly, monthly network scans
- Event-driven: After major changes, new deployments

---

### 1.4 Running a Vulnerability Scan

*draws workflow*

**Preparation**:
1. **Scope definition**: What systems? What networks? IP ranges, exclusions
2. **Timing**: Maintenance windows, production sensitivity
3. **Credentials**: Secure storage, least privilege accounts
4. **Safety checks**: Exclude fragile systems, medical devices, industrial control

**Execution**:
- Configure scan policy (aggressive vs. safe, full vs. quick)
- Monitor for performance impact
- Handle authentication failures
- Document anomalies

**Post-scan**:
- Validate findings (false positive check)
- Correlate with asset inventory
- Prioritize based on context, not just CVSS

---

### 1.5 Analyzing Vulnerability Scans

*writes "Make Sense of the Noise"*

**CVSS scoring** (Common Vulnerability Scoring System):

*draws the formula*

```
Base Score = f(Attack Vector, Attack Complexity, Privileges Required, 
               User Interaction, Scope, Confidentiality Impact, 
               Integrity Impact, Availability Impact)

Score: 0.0-10.0
```

**Limitations of CVSS**:
- Technical severity only, not *your* risk
- No consideration of asset value, exposure, compensating controls
- Static score, doesn't update as exploitation evolves

**Environmental score**: Adjust CVSS for your context. Internet-facing vs. internal? Critical server vs. test lab?

**Temporal score**: Adjust for current threat. Exploit code available? Actively exploited? Patch available?

**Prioritization frameworks**:

*draws triangle*

```
        [CISA KEV]
       /          \
   [Threat intel]  [Asset criticality]
     /                  \
  [Exposure] -------- [Compensating controls]
              \
            [Patch difficulty]
```

**Risk = f(Threat, Vulnerability, Asset Value, Exposure)**

---

### 1.6 Addressing Vulnerabilities

*writes "Actually Fix Things"*

**Remediation options**:

| Approach | When |
|----------|------|
| **Patch** | Preferred. Vendor fix available, tested, deployed. |
| **Upgrade** | New version fixes vulnerability. Test compatibility. |
| **Configuration change** | Disable vulnerable feature, tighten settings. |
| **Compensating control** | WAF rule, IPS signature, network segmentation—temporary. |
| **Accept risk** | Formal decision, documented, reviewed periodically. Rare. |

**Patch management lifecycle**:
1. **Identify**: Scanning, vendor notifications
2. **Evaluate**: Test in non-production, assess impact
3. **Approve**: Change management, scheduling
4. **Deploy**: Automated or manual, staged rollout
5. **Verify**: Rescan, confirm remediation
6. **Document**: Compliance, audit trail

**Emergency patching**: Critical vulnerability, active exploitation. Bypass normal change process? Risk of breaking vs. risk of breach.

**Virtual patching**: WAF/IPS rule blocking exploitation. Buys time for actual patch. Not permanent—performance impact, potential bypass.

---

## SECTION 2: AUDITS AND ASSESSMENTS

*writes "Verify with Human Eyes"*

Automated scanning finds *known* vulnerabilities. Human assessment finds *unknown* weaknesses, logic flaws, configuration errors, process gaps.

---

### 2.1 Internal Audits

*draws internal review*

**Self-assessment**: Organization evaluates own security. Honest, comprehensive, improvement-focused.

**Frameworks**:
- **CIS Controls**: 18 prioritized security controls, implementation groups by maturity
- **NIST CSF**: Identify, Protect, Detect, Respond, Recover
- **ISO 27001**: Information security management system

**Audit components**:
- **Policy review**: Do documents match practice?
- **Configuration review**: System settings, hardening compliance
- **Log review**: Are we logging? Reviewing? Retaining?
- **Interview**: Do staff understand policies? Follow procedures?

**Gap analysis**: Current state vs. desired state. Roadmap for improvement.

---

### 2.2 External Assessments

*draws outside perspective*

**Third-party audit**: Independent evaluation. Objectivity, credibility, regulatory acceptance.

**Types**:
- **SOC 2**: Service organization controls, trust services criteria
- **ISO 27001 certification**: Formal certification audit
- **PCI DSS assessment**: Payment card industry compliance
- **FedRAMP**: US government cloud authorization

**Value**: Customer assurance, regulatory compliance, insurance requirements, board reporting.

---

### 2.3 Penetration Testing

*grins excitedly*

Now we're talking! **Authorized simulated attack**. Ethical hackers attempt to breach defenses.

*draws attack chain*

**Phases**:

1. **Reconnaissance**: Open source intelligence, social media, DNS records, employee names, technology stack identification

2. **Scanning**: Network mapping, service identification, vulnerability scanning (the tools we discussed)

3. **Gaining access**: Exploitation, credential attacks, social engineering

4. **Maintaining access**: Persistence, backdoors, lateral movement

5. **Covering tracks**: Log manipulation, evidence destruction (in simulation, documented for learning)

6. **Reporting**: Findings, exploitation proof, risk rating, remediation guidance

**Types of penetration tests**:

| Type | Scope | Use case |
|------|-------|----------|
| **Black box** | No prior knowledge | Simulates external attacker |
| **Gray box** | Limited credentials, some information | Simulates insider threat, compromised account |
| **White box** | Full access, documentation, source code | Comprehensive assessment, efficiency |
| **External** | From internet | Perimeter security |
| **Internal** | Inside network | Lateral movement, insider threat |
| **Web application** | Web apps specifically | OWASP Top 10, business logic flaws |
| **Wireless** | Wi-Fi networks | WPA2/WPA3, rogue APs |
| **Social engineering** | People | Phishing, vishing, physical access |
| **Red team** | Full scope, no restrictions, long duration | Adversary simulation, test detection/response |
| **Purple team** | Collaborative, red + blue together | Knowledge transfer, immediate feedback |

**Rules of engagement**: Written authorization, scope boundaries, emergency contacts, data handling, no production disruption without approval.

**The difference**: Vulnerability scan = "Here's a list of holes." Penetration test = "I got in through this hole, here's what I accessed, here's how to fix it."

---

## THE VULNERABILITY MANAGEMENT LIFECYCLE

*draws the circle*

```
    [IDENTIFY]
        ↑
[REPORT] ← [ASSESS] ← [PRIORITIZE]
    ↓           ↓
[VERIFY] → [REMEDIATE]
```

**Continuous cycle**: New vulnerabilities discovered daily. New systems deployed. Configurations drift. This never ends.

**Metrics**:
- Mean time to detect (MTTD)
- Mean time to remediate (MTTR)
- Vulnerability density (per asset, per department)
- Patch compliance percentage
- Critical vulnerability backlog trend

---

## KEY TOOLS AND TECHNOLOGIES

*lists rapidly*

| Category | Examples | Purpose |
|----------|----------|---------|
| Vulnerability scanners | Nessus, Qualys, Rapid7 InsightVM, Tenable.sc | Automated vulnerability detection |
| Web app scanners | Burp Suite, OWASP ZAP, Acunetix | Web-specific vulnerabilities |
| Container scanners | Trivy, Snyk, Anchore | Image vulnerability scanning |
| Cloud scanners | Prowler, ScoutSuite, CloudSploit | Cloud misconfiguration detection |
| Penetration testing | Metasploit, Cobalt Strike, Burp Suite, custom tools | Exploitation, post-exploitation |
| Asset management | ServiceNow, Lansweeper, CMDB | Know what you have to secure |
| Ticketing/tracking | Jira, ServiceNow, Remedy | Remediation workflow |

---

## THE DEEPER PRINCIPLE

*leans back thoughtfully*

Vulnerability management is **risk reduction through information**. You cannot fix everything. You *can* fix the things that matter most, that are most likely to be exploited, that protect your most valuable assets.

*draws the curve*

The goal is not zero vulnerabilities—that's impossible. The goal is **acceptable risk**, continuously reassessed, systematically reduced.

The organizations that get breached? Often they *knew* about the vulnerability. They just didn't prioritize it, didn't patch it in time, didn't scan the right system. Process failure, not tool failure.

---

*looks up brightly*

Module 12 in essence: **Know yourself, know your enemy, fix your holes, verify your fixes.** Sun Tzu with a scanner and a patch schedule.

Questions? Shall I walk through how a penetration tester actually exploits a vulnerability step by step? Or explore the economics of vulnerability disclosure—responsible disclosure, bug bounties, the gray market for exploits?

The holes are everywhere. The question is whether you're looking.