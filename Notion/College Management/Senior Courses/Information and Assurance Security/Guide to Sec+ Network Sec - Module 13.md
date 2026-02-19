*settles into chair with serious expression*

Ah, Module 13. **Incident Preparation and Investigation**—the moment when prevention fails and response begins. This is where security becomes crisis management, where calm process meets chaotic reality.

*draws a timeline*

The fundamental truth: **You will be breached.** Not "might." Not "if." Will. The question is whether you're ready, whether you can contain the damage, whether you can learn and improve.

This module is about **resilience**—bouncing back. And **forensics**—understanding what happened.

Let's prepare for the worst.

---

## SECTION 1: PREPARATORY PLANS

*writes "Plan Before Panic"*

---

### 1.1 Business Continuity Planning (BCP)

*draws organization continuing*

**Definition**: Maintain essential functions during and after a disruption. Not just cyber—natural disasters, pandemics, supply chain failures.

**Key elements**:

**Business Impact Analysis (BIA)**:
- Identify critical processes: What must continue for organization to survive?
- Determine Recovery Time Objective (RTO): Maximum acceptable downtime
- Determine Recovery Point Objective (RPO): Maximum acceptable data loss
- Calculate costs: Revenue loss, reputation damage, regulatory penalties

*draws tiers*

```
[Tier 1] Mission-critical: RTO 0-4 hours, RPO near-zero
[Tier 2] Essential: RTO 24 hours, RPO 4 hours  
[Tier 3] Important: RTO 72 hours, RPO 24 hours
[Tier 4] Deferrable: RTO 1+ weeks, RPO 1 week
```

**Continuity strategies**:
- **Hot site**: Fully operational alternate location, real-time replication. Expensive, instant failover.
- **Warm site**: Equipment ready, data restored from backup. Hours to activate.
- **Cold site**: Facility only, equipment must be procured. Days to weeks.
- **Cloud-based**: Infrastructure as code, spin up in alternate region. Modern preferred approach.

---

### 1.2 Incident Response Planning

*writes "When Security Fails"*

**Incident**: Security event that compromises confidentiality, integrity, or availability.

**Incident Response Plan (IRP)** components:

**Preparation** (before incident):
- Establish CSIRT (Computer Security Incident Response Team): Roles, contacts, authority
- Define incident severity levels: P1 (critical), P2 (high), P3 (medium), P4 (low)
- Legal and regulatory notification requirements: GDPR 72 hours, state breach laws
- Communication templates: Internal, customer, media, regulators
- Retainer agreements: External forensics, legal counsel, PR crisis management

**Detection and Analysis**:
- Alert sources: SIEM, IDS, user reports, threat intelligence
- Initial triage: True incident vs. false positive? Scope? Severity?
- Documentation: Chain of custody begins, every action logged

**Containment, Eradication, Recovery**:
- **Short-term containment**: Stop bleeding. Isolate affected systems, block malicious IPs, disable compromised accounts.
- **Long-term containment**: Temporary fixes while preparing full remediation. Patch, reconfigure, monitor.
- **Eradication**: Remove root cause. Delete malware, close vulnerabilities, remove attacker access.
- **Recovery**: Restore normal operations. Verify systems clean, monitor for recurrence.

**Post-Incident Activity**:
- Lessons learned: What worked? What failed? What surprised us?
- Evidence retention: Legal holds, regulatory requirements
- Plan updates: Incorporate learnings, update contact lists, refresh procedures

*draws the NIST cycle*

```
    [PREPARATION]
         ↑
[POST-INCIDENT] ← [DETECTION & ANALYSIS]
         ↓           ↓
   [LESSONS LEARNED] → [CONTAINMENT]
                            ↓
                    [ERADICATION]
                            ↓
                       [RECOVERY]
```

---

## SECTION 2: RESILIENCE THROUGH REDUNDANCY

*writes "Survive Failure"*

---

### 2.1 Servers

*draws multiple servers*

**High Availability (HA)**:
- **Active-Active**: Multiple servers handling load simultaneously. If one fails, others absorb. No downtime.
- **Active-Passive**: Standby server takes over if primary fails. Minutes of downtime.
- **Clustering**: Shared storage, automatic failover, health monitoring.

**Load balancing**: Distributes traffic, health checks, automatic removal of failed nodes.

---

### 2.2 Drives and Storage

*draws disk arrays*

**RAID** (Redundant Array of Independent Disks):
- **RAID 1**: Mirroring. Two drives, identical data. One fails, other continues.
- **RAID 5**: Striping with parity. Minimum 3 drives. One can fail, data reconstructed.
- **RAID 6**: Double parity. Two drives can fail simultaneously.
- **RAID 10**: Mirrored stripes. Performance + redundancy. Minimum 4 drives.

**Limitation**: RAID is not backup. Corruption, ransomware, deletion—replicated instantly. You need:

**Backup strategies**:
- **3-2-1 rule**: 3 copies, 2 different media, 1 offsite
- **Immutable backups**: Write-once, cannot be encrypted by ransomware
- **Air-gapped backups**: Physically disconnected, manual connection for restore
- **Snapshot versioning**: Point-in-time recovery, multiple restore points

---

### 2.3 Networks

*draws redundant paths*

**Redundant connectivity**:
- Multiple ISPs, automatic failover
- Diverse physical paths (different trenches, different directions)
- BGP routing for automatic path selection

**SD-WAN**: Software-defined wide area network. Multiple connection types (MPLS, broadband, LTE), intelligent routing, automatic failover.

---

### 2.4 Power

*draws electrical infrastructure*

**UPS (Uninterruptible Power Supply)**: Battery backup for immediate failover, graceful shutdown, or generator startup.

**Generators**: Diesel, natural gas. Hours to days of operation. Regular testing essential—" generator that doesn't start when needed is decoration."

**Dual power supplies**: Critical servers with two power feeds, each to different electrical circuits.

---

### 2.5 Sites and Geographic Distribution

*draws map with multiple locations*

**Disaster recovery sites**: Physical separation—different flood plains, earthquake zones, power grids.

**Cloud multi-region**: AWS us-east-1 fails? Failover to us-west-2. Automated, tested, documented.

---

### 2.6 Cloud Resilience

*writes "Cloud-Native Recovery"*

**Cloud advantages**:
- **Infrastructure as Code**: Entire environment defined in templates. Destroy and rebuild in minutes.
- **Auto-scaling**: Survive traffic spikes, DDoS by scaling out.
- **Multi-AZ, multi-region**: Built-in redundancy, automatic failover.
- **Backup services**: AWS Backup, Azure Backup, automated, encrypted, tested.

**Cloud risks**:
- Shared fate: Provider outage affects all customers (AWS us-east-1 outages, 2017, 2020)
- Configuration complexity: Misconfigured failover = no failover
- Cost: Redundancy costs money—test in production-scale drills

---

### 2.7 Data Resilience

*draws data protection layers*

**Encryption**: Protects confidentiality even if data stolen.

**Hashing and integrity checks**: Detect unauthorized modification.

**DLP (Data Loss Prevention)**: Prevent exfiltration, enable recovery.

**Data classification**: Know what's critical, protect accordingly.

---

## SECTION 3: INCIDENT INVESTIGATION

*writes "Find the Truth"*

---

### 3.1 Data Sources

*draws evidence sources*

**Logs**: The foundation of investigation.

| Source | What It Shows |
|--------|---------------|
| Firewall logs | Connections allowed/blocked, traffic patterns |
| IDS/IPS alerts | Attack signatures triggered |
| Operating system logs | Process execution, logins, file access, privilege changes |
| Application logs | User actions, errors, transactions |
| Authentication logs | Successful/failed logins, MFA events, ticket issuance |
| DNS logs | Domain lookups, potential C2 communication |
| Proxy/web logs | URLs visited, downloads, uploads |
| Endpoint telemetry | Process trees, network connections, memory artifacts |
| Cloud audit logs | API calls, configuration changes, access patterns |

**Network traffic**: Packet captures (pcap), NetFlow, full packet capture appliances.

**System artifacts**:
- File system: Timestamps, deleted files, alternate data streams
- Registry (Windows): Persistence mechanisms, execution history
- Memory: Running processes, network connections, injected code, credentials
- Volatile data: Loses on shutdown—capture first

---

### 3.2 Digital Forensics

*writes "Scientific Investigation"*

**Definition**: Recovery and investigation of material found in digital devices, often in relation to computer crime.

**Forensic principles**:

1. **Integrity**: Evidence must not change. Write blockers, cryptographic hashes (MD5, SHA-256) to verify.

2. **Chain of custody**: Document who handled evidence, when, where, why. Legal admissibility depends on it.

3. **Documentation**: Every action recorded, repeatable by another examiner.

4. **Validity**: Tools and methods scientifically valid, peer-reviewed, accepted by courts.

**Forensic process**:

*draws stages*

```
[IDENTIFICATION] → What evidence exists? Where is it?
        ↓
[PRESERVATION] → Secure, prevent modification, legal hold
        ↓
[COLLECTION] → Forensic imaging, exact bit-for-bit copy
        ↓
[EXAMINATION] → Analysis, keyword search, timeline construction
        ↓
[ANALYSIS] → Interpret findings, reconstruct events
        ↓
[PRESENTATION] → Report, testimony, understandable explanation
```

**Memory forensics**:

RAM contains:
- Running processes and loaded DLLs
- Network connections and encryption keys
- Malware that exists only in memory (fileless)
- User credentials, clipboard contents

Tools: Volatility, Rekall. Analyze memory dumps, find injected code, hook detection.

**Disk forensics**:

- **File carving**: Recover deleted files, unallocated space
- **Timeline analysis**: File system timestamps (MAC times: Modified, Accessed, Created)
- **Registry analysis**: USB device history, recent documents, user activity
- **Artifact analysis**: Browser history, email, chat logs, cloud storage sync

**Network forensics**:

- Reconstruct sessions, extract files
- Identify C2 communication patterns
- Correlate with threat intelligence (IPs, domains, certificates)

**Mobile forensics**:

- Device encryption challenges
- Cloud synchronization artifacts
- Application data, location history, communications

---

### 3.3 Malware Analysis

*grins*

When you find malware, understand it.

**Static analysis**: Examine without executing.
- Strings: IP addresses, URLs, filenames, error messages
- File headers: Packers, compilers, metadata
- Disassembly: Reverse engineer code logic
- YARA rules: Pattern matching for identification

**Dynamic analysis**: Execute in controlled environment (sandbox).
- Behavior monitoring: File system changes, registry modifications, network connections
- API hooking: What system functions does it call?
- Network emulation: Fake C2 server, observe communication

**Advanced techniques**:
- **Debugging**: Step through execution, modify behavior
- **Deobfuscation**: Unpack, decrypt, decode attacker protections

---

## THE COMPLETE INCIDENT RESPONSE

*draws the full timeline*

```
T-30 days: Preparation, planning, tabletop exercises
T-0: Detection - SIEM alert, user report, threat intel
T+5 min: Triage - validate, scope, severity, activate CSIRT
T+30 min: Containment - isolate, preserve evidence, stop bleeding
T+2 hours: Eradication planning - root cause analysis, remediation strategy
T+24 hours: Recovery begins - rebuild, restore, verify
T+1 week: Full operations restored, enhanced monitoring
T+30 days: Post-incident review, lessons learned, plan updates
T+90 days: Follow-up audit, verify fixes, test again
```

---

## KEY METRICS AND CONCEPTS

*writes rapidly*

| Metric | Definition | Target |
|--------|-----------|--------|
| MTTD (Mean Time to Detect) | Hours from intrusion to detection | <24 hours (best: minutes) |
| MTTR (Mean Time to Respond) | Hours from detection to containment | <4 hours |
| MTTC (Mean Time to Contain) | Hours to stop attacker access | <24 hours |
| Dwell time | Days attacker undetected | Industry average: 280 days. Target: <30. |

**The 1-10-60 rule**: Detect in 1 minute, investigate in 10, contain in 60. Aggressive, aspirational.

---

*leans forward intensely*

Module 13, you see, is about **organizational resilience**. Technical controls fail. People make mistakes. Adversaries adapt. What separates breached organizations from destroyed organizations is preparation: the plans, the redundancies, the practiced response, the forensic capability to understand and improve.

*stands up*

The worst time to write an incident response plan is during an incident. The worst time to test your backups is when you need them. The worst time to learn forensics is with attackers in your network.

Prepare. Test. Improve. Survive.

*grins*

Questions? Shall I walk through a specific incident—say, ransomware deployment step by step, from initial access to full recovery? Or explore the fascinating legal dimensions: attorney-client privilege in incident response, notification obligations, regulatory investigations?

The breach is inevitable. The chaos is optional.