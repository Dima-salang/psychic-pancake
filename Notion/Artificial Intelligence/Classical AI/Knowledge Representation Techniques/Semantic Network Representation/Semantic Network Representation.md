Alternative of predicate logic for knowledge representation

- Can represent our knowledge in the form of graphical networks
    - This network consists of nodes representing objects and arcs which describe the relationship between those objects
- Semantic networks can categorize the object in different forms and can also link those objects
- Semantic networks are easy to understand and can be easily extended.

  

# Semantic Network Components

  

### Nodes

- objects and concepts that appear graphically as circles, ellipses, or rectangles

### Links

- also called arcs, these express the relationship between nodes using arrows
- also called edges

### Labels

- further specify relations between concepts
- labels are placed on links and nodes
    - is a
        - shows that an object belongs to a larger category of objects
    - has a
        - shows the characteristics of an object node

### Bridge Node

- connects two different semantic networks
- For example, the word farm could be a bridge node that connects nodes for different semantic networks, such as animals and crops.

  

![[/image 8.png|image 8.png]]

### Representation is mainly:

  

- IS-A (inheritance)
- Kind-of-relation (Associative)
    - two concepts are linked based on their frequency to occur together

  

![[/image 1 5.png|image 1 5.png]]

  

### Benefits

- Simplicity
- Space efficiency
- Good organization
- Understandable meanings

  

### Challenges

- Lack of inheritance
    - it’s difficult for semantic networks to accurately pass characteristics from one data point to another
- Limited to object links
- Lack of meta-knowledge
- Challenges of expressing specific properties

  

Great — let’s dive into an **in-depth, CS/AI-theory-style lecture on Semantic Network Representation**, a core idea in **knowledge representation and reasoning (KRR)**, especially relevant for building AI that "understands" structured relationships among concepts.

---

# 🧠 Lecture: Semantic Network Representation

---

### 🎯 Objectives

By the end of this lecture, you'll understand:

- What a semantic network is and why it was developed
- Its components and structure (nodes, edges, inheritance, types of relationships)
- How it compares to other knowledge representations
- The theory and formal basis behind it
- How reasoning works using semantic networks
- Limitations and extensions (e.g. description logics, ontologies)

---

## 1. 📜 Motivation: Why Semantic Networks?

In early AI, we needed ways to represent **knowledge about the world** in a form that both humans and machines could use. Propositional logic and FOL were good for deductive reasoning, but they lacked _intuitive structure_ for representing **conceptual relationships** like:

- "A canary is a bird"
- "Birds can fly"
- "Canaries can sing"

We needed:

- A way to **visualize and reason** about relationships
- A **hierarchical structure** for shared knowledge
- **Efficiency** via inheritance (no need to repeat facts)

Thus, **Semantic Networks** were developed in the 1960s and 70s.

---

## 2. 🧩 Core Structure of a Semantic Network

A **semantic network** is a **graph-based structure** used to represent knowledge in the form of **concepts (nodes)** and **relationships (edges)**.

### 🧱 Basic Elements

### **1. Nodes**

- Represent **concepts**, **objects**, or **instances**
- Examples: `Bird`, `Animal`, `Canary`, `Tweety`, `Fly`

### **2. Edges (Labeled Arcs)**

- Represent **relationships** or **properties**
- Common edge types:
    - `is-a` (class/instance)
    - `has-a` (attribute/property)
    - `part-of` (meronymy)
    - `can`, `lives-in`, `eats`, etc.

### 🧠 Example:

```Plain
Tweety ── is-a ──> Canary ── is-a ──> Bird ── is-a ──> Animal
  │                         │
  └── can ──> Sing          └── can ──> Fly
```

This structure allows _inheritance_ — Tweety can fly, because it's a canary, which is a bird, and birds can fly.

---

## 3. ⚙️ Theoretical Foundations

Although often viewed as informal, semantic networks have a **formal semantics** when interpreted in terms of **set theory** or **Description Logics (DLs)**.

### ✳️ Formal Interpretation

- **Concepts (nodes)** → unary predicates: `Canary(x)`, `Bird(x)`
- **Relationships (edges)** → binary predicates: `isa(x, y)`, `can(x, y)`
- Inference over the network can be interpreted using **First-Order Logic (FOL)**

### Example:

The statement:

```Plain
Canary ── is-a ──> Bird
```

translates to:

```Plain
∀x (Canary(x) → Bird(x))
```

This gives semantic networks their logical **entailment capability**.

---

## 4. 🧬 Inheritance and Default Reasoning

A key strength is **inheritance** — the ability for subclasses or instances to **inherit properties** from more general classes.

### ✳️ Inheritance Types:

- **Strict inheritance**: All subclasses must follow superclass rules
- **Default inheritance**: Subclasses can override superclass properties

### Example (default override):

- Birds can fly (default)
- Penguins are birds
- Penguins **cannot fly**

Semantic networks allow exceptions:

```Plain
Bird ─ can ─> Fly
Penguin ─ is-a ─> Bird
Penguin ─ cannot ─> Fly
```

This enables **non-monotonic reasoning** — a key capability in real-world AI.

---

## 5. 🧠 Reasoning with Semantic Networks

Inference in semantic networks includes:

### ✔️ Subsumption / Inheritance

- `Tweety is-a Canary` → `Tweety is-a Bird`

### ✔️ Property Inheritance

- `Birds can fly` + `Tweety is a bird` → `Tweety can fly`

### ✔️ Transitivity

- `Canary is-a Bird`, `Bird is-a Animal` → `Canary is-a Animal`

### ✔️ Slot-filler inference

- `Car has-part Engine`, `Engine needs Fuel` → `Car needs Fuel` (if rules exist)

This is analogous to **frame-based reasoning**, where entities have "slots" (attributes) with values.

---

## 6. 🧰 Applications

Semantic networks have inspired:

- **Ontologies**: Used in the Semantic Web (OWL, RDF)
- **Natural Language Understanding**
- **Expert Systems**: Rule-based AI with inheritance
- **Cognitive Science**: Modeling human concept memory
- **Search Engines and Knowledge Graphs** (e.g. Google Knowledge Graph)

---

## 7. 🧱 Variants and Extensions

|   |   |
|---|---|
|Variant|Description|
|**Typed Semantic Nets**|Adds types (classes) to relationships for formal checking|
|**Partitioned Nets**|Divides knowledge into contexts or "microtheories" (e.g., CYC)|
|**Description Logics**|Provides logic-based formalism for semantic networks (used in OWL)|
|**Frame-based systems**|Adds attributes and inheritance to concepts, like objects|
|**Conceptual Graphs**|Semantic networks with FOL-level semantics, introduced by John Sowa|

---

## 8. ⚠️ Limitations

- **Ambiguity** in informal graphs
- No built-in **temporal** or **modal** logic
- Poor at expressing **procedural knowledge** (how-to)
- **Scalability issues** in very large graphs
- Limited without formal semantics (but can be extended using DLs)

---

## 🔁 Recap

|   |   |
|---|---|
|Concept|Description|
|Semantic Network|Graph structure representing concepts and relationships|
|Nodes|Represent concepts, entities, or instances|
|Edges|Represent relations (is-a, has-a, etc.)|
|Inheritance|Property transmission from superclasses to subclasses|
|Reasoning|Entailment via transitivity, property inheritance, and exceptions|
|Extensions|Frames, description logic, ontologies|

---

Would you like me to generate **Anki flashcards** for this lecture as well?