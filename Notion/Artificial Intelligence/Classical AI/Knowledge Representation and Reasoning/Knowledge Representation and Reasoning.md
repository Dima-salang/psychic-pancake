  

# Knowledge

One striking aspect of intelligent behavior is that it is clearly conditioned by knowledge:

- for a very wide range of activities, we make decisions about what to do based on what we know (or believe) about the world, effortlessly and unconsciously. Using what we know in this way is so commonplace that we only really pay attention to it when it is not there.
- When we say that someone has behaved unintelligently, what we usually mean is not that there is something that the person did not know, but rather that the person has failed to use what he or she did know.
- **We might say, “You weren’t thinking!” Indeed, it is thinking that is supposed to bring what is relevant in what we know to bear on what awe are trying to do.**

If we think about it, we can think of knowledge as information at rest in an SSD. When we are thinking, we are essentially taking into consideration context and only procure knowledge that is relevant to a certain situation (processed by the RAM and CPU).

  

## What is Knowledge?

When we say “John knows that…,” we fill in the blank with a simple declarative sentence. “John knows that Mary will come to the party,” This suggests that, among other things, knowledge is a relation between a knower, like John, and a proposition, that is, the idea expressed by a simple declarative sentence, like “Mary will come to the party.”

  

What matters about propositions is that they are abstract entities that can be true or false, right or wrong. When we say, “John knows that p,” we can just as well say, “John knows that it is true that p.” ==**Either way, to say that John knows something is to say that John has formed a judgment of some sort, and has come to realize that the world is one way and not another.**==

- If you think about it, this is how knowledge works. We have our own knowledge bases made up of thousands, millions of propositions about we think the world works.

  

A related notion is the concept of belief. The sentence “John believes that p” is clearly related to “John knows that p.” We use the former when we do not wish to claim that John’s judgment about the world is necessarily accurate or held for appropriate reasons.

  

# Types of Knowledge

  

## Declarative Knowledge

- DK is to know about something
- It includes concepts, facts, and objects
- it is also called descriptive knowledge and expressed in declarative sentences
- it is simpler than procedural language

  

## Procedural Knowledge

- it is also known as imperative knowledge
- procedural knowledge is a type of knowledge which is responsible for knowing how to do something
- it can be directly applied to any task
- it includes rules, strategies, procedures, agendas, etc.
- Procedural knowledge depends on the task on which it can be applied

  

## Meta-knowledge

- Knowledge about the other types of knowledge

  

## Heuristic Knowledge

- Heuristic knowledge is representing knowledge of some experts in a field or subject
- Heuristic knowledge is rules of thumb based on previous experiences, awareness of approaches, and which are good to work but not guaranteed.

## Structural Knowledge

- structural knowledge is basic knowledge to problem-solving
- it describes relationships between various concepts such as kind of, part of, and grouping something
- it describes the relationship that exists between concepts or objects

  

![[/image.png|image.png]]

![[/image 1.png|image 1.png]]

  

# Representation

  

> [!important] Roughly speaking, representation is a relationship between two domains, where the first is meant to “stand for” or take the place of the second.

- Usually, the first domain, the representor, is more concrete, immediate, or accessible in some way than the second.
- A drawing of a milkshake and a hamburger on a sign might stand for a less immediately visible fast food restaurant;

  

The type of representor we are concerned with is the formal symbol, that is, a character or group of characters taken from some predetermined alphabet.

- The digit 7 stands for the number 7, as does the group of letters “VII”

As will all representation, it is assumed to be easier to deal with symbols (recognize them, distinguish them from each other, display them, etc.) than with what the symbols represent.

  

Of special concern is when a group of formal symbols stands for a proposition: “John loves Mary: stands for the proposition that John loves Mary. The symbolic English sentence is fairly concrete. The proposition, on the other hand, is abstract. It is something like a classification of all the different ways we can imagine the world to be into two groups: those where John loves Mary, and those where he does not.

> [!important] Knowledge representation, then, is the field of study concerned with using formal symbols to represent a collection of propositions believed by some putative agent.

  

# Reasoning

> [!important] In general, it is the formal manipulation of the symbols representing a collection of believed propositions to produce representations of new one. It is here that we use the fact that symbols are more accessible that the propositions they represent: They must be concrete enough that we can manipulate them (move them around, take them apart, copy them, string together) in such a way as to construct representation of new propositions.

  

We might start with the sentences “John loves Mary” and “Mary is coming to the party,” and after a certain amount of manipulation produce the sentence, “Someone John loves is coming to the party.” We would call this form of reasoning logical inference because the final sentence represents a logical conclusion of the propositions represented by the initial ones.

- According to this view, put forward by Gottfried Leibniz, reasoning is a form of calculation, not unlike arithmetic, but over symbols standing for propositions rather than numbers.

  

The hypothesis underlying work in knowledge representation is that we will want to construct systems that contain symbolic representations with two important properties.

1. We (from the outside as observers) can understand them as standing for propositions.
2. That the system is designed to behave the way it does because of these symbolic representations.

This is the Knowledge Representation Hypothesis by Brian Smith

In other words, the KRH implies that we will want to construct systems for which the intentional stance is grounded by design in symbolic representations. We will call such systems knowledge-based systems and the symbolic representations involved their knowledge bases (KBs)

# Knowledge Representation and Reasoning

  

One definition of AI is that it is the study of intelligent behavior achieved through computational means.

> [!important] KRR, then, is that part of AI that is concerned with how an agent uses what it knows in deciding what to do. It is the study of thinking as a computational process.

  

Often referred to as KRR, it is a fundamental area of AI focused on how to **symbolically capture knowledge** in a form that facilitates **inferencing** and **logical reasoning.**

- part of AI which is concerned with **AI agents thinking** and **how thinking contributes to intelligent behavior of agents.**
- Responsible for **representing information about the real world** so that a computer can understand and can **utilize this knowledge to solve complex real-world problems**
- Describes how we can **represent knowledge** in AI.

  

> [!important]
> 
> ### Logical Agents
> 
> - in which we design agents that can form representations of a complex world, use a process of inference to derive new representation about the world, and use these new representations to deduce what to do.

  

# What to Represent?

## Objects

- All the facts about objects in our world domain
    - Guitars contain strings, trumpets are brass instruments

  

## Events

- Events are the actions which occur in our world

## Performance

- it describes behavior which involves knowledge about how to do things.
- how-to

## Meta-knowledge

- knowledge about what we know

## Facts

- facts are the truths about the real world and what we represent

## Knowledge-Base

- Central component of knowledge-based agents. It is represented as KB.
- A Knowledgebase is a group Sentences (something technical).

  

# Knowledge Representation

The hallmark of a knowledge-based system is that by design it has the ability to be told facts about its world and adjust its behavior correspondingly.

  

## 🧠 Part 1: Why Knowledge Representation?

---

### 🏍️ Analogy: Riding a Bike vs. Thinking

When you ride a bike or play piano, you don’t stop to think, _“Now I will move my right hand…”_ — you just _do it_. That’s **procedural knowledge** — embedded skills.

So some AI researchers say:

> “Let’s just hardcode all the logic and skip this complicated ‘knowledge-based’ stuff.”

And they have a point: **Hardcoding is faster.**

BUT...

---

### 🧩 The Hidden Power of Knowledge-Based Systems

Let’s flip the situation:

🧠 **Human thinking is flexible.**

We **do** use declarative knowledge (i.e., facts and rules) when:

- We’re **learning something new**.
- We’re **solving new problems**.
- We’re **adjusting our behavior** based on what we know.

### 🧾 Example: Reading a Book

Suppose you read:

> “Half of Peru’s population lives in the Andes.”

You didn’t know this before. But now you store it in your **knowledge base (your mind)**.

Later, this fact could help you:

- Answer a quiz.
- Understand a map.
- Plan a trip.

You don’t know _yet_ how it will be used — but you store it **flexibly**.

That’s the power of knowledge representation:

> ✅ It allows us to store general, reusable facts that can support open-ended reasoning in the future.

---

### 🔧 Benefits of Knowledge Representation (with PROLOG example):

1. ✅ **Add new tasks easily**
    
    → E.g., Want the system to “list all yellow things”? Just use the existing KB.
    
2. ✅ **Extend behavior by adding beliefs**
    
    → E.g., Add a rule that “canaries are yellow,” and the system now “knows” more without changing code elsewhere.
    
3. ✅ **Debug by fixing facts**
    
    → E.g., If the system wrongly says the sky is pink, just fix that one fact.
    
4. ✅ **Explain behavior**
    
    → E.g., Why did the system say grass is green?
    
    Because:
    
    - Grass is vegetation
    - Vegetation is green
        
        So, grass is green
        
        (← That’s a logical justification!)
        

---

### 🧠 Concept: Cognitive Penetrability

This means:

> Your actions can change based on what you believe.

**Example: Fire alarm**

- If you believe it’s a real fire → You run!
- If you believe it’s a test → You stay seated.

So your reaction is **cognitively penetrable** — it’s based on your beliefs.

Compare that to **blinking when something moves toward your eye**. You’ll blink **even if** you believe it's harmless — that’s **not cognitively penetrable**.

So in intelligent systems, knowledge-based behavior **mimics human flexibility**, not reflexes.

  

To see the motivation behind reasoning in a knowledge-based system, it suffices to observe that we would like action to depend on what the system believes about the world, as opposed to just what the system has explicitly represented.

  

## 🧠 Part 2: Why Reasoning?

Let’s say our KB has these two facts:

1. Patient X is allergic to medication **M**
2. Anyone allergic to **M** is also allergic to **M'**

Now someone asks:

> ❓Can we prescribe M' to Patient X?

You **don’t** have this fact directly written in the KB:

> “Patient X is allergic to M'”

But you **can infer** it by **reasoning** from facts (1) and (2).

### This is why we need reasoning:

> ✅ It helps us derive useful conclusions from general facts, not just retrieve what’s explicitly stored.

This is different from traditional databases, where you can only query **what’s already there**.

---

### ✨ Key Idea: Logical Entailment

We say:

> A KB entails a fact if that fact logically follows from everything in the KB.

So if:

- All the facts in the KB are true
- And they logically lead to **p**

Then we say:

> The system should believe p.

This is the job of a reasoning engine:

🧠 **Find everything that’s logically entailed** by what we already know.

---

### ⚠️ But... There’s a Catch!

### ⚙️ Reasoning is **computationally hard**

- It might take too long or be too complex to figure out all possible logical conclusions.

### 🧪 Sometimes we want to **guess**, not prove

- E.g., If Tweety is a bird, maybe she flies — but maybe not (she could be a penguin). Still, it’s a _reasonable assumption._

So we allow **incomplete or unsound reasoning** when:

- ✅ Full reasoning is too slow
- ✅ We’re okay with intelligent guesses
- ✅ The KB might contain **contradictions** (e.g., multiple conflicting sources)

  

## The Role of Logic

- The reason logic is relevant to knowledge representation and reasoning is simply because at least according to to one view, logic is the study of entailment relations—languages, truth conditions, and rules of inference.

  

  

# Approaches to Knowledge Representation

  

## Simple relational knowledge

- simplest way of storing facts which uses the relational method, and each fact about a set of the object is set out systematically in columns
- famous in db systems where the relationship between different entities is represented
- has little opportunity for inference

  

## Inheritable Knowledge

- all data must be stored into a hierarchy of classes
- all classes should be arranged in a generalized form or a hierarchical manner
- elements inherit values from other members of a class
    - if we say “Birds can fly,” and “Tweety is a canary,” the system infers that Tweety can fly.
- this approach contains inheritable knowledge which shows a relation between instance and class, and it is called instance relation.
- every individual frame can represent the collection of attributes and its value
- objects and values are represented in Boxed nodes.
- uses arrows which point from objects to their values
- great for structuring knowledge logically and reusing it.

![[image 2.png]]

  

## Inferential Knowledge

- represents knowledge in the form of formal logic
- can be used to derive more facts using logic and inference rules
- it guarantees correctness and provable
- can be slow or hard to compute if the knowledge base is large or contradictory.

  

## Procedural Knowledge

- uses small programs and codes which describes how to do specific things, and how to proceed.
- one important rule is the If-Then rule
    - IF temperature > 100 THEN turn on fan.
- In this knowledge, we can use various coding languages such as LISP or Prolog
- we can easily represent heuristic or domain-specific knowledge
- not necessary that we can represent all cases in this approach. doesn’t always allow flexible reasoning or generalization.

  

  

# Requirements for Knowledge Representation System

- Representational Accuracy
    - should have the ability to represent all kind of required knowledge
    - can it express all the knowledge you need?
- Inferential Adequacy
    - should have the ability to manipulate the representational structures to produce new knowledge corresponding to existing structures
    - can it derive new facts logically from what it knows?
- Inferential Efficiency
    - the ability to direct the inferential knowledge mechanism into the most productive directions by storing appropriate guides
    - can it do it fast and smartly?
- Acquisitional Efficiency
    - the ability to acquire the new knowledge easily using automatic methods
    - can it learn new knowledge easily?

  

  

# Knowledge-Based Agents

  

The central component of a knowledge-based agent is its **knowledge base, or KB.** A knowledge base is a set of **sentences**. Each sentence is expressed in a language called a **knowledge representation language** and represents some assertion about the world.

Sometimes we dignify a sentence with the name **axiom**, when the sentence is taken as given without being derive from other sentences.

  

There must be a way to add new sentences to the knowledge base and a way to query what is known. The standard names for these operations are TELL and ASK, respectively. Both operations may involve inference—that is, deriving new sentences from old. Inference must obey the requirement that when one ASKs a question of the knowledge base, the answer should follow from what has been told (or TELLed) to the knowledge base previously. The inference process should not make things up as it goes along.

The agent maintains a knowledge base, KB, which may initially contain some background knowledge.

Each time the agent program is called:

1. TELLs the knowledge base what it perceives.
2. ASKs the knowledge base what action it should perform. In the process of answering this query, extensive reasoning may be done about the current state of the world, about the outcomes of possible actions sequences, and so on
3. The agent program TELLs the knowledge base which action was chosen, and the agent executes the action.

  

  

# KRR LEC1 Quiz

1. What is the main advantage of a knowledge-based system compared to hardcoding all behavior into procedures? - C ✅
    
    A. Faster runtime
    
    B. Better memory efficiency
    
    C. Flexibility and adaptability to new knowledge
    
    D. More accurate arithmetic operations
    
2. Which cognitive concept explains behavior that depends on beliefs, such as not leaving a building if we believe a fire alarm is a test? -C ✅
    
    A. Logical entailment
    
    B. Procedural reasoning
    
    C. Cognitive penetrability
    
    D. Declarative learning
    
3. In the AI cycle, which phase comes after Knowledge Representation and Reasoning? -B ✅
    
    A. Execution
    
    B. Planning
    
    C. Perception
    
    D. Learning
    
4. Why might it be impractical for a system to compute all logical entailments of its knowledge base? -C ✅
    
    A. Knowledge bases are always inconsistent
    
    B. The answers are often wrong
    
    C. Computational resources may be too limited
    
    D. It violates AI laws
    
5. Which philosopher argued that expert behavior is not based on reasoning but on recognition and reaction? -B WRONG! C IS CORRECT
    
    A. Alan Turing
    
    B. Zenon Pylyshyn
    
    C. Hubert Dreyfus
    
    D. John McCarthy
    
    ✅ Explanation:
    
    Hubert Dreyfus was a prominent philosopher who **criticized the symbolic AI approach**, particularly the idea that all intelligence comes from reasoning with formal rules and representations.
    
    He argued that **experts (like master chess players or surgeons)** often don’t rely on deliberate reasoning. Instead, they **intuitively recognize patterns** and act fluidly without needing to derive decisions from a set of logical rules. This insight led to more interest in **embodied cognition and connectionist models**, such as neural networks, which focus more on recognition than step-by-step reasoning.
    
    > Dreyfus → “Expertise is recognition, not rule-following.”
    
      
    
6. In a knowledge-based system, what does **logical entailment** mean? -B ✅
    
    A. Guessing the most likely answer
    
    B. If a set of facts is true, another fact must also be true
    
    C. Believing only things seen in the environment
    
    D. Memorizing all known information
    
7. What is the main reason to prefer a knowledge-based approach in open-ended task domains? -C ✅
    
    A. Faster execution
    
    B. Easier database management
    
    C. Ability to dynamically use knowledge in new situations
    
    D. Reduced memory usage
    
8. What type of knowledge best describes a program that "knows how" to do something, like how to solve a maze? -C ✅
    
    A. Declarative knowledge
    
    B. Inferential knowledge
    
    C. Procedural knowledge
    
    D. Relational knowledge
    
9. Which term describes the system’s ability to adjust behavior based on beliefs it holds about the world? -C ✅
    
    A. Procedural abstraction
    
    B. Epistemic uncertainty
    
    C. Cognitive penetrability
    
    D. Logical completeness
    
10. What happens in a system that performs logically complete reasoning on an inconsistent KB? -D WRONG! C IS CORRECT!
    
    A. It never halts
    
    B. It becomes more efficient
    
    C. It believes everything
    
    D. It ignores contradictions
    
    This is a **classic problem in logic**:
    
    In **classical logic**, if a knowledge base is **inconsistent** (i.e. it contains a contradiction), then **any statement can be logically inferred from it**.
    
    This is known as the **principle of explosion** — "from a contradiction, anything follows."
    
    > Example: If the KB says both A and ¬A, you can prove anything, even that the sky is green or pigs fly.
    
    This means a logically complete reasoner will derive **every possible sentence** — which **defeats the purpose of reasoning**.
    
    That’s why real-world AI systems often use **non-monotonic**, **probabilistic**, or **paraconsistent** logics to avoid this issue.
    

---

### **Part B: Knowledge Representation Approaches**

1. What is a limitation of simple relational knowledge? -C ✅
    
    A. It uses complex logic
    
    B. It’s difficult to store data
    
    C. It offers little opportunity for inference
    
    D. It’s incompatible with AI systems
    
2. What is a key feature of inheritable knowledge? -C ✅
    
    A. It uses decision trees for planning
    
    B. Data is organized in a flat structure
    
    C. Objects inherit attributes from their class
    
    D. Inference is impossible
    
3. Which approach uses hierarchical structures where instances inherit from classes? -C ✅
    
    A. Relational approach
    
    B. Inferential logic
    
    C. Frame-based (inheritable knowledge)
    
    D. Neural representation
    
4. In the inheritable knowledge approach, what are used to represent relationships between objects and their values? -B ✅
    
    A. Matrices
    
    B. Boxes and arrows
    
    C. Hash tables
    
    D. Loops and branches
    
5. Which approach to knowledge representation is best suited for making formal, logically guaranteed inferences? -B ✅
    
    A. Procedural knowledge
    
    B. Inferential knowledge
    
    C. Simple relational knowledge
    
    D. Inheritable knowledge
    
6. What is a common language used in procedural knowledge representation? -C ✅
    
    A. SQL
    
    B. XML
    
    C. LISP
    
    D. Markdown
    
7. The “If-Then” rule is most associated with which KR approach? -B ✅
    
    A. Inferential
    
    B. Procedural
    
    C. Inheritable
    
    D. Declarative
    
8. What type of knowledge representation can automatically propagate updates across routines that use shared knowledge? -B WRONG! C IS CORRECT!
    
    A. Compiled-out knowledge
    
    B. Procedural logic
    
    C. Knowledge-based representation
    
    D. Static lookup
    
9. In a knowledge-based system, what allows us to explain _why_ a system made a certain decision? -B ✅
    
    A. Database queries
    
    B. Justification from stored beliefs and rules
    
    C. Lookup tables
    
    D. Neural embeddings
    
10. What is the **hallmark** of a knowledge-based system? -C ✅
    
    A. Precompiled logic
    
    B. Memory-efficient architecture
    
    C. Behavior depends on explicitly represented knowledge
    
    D. No runtime inference
    

---

### **Part C: Reasoning and Requirements**

1. What does _inferential adequacy_ refer to in a KR system? -C ✅
    
    A. Efficient memory use
    
    B. Ability to acquire knowledge automatically
    
    C. Ability to infer new knowledge from existing representations
    
    D. Representing hierarchical data
    
2. What does _representational accuracy_ ensure in a KR system? -B ✅
    
    A. The system uses the shortest possible data
    
    B. All relevant knowledge can be correctly represented
    
    C. Only a subset of knowledge is encoded
    
    D. Learning is faster
    
3. Which property helps a system focus reasoning in productive directions? -D ✅
    
    A. Cognitive penetrability
    
    B. Inferential adequacy
    
    C. Representational accuracy
    
    D. Inferential efficiency
    
4. What does _acquisitional efficiency_ mean? -C ✅
    
    A. The system is faster at execution
    
    B. The system can infer more facts per second
    
    C. The system can easily acquire new knowledge
    
    D. The system doesn’t require inference
    
5. Which type of reasoning may be acceptable even if not logically sound? -B ✅
    
    A. Random guessing
    
    B. Approximate belief reasoning (e.g., Tweety the bird)
    
    C. Complete database lookups
    
    D. Backward chaining
    
6. What does it mean for a reasoning method to be **incomplete**? -C ✅
    
    A. It always guesses wrong
    
    B. It doesn’t produce answers in time
    
    C. It may miss some valid inferences
    
    D. It uses outdated data
    
7. What might justify using **unsound reasoning**? -B ✅
    
    A. Faster memory lookup
    
    B. The KB has inconsistencies
    
    C. The system doesn’t need to infer anything
    
    D. The facts are always known
    
8. Why is logical entailment essential in knowledge representation? -C ✅
    
    A. It saves storage
    
    B. It avoids debugging
    
    C. It defines what should be believed based on what is known
    
    D. It mimics human reflexes
    
9. In AI, what is the phase where the system interacts with the external environment based on reasoning and planning? -A ✅
    
    A. Execution
    
    B. Perception
    
    C. Learning
    
    D. Knowledge representation
    
10. Why does “compiling out” knowledge (hardcoding) fail in dynamic or unpredictable domains? -C ✅
    
    A. It uses more memory
    
    B. It requires stronger hardware
    
    C. It lacks flexibility to handle unforeseen situations
    
    D. It supports only simple tasks
    

  

## Answer Key

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Q#|Answer|Q#|Answer|Q#|Answer|
|1|C|11|C|21|C|
|2|C|12|C|22|B|
|3|B|13|C|23|D|
|4|C|14|B|24|C|
|5|C|15|B|25|B|
|6|B|16|C|26|C|
|7|C|17|B|27|B|
|8|C|18|C|28|C|
|9|C|19|B|29|A|
|10|C|20|C|30|C|