 I'll create a comprehensive lecture on similarity measures in Information Retrieval, covering the mathematical foundations, geometric interpretations, and practical applications of different similarity metrics.

---

# **Similarity Measures in Information Retrieval: From Vectors to Relevance**

## **Introduction: The Vector Space Representation**

### **From Documents to Vectors**

In the Vector Space Model (VSM), we transform textual documents into mathematical vectors that can be compared computationally.

**The Document-Term Matrix:**

Consider a collection with $n$ documents and a vocabulary of $m$ terms:

$$
\begin{array}{c|cccccc}
 & \text{term}_1 & \text{term}_2 & \text{term}_3 & \cdots & \text{term}_m \\
\hline
\text{Doc}_1 & w_{1,1} & w_{1,2} & w_{1,3} & \cdots & w_{1,m} \\
\text{Doc}_2 & w_{2,1} & w_{2,2} & w_{2,3} & \cdots & w_{2,m} \\
\vdots & \vdots & \vdots & \vdots & \ddots & \vdots \\
\text{Doc}_n & w_{n,1} & w_{n,2} & w_{n,3} & \cdots & w_{n,m} \\
\hline
\text{Query} & q_1 & q_2 & q_3 & \cdots & q_m \\
\end{array}
$$

Where $w_{i,j}$ represents the weight of term $j$ in document $i$ (typically TF-IDF).

---

### **Geometric Interpretation**

Each document becomes a **point** in $m$-dimensional space:

| Vocabulary Size | Space Type | Visualization |
|-----------------|------------|---------------|
| 2 terms | 2D plane | X-Y coordinates |
| 3 terms | 3D space | X-Y-Z coordinates |
| $m$ terms | $m$-dimensional hyperspace | Abstract mathematical space |

**Example with 2D simplification:**

```
        term_2 (Database)
              ↑
              │    D1(2,3)
              │   ╱
              │  ╱  Q(0,2)
              │ ╱  ╱
              │╱  ╱ D2(1,0)
              └──────────→ term_1 (Retrieval)
```

- **Document 1:** Vector $\vec{D_1} = (2, 3)$ — high on both retrieval and database
- **Document 2:** Vector $\vec{D_2} = (1, 0)$ — moderate retrieval, no database
- **Query:** Vector $\vec{Q} = (0, 2)$ — focused on database

**Key Insight:** Similarity becomes a geometric problem—how "close" are these vectors?

---

## **Part 1: Inner Product (Dot Product)**

### **Mathematical Definition**

For vectors $\vec{D} = (d_1, d_2, \ldots, d_m)$ and $\vec{Q} = (q_1, q_2, \ldots, q_m)$:

$$\text{Inner Product}(\vec{D}, \vec{Q}) = \vec{D} \cdot \vec{Q} = \sum_{i=1}^{m} d_i \times q_i$$

---

### **Binary Vectors (0/1 weights)**

When weights are binary (1 = term present, 0 = term absent):

$$\text{Inner Product}_{\text{binary}} = \sum_{i=1}^{m} \mathbb{1}_{[d_i = 1 \land q_i = 1]}$$

**Result:** Simply counts the number of **matching terms**.

**Example:**

| Term | Doc | Query | Product |
|------|-----|-------|---------|
| retrieval | 1 | 1 | 1 |
| database | 1 | 0 | 0 |
| architecture | 1 | 1 | 1 |
| management | 1 | 1 | 1 |
| information | 0 | 0 | 0 |

**Inner Product = 3** (three matching terms: retrieval, architecture, management)

---

### **Weighted Vectors (TF-IDF weights)**

$$\text{Inner Product}_{\text{weighted}} = \sum_{i=1}^{m} \text{TF-IDF}(t_i, D) \times \text{TF-IDF}(t_i, Q)$$

**Example Calculation:**

| Term | $D_1$ weight | Query weight | Product |
|------|-------------|--------------|---------|
| term_1 | 2 | 0 | 0 |
| term_2 | 3 | 0 | 0 |
| term_3 | 5 | 2 | **10** |
| term_4 | 0 | 1 | 0 |

**Inner Product = 10**

---

### **Critical Limitation: The Length Bias**

**The Problem:** Inner product is **unbounded** and favors long documents.

**Mathematical Proof:**

Consider two documents matching the same query terms:
- **Short Document:** 100 terms total, matches 5 query terms with weight 1 each
  - Inner Product = $5 \times 1 = 5$
  
- **Long Document:** 10,000 terms total, matches 50 query terms (by chance) with weight 0.5 each
  - Inner Product = $50 \times 0.5 = 25$

**Result:** Long document scores higher despite potentially lower relevance!

**Why this happens:**
$$\text{Inner Product} = \sum d_i q_i \propto |\{\text{matching terms}\}| \times \text{average weight}$$

Longer documents have more opportunities for term matching, inflating the score.

**Geometric interpretation:**
- Inner product = $|\vec{D}| \times |\vec{Q}| \times \cos(\theta)$
- Depends on **magnitude** (length) of vectors, not just angle

---

## **Part 2: Cosine Similarity**

### **Mathematical Definition**

To eliminate length bias, we **normalize** by vector magnitudes:

$$\text{Cosine Similarity}(\vec{D}, \vec{Q}) = \frac{\vec{D} \cdot \vec{Q}}{|\vec{D}| \times |\vec{Q}|} = \frac{\sum_{i=1}^{m} d_i q_i}{\sqrt{\sum_{i=1}^{m} d_i^2} \times \sqrt{\sum_{i=1}^{m} q_i^2}}$$

**Range:** $[0, 1]$ (for non-negative weights, typical in IR)

---

### **Geometric Interpretation**

$$\text{Cosine Similarity} = \cos(\theta)$$

Where $\theta$ is the angle between vectors:

| Angle $\theta$ | Cosine Value | Interpretation |
|---------------|--------------|----------------|
| 0° (identical direction) | 1.0 | Perfect match |
| 45° | 0.707 | Moderate similarity |
| 90° (perpendicular) | 0.0 | No common terms |
| > 90° | < 0 | Opposite (rare in IR) |

**Visual Example:**

```
        ↑ term_2
        │    ╲ D1
        │     ╲
        │      ╲
        │       ╲
        │   Q────●
        │      ╱
        │     ╱ D2
        └────────→ term_1
```

- **D1** forms small angle with Q → high cosine similarity (0.81)
- **D2** forms large angle with Q → low cosine similarity (0.13)

---

### **Why Cosine Similarity Works**

**Normalization Effect:**

$$\text{Cosine} = \frac{\vec{D}}{|\vec{D}|} \cdot \frac{\vec{Q}}{|\vec{Q}|}$$

- Divides each vector by its length (L2 normalization)
- Creates **unit vectors** (length = 1) pointing in original directions
- Measures **directional similarity**, independent of magnitude

**Comparison with Inner Product:**

| Aspect | Inner Product | Cosine Similarity |
|--------|---------------|-------------------|
| Range | $[0, \infty)$ | $[0, 1]$ |
| Document length effect | Strong bias | Eliminated |
| Measures | Overlap + magnitude | Angular alignment |
| Best for | Fixed-length documents | Variable-length documents |

---

## **Part 3: Jaccard Coefficient**

### **Mathematical Definition**

The Jaccard coefficient measures **set intersection** normalized by **set union**:

$$\text{Jaccard}(D, Q) = \frac{|D \cap Q|}{|D \cup Q|} = \frac{|D \cap Q|}{|D| + |Q| - |D \cap Q|}$$

Where $|D|$ represents the set of terms in document $D$ (or sum of weights).

**Alternative form for weighted vectors:**

$$\text{Jaccard}_{\text{weighted}} = \frac{\sum \min(d_i, q_i)}{\sum \max(d_i, q_i)}$$

---

### **Key Properties**

1. **Penalizes non-matching terms:** Both document-specific and query-specific terms reduce similarity
2. **Bounded:** Always $[0, 1]$
3. **Length-sensitive:** Longer documents with same match count score lower

**Example Analysis:**

| Scenario | $|D \cap Q|$ | $|D|$ | $|Q|$ | Jaccard | Interpretation |
|----------|-------------|-------|-------|---------|----------------|
| Perfect match | 5 | 5 | 5 | 5/5 = **1.0** | Identical |
| Partial overlap | 3 | 5 | 5 | 3/7 ≈ **0.43** | Moderate |
| Large doc, small match | 3 | 100 | 5 | 3/102 ≈ **0.03** | Penalized |

---

### **Jaccard vs. Cosine: The Critical Difference**

| Feature | Cosine | Jaccard |
|---------|--------|---------|
| **Normalization** | By vector magnitude | By union size |
| **Empty terms handling** | Ignores (0 contributes nothing) | Penalizes (increases denominator) |
| **Focus** | Directional alignment | Overlap ratio |
| **Dissimilarity consideration** | Indirect | Explicit (union size) |
| **Best for** | Ranked retrieval | Set comparison, duplicates |

**Concrete Example:**

- **Document 1:** 10 terms, 5 match query
- **Document 2:** 100 terms, 5 match query

| Measure | Doc 1 | Doc 2 | Ratio |
|---------|-------|-------|-------|
| Inner Product | 5 | 5 | 1:1 |
| Cosine | 0.71 | 0.35 | **2:1** |
| Jaccard | 5/10 = 0.5 | 5/100 = 0.05 | **10:1** |

**Jaccard penalizes length more aggressively than Cosine.**

---

## **Part 4: Dice Coefficient**

### **Mathematical Definition**

$$\text{Dice}(D, Q) = \frac{2 \times |D \cap Q|}{|D| + |Q|}$$

**Relationship to other measures:**
- Similar to Jaccard but with different denominator
- Always yields higher values than Jaccard for same overlap

**Example:**
- $|D \cap Q| = 3$, $|D| = 5$, $|Q| = 5$
- Dice = $2 \times 3 / (5 + 5) = 6/10 = 0.6$
- Jaccard = $3 / (5 + 5 - 3) = 3/7 \approx 0.43$

---

### **Limitation: Triangle Inequality**

**Metric Space Requirements:**
For a distance measure $d(x, y)$ to be a valid metric, it must satisfy:
1. Non-negativity: $d(x, y) \geq 0$
2. Identity: $d(x, y) = 0 \iff x = y$
3. Symmetry: $d(x, y) = d(y, x)$
4. **Triangle Inequality:** $d(x, z) \leq d(x, y) + d(y, z)$

**Why Triangle Inequality Matters:**
- Enables efficient similarity search (triangle inequality allows pruning)
- Ensures geometric consistency
- Required for certain clustering algorithms

**Dice Coefficient Issue:**
Dice similarity converted to distance ($1 - \text{Dice}$) **violates** triangle inequality.

**Example violation:**
- Documents A, B, C arranged such that:
  - $d(A, C) > d(A, B) + d(B, C)$
- This creates "shortcuts" that break geometric intuition

**Consequence:** Limited use in IR, more common in image processing where exact metric properties are less critical.

---

## **Part 5: Complete Worked Example**

### **Scenario Setup**

**Vocabulary:** {retrieval, database, architecture, management, information}

**Document 1:** "Text retrieval and database systems for information retrieval"
- Terms: retrieval (2), database (1), information (1)
- Vector: $\vec{D_1} = (2, 1, 0, 0, 1)$

**Document 2:** "Database architecture and management systems"
- Terms: database (1), architecture (1), management (1)
- Vector: $\vec{D_2} = (0, 1, 1, 1, 0)$

**Query:** "Database retrieval systems"
- Terms: database (1), retrieval (1)
- Vector: $\vec{Q} = (1, 1, 0, 0, 0)$

---

### **Step 1: Calculate TF-IDF Weights**

Assume simplified IDF values:
- retrieval: idf = 2.0
- database: idf = 1.5
- architecture: idf = 2.5
- management: idf = 2.0
- information: idf = 0.5

**Weighted Vectors:**

| Term | $D_1$ TF | $D_1$ Weight | $D_2$ TF | $D_2$ Weight | Query TF | Query Weight |
|------|---------|--------------|---------|--------------|---------|--------------|
| retrieval | 2 | 4.0 | 0 | 0.0 | 1 | 2.0 |
| database | 1 | 1.5 | 1 | 1.5 | 1 | 1.5 |
| architecture | 0 | 0.0 | 1 | 2.5 | 0 | 0.0 |
| management | 0 | 0.0 | 1 | 2.0 | 0 | 0.0 |
| information | 1 | 0.5 | 0 | 0.0 | 0 | 0.0 |

---

### **Step 2: Calculate Similarities**

#### **Inner Product**

$$\vec{D_1} \cdot \vec{Q} = (4.0 \times 2.0) + (1.5 \times 1.5) + 0 + 0 + 0 = 8.0 + 2.25 = \mathbf{10.25}$$

$$\vec{D_2} \cdot \vec{Q} = (0 \times 2.0) + (1.5 \times 1.5) + 0 + 0 + 0 = \mathbf{2.25}$$

**Ranking:** $D_1 > D_2$ (4.6× better)

---

#### **Cosine Similarity**

**Magnitudes:**
- $|\vec{D_1}| = \sqrt{4.0^2 + 1.5^2 + 0.5^2} = \sqrt{16 + 2.25 + 0.25} = \sqrt{18.5} \approx 4.30$
- $|\vec{D_2}| = \sqrt{1.5^2 + 2.5^2 + 2.0^2} = \sqrt{2.25 + 6.25 + 4} = \sqrt{12.5} \approx 3.54$
- $|\vec{Q}| = \sqrt{2.0^2 + 1.5^2} = \sqrt{4 + 2.25} = \sqrt{6.25} = 2.5$

**Calculations:**
$$\text{Cosine}(D_1, Q) = \frac{10.25}{4.30 \times 2.5} = \frac{10.25}{10.75} \approx \mathbf{0.95}$$

$$\text{Cosine}(D_2, Q) = \frac{2.25}{3.54 \times 2.5} = \frac{2.25}{8.85} \approx \mathbf{0.25}$$

**Ranking:** $D_1 > D_2$ (3.8× better)

---

#### **Jaccard Coefficient**

Using binary presence for simplicity:

| Set | $D_1$ | $D_2$ | $Q$ |
|-----|-------|-------|-----|
| Terms | {retrieval, database, information} | {database, architecture, management} | {retrieval, database} |

**Intersections:**
- $|D_1 \cap Q| = |\{\text{retrieval, database}\}| = 2$
- $|D_2 \cap Q| = |\{\text{database}\}| = 1$

**Unions:**
- $|D_1 \cup Q| = |\{\text{retrieval, database, information}\}| = 3$
- $|D_2 \cup Q| = |\{\text{retrieval, database, architecture, management}\}| = 4$

**Calculations:**
$$\text{Jaccard}(D_1, Q) = \frac{2}{3} \approx \mathbf{0.67}$$

$$\text{Jaccard}(D_2, Q) = \frac{1}{4} = \mathbf{0.25}$$

**Ranking:** $D_1 > D_2$ (2.7× better)

---

#### **Dice Coefficient**

$$\text{Dice}(D_1, Q) = \frac{2 \times 2}{3 + 2} = \frac{4}{5} = \mathbf{0.80}$$

$$\text{Dice}(D_2, Q) = \frac{2 \times 1}{3 + 2} = \frac{2}{5} = \mathbf{0.40}$$

**Ranking:** $D_1 > D_2$ (2× better)

---

### **Comparison Summary**

| Measure | $D_1$ Score | $D_2$ Score | $D_1$ Advantage | Key Factor |
|---------|-------------|-------------|-----------------|------------|
| Inner Product | 10.25 | 2.25 | 4.6× | Raw overlap magnitude |
| Cosine | 0.95 | 0.25 | 3.8× | Directional alignment |
| Jaccard | 0.67 | 0.25 | 2.7× | Overlap ratio (strict) |
| Dice | 0.80 | 0.40 | 2× | Overlap ratio (lenient) |

**Insight:** All measures agree $D_1$ is more relevant, but the **degree of advantage varies significantly** based on what aspect of similarity is emphasized.

---

## **Part 6: Practical Applications**

### **Using Similarity Measures in IR Systems**

#### **1. Document Ranking**

```
For each document D in collection:
    score = Similarity(D, Q)  # Choose: Cosine, Jaccard, etc.
    
Sort documents by score descending
Return top-K results
```

#### **2. Threshold-Based Retrieval**

```
Retrieve only documents where:
    Similarity(D, Q) > θ  # θ = threshold (e.g., 0.5)
    
Benefit: Precision control—exclude low-confidence matches
```

#### **3. Relevance Feedback (Query Reformulation)**

**Rocchio Algorithm:**
$$\vec{Q}_{\text{new}} = \alpha \vec{Q}_{\text{original}} + \beta \frac{1}{|R|} \sum_{\vec{D} \in R} \vec{D} - \gamma \frac{1}{|N|} \sum_{\vec{D} \in N} \vec{D}$$

Where:
- $R$ = set of relevant documents (user-marked)
- $N$ = set of non-relevant documents
- $\alpha, \beta, \gamma$ = tuning parameters

**Process:**
1. User queries with $\vec{Q}$
2. System returns ranked list
3. User marks some results as relevant/non-relevant
4. System **adds vectors of relevant docs** to query (moves query toward relevant documents)
5. System **subtracts vectors of non-relevant docs** (moves away from irrelevant documents)
6. Re-execute search with $\vec{Q}_{\text{new}}$

---

## **Summary: Choosing the Right Similarity Measure**

| Use Case | Recommended Measure | Rationale |
|----------|---------------------|-----------|
| **General web search** | Cosine similarity | Handles variable document lengths, efficient |
| **Duplicate detection** | Jaccard coefficient | Sensitive to exact overlap, ignores order |
| **Fixed-length documents** | Inner product | Simple, no normalization needed |
| **Clustering documents** | Cosine or Jaccard | Metric properties enable efficient algorithms |
| **Image retrieval** | Dice coefficient | Historical prevalence, tolerant of noise |
| **Short text (tweets)** | Jaccard | Sparse vectors, exact match important |
| **Long documents** | Cosine or BM25 | Length normalization critical |

---

## **Key Takeaways**

1. **Vector representation** transforms text comparison into geometric computation
2. **Inner product** measures raw overlap but suffers from length bias
3. **Cosine similarity** normalizes by length, measuring directional alignment (most popular in IR)
4. **Jaccard coefficient** explicitly penalizes non-matching terms, good for set comparison
5. **Dice coefficient** similar to Jaccard but less strict, violates metric properties
6. **No single measure is universally best**—choice depends on document characteristics and retrieval goals
7. **Similarity enables ranking, thresholding, and feedback**—the core operations of search systems