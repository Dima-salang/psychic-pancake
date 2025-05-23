---

## 🧠 **The Old Master’s Maxims of Software Testing**

### 1. **“Bugs hide where you don’t look.”**

- **Always test the code paths you assume are safe.**
- That “just a one-liner” you didn’t test? That’s where prod will go down.

---

### 2. **“If you didn’t test it, it doesn’t work.”**

- Untested code is **broken by default**.
- Tests are your **contract with reality**.

---

### 3. **“Start testing before you start coding.”**

- Think of **how** you’ll prove the system works _before_ you build it.
- That’s how architects think — not carpenters.

---

### 4. **“Testing reveals absence of success, not presence of perfection.”**

- Just because it passes all your tests doesn’t mean it works for all cases.
- Testing **reduces risk** — it doesn’t eliminate it.

---

## 🔧 Techniques That Have Never Failed Me

### ✅ **Test Pyramid (Don’t Flip It!)**

1. **Unit Tests** — Fast, focused, cover the “building blocks”.
2. **Integration Tests** — Ensure components talk correctly.
3. **End-to-End Tests** — Mimic real user workflows, but keep them minimal.

> 💬 “If your test suite takes 30 minutes to run, no one will run it.”

---

### 🔁 **Test First, Refactor Later**

- Writing tests **before** you code makes your design naturally modular.
- It’s like placing guardrails before driving a winding mountain road.

---

### 🧪 **Test the Contracts, Not the Internals**

- Don’t test **implementation details**. Test **behavior**.
- Tests should break only when **behavior** changes, not because you renamed a variable.

---

### 🧯 **Failure Cases Are More Important Than Happy Paths**

- Test **edge cases**, **invalid inputs**, **timeouts**, **nulls**, and **concurrent access**.
- Most real-world bugs lie where assumptions break.

---

### 📐 **Deterministic Tests or Bust**

- A flaky test is worse than no test.
- Every test must either **pass** or **fail** for a predictable reason, always.

---

## 🥋 Best Practices You Never Outgrow

### 1. **Mock with discipline.**

- Mocking everything leads to **false confidence**.
- Only mock things you truly **don’t control** (e.g., networks, databases, time).

---

### 2. **Run tests locally, run tests on CI, run tests on merge.**

- Testing must be **automated** and **enforced** in the pipeline.
- Trust, but **verify** — automatically.

---

### 3. **Keep tests close to the code.**

- Tests should live near what they test. Make them a **first-class citizen** in your repo.

---

### 4. **Test names should read like documentation.**

- `test_fails_when_input_is_empty()` is better than `test1()`.
- Good tests explain **what** they check and **why**.

---

### 5. **Use coverage tools, but don’t worship them.**

- 100% code coverage doesn't mean 100% test quality.
- Focus on **critical paths**, **high-risk code**, and **security-relevant logic**.

---

## 🧘 Final Wisdom

> “Your future self will thank you for every test you wrote with care.”

You don’t write tests just for today. You write them for:

- Future you at 3am during an outage.
- A junior dev who’s afraid to touch your code.
- A system that must _still work_ five years from now.

---

Would you like me to generate a printable **"Testing Maxims & Best Practices Cheat Sheet"** for your desk or study wall?

  

Right, here are some detailed and in-depth notes for Week 2, drawing on the provided source material on Software Testing:

**Week 2: Software Testing**

The primary focus for Week 2 is **Software Testing**. Testing is a crucial part of the software development process, with specific goals and techniques.

**Program Testing Goals**

Testing serves two main purposes:

- To **demonstrate to the developer and the customer that the software meets its requirements**. For custom software, this typically means having at least one test for every requirement. For generic software products, tests should cover all system features and combinations of features in the release.
- To **discover situations in which the behavior of the software is incorrect, undesirable, or does not conform to its specification**. This is known as **Defect Testing**, aimed at finding unwanted behaviour like system crashes, incorrect computations, or data corruption.

  

![[/image 13.png|image 13.png]]

Crucially, testing can only **reveal the presence of errors, NOT their absence**.

When you test software, you execute a program using artificial data

**Validation vs. Verification**

Testing plays a key role in the Verification and Validation (V&V) process.

- **Verification** asks, "Are we building the product right?". It is concerned with ensuring the software conforms to its specification.
- **Validation** asks, "Are we building the right product?". It is concerned with ensuring the software does what the user actually requires.

The aim of V&V is to **establish confidence that the system is ‘fit for purpose’**. The level of confidence required depends on factors like the system's criticality, user expectations, and the marketing environment.

  

Validation Testing

- you expect the system to perform correctly using a given set of test cases that reflect the system’s expected use.
- to demonstrate to the developer and the system customer that the software meets its reqs
- a successful test shows that the system operates as intended.

Defect Testing

- the test cases are designed to expose defects. The test cases in defect testing can be deliberately obscure and need not reflect how the system is normally used.
- To discover faults or defects in the software where its behavior is incorrect or not in conformance with its specification
- **a successful test is a test that makes the system perform incorrectly and so exposes a defect in the system.**

**Inspections and Testing**

Software V&V involves both **inspections** and **testing**, which are complementary techniques.

- **Software Inspections** involve static analysis, examining the system representation (like source code, requirements, design) to find problems without executing the system. They can be used before implementation and are effective at discovering program errors. A key advantage is that errors cannot mask other errors during inspections, unlike in dynamic testing. Inspections can also assess broader quality attributes like compliance with standards, portability, and maintainability.
- **Software Testing** involves dynamic verification, executing the system with test data and observing its behaviour.

While inspections can check conformance with a specification, they cannot check conformance with the customer's real requirements or non-functional characteristics like performance or usability. Therefore, **both techniques should be used**.

![[/image 1 9.png|image 1 9.png]]

  

Software Inspections

- these involve people examining the source representation with the aim of discovering anomalies and defects
- inspections not require execution of a system so may be used before implementation
- they may be applied to any representation of the system (reqs, design, config data, test data, prototype, etc…)
- have been shown to be an effective technique for discovering program errors

### ✅ **Why Software Inspections Are Effective:**

1. **They Catch Defects Early (Before Testing)**
    - Many bugs come from **requirements misunderstandings**, logic errors, or **simple mistakes** (e.g., off-by-one errors, missing conditions).
    - These can often be **spotted just by reading** the code or design carefully — no execution needed.
2. **They Are Systematic**
    - Inspections follow **structured checklists** and techniques. Reviewers focus on specific types of errors (e.g., logic errors, interface mismatches, missing edge cases).
    - Because the review is **deliberate and thorough**, it often catches subtle bugs missed during testing.
3. **They Reduce Cost of Defects**
    - Finding a bug in code inspection (before testing or deployment) is **10x–100x cheaper** than fixing it after users encounter it.
    - It also prevents **cascading bugs** that stem from a single root cause.
4. **They Improve Code Quality Beyond Bug Fixes**
    - Inspections help spot **style inconsistencies**, **bad practices**, **unclear logic**, and **security issues**.
    - This leads to more **maintainable**, **readable**, and **robust** code.
5. **They Encourage Team Learning**
    - Code reviews expose developers to each other’s work. This spreads knowledge about design decisions, coding standards, and the system architecture.
6. **They’re Independent of Tools**
    - Unlike dynamic testing, inspections don't need the system to compile, run, or have test cases ready.
    - This is useful in early development stages when the codebase is still incomplete.

  

![[/image 2 7.png|image 2 7.png]]

**Stages of Testing**

The testing process is typically divided into three main stages:

1. **Development testing:** Testing performed by the development team during development to find bugs and defects.
2. **Release testing:** Testing performed by a separate team on a complete version of the system before it is released.
3. **User testing:** Testing performed by actual users or potential users in their own environment.

**Development Testing**

This encompasses testing activities carried out by the team developing the system. It includes:

- **Unit testing:** Testing individual program units, object classes, functions, or methods in isolation. It is primarily a defect testing process. For object classes, complete test coverage means testing all operations, setting/interrogating attributes, and exercising the object in all states. Inheritance makes object class testing more complex.
- **Component testing:** Testing composite components made up of several integrated units or objects. The focus is on testing the component interfaces, assuming individual units have been unit tested.
- Interface testing aims to detect faults due to interface errors or invalid assumptions. Interface types include
    - parameter - data passed from one method or procedure to another
    - shared memory - block of memory is shared between procedures or functions
    - procedural - sub-system encapsulates a set of procedures to be called by other sub-systems
    - and message passing interfaces - sub-systems request services from other sub-systems
    - Common interface errors include misuse (e.g., wrong parameter order), misunderstanding (incorrect assumptions about component behaviour), and timing errors (in real-time systems with shared memory or message passing).
    - Interface testing guidelines
        - Design tests so that parameters to a called procedure are at the extreme ends of their ranges
        - always test pointer parameters with null pointers
        - design tests which cause the component to fail
        - use stress testing in message passing systems
        - in shared memory systems, vary the order in which components are activated.
- **System testing:** Integrating some or all components and testing the system as a whole. The focus is on testing the interactions between components and checking emergent behaviour. System testing can involve newly developed components, reusable components, and off-the-shelf systems. It is a collective process, sometimes involving a separate testing team.

**Automated Testing**

Wherever possible, unit testing should be **automated** to run and check tests without manual intervention. Test automation frameworks (like JUnit) provide tools to write and run automated tests. An automated test typically includes a **setup part (initializing inputs and expected outputs), a call part (executing the code under test), and an assertion part (comparing the actual result with the expected result).**

**Choosing Unit Test Cases**

Test cases should show that the component works as expected in normal operation and also **reveal defects**. This leads to two types of unit test cases: **those reflecting normal operation and those based on testing experience using abnormal inputs that might cause problems.**

Testing strategies include:

- **Partition testing:** Dividing inputs into groups with common characteristics and choosing tests from each group.
- **Guideline-based testing:** Using testing guidelines derived from common programming errors to choose test cases. General guidelines include testing inputs that cause error messages, input buffer overflow, repeated inputs, invalid outputs, or extreme computation results.

  

General Testing Guidelines

- Choose inputs that force the system to generate all error messages
- design inputs that cause input buffers to overflow
- repeat the same input or series of inputs numerous times
- force invalid outputs to be generated
- force computation results to be too large or too small

  

**Use-Case Testing**

Use cases, which describe system interactions, can be a basis for system testing. Testing a use case forces interactions between multiple components. Sequence diagrams associated with use cases can document the components and interactions being tested. An example from the Mentcare system shows how a user scenario can be used to derive test cases covering various system features like authentication, data transfer, encryption/decryption, record handling, database links, and prompting systems.

**Testing Policies**

Exhaustive system testing is impossible. Therefore, testing policies are developed to define required system test coverage. Examples include testing all menu-accessed functions, combinations of related functions, and functions with both correct and incorrect user input.

**Test-Driven Development (TDD)**

TDD is an approach that inter-leaves testing and code development. **Tests are written** _**before**_ **the code** and passing the tests drives the development. Code is developed incrementally along with its test. The process involves identifying a small functional increment, writing an automated test for it (which will initially fail), implementing the functionality, and re-running all tests. Development proceeds to the next increment only when all tests pass.

Benefits of TDD include:

- **Code coverage:** Every code segment has at least one associated test.
- **Regression testing:** A regression test suite is built incrementally.
- **Simplified debugging:** When a test fails, the problem is likely in the newly written code.
- **System documentation:** The tests themselves serve as documentation of what the code should do.

![[/image 3 6.png|image 3 6.png]]

**Regression Testing**

Regression testing involves testing the system after changes to ensure previously working code has not been broken. This is expensive with manual testing but simple and straightforward with automated testing, as all tests can be rerun after every code change. Tests must pass before a change is committed.

**Release Testing**

Release testing is conducted on a version of the system intended for external use. Its goal is to **convince the supplier that the system is good enough for use**. This means demonstrating specified functionality, performance, and dependability, and showing it doesn't fail in normal use. Release testing is typically a **black-box testing** process, deriving tests solely from the system specification. It is a form of system testing, but importantly, it should be performed by a separate team not involved in development. While system testing by developers focuses on finding bugs (defect testing), release testing aims to check if the system meets requirements and is good enough for external use (validation testing).

**Requirements-Based Testing**

This involves examining each system requirement and developing one or more tests for it. Examples from the Mentcare system show how requirements regarding drug allergies and warning overrides can be translated into specific test cases to verify the system's behaviour.

**Performance Testing**

Part of release testing can include testing emergent properties like performance and reliability. Tests should reflect the likely system usage profile. Performance tests often involve steadily increasing the load until performance is unacceptable. **Stress testing** is a type of performance testing that deliberately overloads the system to test its failure behaviour.

**User Testing**

This stage involves users or customers providing input and advice on system testing. It is essential because influences from the user's working environment (which cannot be perfectly replicated in a lab) significantly affect reliability, performance, usability, and robustness.

Types of user testing include:

- **Alpha testing:** Users test the software at the developer's site, often working closely with the development team.
- **Beta testing:** A release is given to users to experiment with in their own environment and report problems.
- **Acceptance testing:** Customers test a system (primarily for custom systems) to decide if it meets their requirements and is ready for deployment.

![[/image 4 5.png|image 4 5.png]]

The acceptance testing process involves defining criteria, planning tests, deriving tests, running tests, negotiating results, and finally accepting or rejecting the system.

In agile methods, the user/customer is often part of the development team and directly responsible for deciding system acceptability. Tests are defined by the user/customer and integrated into the automated test suite, so there is no separate acceptance testing process. A potential issue here is whether the embedded user accurately represents all stakeholders' interests.