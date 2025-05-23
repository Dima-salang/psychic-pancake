Logical representation is a language with some concrete rules which deals with propositions and has no ambiguity in representation.

- It draws a conclusion based on various conditions
- consists of precisely defined syntax and semantics which support the sound inference
- each sentence can be translated into logics using syntax and semantics

  

What does it mean to have a language?

  

1. Syntax
    - are the rules which decide how we can construct legal sentences in the logic
    - it determines which symbol we can use in knowledge representation
    - how to write those symbols
2. Semantics
    - are the rules by which we can interpret the sentence in the logic
    - also involves assigning a meaning to each sentence. We need to be clear about what idea about the world is being expressed. without such an account, we cannot expect to say what believing one of them amounts to.
    - a logic must define the semantics or meaning of sentences. the semantics defines the truth of each sentence with respect to each possible world.
        - x + y = 4 is true in a world where x = 2 and y = 2 but false in a world where x = 1 and y = .
        - In standard logics, every sentence must either be true or false in each possible world—there is no ‘in-between’
3. Pragmatics (from KRR by Brachman)
    - we need to specify how the meaningful expressions in the language are to be used.

  

# Logic

- **Syntax**: What sentences look like (their structure).
- **Semantics**: What sentences mean (their truth in possible worlds).
- **Models**: Mathematical representations of possible worlds.
- **Satisfaction**: When a sentence is true in a model.
- **Entailment**: When one sentence logically follows from another.
- **Inference**: The process of discovering that entailment.
- **Soundness & Completeness**: Properties of inference methods.
- **Grounding**: How logic inside an agent connects to the real world.

  

### Models

- A model is a mathematical snapshot of a world. it is a complete snapshot where every relevant fact has been decided
- in a model, every sentence has a defined truth value, true or false. it defines what is true and what is false for all sentences you’re interested in.
- A model does two things
    - assigns truth values to every atomic sentence or proposition
    - based on those, it determines the truth of more complex sentences
- We use the notation `**M(α)**` to mean: the set of all models where `α` is true.

Say your domain is:

- A: "It is raining"
- B: "The ground is wet"

Then a **model** is just an assignment of truth to A and B.

There are **4 possible models**:

|   |   |   |
|---|---|---|
|Model|A (raining)|B (wet ground)|
|M1|True|True|
|M2|True|False|
|M3|False|True|
|M4|False|False|

Each of these is a **different way the world could be**. Some make sense, some don’t (e.g., M2 says it’s raining but the ground is dry — maybe a roof exists!).

Let’s add a sentence:

`A → B` (If it's raining, then the ground is wet)

We now ask: in which models is this sentence **true**?

Only false in **M2**, because A is true but B is false — the implication fails.

So we say:

> The model M2 does not satisfy A → B
> 
> But models M1, M3, and M4 **do**.

  

### Satisfaction

- A model `m` **satisfies** a sentence `α` if `α` is true in that model.
- this of this as the model agreeing with the statement

### Entailment

- the idea that a sentence follows logically from another sentence.
- We say `α` entails `β` if **every model that makes** `**α**` **true also makes** `**β**` **true**.
- Formally: `α |= β` ⇔ `M(α) ⊆ M(β)`
    - means that the sentence `α` entails the sentence `β`
- This means: `α` rules out more worlds than `β`, so `α` is stronger.
- Example: If `x = 0`, then `xy = 0` must also be true—so `x = 0 |= xy = 0`

### Visualizing with a tiny finite example

|   |   |   |   |
|---|---|---|---|
|x|y|x=0?|x·y=0?|
|0|0|T|T|
|0|1|T|T|
|0|2|T|T|
|1|0|F|T|
|1|1|F|F|
|1|2|F|F|

‑ Look only at the rows where **x=0** (rows 1–3).

‑ In each of those, **x·y=0** is also true.

That exactly shows M(x=0)⊆M(xy=0)M(x=0) so indeed **x=0 entails x·y=0**.

  

This just really means that for all possible arguments for f(x) = True, g(x,y) must also be True given the same arguments. So, f(0) = g(0,y) = True. These are the models in g that logically follows from the models in function f.

  

### Inference = Discovering Entailment

- If entailment is about **truth**, inference is about **procedure**.
- If you can **derive** `β` from `α` using an algorithm, that's inference.
- Written as: `α ⊢ β` (you “prove” `β` from `α`)
- An inference algorithm that derives only detailed sentences is called sound or truth-preserving. Soundness is a highly desirable property. An unsound inference procedure essentially makes things up as it goes along.
- An inference algorithm is complete if it can derive any sentence that is entailed. For real haystacks, which are finite in extent, a systematic examination of model checking can decide whether the needle is in the haystack. However, for many KBs, the haystack of consequences is infinite, and completeness becomes an important issue.

> [!important] If KB is true in the real world, then any sentence α derived from KB by a sound inference procedure is also true in the real world.

### Important Distinction:

- `|=` means **truth** (semantic).
- `⊢` means **proof** (syntactic/inference).
- A good logic system ensures: if `α ⊢ β` then `α |= β` (this is **soundness**).
- If `α |= β` implies `α ⊢ β`, that’s **completeness**.

  

  

### Model Checking

- A brute-force inference method.
- It just **checks all possible models** to see if `α |= β`.
- Only works when the model space is **finite**.
- In programming, it just checks for all possible combinations of the arguments `α` and putting those combinations in `β` and checking which models output True for both functions.

  

### Grounding: Linking Logic to Reality

This part gets **philosophical**.

- A sentence in KB is **just syntax** (some bits in memory).
- How do we know it's _true_ about the real world?
    - Through **sensors**: e.g., the agent senses a breeze → adds `Breeze(2,1)` to KB.
    - That sentence is true _because_ of what’s out there in the world.
- More abstract rules (e.g., “breezes mean adjacent pits”) come from **learning**.
    - These are generalizations from experience.
    - They may be fallible (not always true), but useful.

So logic is grounded in the world through:

- **Perception → perceptual facts**
- **Learning → general rules**

## Advantages

- logical representation allows us to do logical reasoning
- logical representation is the basis for programming languages

## Disadvantages

- have some restrictions and are challenging to work with
- logical representation technique may not be very natural, and inference may not be so efficient

## Categories

  

### Propositional Logic

- is also called Boolean logic as it works on 0 and 1
- we use symbolic variables to represent the logic, and we can use any symbol for representing a proposition, such as A, B, C,…
- Propositions can be either true or false, but it cannot be both
- Propositional logic consists of an object, relations or function, and logical connectives.
- These connective are also called logical operators
- The propositions and connectives are the basic elements of propositional logic

  

  

### Syntax

- defines the allowable sentences.
- The **atomic sentences or propositions** consist of single proposition symbol. Each such symbol stands for a proposition that can be true or false. They are usually uppercase letters.
- There are two proposition symbols with fixed meanings:
    - True is the always-true proposition;
    - False is the always-false proposition
- **Complex sentences or compound propositions** are constructed from simpler sentences, using parentheses and logical connectives. There are five connectives in common:
    
    ![[/image 7.png|image 7.png]]
    
    ![[/image 1 4.png|image 1 4.png]]
    

  

### Semantics

The semantics defines the rules for determining the truth of a sentence with respect to a particular model. In propositional logic, a model simply fixes the truth value—true or false—for every proposition symbol.

$$m_1 = \{P_{1,2}=\text{false}, P_{2,2}=\text{false},P_{3,1}=\text{true}\}$$

With three proposition symbols, there are $2^3=8$ possible models.

The semantics for propositional logic must specify how to compute the truth value of any sentence, given a model. This is done recursively. All sentences are constructed from atomic sentences and the five connectives; therefore, we need to specify how to compute the truth of atomic sentences and how to compute the truth of sentences formed with each of the five connectives. Atomic propositions are easy:

- True is true in every model and False is false in every model
- The truth value of every other proposition symbol must be specified directly in the model. For example, in the model $m_1$ given, $P_{1,2}$ is false.

  

For complex sentences, we have five rules which hold for any subsentences P and Q in any model m.

- ¬P is true iff P is false in m.
- P ∧ Q is true iff both P and Q are true in m.
- P ∨ Q is true iff either P or Q is true in m.
- P ⇒ Q is true unless P is true and Q is false in m.
- P ⇔ Q is true iff P and Q are both true or both false in m.

These can also be represented with truth tables

### Logical Equivalence

- Two propositions are said to be logically equivalent if and only if the columns in the truth table are identical to each other.
- An alternative definition of equivalence is as follows: any sentences α and β are equivalent iff each of them entails the other:
    - α ≡ β if and only if α |= β and β |= α .

![[/image 2 4.png|image 2 4.png]]

### Validity

- A sentence is valid if it is true in all models. For example, the sentence P ∨ ¬P is valid.
- A proposition formula which is always true is called a tautology, and it is also called a valid sentence or proposition. They are also logically equivalent to True.
- A proposition formula which is always false is called a Contradiction.
- A proposition formula which has both true and false values is called
- Statements which are questions, commands, or opinions are not propositions such as “Where is Rohini” are not propositions.
- But what good are valid sentences?

### Deduction Theorem

From the definition of entailment, we can derive the deduction theorem, which was known to the ancient Greeks:

For any sentences α and β, α |= β if and only if the sentence (α ⇒ β) is valid.

- Hence, we can decide if α |= β by checking that (α ⇒ β) is true in every model.
- Conversely, the deduction theorem states that every valid implication sentence describes a legitimate inference

  

### Monotonicity

- the set of entailed sentences can only increase as information is added KB.

### Properties of Operators

![[/image 3 4.png|image 3 4.png]]

  

### Limitations of Propositional Logic

- We cannot represent relations like ALL, some, or none with propositional logic. There are no quantifiers
- Propositional logic has limited expressive power
- In propositional logic, we cannot describe statements in terms of their properties or logical relationships.

  

---

## **I. Propositional Logic Inference Rules**

Let’s say `P`, `Q`, `R` are propositions (e.g., “It’s raining”, “The ground is wet”).

### 1. **Modus Ponens (→ Elimination)**

**If** P → Q and P is true, **then** Q must be true.

- **Form:**
    
    P
    
    P → Q
    
    **∴ Q**
    
- **Example:**
    
    If it rains, the ground is wet.
    
    It rains.
    
    → So, the ground is wet.
    

---

### 2. **Modus Tollens**

**If** P → Q and Q is false, **then** P must be false.

- **Form:**
    
    P → Q
    
    ¬Q
    
    **∴ ¬P**
    
- **Example:**
    
    If the alarm rings, there's a fire.
    
    The alarm didn’t ring.
    
    → Then, no fire.
    

---

### 3. **Hypothetical Syllogism (Transitivity)**

**If** P → Q and Q → R, then P → R.

- **Form:**
    
    P → Q
    
    Q → R
    
    **∴ P → R**
    
- **Example:**
    
    If I study, I’ll pass.
    
    If I pass, I’ll graduate.
    
    → So if I study, I’ll graduate.
    

---

### 4. **Disjunctive Syllogism**

If P ∨ Q is true and one is false, the other must be true.

- **Form:**
    
    P ∨ Q
    
    ¬P
    
    **∴ Q**
    
- **Example:**
    
    I will eat pizza or pasta.
    
    I didn’t eat pizza.
    
    → So, I ate pasta.
    

---

### 5. **Addition**

If P is true, then P ∨ Q is true for any Q.

- **Form:**
    
    P
    
    **∴ P ∨ Q**
    
- **Example:**
    
    It's sunny.
    
    → So it’s sunny or raining.
    

---

### 6. **Simplification**

From P ∧ Q, we can infer P (or Q).

- **Form:**
    
    P ∧ Q
    
    **∴ P**
    
- **Example:**
    
    I’m hungry and tired.
    
    → So, I’m hungry.
    

---

### 7. **Conjunction**

If P is true and Q is true, then P ∧ Q is true.

- **Form:**
    
    P
    
    Q
    
    **∴ P ∧ Q**
    

---

### 8. **Resolution** (used in automated reasoning)

Used to eliminate variables in proofs by contradiction.

- **Form:**
    
    P ∨ Q
    
    ¬Q ∨ R
    
    **∴ P ∨ R**
    
      
    

![[/image 4 3.png|image 4 3.png]]

---

![[/image 5 3.png|image 5 3.png]]

  

## **How These Make Sense (Intuition)**

These rules work like secure plumbing: they guarantee that truth "flows" from premises to conclusions. If the premises are true, applying a valid rule of inference **always** gives you a true conclusion.

Think of it like legal reasoning:

- **Modus Ponens** is like: _“If law says A causes B, and A happened, then B is a legal consequence.”_
- **Simplification** is like: _“If I signed and paid, then I definitely signed.”_

---

### Predicate Logic

---

### 🎯 Objectives:

By the end of this lecture, you'll understand:

- The motivation for predicate logic
- Syntax and semantics
- Quantifiers (∀, ∃)
- Translating natural language into logic
- The role of predicates, variables, and domains
- How inference and entailment work in FOL
- Applications in AI and CS

---

## 1. 🔍 Why Predicate Logic?

**Propositional logic** is limited. It tells us whether _whole statements_ are true or false but can’t talk _about objects inside statements_. For example:

- ❌ _"Socrates is a man"_ would be just `P`, a black box.
- ❌ _"All men are mortal"_ is inexpressible!

### Solution: **Predicate Logic (aka First-Order Logic or FOL)**

This allows us to:

- Refer to _individuals_, _relationships_, and _properties_
- Use _quantifiers_ like "for all" (∀) and "there exists" (∃)

---

## 2. 🧱 Syntax of Predicate Logic

FOL builds expressions using:

### ✳️ **Constants**

- Refer to specific objects: `socrates`, `alice`, `3`

### ✳️ **Variables**

- Stand for arbitrary elements: `x`, `y`, `z`

### ✳️ **Predicates**

- Represent properties or relations: `Man(x)`, `Loves(x, y)`

### ✳️ **Functions**

- Return an object: `FatherOf(x)`, `Max(x, y)`

### ✳️ **Quantifiers**

- **Universal (∀)**: "for all"
- **Existential (∃)**: "there exists"

### ✳️ **Logical connectives**

- `¬` not, `∧` and, `∨` or, `→` implies, `↔` iff

---

### ✅ Well-Formed Formula (WFF)

A WFF is a legal expression like:

```Plain
∀x (Man(x) → Mortal(x))
```

This says: **"For all x, if x is a man, then x is mortal."**

---

## 3. 🔬 Semantics: What Does It _Mean_?

Predicate logic is interpreted over a **domain of discourse** — the set of objects we care about.

### ✳️ Example:

Let the domain be all people.

```Plain
Man(Socrates) → Mortal(Socrates)
```

is **true** if the interpretation of `Man(Socrates)` is true and `Mortal(Socrates)` is true.

**Quantifiers affect truth:**

- `∀x P(x)` is true _if P is true for every x_
- `∃x P(x)` is true _if there's at least one x making P true_

---

## 4. 💬 Translating Natural Language

|   |   |
|---|---|
|Sentence|FOL Expression|
|All humans are mortal|`∀x (Human(x) → Mortal(x))`|
|Some humans are doctors|`∃x (Human(x) ∧ Doctor(x))`|
|Every student loves some course|`∀x (Student(x) → ∃y (Course(y) ∧ Loves(x, y)))`|
|No cat is a dog|`¬∃x (Cat(x) ∧ Dog(x))` or `∀x (Cat(x) → ¬Dog(x))`|

---

## 5. ⚙️ Inference in Predicate Logic

We can **reason** using FOL by applying inference rules:

- **Universal Instantiation** (UI):
    
    From `∀x P(x)` infer `P(a)`
    
- **Existential Instantiation** (EI):
    
    From `∃x P(x)` infer `P(c)` for some _fresh_ constant `c`
    
- **Modus Ponens**:
    
    From `P → Q` and `P`, infer `Q`
    

These are used in **automated theorem proving**, **AI inference engines**, and **logic programming** (e.g. Prolog).

---

## 6. 🧠 Common Pitfalls

- Confusing `∀x ∃y` with `∃y ∀x` — they are **not the same!**
    - `∀x ∃y Loves(x, y)` → Everyone loves _someone_
    - `∃y ∀x Loves(x, y)` → There's _one person_ everyone loves
- Assuming quantifiers distribute over connectives:
    - `∀x (P(x) ∨ Q(x)) ≠ ∀x P(x) ∨ ∀x Q(x)`

---

## 7. 📌 Applications in CS

- **AI**: Knowledge representation (ontologies, expert systems)
- **Databases**: SQL is based on first-order logic
- **Formal Verification**: Proving properties of systems
- **Compiler design**: Type systems use FOL-style logic
- **Theorem provers**: Like Coq, Isabelle

---

## 🔁 Recap:

|   |   |
|---|---|
|Concept|Summary|
|**Predicate**|A property or relation on variables|
|**Quantifiers**|`∀` for all, `∃` there exists|
|**Inference**|Use rules like UI, EI, Modus Ponens|
|**Translation**|Map English → FOL carefully|
|**Applications**|AI, verification, databases, logic programming|

---

Would you like some Anki flashcards based on this lecture too?