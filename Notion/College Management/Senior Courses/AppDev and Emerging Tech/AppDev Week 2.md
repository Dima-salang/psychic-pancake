## 🧠 Lecture 4: Systems Development Concepts and the SDLC

Welcome back, class. In our previous lecture, we focused on application types and frameworks. Today, we elevate our perspective to the broader concept of **Systems Development**, a process that encompasses more than just writing code, and we'll conduct a detailed examination of the classical **Systems Development Life Cycle (SDLC)**.

---

### 1. Distinguishing Information Systems from Computer Programs

It is absolutely crucial to understand the difference between a computer program and a full Information System (IS).

- **Computer Program:** The lecture notes that a computer program **has three components: Hardware, software, and data**. This is the technical core of the solution—the code that executes.
    
- **Information System (IS):** In contrast, the Information System is the complete organizational solution, which **has five components: Hardware, software, data, procedures, and people.**
    

**Theoretical Importance:** The addition of **Procedures** (instructions on how to use the system) and **People** (the users, administrators, and maintainers) means that systems development is an **organizational change process**, not just a technical one. A brilliant program is useless if users don't know how to use it or if the business process doesn't adapt to it.

- **Custom Nature:** Systems development focuses on creating IS solutions that **feed the business objective and users requirements**. This means **it is Never Off the Shelf**; it is always customized to some degree.
    
- **Maintenance:** A system is never truly finished. **The maintenance Information Systems fix the problem in adopt change.**
    

### 2. Types of Information Systems

Information Systems can be categorized by their scope and the number of users they support:

- **Personnel:** Supports **one person with limited set of requirements**. (E.g., a personal to-do list app, a specialized spreadsheet model).
    
- **Group:** Supports **a group of people normally with a single application**. (E.g., a departmental budgeting tool).
    
- **Enterprise:** Supports **many work groups with many different applications**. This involves systems that cross multiple departments within one organization. (E.g., a large ERP system managing finance, HR, and manufacturing).
    
- **Inter-Enterprise:** Supports **many different organizations with many different cultures in different countries and heritages**. This is the highest level of complexity, often involving supply chain or global logistics systems. (E.g., systems connecting Amazon to its third-party sellers and logistics partners).
    

### 3. Systems Development Challenges

Building complex systems is inherently difficult due to several organizational and technical challenges:

- **Requirements and Planning:** The initial difficulty lies in **determining requirements of the project** and **estimating schedule and budget**. Requirements uncertainty and poor estimation are leading causes of project failure.
    
- **Technology Volatility:** We constantly face the challenge of **changing of technology**. What is state-of-the-art today might be legacy tomorrow.
    
- **The Economics of Scale (Brooks' Law):** The lecture highlights a key principle: **as a development teams become larger the average contribution for worker decreases.** This is due to overhead, coordination, and training.
    
    - **Brooks' Law** formalizes this: **adding more people to a late project makes the project later.** This happens because the increased communication paths overwhelm the productivity gain. **Training and coordination is also part of the challenges** that contribute to this delay.
        

### 4. Alternative Systems Development Methods

While we have previously discussed **Agile, the Systems Development Life Cycle (SDLC), and RAD**, the industry employs many other methods, including:

- **Object-Oriented Development (OOD):** Focuses on modeling the system using objects (data and behavior combined) that interact with each other. This often leads to more reusable and maintainable code.
    
- **Extreme Programming (XP):** An Agile framework characterized by short development cycles, pair programming, extensive code review, and customer involvement.
    

**Crucial Caveat:** **Bear in mind that there are no single method works for All Information Systems.** The project manager's job is to select the method that best fits the system's type, complexity, and requirements volatility.

---

## 5. The Classical Systems Development Life Cycle (SDLC)

The **Systems Development Life Cycle is a classical approach with five faces.** It provides a highly structured, phase-based approach.

![Image of the classical Waterfall SDLC with sequential phases labeled System Definition, Requirements Analysis, Component Design, Implementation, and Maintenance](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcRW0jpjpuXBYSCYw-DPlIDl4AVhEbUg4G63quOE9mDRfgJdvp6V1qI0qXaSDvmUeDd1AJk9OtqPNN2jNfUXfki65zfAMEGkfGJlYaJL78wrEbxu0Qc)

Shutterstock

The SDLC **starts with the business planning process** which determines the **system need**.

### A. Phase 1: System Definition

This is the initiation phase, formalizing the need into a concrete project.

- **Objectives:** **Defining the project, the goals and objectives are set and determining the scope and statement of work is given.**
    
- **Feasibility:** A critical step is **assessing visibility** across several dimensions: **cost, organizational or the operational possibility, schedule and Technical** feasibility.
    
- **Team Formation:** **In forming a project team** we appoint a **project manager**, engage the **ID staff** and potentially **outside consultants**, and most importantly, include **user Representatives as domain expert which includes the management and staff**.
    

### B. Phase 2: Requirements Analysis

This is arguably **the most important phase in systems development life cycle** because errors here are the costliest to fix later.

- **Elicitation:** This is where we gather the necessary details, often through **user interviews are being conducted** and **evaluating existing systems**.
    
- **Documentation:** Activities include **determining new forms, reports or queries, identifying new application features and functions**.
    
- **Modeling:** Crucially, there is the **creation of data models** and **development of requirements for the five components** (Hardware, Software, Data, People, Procedures).
    
- **Sign-off:** The phase concludes with **obtaining the user approval through the user acceptance testing** plan, ensuring the requirements are agreed upon and testable.
    

### C. Phase 3: Component Design

The requirements are transformed into technical specifications.

- **Design of Components:** This involves defining specifications for the five components, including **the hardware specifications, the software** architecture, **creating the data model and databases**, and defining **normal backup recovery for both user and operator**.
    
- **People and Procedures:** The design must include the human element, defining **job description of Duty and responsibility for both user and operator**.
    
- **Alternatives:** The team is responsible for **determining Alternatives and evaluating them against requirements**, followed by the selection of **the best alternative that meets the requirement**.
    

#### Security Consideration (Cross-Cutting Concern)

Security is not a single phase but a cross-cutting concern, heavily addressed in design:

- **Access Control:** This includes defining **users authentication, user groups**, and establishing **system restrictions with minimum rights and permissions to user groups for specific features and functions**. This adheres to the **Principle of Least Privilege**.
    

---


## ⚙️ Lecture 5: SDLC Implementation, Maintenance, and Advanced Methodologies

---

### 1. The Implementation Phase (Phases 1 & 2)

The Implementation phase is where the planned system becomes a reality for the end-user. It involves two major components: system build/test and system conversion.

#### A. Implementation Phase 1: Build and Test

This is the technical execution of the design. **This is to build tests and convert to the new system.**

* **Build Components:** This involves **system design for the building of the system components**, followed by **conducting unit testing** (checking individual components) and then **integrating those components to conduct integrated tests** (checking how components work together).
* **Preparation:** Preparation for user adoption is key: **user training, document review and test procedures are also part of the process.**
* **System Testing:** Comprehensive testing verifies quality. The lecture mentions the need for a **test plan** involving **IT professional and the user product quality assurance**. Testing must cover **normal and incorrect action** cases, often concluding with **beta testing**—testing performed by real users in a simulated or actual environment.

#### B. Implementation Phase 2: System Conversion

This is the process of switching from the old system to the new system. **This can be of different types** and the choice determines the risk profile and cost. 

[Image of the four major system conversion types: Pilot, Phased, Parallel, and Plunge/Direct]


| Conversion Type | Description (Word-for-Word) | Elaboration (Risk/Cost) |
| :--- | :--- | :--- |
| **Pilot** | **There is only one department for testing the system and afterwards there could be other.** | **Controlled risk.** **Pilot testing or the pilot conversion can be the control of the negative impact of the system that is new.** If the system fails, only one department is affected. |
| **Parallel** | **Parallel is to save but expensive when you say parallel conversion... so all departments for example are simultaneously doing and using the same system.** | **Lowest risk, highest cost.** Both the old and new systems run simultaneously for a period. This is expensive due to duplication of effort but provides a full backup if the new system fails. |
| **Phased** | **It can start from one Department to another when one department is done it is the time for another department to do such implementation of the new system.** | **Moderate risk.** Components or departments are converted in planned stages. Spreads the cost and risk over time. |
| **Plunge (Direct)** | **The plunge or the direct is to implement the new system right away.** | **Highest risk, lowest cost.** The old system is shut off and the new system is immediately activated. Suitable only when system failure risk is minimal. |

---

### 2. The Maintenance Phase

The final, and often longest, phase of the SDLC. **The maintenance phase is fixing the system to work correctly or adopting the system to changes in requirements.**

* **Core Activities:** It involves **tracking failure or enhancement requests for all five components** and **prioritizing the different requests.**
* **Fixing Failures:** Requests are categorized to determine the release schedule:
    * **Hot fix:** **High priority failures** (e.g., critical security flaws or system crashes).
    * **Service Pack:** **Low priority failures** (a collection of minor patches).
    * **New Release:** **Major enhancement** or a substantial collection of new features.

---

### 3. SDLC Problems and the Need for Adaptability

While the SDLC provides comprehensive structure, it suffers from several inherent flaws:

* **Inflexibility:** **There is a need to crawl back to the waterfall**, meaning the rigidity makes change difficult and expensive.
* **Documentation Burden:** **Unusable documenting requirements are part also of the problems of sdlc.** Teams often spend excessive time on documentation that is out-of-date before the system is built.
* **Estimation Difficulty:** **Scheduling and budgeting difficulty** persists because the linear model demands precise, long-term estimates upfront.

These problems necessitated the development of iterative methods like **Rapid Application Development (RAD)**.

#### Rapid Application Development (RAD) Revisited

RAD, as **proposed by James Martin**, addresses the rigidity of the SDLC by promoting **continuous user involvement** and breaking up **the design and implementation phase of the stlc into smaller pieces** (iterations).

* **Key Techniques:** RAD relies on **prototypes** and **joint application design or JAD** (which **includes the user, developer, and the program quality assurance personnel** working together).
* **Tools:** It requires the use of **visual development tools** and **computer assisted software engineering or computer assisted systems engineering (CASE)** tools to rapidly generate code and manage project artifacts.
* **Flexibility:** Unlike Waterfall, where **no iteration is allowed for the waterfall process**, RAD allows for fixing the design problem by having the team **go back from implementation to design**.

---

### 4. Object-Oriented Development (OOD) and the Unified Process (UP)

**Object-oriented development** is a system development methodology that moves from the sequential process view to a model based on interacting **objects**.

* **Modeling Language:** It uses **UML or the unified modeling language**, which is **a series of diagramming techniques to facilitate object-oriented programming development.** 

[Image of a Unified Modeling Language (UML) Class Diagram]

* **Unified Process (UP):** The **unified process is for developing computer program** (often used for IS development today) and is a specific iterative, incremental framework based on OOD principles. It has five phases:
    1.  **Inception:** **New system definition.** (Like SDLC definition phase).
    2.  **Elaboration:** **Construct and test the framework and architecture of most risk and uncertainty.** This is where high-risk items are tackled early.
    3.  **Construction:** **Lowest features and functions use case requirement.** Building the bulk of the system incrementally.
    4.  **Transition:** **Conversion** to the new system.
    5.  **Maintenance.**
* **Key UP Principle:** The illustration shows that **construction in elaboration has iterations**, demonstrating its core difference from Waterfall.

#### Unified Process Principles (Theoretical Foundation)

UP follows eight key principles that emphasize early risk management and continuous feedback:
1.  **Develop incrementally.**
2.  **State requirements with use cases** (requirements defined by user interaction).
3.  **Address high risk functions early.**
4.  **Build cohesive architecture early.**
5.  **Test and verify qualities early and often.**
6.  **Involve users continuously.**
7.  **Manage requirements.**
8.  **Manage change requests.**

---

### 5. Extreme Programming (XP)

**Extreme Programming (XP)** is a specific, lightweight **Agile** framework that embodies many UP principles.

* **Focus:** It is an **emerging technique for developing computer programs** that is **not useful for large-scale development systems that require business processes and procedures** (due to its intense focus on coding).
* **Characteristics:**
    * **Customer-centric:** **Customer working full-time in the development project.**
    * **Design Philosophy:** **Just in time design for programming (JITD)**, meaning design is done only when needed, not upfront.
    * **Quality Control:** **Paired programming to design error and maintaining effort.** (Two developers work at one workstation).

### 6. Summary Comparison of Methodologies

The lecture concludes with a valuable comparison:

* **SDLC (Classical Waterfall):** **Advantages include comprehensive** and **address both business and technical issues**. **Disadvantages include requirements analyzes may lead to analysis paralysis** and is **waterfall in nature which is unrealistic.**
* **RAD:** **Advantages include operative nature that reduces the risk** and **implements JAD that improves design.** **Disadvantages include less suited to very large projects and long projects** (**greater than or more than six months**).
* **OOD (UP):** **Advantages include use of cases that are effective requirements** and **each iteration terminates with the working system.** **Disadvantages include less useful for business systems development than for program development** and the **danger also of sinking into elaboration black hole** (getting stuck in the design/risk phase).
* **Extreme Programming (XP):** **Advantages is that customer or the user is always involved** and **paired programming that improves quality and reduces risk.** **Disadvantages include focus is on the programming** and **less useful when system involves many users having different possibly conflicting requirements.**

---

Our next lecture will bring these concepts into a modern context by discussing how **Cloud Computing Architectures** (e.g., Serverless, Microservices) are deployed using these methodologies, especially Agile and DevOps.

Would you like to proceed with a lecture on **Cloud Computing Architectural Patterns**?