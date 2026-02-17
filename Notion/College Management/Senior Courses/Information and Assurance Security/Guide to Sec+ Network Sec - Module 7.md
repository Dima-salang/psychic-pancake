*cracks knuckles and draws a complex diagram of authentication flows, access control matrices, and identity federation*

Ah, Module 7! **Identity and Access Management (IAM)**! This is the beating heart of security - who are you, how do I know, and what are you allowed to do? Every access decision flows through IAM. Get this wrong, and attackers walk right in. Get this right, and you enable secure, usable systems.

*steps back to admire the diagram*

Identity is the new perimeter. Firewalls matter, but ultimately, we're protecting data and systems accessed by **identities** - human and machine. Master IAM, and you've mastered the central challenge of information security.

---

## **The Fundamentals: Authentication, Authorization, Accounting**

*writes "AAA" in large letters*

I introduced this in Module 1. Now we go deep.

| Component | Question Answered | Technical Mechanism |
|-----------|-------------------|---------------------|
| **Authentication** | Are you who you claim to be? | Passwords, tokens, biometrics, certificates |
| **Authorization** | What are you allowed to do? | Access control lists, roles, attributes, policies |
| **Accounting** | What did you actually do? | Logging, auditing, monitoring |

These are distinct! Authentication without authorization: "I know who you are, but I don't know what you can access." Authorization without authentication: "Anyone can do anything if they know the right incantation." Both fail catastrophically.

---

## **Authentication: Proving Identity**

*draws a pyramid of authentication factors*

### **Authentication Factors - Something You...**

| Factor Category | What It Is | Examples | Strengths | Weaknesses |
|-----------------|------------|----------|-----------|------------|
| **Know** | Information | Password, PIN, security questions | Easy to deploy, well understood | Can be guessed, phished, shared, forgotten |
| **Have** | Physical object | Smart card, phone, token, YubiKey | Hard to duplicate, possession required | Can be lost, stolen, cloned |
| **Are** | Biometric characteristic | Fingerprint, face, iris, voice | Unique, always with you | Can't be changed if compromised, privacy concerns, false accept/reject rates |
| **Do** | Behavioral pattern | Typing rhythm, gait, mouse movements | Passive, continuous authentication | Accuracy varies, privacy concerns |

*emphasizes*

**Multi-factor authentication (MFA)** combines factors from different categories. Password + SMS code? Two factors (know + have). Password + fingerprint? Two factors (know + are). Password + security question? **NOT MFA** - both are "know."

MFA is the single most effective control against credential theft. Google reported: 100% of automated bot attacks blocked, 99% of bulk phishing attacks blocked, 90% of targeted attacks blocked, with MFA enabled.

---

## **Passwords: The Persistent Problem**

*shakes head*

Passwords are terrible, but they're everywhere. Let's understand why and how to manage them.

### **Password Vulnerabilities**

| Attack Type | How It Works | Defense |
|-------------|--------------|---------|
| **Brute force** | Try all possible passwords | Account lockout, rate limiting, long passwords |
| **Dictionary attack** | Try common words and variations | Block common passwords, require complexity |
| **Credential stuffing** | Try username/password pairs from breached databases | Unique passwords per site, MFA, breach detection |
| **Phishing** | Trick user into entering password on fake site | User training, password managers, FIDO2/WebAuthn |
| **Keylogging** | Malware records keystrokes | Endpoint protection, on-screen keyboards for sensitive entry |
| **Shoulder surfing** | Observer watches password entry | Privacy screens, awareness, non-visual authentication |

### **Password Best Practices**

*writes guidelines on the board*

| Aspect | Recommendation | Rationale |
|--------|---------------|-----------|
| **Length** | Minimum 12-16 characters | Longer is stronger than complex; 12 random chars > 8 complex |
| **Complexity** | Require variety, but don't over-specify | Some complexity helps, but "P@ssw0rd!" is still weak |
| **Uniqueness** | Never reuse across sites | Breach of one site shouldn't compromise others |
| **Rotation** | Only on compromise, not periodic | Forced rotation leads to predictable patterns (Password1, Password2) |
| **Storage** | Salted, iterated hashing (Argon2, bcrypt, PBKDF2) | Prevents rainbow table attacks, slows brute force |
| **Transmission** | Never in plaintext, use TLS | Prevents network interception |

*explains password hashing*

**Proper password storage:**
1. Generate random **salt** (unique per password)
2. Hash: `hash = Argon2(password + salt)`
3. Store: `salt + hash`
4. Verification: recompute hash with stored salt, compare

**Argon2** (winner of Password Hashing Competition) is memory-hard - requires significant RAM to compute. Makes GPU/ASIC attacks expensive. Parameters: memory cost, time cost, parallelism.

### **Password Managers**

The practical solution to password uniqueness problem:

| Type | How It Works | Examples | Considerations |
|------|--------------|----------|----------------|
| **Local** | Database encrypted on device | KeePass, 1Password (local mode) | Device loss = loss of passwords (with backups!) |
| **Cloud-synced** | Encrypted database synced across devices | 1Password, Dashlane, Bitwarden | Trust vendor's security, enable MFA |
| **Browser-integrated** | Built into browser | Chrome, Firefox, Safari | Convenient, but browser compromise = password compromise |
| **Enterprise** | Centralized management, policy enforcement | LastPass Enterprise, CyberArk | Audit, sharing, lifecycle management |

*recommends*

Use a password manager! Generate unique, random 20+ character passwords for every site. Remember one strong master password. Enable MFA on the password manager itself.

---

## **Modern Authentication: Beyond Passwords**

### **FIDO2 / WebAuthn - The Passwordless Future**

*gets excited*

This is revolutionary! **FIDO2** (Fast Identity Online) standard, with **WebAuthn** (Web Authentication) component, enables **public key cryptography** for web authentication.

**How it works:**

1. **Registration:**
   - User authenticates to site (existing method, or during account creation)
   - Authenticator (YubiKey, phone, laptop TPM) generates key pair
   - Private key stays in authenticator, protected by biometrics/PIN
   - Public key sent to server

2. **Authentication:**
   - Server sends random challenge
   - Authenticator signs challenge with private key
   - Server verifies signature with stored public key

*draws the flow*

```
User + Authenticator                          Server
       |                                         |
       | <----------- Challenge ----------------- |
       |                                         |
       | [User verifies with biometric/PIN]      |
       | [Authenticator signs challenge]         |
       | ----------- Signed response -----------> |
       |                                         |
       | [Server verifies with public key]       |
       |                                         |
       | <----------- Authenticated ------------- |
```

**Security properties:**
- **Phishing-resistant:** Origin-bound credentials - key only works for registered site
- **No shared secrets:** No password to steal from server breach
- **No replay:** Challenge-response prevents replay attacks
- **Device-bound:** Private key never leaves authenticator

*explains attestation*

**Attestation:** Authenticator proves it's genuine (not malware emulating authenticator). Important for high-security scenarios. Privacy concern: attestation can be used for tracking. Modern authenticators use **direct anonymous attestation** or **none** to preserve privacy.

### **OAuth 2.0 and OpenID Connect - Delegated Authentication**

*draws authorization flow*

"Login with Google/Facebook/Apple" - ubiquitous, but often misunderstood.

| Component | Purpose |
|-----------|---------|
| **OAuth 2.0** | Authorization framework - "Can app X access my data Y?" |
| **OpenID Connect (OIDC)** | Authentication layer on top of OAuth 2.0 - "Who is this user?" |

**OAuth 2.0 Flow (Authorization Code Grant):**

```
User                            App (Client)                    Authorization Server (Google)
 |                                  |                                      |
 |----- Click "Login with Google" ->|                                      |
 |                                  |----- Authorization Request -------->|
 |                                  |    (client_id, scope, redirect_uri)  |
 |                                  |                                      |
 |<---- Redirect to Google login ---|                                      |
 |                                  |                                      |
 |----- Authenticate to Google ----------------------------------------->|
 |                                  |                                      |
 |<---- Redirect back to app with authorization code --------------------|
 |                                  |                                      |
 |                                  |----- Token Request ----------------->|
 |                                  |    (authorization code, client_secret)|
 |                                  |                                      |
 |                                  |<---- Access Token + ID Token --------|
 |                                  |                                      |
 |                                  | [Use access token to call APIs]      |
 |                                  | [Use ID token to identify user]      |
```

*warns*

**Common OAuth mistakes:**
- Implicit flow in mobile apps (token exposed to interception)
- Missing state parameter (CSRF attacks)
- Overly broad scopes (app requests more permissions than needed)
- Not verifying ID token signature and claims

**PKCE (Proof Key for Code Exchange):** Extension for public clients (mobile apps) where client secret can't be kept confidential. Prevents authorization code interception attacks. Should be used for all mobile OAuth implementations.

### **SAML - Enterprise Federation**

*draws enterprise diagram*

**Security Assertion Markup Language** - XML-based standard for enterprise single sign-on (SSO).

| Component | Role |
|-----------|------|
| **Identity Provider (IdP)** | Authenticates users, issues assertions (Okta, Azure AD, ADFS) |
| **Service Provider (SP)** | Relies on IdP for authentication (applications) |
| **Assertion** | XML document stating "User X authenticated at time Y" |

**SAML Flow:**
1. User attempts to access SP application
2. SP redirects to IdP with authentication request
3. IdP authenticates user (if not already authenticated)
4. IdP generates signed SAML assertion
5. User submits assertion to SP
6. SP verifies signature, grants access

*compares*

| Aspect | SAML | OIDC/OAuth 2.0 |
|--------|------|----------------|
| Format | XML | JSON |
| Complexity | Higher | Lower |
| Mobile-native | Poor | Excellent |
| Enterprise adoption | Established | Growing rapidly |
| Modern recommendation | Legacy maintenance | New implementations |

---

## **Authorization: Controlling Access**

*clears section of board for access control models*

Authentication asks "who are you?" Authorization asks "what can you do?"

### **Access Control Models**

| Model | How It Works | Best For | Limitations |
|-------|--------------|----------|-------------|
| **Discretionary Access Control (DAC)** | Resource owner decides who can access | Simple systems, personal files | No centralized control, propagation of access |
| **Mandatory Access Control (MAC)** | System-enforced labels (classification levels) | High-security, military, government | Inflexible, complex administration |
| **Role-Based Access Control (RBAC)** | Permissions assigned to roles, users assigned to roles | Enterprises, organized hierarchies | Role explosion, doesn't handle context |
| **Attribute-Based Access Control (ABAC)** | Policies based on attributes of user, resource, environment | Complex, dynamic environments | Policy complexity, performance |
| **Policy-Based Access Control (PBAC)** | Centralized policies evaluated at decision point | Modern, distributed systems | Infrastructure requirements |

*explains ABAC with example*

**ABAC Policy Example:**
```
PERMIT IF:
  user.department == "Finance" AND
  resource.sensitivity == "Internal" AND
  resource.type == "Budget" AND
  environment.time.hour >= 9 AND
  environment.time.hour <= 17 AND
  environment.location.country == "US"
```

Dynamic, context-aware, fine-grained. But writing and debugging these policies is hard!

### **RBAC in Detail - The Enterprise Standard**

*draws role hierarchy*

```
                    Senior Manager
                         |
          -----------------------------
          |                           |
      Manager A                   Manager B
          |                           |
    -------------               -------------
    |           |               |           |
  Employee 1  Employee 2    Employee 3  Employee 4

Roles: Employee, Manager, Senior Manager
Permissions assigned to roles, inherited down hierarchy
```

**RBAC Components:**
- **User**: Person or system entity
- **Role**: Job function or responsibility
- **Permission**: Authorization to perform operation on resource
- **Session**: Mapping of user to subset of authorized roles

**RBAC Levels (ANSI standard):**
- **RBAC0**: Core - users, roles, permissions, sessions
- **RBAC1**: Hierarchical - roles can inherit from other roles
- **RBAC2**: Constraints - separation of duties, cardinality
- **RBAC3**: Combined - RBAC1 + RBAC2

*emphasizes separation of duties*

**Separation of Duties (SoD):** No single user can complete sensitive transaction alone. Example: one role initiates wire transfer, different role approves it. Prevents fraud, errors, insider threats.

### **Access Control Lists (ACLs) and Capabilities**

**ACLs:** Resource-centric. "This file can be read by Alice, written by Bob."

| File | Owner | Group | Others |
|------|-------|-------|--------|
| report.doc | Alice: rw | Finance: r | -- |

Unix file permissions simplified. Modern systems use **Access Control Entries (ACEs)** in **Access Control Lists (ACLs)** for finer granularity.

**Capabilities:** Subject-centric. "Alice possesses tokens allowing read of File1, write of File2."

*compares*

| Aspect | ACLs | Capabilities |
|--------|------|--------------|
| Revocation | Easy (remove from resource) | Hard (find all subjects with capability) |
| Delegation | Hard | Easy (pass capability to another) |
| Audit | "Who can access this?" easy | "What can Alice access?" easy |
| Implementation | Most common (Unix, Windows) | Some systems (Kerberos tickets, smart cards) |

---

## **Directory Services - The Identity Store**

*draws directory tree*

Where do all these identities live? **Directory services**.

### **Active Directory (AD) - The Dominant Platform**

Microsoft's directory service, ubiquitous in enterprises.

| Component | Function |
|-----------|----------|
| **Domain Controllers** | Servers hosting AD database, handling authentication |
| **Domains** | Administrative boundary, security policy container |
| **Forests** | Collection of domains with shared schema |
| **Trees** | Hierarchy of domains with contiguous DNS namespace |
| **Organizational Units (OUs)** | Containers for users, computers, groups, enabling delegation |
| **Group Policy Objects (GPOs)** | Centralized configuration and security settings |

**AD Authentication Protocols:**
- **Kerberos** (preferred): Ticket-based, mutual authentication, SSO
- **NTLM** (legacy): Challenge-response, vulnerable to relay attacks
- **LDAP** (directory access): Query and modify directory objects

*explains Kerberos*

**Kerberos Authentication:**
1. User authenticates to Authentication Server (AS), receives Ticket Granting Ticket (TGT)
2. User presents TGT to Ticket Granting Server (TGS), receives service ticket
3. User presents service ticket to application server
4. Mutual authentication achieved

Tickets are time-limited, encrypted, contain authorization data. No passwords sent after initial authentication.

**AD Security Concerns:**
- **Pass-the-hash**: Steal NTLM hash, authenticate without password
- **Golden ticket**: Forge TGT with krbtgt account hash - domain compromise
- **Silver ticket**: Forge service ticket for specific service
- **DCShadow**: Register rogue domain controller, replicate malicious changes
- **ACL attacks**: Abuse excessive AD permissions for privilege escalation

*recommends*

**Active Directory security best practices:**
- Regular AD security assessments (BloodHound tool excellent for this)
- Least privilege for AD admin accounts
- Privileged Access Workstations (PAWs) for admin tasks
- Just-in-time admin access (Privileged Access Management)
- Attack surface reduction (remove NTLM where possible)

### **LDAP and Modern Directory Services**

**Lightweight Directory Access Protocol** - standard for directory services.

| Implementation | Characteristics |
|----------------|---------------|
| **OpenLDAP** | Open source, flexible, complex configuration |
| **Active Directory** | Microsoft's extended LDAP + Kerberos + proprietary |
| **Azure AD** | Cloud-native, modern authentication, hybrid capable |
| **Okta/OneLogin** | Cloud identity providers, SaaS-focused |
| **Amazon Cognito** | AWS-integrated, developer-friendly |

*notes cloud shift*

Modern trend: **cloud directory services**. Azure AD (now Entra ID) has surpassed traditional AD in new deployments. Okta, Ping Identity for SaaS integration. Traditional AD remains for on-premises resources, but **hybrid identity** (synchronized cloud and on-premises) is standard.

---

## **Privileged Access Management (PAM)**

*becomes serious*

Regular users have limited access. **Privileged accounts** - administrators, service accounts, root - have extensive access. Compromise of privileged account = game over.

**PAM Components:**

| Component | Function |
|-----------|----------|
| **Privileged Account Discovery** | Find all privileged accounts across systems |
| **Password Vaulting** | Store privileged credentials securely, rotate regularly |
| **Session Recording** | Monitor and record all privileged sessions |
| **Just-in-Time (JIT) Access** | Grant privilege only when needed, for limited time |
| **Privilege Elevation** | Standard user can request temporary elevation |
| **Application-to-Application Password Management** | Secure credential injection for service accounts |

*explains break-glass*

**Break-glass accounts:** Emergency access accounts with standing privilege, heavily monitored, credentials stored in physical safe. For when PAM system itself is down. Use = instant security incident investigation.

---

## **Identity Governance and Administration (IGA)**

*draws lifecycle diagram*

Identity management across the **entire lifecycle**:

| Stage | Activities |
|-------|------------|
| **Joiner** | Onboard new identity, provision accounts, assign initial access |
| **Mover** | Role change, transfer - modify access accordingly |
| **Leaver** | Termination - revoke all access promptly |
| **Access Review** | Periodic certification that access is still appropriate |

**IGA Functions:**
- **Provisioning/deprovisioning** - Automated account lifecycle
- **Access request workflow** - Self-service with approval
- **Access certification** - Managers review direct reports' access
- **Segregation of duties enforcement** - Prevent toxic combinations
- **Audit and compliance reporting** - Prove who had what access when

*emphasizes timely deprovisioning*

**Orphaned accounts** - accounts belonging to departed employees, still active. Major vulnerability! Studies show average time to deprovision is days to weeks. Should be hours. Automated provisioning/deprovisioning essential.

---

## **Zero Trust Architecture - The Modern Paradigm**

*draws new architecture diagram*

Traditional security: **trust but verify**. Inside the firewall = trusted. Outside = untrusted.

**Zero Trust:** **Never trust, always verify**. Every access request is fully authenticated, authorized, and encrypted, regardless of origin.

| Principle | Implementation |
|-----------|---------------|
| **Verify explicitly** | Always authenticate and authorize based on all available data points |
| **Use least privilege access** | Just-in-time, just-enough access with adaptive policies |
| **Assume breach** | Minimize blast radius, segment access, verify end-to-end encryption |

**Zero Trust Pillars:**

| Pillar | Focus |
|--------|-------|
| **Identity** | Strong authentication, continuous validation, risk-based policies |
| **Devices** | Device health verification, compliance checking |
| **Applications** | Application-aware access controls, secure configuration |
| **Data** | Classification, encryption, access controls |
| **Infrastructure** | Versioning, configuration, JIT access |
| **Network** | Micro-segmentation, real-time threat protection, encryption |

*explains continuous authentication*

**Continuous authentication:** Not just "authenticate at login." Re-evaluate risk constantly:
- Device posture changed? (jailbreak detected, security software disabled)
- Location anomalous? (impossible travel, unexpected country)
- Behavior unusual? (accessing unusual resources, at unusual time)
- Threat intelligence indicates risk? (IP address in botnet list)

Response: step-up authentication (MFA challenge), restrict access, or terminate session.

---

## **Emerging Identity Technologies**

| Technology | What It Does | Status |
|------------|--------------|--------|
| **Decentralized Identity (DID)** | Self-sovereign identity, user controls own credentials | Emerging standards, limited adoption |
| **Verifiable Credentials** | Cryptographically signed claims, privacy-preserving | W3C standard, pilot implementations |
| **Blockchain identity** | Distributed trust, immutable audit | Hype exceeding practical use |
| **Continuous authentication** | Behavioral biometrics, risk-based step-up | Growing adoption |
| **Passwordless** | FIDO2, WebAuthn, biometrics | Rapidly becoming standard |

*explains decentralized identity*

**Traditional:** Identity provider (Google, government) issues credential, controls your identity.

**Decentralized:** You hold cryptographic key pair. Issuers (university, employer) issue **verifiable credentials** - signed statements about you. You present these to verifiers (bank, service). No central identity provider tracks all your interactions. Privacy-preserving, user-controlled.

---

## **Summary - The Identity-Centric Security Model**

*steps back, surveying the complex board*

Module 7 teaches us that **identity is the primary security control plane**.

The evolution:
- **Perimeter-centric** (firewalls, network zones) → insufficient for mobile, cloud
- **Data-centric** (classification, encryption) → necessary but not sufficient
- **Identity-centric** (who accesses what, from where, with what risk) → modern approach

Key principles:

1. **Strong authentication** - MFA, moving toward passwordless
2. **Least privilege** - minimal access necessary, just-in-time when possible
3. **Continuous verification** - trust degrades over time, re-evaluate constantly
4. **Comprehensive authorization** - RBAC, ABAC, policy-based, context-aware
5. **Lifecycle management** - provision correctly, review regularly, deprovision promptly
6. **Privileged protection** - special controls for powerful accounts
7. **Audit everything** - who did what, when, from where - essential for forensics

*leans on the desk, thoughtful*

IAM is where security meets usability. Too strict, and users circumvent controls (shadow IT, password sharing). Too loose, and attackers exploit gaps. The art is finding the balance - enabling business while managing risk.

Modern identity systems are complex distributed systems, spanning on-premises and cloud, human and machine identities, employees and partners and customers. Architecting, implementing, and operating these systems is among the most challenging and important work in security.

What calls to you? The cryptography of FIDO2? The operational complexity of Active Directory security? The policy engineering of ABAC? The future of decentralized identity? Let's explore!