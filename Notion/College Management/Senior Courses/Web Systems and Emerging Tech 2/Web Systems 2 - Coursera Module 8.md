   Ah, HITS! This is a beautiful algorithm—one of those ideas that seems obvious in retrospect but was genuinely revolutionary. Kleinberg saw something that Page and Brin missed: the web has **two kinds** of important pages, not one. Let me walk you through this carefully, because the mathematics here is elegant and the intuition is deep.

---

# **HITS: Hubs and Authorities—The Dual Nature of the Web**

## **Part 1: The Insight—Two Types of Value**

### **The Limitation of PageRank**

PageRank gives every page a single number: importance. But think about what you actually do on the web:

| Activity | Example Page | What It Provides |
|----------|-----------|----------------|
| **Find comprehensive lists** | "Best resources for learning physics" | Curated pointers to good stuff |
| **Read original content** | Feynman's actual lecture notes | The valuable information itself |

These are fundamentally different roles! One **organizes**, the other **creates**.

---

### **The Four Quadrants of Web Pages**

Kleinberg realized every page has two independent qualities:

```
                    HIGH QUALITY CONTENT
                           ↑
                           │
    LOW QUALITY LINKS      │      HIGH QUALITY LINKS
         ┌─────────────────┼─────────────────┐
         │   (3) Junk      │   (1) Good Hub  │
         │   Neither       │   Great lists   │
         │   creates nor   │   points to     │
         │   curates well  │   great stuff   │
←────────┼─────────────────┼─────────────────┼────────→
         │   (4) Spam      │   (2) Good      │
         │   Tries to      │   Authority     │
         │   manipulate    │   Great content │
         │   both          │   cited by good │
         │                 │   hubs          │
         └─────────────────┼─────────────────┘
                           │
    LOW QUALITY CONTENT    │    HIGH QUALITY CONTENT
                           ↓
```

| Quadrant | Content | Links | Example |
|----------|---------|-------|---------|
| **(1) Good Hub** | Modest | Excellent | Directory page, bibliography |
| **(2) Authority** | Excellent | (Doesn't matter for authority) | Original research, definitive guide |
| **(3) Junk** | Poor | Poor | Abandoned blog, placeholder |
| **(4) Spam** | Manipulated | Manipulated | Link farm, keyword-stuffed doorway |

**The soccer example from lecture:**
- Content: "Soccer is interesting. Man Utd is famous." (boring, generic)
- Links: FIFA, EPL official, team pages (excellent, authoritative)
- **Verdict:** Good **Hub**, poor **Authority**

---

## **Part 2: The Mutual Reinforcement Principle**

### **The Circular Definition (Beautiful!)**

HITS is built on a recursive insight—like PageRank, but **two-dimensional**:

```
Good Authorities are pointed to by Good Hubs
        ↑                           ↓
Good Hubs point to Good Authorities
```

**Authority:** A page is authoritative if **good hubs** link to it.
**Hub:** A page is a good hub if it links to **good authorities**.

**Neither exists without the other!** At the beginning of the web (in the algorithm's imagination), there were no scores. As links were created by humans making judgments, the scores **emerged** from the pattern of connections.

---

### **The Mathematical Setup**

For page $p$:

| Score | Definition | Formula |
|-------|-----------|---------|
| **Authority score** $a(p)$ | How much good hubs trust $p$ | $a(p) = \sum_{q \to p} h(q)$ |
| **Hub score** $h(p)$ | How much $p$ trusts good authorities | $h(p) = \sum_{p \to q} a(q)$ |

Where $q \to p$ means "page $q$ links to page $p$."

**In words:**
- Your **authority** is the sum of hub scores of pages that point to you
- Your **hub** score is the sum of authority scores of pages you point to

---

## **Part 3: The Connectivity Matrix**

### **Building the Adjacency Matrix**

For $N$ pages, define matrix $A$ where:

$$A_{ij} = \begin{cases} 1 & \text{if page } i \text{ links to page } j \\ 0 & \text{otherwise} \end{cases}$$

**Example (4 pages):**

```
Page 1 → Page 2
Page 2 → Page 3, Page 4
Page 3 → Page 1, Page 2
Page 4 → (no outgoing links)
```

**Matrix $A$:**

$$A = \begin{bmatrix} 
0 & 1 & 0 & 0 \\  % 1→2
0 & 0 & 1 & 1 \\  % 2→3,4
1 & 1 & 0 & 0 \\  % 3→1,2
0 & 0 & 0 & 0    % 4→(nothing)
\end{bmatrix}$$

**Matrix $A^T$ (transpose, swaps rows/columns):**

$$A^T = \begin{bmatrix} 
0 & 0 & 1 & 0 \\  % 1←3
1 & 0 & 1 & 0 \\  % 2←1,3
0 & 1 & 0 & 0 \\  % 3←2
0 & 1 & 0 & 0    % 4←2
\end{bmatrix}$$

**Interpretation of $A^T$:** $A^T_{ij} = 1$ means page $j$ is **pointed to by** page $i$.

---

## **Part 4: The Iterative Algorithm**

### **Vector Formulation**

Let:
- $\vec{a} = [a_1, a_2, \ldots, a_N]^T$ = authority scores (column vector)
- $\vec{h} = [h_1, h_2, \ldots, h_N]^T$ = hub scores (column vector)

**The update equations:**

| Operation | Matrix Form | Meaning |
|-----------|-------------|---------|
| Hub update | $\vec{h} \leftarrow A \cdot \vec{a}$ | Sum authorities of pages you point to |
| Authority update | $\vec{a} \leftarrow A^T \cdot \vec{h}$ | Sum hubs of pages pointing to you |

**Combined:**
$$\vec{a} \leftarrow A^T \cdot A \cdot \vec{a}$$
$$\vec{h} \leftarrow A \cdot A^T \cdot \vec{h}$$

---

### **Worked Example: First Iteration**

**Initial state:** All authority scores = 1, all hub scores = 1.

$$\vec{a}_0 = \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \end{bmatrix}, \quad \vec{h}_0 = \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \end{bmatrix}$$

**Step 1: Compute new hub scores** $\vec{h}_1 = A \cdot \vec{a}_0$

$$\vec{h}_1 = \begin{bmatrix} 
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix} \cdot \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \\ 2 \\ 0 \end{bmatrix}$$

**Interpretation:**
- Page 1: hub score 1 (points to page 2, which has authority 1)
- Page 2: hub score 2 (points to pages 3,4, total authority 2)
- Page 3: hub score 2 (points to pages 1,2, total authority 2)
- Page 4: hub score 0 (points to nothing)

**Step 2: Compute new authority scores** $\vec{a}_1 = A^T \cdot \vec{h}_1$

$$\vec{a}_1 = \begin{bmatrix} 
0 & 0 & 1 & 0 \\
1 & 0 & 1 & 0 \\
0 & 1 & 0 & 0 \\
0 & 1 & 0 & 0
\end{bmatrix} \cdot \begin{bmatrix} 1 \\ 2 \\ 2 \\ 0 \end{bmatrix} = \begin{bmatrix} 2 \\ 3 \\ 2 \\ 0 \end{bmatrix}$$

**Interpretation:**
- Page 1: authority 2 (pointed to by page 3, hub score 2)
- Page 2: authority 3 (pointed to by pages 1,3, hub scores 1+2=3)
- Page 3: authority 2 (pointed to by page 2, hub score 2)
- Page 4: authority 0 (pointed to by page 2, but wait...)

*Correction: Page 4 is pointed to by page 2. Let me recheck...*

Actually: $A^T_{4,2} = 1$ (page 2 points to page 4), so:
- Page 4 authority = $h_2 = 2$? No wait, let me recheck the matrix...

Looking at original $A$: row 2 is $[0, 0, 1, 1]$, so page 2 points to pages 3 and 4.
Thus $A^T$ column 2 should have 1s at rows 3 and 4.

Correct $A^T$:
$$A^T = \begin{bmatrix} 
0 & 0 & 1 & 0 \\  % page 1 pointed to by page 3
1 & 0 & 1 & 0 \\  % page 2 pointed to by pages 1,3
0 & 1 & 0 & 0 \\  % page 3 pointed to by page 2
0 & 1 & 0 & 0     % page 4 pointed to by page 2
\end{bmatrix}$$

So $\vec{a}_1[4] = 0 \cdot h_1 + 1 \cdot h_2 + 0 \cdot h_3 + 0 \cdot h_4 = h_2 = 2$? 

Wait, let me recheck: $A^T_{4,2}$ means row 4, column 2. Row 4 is $[0, 1, 0, 0]$, so:
- $\vec{a}_1[1] = 0\cdot1 + 0\cdot2 + 1\cdot2 + 0\cdot0 = 2$ ✓
- $\vec{a}_1[2] = 1\cdot1 + 0\cdot2 + 1\cdot2 + 0\cdot0 = 3$ ✓
- $\vec{a}_1[3] = 0\cdot1 + 1\cdot2 + 0\cdot2 + 0\cdot0 = 2$ ✓
- $\vec{a}_1[4] = 0\cdot1 + 1\cdot2 + 0\cdot2 + 0\cdot0 = 2$ 

So page 4 has authority 2, not 0. My earlier calculation was wrong.

---

## **Part 5: Convergence to Eigenvectors**

### **The Mathematical Beauty**

After many iterations with normalization:

$$\vec{a} \propto (A^T A)^k \cdot \vec{a}_0 \to \text{principal eigenvector of } A^T A$$

$$\vec{h} \propto (A A^T)^k \cdot \vec{h}_0 \to \text{principal eigenvector of } A A^T$$

**Key facts:**
- $A^T A$ and $A A^T$ are symmetric, positive semi-definite
- They share the same non-zero eigenvalues
- The principal (largest) eigenvalue gives the dominant scores
- Convergence is guaranteed, independent of initial values

**Why this works:** Just like PageRank, we're doing power iteration on a matrix. The difference is HITS uses **two** matrices ($A^T A$ for authorities, $A A^T$ for hubs) that are **symmetric** (unlike PageRank's non-symmetric matrix).

---

## **Part 6: HITS vs. PageRank—Deep Comparison**

| Aspect | PageRank | HITS |
|--------|----------|------|
| **Scores per page** | 1 (importance) | 2 (hub + authority) |
| **Query dependence** | Query-independent (precomputed) | **Query-dependent** (computed on subgraph) |
| **Computation** | Global, offline | Local to query, online |
| **Matrix** | Row-stochastic (teleportation) | Symmetric $A^T A$, $A A^T$ |
| **Convergence** | Unique due to teleportation | Multiple equilibria possible (no teleportation) |
| **Spam resistance** | Good (hard to get important links) | Poor (easy to add out-links) |

---

### **The Critical Difference: Query Dependence**

**PageRank:** Compute once for all pages. Use for any query.

**HITS:** For each query:
1. Find all pages matching query (root set)
2. Expand to include linked pages (base set)
3. Run HITS **only on this subgraph**
4. Get query-specific hub and authority scores

**Example:**

| Query | Root Set | Authorities Found |
|-------|----------|-------------------|
| "quantum computing" | Pages with these words | Quantum experts, research labs |
| "quantum computing tutorials" | Pages with these words | Educational sites, course pages |

Same page might be authority for first query, hub for second!

---

## **Part 7: Why HITS Lost to PageRank**

### **Practical Weaknesses**

| Problem | Why It Hurts | PageRank Advantage |
|---------|-----------|-------------------|
| **Query-time computation** | Too slow for billions of queries | Precomputed, instant lookup |
| **Spam vulnerability** | Easy to create out-links to good pages | Hard to get in-links from good pages |
| **Stability** | Small graph changes cause big score swings | Global computation dampens local changes |
| **Scalability** | Subgraph extraction is expensive | Single global computation |

**The irony:** HITS is mathematically elegant and intuitively appealing, but **engineering reality** favored PageRank's precomputation model.

---

## **Part 8: Modern Relevance**

### **Where HITS Ideas Survive**

| Application | How HITS Lives On |
|-------------|-------------------|
| **Topic-specific PageRank** | Personalized vectors (partial HITS spirit) |
| **Recommendation systems** | User-item bipartite graphs (similar mathematics) |
| **Citation analysis** | Hub journals vs. authoritative papers |
| **Social networks** | Influencers (hubs) vs. content creators (authorities) |

**The core insight—dual roles in networks—remains powerful**, even if the original algorithm isn't used at web scale.

---

## **Summary: The Mathematics of Mutual Recognition**

```
HITS ALGORITHM
─────────────────────────────────────────
DATA: Web subgraph for query
     ↓
CONSTRUCT: Adjacency matrix A
     ↓
ITERATE:
    h ← A · a     (hubs aggregate authorities they point to)
    a ← Aᵀ · h    (authorities aggregate hubs pointing to them)
    normalize
     ↓
CONVERGES TO:
    a = principal eigenvector of AᵀA
    h = principal eigenvector of AAᵀ
     ↓
OUTPUT: Query-specific hub and authority rankings
─────────────────────────────────────────
```

**The philosophical lesson:** Value in networks is **relational**, not intrinsic. A page is authoritative because good hubs say so; a hub is good because it finds authorities. The scores **co-evolve**, emerging from the pattern of human judgments encoded in links.

This is why the web, despite being created by millions of independent actors, has **structure**—and why algorithms can discover that structure.