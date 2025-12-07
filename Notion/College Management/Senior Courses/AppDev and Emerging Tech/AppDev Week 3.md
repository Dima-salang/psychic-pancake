## 🛒 Lecture 6: System Acquisition Strategies and Detailed SDLC Phases

Good day, class. Today's lecture is highly practical, focusing on the critical **Acquisition Strategy**—the fundamental choice every organization faces: Should we **subscribe, buy, or build** our software? We will then review the initial phases of the **SDLC** with greater operational detail.

---

### 1. System Acquisition Strategies: Subscribe vs. Buy vs. Build

The choice of acquiring software is a strategic one, balancing **cost, control, customization, and time-to-market**.

#### A. Strategy 1: Subscribe (Software as a Service - SaaS)

This approach involves renting access to software hosted by a vendor (e.g., Salesforce, Microsoft 365).

| Characteristic | Pros (Advantages) | Cons (Disadvantages) |
| :--- | :--- | :--- |
| **Cost** | **Software can be a more cost-effective solution for small projects and a good feed for temporary needs.** **Lower implementation costs.** | **An organization incurs recurring licensing costs.** |
| **Maintenance** | **Software updates and upgrades are completed by the vendor after being tested for consistency.** **Technical support is typically available 24/7.** | Must meet the needs that **requires stable internet** and **security with privacy concerns.** |
| **Customization** | **Disadvantage on-demand software is usually offered as ease and cannot be modified to match the organization's needs.** | Lack of customization means the organization may have to change its processes to fit the software. |
| **Speed** | **Many advantages includes how quickly the company can benefit from the software.** | N/A |

#### B. Strategy 2: Buy (Off-the-Shelf Package)

This involves purchasing a packaged software license and installing it on premises (or on a dedicated cloud instance).

* **Pros:** **A software solution can be acquired and deployed relatively quickly.** An organization can **test drive software before acquiring it.**
* **Cons:** **Unmodified software may not be a good match to an organization's needs.** **Maintenance and support cost can become excessive** (patching, upgrades, and internal support).

#### C. Strategy 3: Build (In-House Development)

This is the process of developing **customized software** as discussed in our previous lectures.

* **Pros:** **It is a customized software which is more likely to be a good match to an organization's needs.** A custom application **provides the potential to achieve competitive advantage** (solving a problem no off-the-shelf software can).
* **Cons:** **The cost wherein it is quite high compared to the cost of purchasing off the shelf software.** **Customized software can take months or even years to deploy.**

---

### 2. Software Package Implementation Process

When opting to **buy off the shelf software**, the traditional Waterfall SDLC is altered. This approach **eliminates several of the phases of the waterfall approach** by substituting the lengthy design and construction phases with a structured **package evaluation**.

The process typically involves: **Investigation, systems analysis, package evaluation, integration and testing, and implementation.**

#### Package Evaluation Steps

This becomes the critical phase for purchased solutions:
1.  **Identify potential solutions.**
2.  **Select top contenders.**
3.  **Research top contenders.**
4.  **Perform final evaluation of leading solutions.**
5.  **Make selection.**
6.  **Finalize contract.**

---

### 3. Detailed Waterfall System Development Process

The **waterfall system development process is a sequential multi-stage system process.** The key rule is: **the next stage cannot begin until results of current stage are reviewed and approved or modified.**

The six phases are: **investigation, analysis, design, construction, integration and testing, implementation.**

As previously discussed, **the problem with waterfall system development process is that you cannot go back to another phase or to the previous phase.** For this reason, **it is recommended to use the modified waterfall system rather than the sequential all the traditional waterfall system development** to allow for minor iterations between adjacent phases.

#### A. Phase 1: System Investigation

The purpose is fundamental: **to gain a clear understanding of the specifics of the problem to solve or the opportunity to address.**

* **Key Activities:** **Reviewing of systems investigation requests**, **identifying and recruiting team leader and members**, **developing budget and schedule for investigation**, and **perform investigation.**
* **Tools:** **Joint application development (JAD)** sessions and **functional decomposition** are included in the investigation. A **functional decomposition chart** breaks down the system into its core functions (e.g., a **stock management system** decomposes into **manage stock** and **manage of suppliers**). 
* **Feasibility:** Perform **preliminary feasibility analyses** that focus on **technical, economic, legal, operational, and schedule** factors.
* **Outcome:** The investigation report recommends whether to **redefine the project and redo investigation, you continue or drop the project.**

#### B. Phase 2: Systems Analysis

The overall **emphasis of systems analysis is on wherein gathering of data on the existing system is included, determining requirements for the new system, considering alternatives within identified constraints and investigating visibility of alternative solutions.**

* **Core Activities:** **Study existing system and develop prioritized set of requirements.** This covers **processes, data flow diagram, databases, security and control and system performance.**
* **Modeling:** This phase involves creating conceptual models of the data, primarily using the **Entity Relationship Diagram (ERD)**. An example of an ERD for a customer order database shows entities like **salesperson, customer, orders, line items, and product** and how they relate. 
* **Decision:** This phase concludes by **identifying and evaluate alternative solutions** (including build, buy, or subscribe) and preparing the analysis report for the steering committee review.

#### C. Phase 3: System Design

The **system design creates a complete set of specifications used to construct information systems.**

* **Key Design Outputs:** Design user interface, system security and controls, **disaster recovery plan**, and database design.
* **Feasibility Check:** It involves performing a final, detailed **feasibility analyses** on the chosen solution.

#### D. Phase 4: Construction

**Construction system converts the system design into an operational system.**

* **Steps:** **Code software components**, **creation and loading of data**, and **performing unit tests.**

The remaining phases, **Integration and Testing** (covered in Lecture 5 as implementation phase 1) and **Implementation** (conversion, covered in Lecture 5 as implementation phase 2), complete the cycle.

---

Our next lecture will discuss how modern **Architectural Patterns** (like Microservices) are utilized to build these complex systems, aligning with the principles of agility and scalability.

Would you like to proceed with a lecture on **Application Architectural Patterns**?


## 🛠️ Lecture 7: Integration, Testing, Implementation, and Modern System Development

Welcome back, class. We are concluding our in-depth study of the Systems Development Life Cycle (SDLC) by focusing on the crucial final phases: **Integration and Testing**, and **Implementation and Maintenance**. We will also briefly revisit **Agile** and the critical role of **DevOps** in modern system delivery.

---

### 1. Integration and Testing

Once system components are built, they must be rigorously tested to ensure quality, functionality, and performance.

#### A. Types of Testing

Testing is a multi-layered process, each layer serving a specific purpose:

* **Integration Testing:** This is **the linking of individual components together and testing them as a group to uncover any defects.** The focus is on the interfaces and data flow between modules.
* **System Testing:** This involves **testing of the complete integrated system to validate that the information system meets all specified requirements.** This verifies the system's compliance with the original specifications.
* **Volume Testing:** This **evaluates the performance of the information system under varying yet realistic work volume and operating conditions.** This is a type of non-functional testing, ensuring the system can handle the expected workload (e.g., peak transaction loads).
* **User Acceptance Testing (UAT):** This is the final verification stage. It is **verification that the system can complete required tasks in a real world operating environment and perform according to the system design specifications.** This test is typically conducted by end-users or the client/product owner.

---

### 2. The Implementation Phase

The implementation phase is the bridge from the development environment to the live production environment.

#### A. Implementation Steps
The steps involved in implementation include the necessary preparations:
* **User Preparation:** This is **the process of readying managers, decision makers, employees, other users and stakeholders to accept and use the new system.** This involves training and change management.
* **Site Preparation:** This is **the preparing of a local for the hardware associated with the new system.**
* **Installation:** This is the **process of physically placing the computer equipment on the site and making it operational.**

#### B. System Cutover Strategies
The **cut over is the process of switching from an old information system to a replacement system.** The choice of conversion strategy dictates the project risk:

| Cutover Type                   | Description (Word-for-Word)                                                                                      | Risk Profile                                                     |
| :----------------------------- | :--------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------- |
| **Direct Conversion (Plunge)** | **Direct conversion from old system to a new system.**                                                           | Highest Risk. Immediate and permanent switch.                    |
| **Parallel Startup**           | **The old system is still being used while the new system is also being implemented.**                           | Lowest Risk. Both systems run concurrently. (Very expensive).    |
| **Pilot Startup**              | **A particular department only is using it.**                                                                    | Controlled Risk. Deploys the system to one small group first.    |
| **Phased Approach**            | **A particular department starts using it and then it will follow by another department by another department.** | Moderate Risk. Gradual rollout, often by function or department. |



[Image of System Cutover Strategies showing Direct, Parallel, Pilot, and Phased conversions]


---

### 3. System Operation and Maintenance

Once implemented, the system enters the longest phase of its life.

* **Operation:** **Using the new or modified system under all kinds of operating conditions.**
* **Maintenance:** **Changing and enhancing the system to make it more useful in achieving user and organizational goals.** Maintenance includes fixing bugs, adapting to environment changes, and adding new features.
* **Disposal:** The final step involves **activities ensuring the orderly dissolution of the system.** This includes a series of critical steps: **communicating the intent, terminating contracts, backups of data are being made, deletion of sensitive data and disposing of hardware.**

---

### 4. Modern Development Methods

The traditional SDLC has been largely superseded by iterative and rapid approaches, driven by the need for speed and responsiveness.

#### A. Agile Development
**Agile development** is an **iterative process that develops the system in sprint increments lasting from two weeks to two months.** Its core focus is to **concentrate on maximizing the themes of the ability to deliver quickly and respond to emerging requirements.**

* **Scrum:** **Scrum method to keep the agile system development effort focused and moving quickly.** The **Scrum Master coordinates all activities** and acts as a facilitator, ensuring the process is followed.
* **Scrum Process Flow:** The process involves a structured flow:
    1.  **Requirements Refinement Meeting** to create a prioritized list of requirements (the **product backlog**).
    2.  **Sprint Planning Session** to select the requirements to implement during the iteration (the **sprint backlog**).
    3.  The **2 to 8 week sprint** includes a **daily scrum meeting** (short stand-up).
    4.  The sprint concludes with a **sprint review meeting** that presents the **working system of requirements implemented so far.**


[Image of the Scrum Agile Software Development Process showing Product Backlog, Sprint Planning, Daily Scrum, Sprint and Sprint Review]


* **Extreme Programming (XP):** **Extreme Programming or XP promotes incremental development of a system using short development cycles to improve productivity accommodate new customer requirements.**

#### B. DevOps: The Enabler of Continuous Delivery
**DevOps is the practice of blending the tasks performed by the development staff in the it operations groups to enable faster and more reliable software releases.** This culture shift automates the delivery pipeline to support Agile's speed.

* **Continuous Deployment:** Organizations often **go live with new software releases every two to four weeks**, but with full automation, **releases are launched daily** as part of a **continuous deployment strategy.**
* **DevOps Process:** The core cycle includes: **plan, define acceptance criteria, build, automated of testing, release, deploy, operate and continuous monitoring.** This creates a closed loop of continuous feedback and improvement.


[Image of the DevOps Cycle showing Plan, Code, Build, Test, Release, Deploy, Operate, Monitor]


---

### 5. Summary of Software Acquisition

In summary, organizations face three strategic choices to **obtain software: subscribe, buy or build.** When integrating any non-custom solution, an organization must consider **the effort required to modify both the new software package and the existing software so that they work well together.**

Our lectures have now covered the entire life cycle of systems development, from initial acquisition strategy through final disposal, and the evolution of methodologies from Waterfall to DevOps.

Would you like to conclude this section of our course with a final comprehensive review, or perhaps move to a lecture on **Application Architectural Patterns**?