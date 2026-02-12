I'll create comprehensive, in-depth lecture notes on Chapter 6: Intellectual Property. Let me search for current developments to supplement the textbook content.
Now I have comprehensive current information on intellectual property. Let me create detailed, intuitive lecture notes on Chapter 6: Intellectual Property.

---

# **CHAPTER 6: INTELLECTUAL PROPERTY**
## **Ethics in Information Technology — Comprehensive Lecture Notes**

---

## **I. OPENING SCENARIO: THE MODERN IP LANDSCAPE**

> *"Intellectual property is the oil of the 21st century."* — Mark Getty

**The Digital Dilemma:** In the physical world, if I steal your car, you no longer have it. In the digital world, if I "steal" your software, you still have it—but I have it too. This fundamental difference makes intellectual property one of the most complex and contested areas of IT ethics.

**2024 Flashpoint:** The AI revolution has created unprecedented IP questions:
- Can AI-generated content be copyrighted? (No—U.S. Copyright Office, 2024)
- Is training AI on copyrighted works fair use or infringement? (Lawsuits pending)
- Who owns AI-assisted creations? (Human with sufficient creative control)

---

## **II. WHAT IS INTELLECTUAL PROPERTY?**

### **A. Definition and Core Concept**

**Intellectual Property (IP):** Creations of the mind—inventions, literary and artistic works, symbols, names, images, and designs used in commerce.

**The Social Contract of IP:**
```
SOCIETY grants TEMPORARY MONOPOLY → CREATOR discloses/innovates → PUBLIC benefits eventually
         (exclusive rights)            (patent publication,         (works enter public domain,
                                       copyright registration)        knowledge spreads)
```

**Why Protect IP?**

| Rationale | Explanation | Criticism |
|-----------|-------------|-----------|
| **Incentive Theory** | Creators need rewards to invest time/resources | Many create for non-monetary reasons; may stifle follow-on innovation |
| **Natural Rights** | Creators have moral right to fruits of labor | Ideas are non-rivalrous; "property" metaphor is strained |
| **Disclosure Theory** | Patents require public disclosure | Trade secrets bypass this; patent thickets block research |
| **Personality Theory** | Creations express creator's identity | Difficult to apply to corporate works, AI-generated content |

---

## **III. THE FOUR PILLARS OF INTELLECTUAL PROPERTY**

```
                    INTELLECTUAL PROPERTY
                           |
        ___________________|___________________
       |         |          |          |
   COPYRIGHT   PATENT    TRADE      TRADEMARK
               SECRET
       |         |          |          |
   Creative    Inventions  Confidential  Brand
   works       (novel,     business      identity
               useful,     information
               non-obvious)
```

---

## **IV. COPYRIGHT: PROTECTING CREATIVE EXPRESSION**

### **A. What Copyright Protects**

**Protected Works:**
- Literary works (software code = literary work)
- Musical works and sound recordings
- Dramatic works
- Pictorial, graphic, and sculptural works
- Motion pictures and audiovisual works
- Architectural works

**Not Protected:**
- Ideas, facts, systems, methods of operation
- Titles, names, short phrases, slogans (trademark territory)
- Works by U.S. government employees
- **2024 addition:** Works generated solely by AI (no human authorship)

### **B. The Requirements for Copyright**

| Requirement | Explanation | Example |
|-------------|-------------|---------|
| **Originality** | Independently created, minimal creativity | Your grocery list is original; copying someone else's is not |
| **Fixation** | Embodied in tangible medium | Improvised jazz = not fixed; recorded jazz = fixed |
| **Human Authorship** | Created by human being | AI-generated image = no copyright (2024) |

**The Human Authorship Requirement (2024 Update):**

U.S. Copyright Office Guidance:
- **Pure AI output:** Not copyrightable
- **AI-assisted with human creative control:** Potentially copyrightable
- **Human modification of AI output:** Copyrightable for human contributions

**Case Study:** *Thaler v. Perlmutter* (2023) — Court affirmed that purely AI-generated work cannot be copyrighted.

### **C. Copyright Rights and Duration**

**Exclusive Rights (§106):**
1. **Reproduction** — make copies
2. **Derivative works** — adaptations, translations
3. **Distribution** — sell, rent, lease
4. **Public performance** — plays, music, movies
5. **Public display** — art, photos, software GUIs
6. **Digital transmission** — sound recordings (Digital Performance Right in Sound Recordings Act)

**Duration:**

| Work Type | Duration |
|-----------|----------|
| Individual author | Life + 70 years |
| Corporate/work for hire | 95 years from publication or 120 from creation |
| Pre-1978 works | Complex; many now public domain |

**The Public Domain:** Works free for all to use
- 2024: Works published in 1928 entered public domain (including early Mickey Mouse)
- **Orphan works:** Copyrighted but owner unknown/unlocatable

### **D. Software Copyright Special Issues**

#### **1. The Idea-Expression Dichotomy**

**Fundamental Principle:** Copyright protects expression, not ideas.

**Software Application:**
```
IDEA (unprotected)                    EXPRESSION (protected)
"Sort a list of names"                Specific code implementing quicksort algorithm
"Search a database"                   Unique SQL query structure and optimization
"Display a window"                    Particular API calls and rendering code
```

**The Merger Doctrine:** When idea and expression merge (only one or few ways to express idea), expression is not protected.

#### **2. Copyright Infringement in Software**

**Literal Copying:** Copying actual source or object code
**Non-Literal Copying:** Copying structure, sequence, organization (SSO)

**The Abstraction-Filtration-Comparison Test (*Computer Associates v. Altai*, 1992):**

```
Step 1: ABSTRACTION
        Break down program into constituent parts
        (main program → modules → routines → instructions)

Step 2: FILTRATION
        Remove unprotected elements:
        - Ideas
        - Elements dictated by efficiency (merger)
        - Elements dictated by external factors (compatibility requirements)
        - Elements taken from public domain

Step 3: COMPARISON
        Compare remaining protected expression
        for substantial similarity
```

### **E. Fair Use: The Safety Valve**

**Purpose:** Allow limited use without permission for socially beneficial purposes

**Four Factors (§107):**

| Factor | Favors Fair Use | Against Fair Use |
|--------|---------------|------------------|
| **1. Purpose & character** | Transformative, nonprofit, educational | Commercial, merely reproductive |
| **2. Nature of work** | Factual, published | Highly creative, unpublished |
| **3. Amount used** | Small portion, not "heart" | Substantial portion or entire work |
| **4. Market effect** | No effect on potential market | Supersedes demand for original |

**Transformative Use (Key 2024 Issue):** Does new work add new meaning, message, or purpose?

**AI Training and Fair Use (Pending):**

| Argument for Fair Use | Argument Against |
|----------------------|------------------|
| Training is "learning," not copying | Commercial use of entire works |
| Output is transformative | Outputs compete with original works |
| Necessary for innovation | Licenses available but ignored |
| Similar to human learning | Massive scale exceeds any educational use |

**2024 Litigation:** *Authors Guild v. OpenAI*, *Andersen v. Stability AI* — courts will determine if AI training is fair use.

---

## **V. PATENTS: PROTECTING INVENTIONS**

### **A. What Patents Protect**

**Requirements (35 U.S.C. §101-103):**

| Requirement | Standard | Example |
|-------------|----------|---------|
| **Patentable Subject Matter** | Process, machine, manufacture, composition of matter | Software (sometimes), hardware, biotech |
| **Novelty** | Not previously known or used | First to invent the wheel (in modern era) |
| **Non-obviousness** | Not obvious to person having ordinary skill in the art (PHOSITA) | Combining two known inventions in predictable way = obvious |
| **Utility** | Specific, substantial, credible utility | Perpetual motion machine = not credible |

### **B. Software Patents: The Controversy**

**The Evolution:**

| Era | Approach | Key Case |
|-----|----------|----------|
| Pre-1981 | Software = unpatentable abstract idea | *Gottschalk v. Benson* (1972) |
| 1981-1998 | Software + hardware = patentable | *Diamond v. Diehr* (1981) |
| 1998-2014 | Business methods + software broadly patentable | *State Street Bank* (1998) |
| 2014-present | Abstract ideas require "something more" | *Alice Corp. v. CLS Bank* (2014) |

**The Alice Test (Current Law):**

```
Step 1: Is the claim directed to a patent-ineligible concept?
        (law of nature, natural phenomenon, abstract idea)
        
        If NO → Patentable
        If YES → Step 2

Step 2: Does the claim contain an "inventive concept"?
        (element or combination of elements sufficient to ensure
        that the patent amounts to significantly more than the
        abstract idea itself)
        
        If YES → Patentable
        If NO → Not patentable
```

**2024 Update:** *Emotional Perception AI v. Comptroller-General* (UK) — AI using neural network for music recommendations held unpatentable as "computer program as such"; appeal to UK Supreme Court pending.

### **C. Patent Rights and Duration**

**Duration:** 20 years from filing date (utility patents)

**Exclusive Rights:**
- Make, use, sell, offer to sell, import

**Disclosure Requirement:** Must enable PHOSITA to practice invention

### **D. Patent Infringement and Remedies**

**Types of Infringement:**
- **Direct:** Making, using, selling patented invention
- **Indirect:** Inducing or contributing to infringement
- **Literal:** All claim elements present
- **Doctrine of Equivalents:** Substantially same function, way, result

**Remedies:**
- Injunctions (stop infringement)
- Damages (reasonable royalty or lost profits)
- **2024:** Enhanced damages for willful infringement

---

## **VI. TRADE SECRETS: PROTECTING CONFIDENTIAL INFORMATION**

### **A. What Are Trade Secrets?**

**Definition (Uniform Trade Secrets Act):** Information that:
1. Derives independent economic value from not being generally known
2. Is subject to reasonable efforts to maintain secrecy

**Examples:**
| Category | Examples |
|----------|----------|
| Technical | Source code, algorithms, formulas, designs |
| Business | Customer lists, pricing strategies, marketing plans |
| Negative knowledge | Failed experiments (know what doesn't work) |

### **B. Trade Secrets vs. Patents**

| Factor | Trade Secret | Patent |
|--------|--------------|--------|
| **Protection** | Unlimited (while secret) | 20 years |
| **Cost** | Low (security measures) | High (filing, prosecution, maintenance) |
| **Public disclosure** | None required | Full disclosure required |
| **Reverse engineering** | Allowed | Not relevant (patent is public) |
| **Independent invention** | No protection | Protected |
| **Enforcement** | State law, criminal (EEA) | Federal law |

### **C. Trade Secret Misappropriation**

**The Economic Espionage Act (1996, amended 2016):**

**Criminal penalties:**
- Individual: Up to 10 years imprisonment, $5 million fine
- Organization: Up to $10 million fine
- **Theft for foreign government:** Up to 15 years, $5 million

**Defend Trade Secrets Act (2016):** Created federal civil cause of action

**Common Misappropriation Scenarios:**

```
SCENARIO 1: Employee Theft
Employee downloads customer list before leaving for competitor
→ Criminal and civil liability

SCENARIO 2: Corporate Espionage
Competitor hacks into company database
→ Economic espionage (if foreign benefit) or trade secret theft

SCENARIO 3: Accidental Disclosure
Employee emails confidential design to wrong recipient
→ Not misappropriation unless recipient knows and uses

SCENARIO 4: Reverse Engineering
Competitor buys product, disassembles, discovers secret
→ Generally legal (unless contract prohibits)
```

### **D. Protecting Trade Secrets: Best Practices**

| Measure | Implementation |
|---------|---------------|
| **Access control** | Need-to-know basis, password protection |
| **Physical security** | Locked facilities, visitor logs |
| **Employee agreements** | NDAs, non-competes (state law varies) |
| **Exit procedures** | Exit interviews, return of materials, reminder of obligations |
| **Vendor controls** | Contracts requiring confidentiality |
| **Documentation** | Marking confidential, audit trails |

---

## **VII. TRADEMARKS: PROTECTING BRAND IDENTITY**

### **A. What Trademarks Protect**

**Types of Marks:**

| Type | Protects | Example |
|------|----------|---------|
| **Trademark** | Goods | Nike swoosh on shoes |
| **Service mark** | Services | McDonald's golden arches for restaurant services |
| **Certification mark** | Compliance with standards | UL (Underwriters Laboratories) |
| **Collective mark** | Membership in organization | Realtor® |

**Requirements for Protection:**
1. **Distinctive** — capable of identifying source
2. **Used in commerce** — actual use in selling goods/services
3. **Not confusingly similar** to existing marks

**Spectrum of Distinctiveness:**

```
WEAK ←————————————————————————————————→ STRONG
Generic   Descriptive   Suggestive   Arbitrary   Fanciful
  |           |            |            |           |
"Computer" "International" "Netflix"   "Apple"    "Exxon"
(for        (for          (suggests    (arbitrary  (invented
computers)  shipping)     internet +   word for    word)
                          flicks)      computers)
```

### **B. Trademark Infringement and Dilution**

**Likelihood of Confusion Test (Infringement):**

| Factor | Consideration |
|--------|---------------|
| Strength of plaintiff's mark | Famous marks get broader protection |
| Similarity of marks | Visual, phonetic, conceptual similarity |
| Similarity of goods/services | Related markets? Same channels? |
| Actual confusion | Evidence of real confusion |
| Defendant's intent | Deliberate copying suggests likelihood |
| Marketing channels | Same stores, same advertising? |
| Consumer sophistication | Sophisticated buyers less likely confused |

**Trademark Dilution (Famous Marks Only):**

| Type | Definition | Example |
|------|------------|---------|
| **Blurring** | Association impairing distinctiveness | "Kodak" for bicycles |
| **Tarnishment** | Association harming reputation | "Tiffany" for escort service |

### **C. Domain Names and Cybersquatting**

**The Problem:** Registering domain names identical or confusingly similar to trademarks, typically to sell to trademark owner or exploit confusion.

**Anticybersquatting Consumer Protection Act (ACPA, 1999):**

**Bad faith factors:**
- Prior use of domain name for bona fide offering of goods/services
- Legal or commonly known name of person
- Noncommercial or fair use
- **Intent to divert consumers for commercial gain**
- **Pattern of registering marks**
- **History of selling domains without bona fide use**

**UDRP (Uniform Domain-Name Dispute-Resolution Policy):**
- Faster, cheaper than court
- Requires: (1) identical/confusingly similar, (2) no legitimate interest, (3) bad faith

**2024 Development:** *Lifestyle Equities v. Amazon UK* — UK Supreme Court held Amazon's US website targeting UK consumers infringed UK trademarks; mere accessibility insufficient, but active targeting creates liability.

---

## **VIII. CURRENT INTELLECTUAL PROPERTY ISSUES**

### **A. Plagiarism**

**Definition:** Presenting another's work or ideas as your own, without attribution.

**Plagiarism vs. Copyright Infringement:**

| Aspect | Plagiarism | Copyright Infringement |
|--------|-----------|----------------------|
| **Nature** | Ethical violation | Legal violation |
| **Standard** | Academic/professional norms | Substantial similarity + access |
| **Protected material** | Ideas, facts (not just expression) | Expression only |
| **Remedy** | Academic discipline, reputational harm | Damages, injunctions |

**Types of Plagiarism:**
- **Direct:** Copying verbatim without quotation
- **Mosaic:** Mixing copied phrases with own words
- **Paraphrase:** Rewording without attribution
- **Self-plagiarism:** Reusing own work without disclosure

**Detection:** Turnitin, Grammarly, manual checking

### **B. Reverse Engineering**

**Definition:** Starting with known product and working backward to discover trade secrets.

**Legal Status:**

| Jurisdiction | Generally Legal? | Conditions |
|--------------|----------------|------------|
| **USA** | Yes | Lawfully acquired product; contract doesn't prohibit |
| **EU** | Yes | Similar conditions |
| **Some jurisdictions** | Restricted | May require authorization |

**Software Reverse Engineering:**
- **Clean room technique:** One team disassembles, documents; separate team implements from documentation
- **Decompilation:** May be permitted for interoperability (EU Software Directive)

### **C. Open Source Software**

**The Open Source Model:**

```
TRADITIONAL SOFTWARE              OPEN SOURCE SOFTWARE
     |                                    |
  Proprietary                        Collaborative
  Secret source code                 Public source code
  License fees                       Free (as in speech and beer)
  Vendor control                     Community governance
```

**Common Open Source Licenses:**

| License | Key Features | Example Projects |
|---------|------------|----------------|
| **MIT** | Permissive, minimal restrictions | React, Node.js |
| **Apache 2.0** | Permissive, patent grant | Android, Kubernetes |
| **BSD** | Permissive, no copyleft | FreeBSD, macOS (partially) |
| **GPL (v2/v3)** | Copyleft, derivative works must be GPL | Linux, GNU |
| **LGPL** | Weak copyleft, libraries can link to proprietary | GTK |
| **Mozilla (MPL)** | File-level copyleft | Firefox, Thunderbird |

**Copyleft Explained:**
```
PROPAGATION OF COPYLEFT OBLIGATIONS

GPL Code ──► Modified GPL Code ──► Distributed ──► Must release source
     |              |
     └──────────────┘
   (Derivatives inherit GPL)
```

**2024 Trend:** Increased scrutiny of open source compliance in M&A; SBOMs (Software Bill of Materials) required by Executive Order 14028 for government contractors.

### **D. Competitive Intelligence**

**Definition:** Ethical gathering of publicly available information about competitors.

**Legal Methods:**
- Public filings and reports
- Patent searches
- Trade show observation
- Reverse engineering (lawful acquisition)
- Customer interviews (non-confidential information)

**Illegal Methods (Industrial Espionage):**
- Hacking
- Bribery of employees
- Theft of documents
- Wiretapping
- **2024:** Using AI to scrape proprietary data at scale

### **E. AI and Intellectual Property (2024 Critical Issue)**

#### **1. AI-Generated Content**

**U.S. Copyright Office Position (2024):**
- Pure AI output: **Not copyrightable**
- Human creative control + AI assistance: **Potentially copyrightable**
- Human modifications to AI output: **Copyrightable for human contributions**

**International Variation:**

| Jurisdiction | AI Copyright Approach |
|--------------|----------------------|
| **USA** | Human authorship required |
| **UK** | "Computer-generated" works protected (50 years) |
| **EU** | Human originality required |
| **China** | AI-generated works may be protected if human involvement |

#### **2. AI Training and Copyright**

**The Core Conflict:**

```
AI COMPANY VIEW                    CREATOR VIEW
     |                                  |
Training is "learning"               Training is "copying"
Fair use: transformative             No transformation; commercial use
Necessary for innovation             Licenses available but ignored
Similar to human learning            Massive scale exceeds any educational use
```

**2024 Litigation Status:**

| Case | Plaintiffs | Defendants | Status |
|------|-----------|------------|--------|
| *Andersen v. Stability AI* | Visual artists | Stability AI, Midjourney, DeviantArt | Class action certified; fair use not decided |
| *Authors Guild v. OpenAI* | Authors (G.R.R. Martin, etc.) | OpenAI, Microsoft | Class action allowed to proceed |
| *NYT v. OpenAI* | New York Times | OpenAI, Microsoft | Discovery ongoing |

**Legislative Responses (2024):**
- **Generative AI Copyright Disclosure Act:** Requires disclosure of training datasets
- **TRAIN Act:** Subpoena power for copyright owners to identify if works used in training
- **NO AI FRAUD Act:** Protects against unauthorized digital replicas

#### **3. Patenting AI Inventions**

**The Inventorship Problem:**
- Can AI be named as inventor? **No** (*Thaler v. Vidal*, 2022; affirmed internationally)
- Must human contribution be acknowledged? **Yes**

**2024 UK Case:** *Emotional Perception AI* — AI using neural network for music recommendations held unpatentable as "computer program as such"; technical contribution required beyond mere computer implementation.

---

## **IX. INTERNATIONAL INTELLECTUAL PROPERTY**

### **A. Major International Agreements**

| Agreement | Purpose | Key Features |
|-----------|---------|--------------|
| **Paris Convention (1883)** | Trademark and patent protection | National treatment, priority rights |
| **Berne Convention (1886)** | Copyright protection | Automatic protection, no formalities |
| **TRIPS (1994)** | Minimum IP standards | Enforcement mechanisms, dispute resolution |
| **PCT (Patent Cooperation Treaty)** | Streamlined patent filing | Single application, multiple countries |
| **Madrid Protocol** | International trademark registration | Single application, multiple jurisdictions |

### **B. Regional Systems**

| System | Coverage | Feature |
|--------|----------|---------|
| **European Patent Office (EPO)** | 39 European countries | Single examination, national validation |
| **Unified Patent Court (UPC)** | EU countries (2023 launch) | Single court for patent disputes |
| **EUIPO** | European Union | EU trademarks and designs |

---

## **X. ETHICAL FRAMEWORK FOR INTELLECTUAL PROPERTY**

### **A. Balancing Competing Interests**

```
CREATOR RIGHTS ←————————————————————→ PUBLIC ACCESS
      |                                    |
   Incentives                          Follow-on innovation
   Fair compensation                    Education and research
   Moral rights                         Cultural development
   Control over work                    Competition and choice
```

### **B. Ethical Decision Framework**

| Question | Consideration |
|----------|---------------|
| **Is this creation original?** | Building on others vs. copying |
| **Am I respecting licenses?** | Open source obligations, terms of use |
| **Is this fair use or infringement?** | Transformative purpose, market effect |
| **Am I protecting confidentiality?** | Trade secret obligations |
| **Is this attribution adequate?** | Giving credit, avoiding plagiarism |
| **Does this promote innovation?** | Net effect on creative ecosystem |

---

## **XI. MANAGER'S CHECKLIST: INTELLECTUAL PROPERTY PROTECTION**

| Question | Yes | No | Action if No |
|----------|-----|-----|-------------|
| Do we have IP inventory (patents, trademarks, trade secrets)? | | | Conduct audit |
| Are employee IP agreements in place? | | | Implement standard agreements |
| Do we respect third-party IP (licenses, no unauthorized use)? | | | Training and compliance program |
| Are trade secrets adequately protected? | | | Implement security measures |
| Do we have patent strategy aligned with business goals? | | | Develop prosecution and maintenance plan |
| Are trademarks registered and monitored? | | | File applications, set up watching service |
| Do we have open source compliance program? | | | Implement SBOM and license review |
| Are we prepared for IP litigation? | | | Develop enforcement and defense strategy |
| Do we have AI IP policy? | | | Address ownership, training data, output rights |

---

## **XII. CHAPTER SUMMARY**

### **Key Takeaways:**

1. **Intellectual property protects creations of the mind through four main mechanisms:** copyright (expression), patents (inventions), trade secrets (confidential information), and trademarks (brand identity).

2. **Software presents unique IP challenges** at the intersection of copyright (code as literary work) and patents (algorithms and methods), with the idea-expression dichotomy being critical.

3. **The AI revolution is disrupting IP fundamentals** — from copyrightability of AI outputs to fair use of training data to patentability of AI-assisted inventions.

4. **Trade secrets offer powerful protection** for proprietary information but require rigorous security measures and are vulnerable to reverse engineering.

5. **Open source software creates both opportunities and obligations** — understanding license terms is essential for compliance.

6. **International IP protection requires strategic planning** using treaties and regional systems to secure rights globally.

7. **Ethical IP management balances creator rights with public access**, promoting innovation while respecting the creative ecosystem.

---

## **XIII. KEY TERMS**

| Term                               | Definition                                                 |
| ---------------------------------- | ---------------------------------------------------------- |
| **Intellectual Property (IP)**     | Creations of the mind protected by law                     |
| **Copyright**                      | Protection for original works of authorship                |
| **Patent**                         | Exclusive right to invention for limited time              |
| **Trade Secret**                   | Confidential business information                          |
| **Trademark**                      | Source-identifying mark for goods/services                 |
| **Fair Use**                       | Limited use without permission for transformative purposes |
| **Plagiarism**                     | Presenting another's work as your own                      |
| **Reverse Engineering**            | Working backward from product to discover secrets          |
| **Open Source**                    | Software with publicly available source code               |
| **Copyleft**                       | License requiring derivatives to be open source            |
| **Cybersquatting**                 | Bad faith registration of domain names                     |
| **Infringement**                   | Violation of exclusive IP rights                           |
| **Public Domain**                  | Works free for all to use                                  |
| **Work for Hire**                  | Employer owns employee's creative work                     |
| **Non-Disclosure Agreement (NDA)** | Contract protecting confidential information               |

---

