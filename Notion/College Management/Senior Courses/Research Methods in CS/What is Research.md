# RESEARCH METHODS IN COMPUTER SCIENCE — LECTURE 12

*(with a networking/software-engineering lens, precise definitions preserved, plus side notes and practical tips)*

---

## Outline

- What is research?
- Characteristics of research
- Types of research
- Outcomes of research
- Originality in research
- Accepted proposals (what typically passes)
- Assignment (how to do it well)

---

## 1) What is research?

### Canonical definitions (preserved)

- **“The systematic investigation into and study of materials and sources in order to establish facts and reach new conclusions.”**
    
    *— Concise Oxford English Dictionary*
    
- **“Scientific investigation that is performed in order to discover new information or to develop or improve products and technology.”**
    
    *— Academic Press Dictionary of Science & Technology*
    
- **Scientific or scholarly inquiry or investigation and the proper communication of the findings.**
- **The process of searching for, in a broader sense, general answers in any field of study or, in a limited fashion, a solution to one particular problem.**
- **A systematic, controlled, empirical, rigorous, and precise method used to obtain solutions or to discover and interpret new information.**
- **A careful, systematic, patient study and investigation in some field of knowledge, undertaken to establish facts or principles.**
- **A structured inquiry that utilizes acceptable scientific methodology to solve problems, and creates new knowledge that is generally acceptable.**
- **A systematic investigation to find answers to a problem.**

> Side note (translation for engineers):
> 
> 
> Research = define a precise question → design a **method** to test it → gather **evidence** (data) under **control** → analyze → draw **reproducible** conclusions → **communicate** (paper, code, dataset). If someone else repeats your method, they should land “close enough” to your conclusions.
> 

> Practical check: If your “research” is indistinguishable from building a feature without measurement, it isn’t research. Attach a hypothesis, a measurable outcome, and an evaluation plan.
> 

---

## 2) Characteristics of research

### Controlled

- Many interacting parameters → hold all but one constant to isolate effects.
    - Example (networks): studying **TCP congestion control** variant performance → control **RTT, loss rate, queue size, CCA**, background traffic; vary exactly one (e.g., loss rate).
- Use **assumptions** when warranted (e.g., standard atmospheric pressure, i.i.d. packet losses) and **state them explicitly**.
- Employ **external control mechanisms**: testbeds (e.g., Mininet, ns-3), emulators (tc/netem), or calibrated hardware.

> Pitfall: Confounding variables. If you change queue algorithm and traffic mix simultaneously, you can’t attribute the effect.
> 

### Rigorous (strictly accurate)

- Procedures must be **relevant, appropriate, and justified**.
- Choose **correct statistical tests**, ensure **power**, verify **threats to validity** (internal, external, construct, conclusion).

> Practical tip: Pre-register your experimental plan or at least freeze it before peeking at results to avoid p-hacking.
> 

### Systematic

- Follow a **logical sequence**: problem → hypotheses → method → data collection → analysis → interpretation → limitations → conclusions.
- Use **carefully designed plans**; randomness/lack of direction isn’t tolerated.

> Artifact checklist: design doc, variable dictionary, scripts to reproduce, seed control, versioned datasets.
> 

### Valid and verifiable

- Findings must be **correct** and **repeatable/reproducible** (by you and others).
- Provide **artifacts**: code, configs, seeds, notebooks, data schemas, exact environment (container/conda lockfile).

> Rule of thumb: If your repo can’t rebuild the figures from raw data with one command, reproducibility is fragile.
> 

### Empirical

- Conclusions rest on **evidence**: measurements, observations, or real experience—not solely opinion.
- In CS: empirical = **benchmarks**, **ablation studies**, **trace-driven simulation**, **user studies** (HCI), **field data** (telemetry).

### Critical

- Methods must be **well-devised**, minimize error, and avoid known drawbacks.
- Include **sanity checks** (e.g., null experiments), **placebo baselines**, **negative controls**.

### Innovative

- Involves **innovation** (new idea, method, application) + sometimes **luck**.
- Innovation ≠ novelty for novelty’s sake—must **advance understanding or capability**.

### Usually expensive

- Costs: **people**, **equipment**, **compute**, **data**.
- Be conscious of **GERD** (Gross Domestic Expenditure on R&D) vs. **GDP** context; align scope with resources.
- For students: leverage **open datasets**, **cloud credits**, **shared testbeds** (e.g., CloudLab), **simulators**.

---

## 3) Types of research

### Pure research (Basic research)

- Purpose: **expand the knowledge base**; develop/test **theories and hypotheses**.
- Often in **universities**, typically **publicly funded**.
- Impact is **long-term**; high intellectual challenge.

> Example (networks): Mathematical analysis of queueing delay bounds for new congestion models; proving properties of BGP stability.
> 

### Applied research

- Goal: **develop a new product** or **next-generation** of an existing one.
- Common in **industry labs**, also **universities** via collaborations.

> Example (systems): A faster RPC framework with zero-copy I/O; an adaptive bitrate algorithm improving QoE by 10% in real CDNs.
> 

---

### Other common classifications

- **Descriptive research**
    
    *Characterizes* a phenomenon. No causal claims.
    
    - Ex: Internet **traffic classification** across regions; **bug taxonomies** in microservices.
- **Exploratory research** *(feasibility/pilot)*
    
    Tests viability, identifies variables, and shapes hypotheses.
    
    - Ex: Small-scale study of **eBPF** observability overhead before full deployment.
- **Correlational research**
    
    Examines associations between variables; not causal.
    
    - Ex: Correlation between **code churn** and **incident rate**.
- **Explanatory research**
    
    Seeks **causal** explanations.
    
    - Ex: Does **scheduler X** reduce tail latency **because** of shorter critical sections?
- **Quantitative research**
    
    Uses numerical data, statistics, controlled experiments.
    
    - Ex: A/B tests, large-scale benchmarks, simulation results, hypothesis testing.
- **Qualitative research**
    
    Uses interviews, thematic analysis, ethnography; often for HCI, SE processes.
    
    - Ex: Developer interviews on **on-call fatigue**; thematic coding of postmortems.

> Side note: Many strong CS papers are mixed-methods: quantitative measurements + qualitative developer/user insights.
> 

---

## 4) Outcomes of research

- **A new or improved product**
    - In research, the distinction isn’t crucial; what matters is the **contribution** and **evaluation**.
- **A new theory** or **reinterpretation of an existing theory**
    - Graduate-level work often refines or reinterprets.
    - Ex: New model of **latency as a first-class SLO** in distributed systems.
- **A new or improved research tool or technique**
    - Ex: A **packet-capture timestamping** method with nanosecond precision; a novel **microscope** for nanoparticles (analogy).
- **A new or improved model or perspective**
    - Ex: Treating **time as a fourth dimension** in data models; **tail-at-scale** as a design paradigm.
- **An in-depth study**
    - Ex: Longitudinal analysis of **TLS deployment** across the web; deep dive into **Jupiter moons** (analogy: exhaustive analysis using Galileo probe–like datasets → in CS, think **MAWI**, **CAIDA**, **GitHub Archive**).
- **An exploration of a topic area or field**
    - Mapping the landscape, gaps, and challenges (survey).
- **A critical analysis**
    - Ex: Safety/feasibility analysis of **nuclear vs. fossil** (analogy). In CS: **privacy trade-offs** of telemetry in mobile OS.
- **A portfolio of work**
    - Tools + datasets + papers that collectively advance a theme.
- **A fact or conclusion, or a collection of facts or conclusions**
    - Ex: Measured **SYN backlog** exhaustion thresholds under stress across OS versions.

> Evaluation standards: clarity of the research question, novelty, methodological soundness, evidence strength, reproducibility, and significance.
> 

---

## 5) Originality in research

### Where originality appears

- **Tools, techniques, and procedures**
    - Use existing methods in **new/untested ways** or for a specific/new purpose.
    - Ex: Repurposing **RL bandits** for **congestion window** tuning.
- **Exploring the unknown**
    - Investigating something **never studied** (new workload, new protocol behavior, new dataset).
    - Ex: First measurement study of **QUIC** behavior on satellite links.
- **Exploring the unanticipated (sidetracks)**
    - Serendipitous findings can open **alternative pathways**—but beware dead ends; keep aligned to the main plan.
- **Use of data**
    - Data may reveal **side-products** or **unseen benefits**.
    - Ex: A dataset collected for performance reveals **security anomalies** worth a separate paper.
- **Outcomes crossing disciplines**
    - Not new in one field, but **novel and impactful** in another.
    - Ex: **Aspirin analogy** → In CS: a standard **streaming join** technique enabling **real-time fraud detection** in fintech.
- **Originality in byproducts**
    - Benchmarks, frameworks, or datasets that others adopt.
- **Originality in experience**
    - New **methodological lessons** (e.g., how to run unbiased **latency** experiments on shared clouds).
- **Originality as “potentially publishable”**
    - Publication in a **peer-reviewed (refereed) journal or top conference** is practical evidence of originality.

### Facts, ideas, and originality

- **(1) New facts + New ideas** → original.
- **(2) New facts + Old ideas** → original (better measurement advances the field).
- **(3) Old facts + New ideas** → original (novel theory/model/explanation).
- **(4) Old facts + Old ideas** → **not** original.

> Balance between originality and conformity
> 
> 
> Push boundaries, but anchor in the **literature** and **advisor** guidance. Respect **ethics** and **copyright**; ensure **ownership** and **proper attribution**.
> 

---

## 6) Key trends in research

- **Interdisciplinarity**
    - Single-author pieces are increasingly rare; **team science** across networking, systems, ML, HCI, security is common.
- **International collaboration**
    - Diverse environments/datasets increase **external validity** and impact.

> Actionable tip: Build collaborations early; share intermediate artifacts to catalyze joint work.
> 

---

## 7) Research topics in Computer Science (illustrative, aligned with the slide list)

- **Data Mining / Big Data** — scalable pattern discovery, data aggregation, drift detection.
- **Machine Learning** — robust training, interpretability, resource-aware inference.
- **Digital Image Processing / Computer Vision** — low-light enhancement, edge deployment.
- **Internet of Things (IoT)** — secure onboarding, energy-aware protocols, federated analytics.
- **Artificial Intelligence** — planning under uncertainty, alignments with safety constraints.
- **Networking** — congestion control, programmable data planes (P4), QUIC/HTTP/3, 5G/6G, L4S.
- **Cloud Computing** — multi-tenant interference, serverless scheduling, cost/performance SLOs.
- **Software Engineering** — CI/CD optimization, flakiness reduction, static/dynamic analysis at scale.

> Choosing a topic: intersect your skills, available data/testbeds, and clear, evaluable questions.
> 

---

## 8) Accepted proposals — what typically passes (from the slide list, expanded)

- **Continuation of Previous Studies**
    - Solid if you identify **gaps/limitations** and propose **meaningful extensions**.
- **Enhance Methodology to Enhance Accuracy**
    - Strong when you justify **why** accuracy improves and **how** you’ll validate (not just “more layers”).
- **Change Approach**
    - New algorithm/model/theory; compare **head-to-head** with prior SOTA on **the same datasets/benchmarks**.
- **Add Parameters** (e.g., 3 → 10)
    - Only meaningful if parameters are **well-motivated**, avoid overfitting, and you provide **ablation**.
- **Change Dataset** (A → B)
    - Good for testing **generalization** and **external validity**; document dataset **biases**.
- **Something New**
    - Clearly articulate **novelty**, **evaluation plan**, and **risk mitigation**.

> Reviewer’s mental checklist: Is the problem important? Is the idea new? Is the method sound? Are results convincing? Are artifacts available?
> 

---

## 9) Assignment — How to execute at a high standard

**Task (preserved):**

- Read **10 research articles** for **each** chosen topic (specific, closely related).
- Sources: **Scopus-indexed, ACM, IEEE** (e.g., via your library portal).
- Write a **summary report** for each article including:
    - **Title**
    - **Author(s)**
    - **Objective**
    - **Scope**
    - **Methodology**
    - **Dataset**
    - **Conceptual Framework** *(include algorithm)*
    - **Result**
    - **Further Research**

### How to do this efficiently (step-by-step)

1. **Scope & keywords**
    - Define 3–5 tight keywords per topic (e.g., *“QUIC congestion control evaluation,” “L4S ECN deployment,” “AQM CoDel BBR interaction”*).
2. **Filtering**
    - Target last **5 years** for applied work; include **seminal** older papers for theory.
    - Skim **abstract → figures → method → results**; reject off-scope quickly.
3. **Extraction template** *(use consistently)*
    - Copy the 9 fields above; add **Assumptions**, **Threats to Validity**, **Baselines**, **Metrics**, **Artifacts** (code/data?).
4. **Synthesis** (after the 10 papers)
    - Build a **comparison table** (methods, datasets, metrics, best/worst cases).
    - Identify **consensus**, **disagreements**, and **gaps** → this seeds your proposal.
5. **Reproducibility notes**
    - Record environment details; if code is available, note **commit/tag**, **config**, **seed**.
6. **Ethics & citations**
    - Track BibTeX early; avoid missing references. Follow venue **citation style**.

### Example (mini entry, condensed)

- **Title:** “BBR vs. CUBIC over L4S-enabled Paths”
- **Authors:** X et al.
- **Objective:** Compare throughput/latency trade-offs of BBR and CUBIC under L4S.
- **Scope:** WAN emulation; 10–100 ms RTT; 0–1% loss; AQM: DualQ.
- **Methodology:** ns-3 + tc; factorial design varying {RTT, loss, queue}.
- **Dataset:** Synthetic traffic traces + public CAIDA traces.
- **Conceptual Framework/Algorithm:** Dual-queue ECN marking, BBR probing phases.
- **Result:** L4S reduces median latency 25–40% for CUBIC; BBR gains limited unless pacing adjusted.
- **Further Research:** Real-world L4S deployments, cross-traffic fairness, Wi-Fi edge effects.

---

## 10) Practical mini–toolbox

- **Design of Experiments (DoE):** define factors, levels, randomization, replication.
- **Metrics:** mean/median, **tail** (p95/p99), **Jain’s fairness**, **AUC**, **TTFB**, **SLO hit rate**.
- **Stats:** t-test/Mann-Whitney (2 groups), ANOVA/Kruskal-Wallis (≥3), **Cliff’s delta**, **bootstrap CIs**.
- **Validity threats:**
    - *Internal* (confounders), *External* (generalization), *Construct* (metric matches concept), *Conclusion* (statistical errors).
- **Artifact packaging:** Dockerfile/Conda env, `Makefile`/`justfile`, seed control, data schema, README with “reproduce the paper” script.

---

## 11) Quick “am I doing research?” checklist

- Clear **research question** and **hypotheses**
- **Controlled** variables and documented **assumptions**
- **Systematic** plan (frozen pre-analysis)
- **Empirical** evidence + correct **statistics**
- **Reproducible** artifacts (code/data/seed)
- **Critical** analysis (limitations, ablations)
- **Original** contribution (fits 1–3 in facts/ideas matrix)
- **Proper communication** (paper-quality writing, figures, and citations)

---

### Final advice

Start small, instrument obsessively, and write the *Results* section you wish you already had—then go collect exactly that evidence. That’s how solid CS (and networking) research gets done.

If you want, tell me your initial topic (e.g., “QUIC over Wi-Fi” or “CI/CD flakiness in microservices”), and I’ll sketch a concrete research question, experiment plan, metrics, and a reading list of 10 must-read papers tailored to it.