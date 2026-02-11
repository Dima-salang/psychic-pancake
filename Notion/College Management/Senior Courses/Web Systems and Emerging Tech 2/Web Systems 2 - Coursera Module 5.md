  I'll create a comprehensive lecture on inverted indexes, covering their structure, construction, query processing, and optimization techniques.

---

# **Inverted Indexes: The Engine of Efficient Information Retrieval**

## **Introduction: The Problem with Naive Search**

### **The Linear Scan Approach**

Without indexing, searching requires examining every document:

```
Algorithm: Naive Document Retrieval
─────────────────────────────────────
For each document D in collection (1 to N):
    For each query term q in Query:
        If q exists in D:
            Calculate partial similarity(q, D)
    Sum all partial similarities
    Store final score for D
    
Sort all N documents by score
Return top-K results
```

**Complexity:** $O(N \times |Q| \times |D|)$ where:
- $N$ = number of documents (potentially billions)
- $|Q|$ = number of query terms (typically 2-5)
- $|D|$ = average document length

**For web scale:** $10^9 \times 3 \times 1000 = 3 \times 10^{12}$ operations per query!

**The Fundamental Insight:** Most documents don't match most queries. We need to **skip** non-matching documents efficiently.

---

## **Part 1: Inverted Index Structure**

### **Core Concept: From Documents to Terms**

**Forward Index (Document-Centric):**
```
Document 1 → [computer, database, science, system]
Document 2 → [human, novel, paper, science]
Document 3 → [computer, human, system]
```

**Inverted Index (Term-Centric):**
```
computer → [Doc1, Doc3]
database → [Doc1]
human → [Doc2, Doc3]
novel → [Doc2]
paper → [Doc2]
science → [Doc1, Doc2]
system → [Doc1, Doc3]
```

**Key Advantage:** To find documents containing "computer," we jump directly to the posting list instead of scanning all documents.

---

### **Complete Inverted Index Components**

A production inverted index contains:

| Component | Description | Example |
|-----------|-------------|---------|
| **Vocabulary (Dictionary)** | All unique terms in collection | {computer, database, human, ...} |
| **Document Frequency (DF)** | Number of docs containing term | computer: DF=2 |
| **Postings List** | Documents containing term | computer → [Doc1, Doc3] |
| **Term Frequency (TF)** | Count per document | computer → [(Doc1, 2), (Doc3, 1)] |
| **Positions (optional)** | Word locations for phrase search | computer → [(Doc1, [23, 97, 104])] |

---

### **Visual Index Structure**

```
┌─────────────────────────────────────────────────────────┐
│                    INVERTED INDEX                        │
├─────────────────┬──────────┬────────────────────────────┤
│   TERM          │    DF    │      POSTINGS LIST         │
├─────────────────┼──────────┼────────────────────────────┤
│ computer        │    3     │ Doc7:4, Doc12:2, Doc45:1   │
│ database        │    2     │ Doc1:3, Doc8:1             │
│ science         │    5     │ Doc1:2, Doc2:1, Doc9:3...  │
│ system          │    4     │ Doc1:1, Doc3:2, Doc7:1...  │
│ ...             │   ...    │ ...                        │
└─────────────────┴──────────┴────────────────────────────┘
                    ↑
              Doc7:4 means:
              • Document ID: 7
              • Term frequency: 4 (appears 4 times)
              
Optional position data:
              Doc7:4:[23, 45, 67, 89] (positions in doc)
```

---

## **Part 2: Query Processing with Inverted Index**

### **The Query Execution Algorithm**

**Given Query:** $Q = \{q_1, q_2, q_3\}$ (e.g., "database text information")

**Step 1: Lookup Each Query Term**

```
Lookup q₁="database" → Postings: [Doc5:3, Doc6:1, Doc8:2]
Lookup q₂="text"     → Postings: [Doc1:2]  
Lookup q₃="information" → Postings: [Doc1:4, Doc5:2]
```

**Step 2: Calculate Partial Scores**

For each document in each posting list, compute similarity between query term and document:

| Query Term | Document | TF | IDF | Partial Score |
|------------|----------|-----|-----|---------------|
| database | Doc5 | 3 | 2.5 | 7.5 |
| database | Doc6 | 1 | 2.5 | 2.5 |
| database | Doc8 | 2 | 2.5 | 5.0 |
| text | Doc1 | 2 | 3.0 | 6.0 |
| information | Doc1 | 4 | 2.0 | 8.0 |
| information | Doc5 | 2 | 2.0 | 4.0 |

**Step 3: Accumulate Scores (Score Aggregation)**

```
Initialize empty score accumulator

For each query term q in Q:
    Retrieve postings list for q
    For each posting (DocID, tf) in list:
        partial_score = Similarity(q, DocID)  # Using TF-IDF, BM25, etc.
        accumulator[DocID] += partial_score    # Sum partial scores
```

**Accumulation Results:**

| Document | database | text | information | **Total Score** |
|----------|----------|------|-------------|-----------------|
| Doc1 | - | 6.0 | 8.0 | **14.0** |
| Doc5 | 7.5 | - | 4.0 | **11.5** |
| Doc6 | 2.5 | - | - | **2.5** |
| Doc8 | 5.0 | - | - | **5.0** |

**Step 4: Rank and Return**

Sort by total score: Doc1 (14.0) > Doc5 (11.5) > Doc8 (5.0) > Doc6 (2.5)

---

### **Available Statistics for Scoring**

The inverted index provides efficient access to:

| Statistic | Symbol | Source | Use |
|-----------|--------|--------|-----|
| Term Frequency | TF | Postings list | Local term importance |
| Document Frequency | DF | Index header | Global term rarity (IDF) |
| Collection Size | N | Global metadata | IDF calculation |
| Term Positions | - | Extended postings | Phrase matching, proximity |
| Document Length | $|d|$ | Forward index | Length normalization (BM25) |
| Max Term Frequency | TF_MAX | Forward index | TF normalization |

**Why not store IDF directly?**
- IDF = $\log(N/\text{DF})$ depends on $N$ (total documents)
- $N$ changes when documents are added/deleted
- DF changes when documents are updated
- **Recomputing IDF for all terms on every update is expensive**
- **Solution:** Store DF and N, compute IDF on-the-fly

---

## **Part 3: Index Construction**

### **The Build Process**

**Phase 1: Document Parsing and Tokenization**

```
Raw Document → Tokenization → Stop word removal → Stemming → Terms
```

Example:
```
Document: "Human computer interaction is important in computer science"
↓
Terms: [human, computer, interact, import, computer, scienc]
```

**Phase 2: Sorting and Aggregation**

```
Document 1: [human, novel, novel, paper]
Document 2: [human, paper, science]
Document 3: [computer, human, system]

↓ Extract all (term, docID, position) tuples

(human, 1, 1), (novel, 1, 2), (novel, 1, 3), (paper, 1, 4),
(human, 2, 1), (paper, 2, 2), (science, 2, 3),
(computer, 3, 1), (human, 3, 2), (system, 3, 3)

↓ Sort by term, then docID

(computer, 3, 1),
(human, 1, 1), (human, 2, 1), (human, 3, 2),
(novel, 1, 2), (novel, 1, 3),
(paper, 1, 4), (paper, 2, 2),
(science, 2, 3),
(system, 3, 3)

↓ Aggregate into postings

computer → [Doc3:1:[1]]
human → [Doc1:1:[1], Doc2:1:[1], Doc3:1:[2]]
novel → [Doc1:2:[2,3]]
paper → [Doc1:1:[4], Doc2:1:[2]]
science → [Doc2:1:[3]]
system → [Doc3:1:[3]]
```

---

### **Integer ID Mapping**

For efficiency, strings are mapped to integers:

| String Term | Integer ID |
|-------------|-----------|
| computer | 145324 |
| database | 89201 |
| human | 56002 |
| science | 12045 |

**Benefits:**
- Fixed-size storage (32-bit integers vs. variable-length strings)
- Faster comparison (integer vs. string)
- Smaller index size

**Implementation:**
- **Vocabulary file:** Term → ID mapping (stored as B-tree or hash table)
- **Postings file:** Integer IDs only

---

## **Part 4: Boolean Query Processing**

### **Set Operations on Postings Lists**

| Boolean Operator | Set Operation | Algorithm |
|-----------------|---------------|-----------|
| **AND** | Intersection | Merge two sorted lists, keep common elements |
| **OR** | Union | Merge two sorted lists, keep all unique elements |
| **AND NOT** | Difference | Remove elements of second list from first |

---

### **AND Operation (Intersection)**

**Query:** "computer AND database"

```
computer → [Doc1, Doc3, Doc7, Doc12]
database → [Doc1, Doc7, Doc15]

Intersection: [Doc1, Doc7]  (common to both lists)
```

**Algorithm (Linear Merge):**
```
Initialize pointers i=0, j=0
While i < len(list1) and j < len(list2):
    If list1[i] == list2[j]:
        Add to result
        i++, j++
    Else if list1[i] < list2[j]:
        i++
    Else:
        j++
```

**Complexity:** $O(|L_1| + |L_2|)$ vs. $O(|L_1| \times |L_2|)$ for naive nested loop

---

### **OR Operation (Union)**

**Query:** "computer OR database"

```
computer → [Doc1, Doc3, Doc7]
database → [Doc1, Doc7, Doc15]

Union: [Doc1, Doc3, Doc7, Doc15]  (all unique docs)
```

---

### **AND NOT Operation (Difference)**

**Query:** "computer AND NOT database"

```
computer → [Doc1, Doc3, Doc7, Doc12]
database → [Doc1, Doc7, Doc15]

Difference: [Doc3, Doc12]  (in computer but not in database)
```

---

### **Complex Boolean Queries**

**Query:** "(computer OR database) AND science AND NOT human"

**Execution Plan:**
```
Step 1: computer OR database
        → [Doc1, Doc3, Doc7] ∪ [Doc1, Doc8]
        = [Doc1, Doc3, Doc7, Doc8]
        
Step 2: (Result) AND science
        → [Doc1, Doc3, Doc7, Doc8] ∩ [Doc2, Doc3, Doc9]
        = [Doc3]
        
Step 3: (Result) AND NOT human
        → [Doc3] - [Doc2, Doc3, Doc5]
        = []  (empty set)
```

---

## **Part 5: Query Optimization**

### **The Optimization Principle**

**Goal:** Minimize intermediate result sizes to reduce computation.

**Key Insight:** Process **most selective** terms first (smallest DF).

---

### **Optimization Example**

**Query:** "Hong Kong AND Dik Lee AND HKUST"

| Term | Document Frequency (DF) | Selectivity |
|------|------------------------|-------------|
| Dik Lee | 20 | **High** (rare) |
| HKUST | 1,000 | Medium |
| Hong Kong | 500,000 | **Low** (common) |

**Naive Order:** Hong Kong → HKUST → Dik Lee
- Start with 500,000 documents
- Filter to ~1,000 (assuming independence)
- Filter to ~20
- **Operations:** 500,000 + 1,000 + 20 ≈ 501,020

**Optimized Order:** Dik Lee → HKUST → Hong Kong
- Start with 20 documents
- Check which contain HKUST (~10?)
- Check which contain Hong Kong (~5?)
- **Operations:** 20 + 10 + 5 ≈ 35

**Speedup:** ~14,000× faster!

---

### **General Optimization Strategies**

| Strategy | Application |
|----------|-------------|
| **Process most restrictive terms first** | Conjunctive queries (AND) |
| **Lazy evaluation** | Defer expensive operations |
| **Skip pointers** | Jump ahead in long postings lists |
| **Caching** | Store frequent query results |
| **Bloom filters** | Fast negative checks before full intersection |

---

## **Part 6: Advanced Index Features**

### **Position Information for Phrase Search**

**Standard Index (no positions):**
```
computer → [Doc1, Doc7]
graphics → [Doc1, Doc7]
```

**Query:** "computer graphics" (phrase)

**Problem:** Both terms appear in Doc1 and Doc7, but are they adjacent?

**Solution: Positional Index**
```
computer → [Doc1:[23, 97, 104], Doc7:[15, 89]]
graphics → [Doc1:[24, 105], Doc7:[16, 90]]
```

**Phrase Matching Algorithm:**
```
For each document in intersection:
    For each position p of term1:
        If position (p+1) exists in term2's position list:
            Match found!
```

**Doc1 Analysis:**
- computer at 23, graphics at 24 → **Adjacent!** ✓
- computer at 97, graphics at 105 → Not adjacent
- computer at 104, graphics at 105 → **Adjacent!** ✓

---

### **Wildcard and Prefix Search**

**Problem:** Query "databas*" should match "database", "databases", "databasing"

**Solution: B-tree or Trie Structure**

```
Index Structure (B-tree):
                    [data]
                   /      \
            [computer]    [database]
                           /      \
                     [databased]  [databases]
                     
Range query: "databas" ≤ term < "databat"
Returns: database, databased, databases
```

---

### **Index Storage Structures**

| Structure | Best For | Characteristics |
|-----------|----------|-----------------|
| **Hash Table** | Exact term lookup | O(1) access, no range queries |
| **B-Tree/B+ Tree** | Range queries, prefix search | O(log n) access, sorted order |
| **Trie** | Prefix matching, autocomplete | Space-efficient for shared prefixes |
| **FST (Finite State Transducer)** | Compact storage, fast lookup | Used in Lucene, memory efficient |

---

## **Part 7: Forward Index and Document Deletion**

### **The Forward Index**

While inverted index maps term → documents, **forward index** maps document → terms:

```
Forward Index:
Doc1 → [computer:2, database:1, science:1, system:1]
Doc2 → [human:1, novel:2, paper:1]
Doc3 → [computer:1, human:1, system:1]
```

**Purpose:**
- Store document metadata (length, max TF, etc.)
- Enable document updates and deletions
- Support result snippet generation

---

### **Handling Document Deletion**

**Challenge:** Removing Doc2 requires updating all terms in Doc2's posting lists.

**Without Forward Index:**
- Scan all posting lists to find and remove Doc2 references
- $O(V)$ where $V$ = vocabulary size (expensive!)

**With Forward Index:**
```
1. Look up Doc2 in forward index → [human, novel, paper]
2. For each term in Doc2's term list:
   - Remove Doc2 from term's posting list in inverted index
3. Remove Doc2 from forward index
4. Update DF for affected terms
```

**Complexity:** $O(|D|)$ where $|D|$ = terms in deleted document (much smaller than $V$)

---

## **Summary: Inverted Index Architecture**

```
┌─────────────────────────────────────────────────────────────┐
│                     QUERY PROCESSING                         │
│  1. Parse query → extract terms                             │
│  2. Lookup term IDs in vocabulary (B-tree/Hash)             │
│  3. Retrieve postings lists from inverted index             │
│  4. Apply Boolean operations (AND/OR/NOT)                   │
│  5. Calculate similarity scores (TF-IDF/BM25)               │
│  6. Rank and return top-K results                           │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    INVERTED INDEX                            │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│  │  Vocabulary │───→│  Postings   │───→│  Positions  │     │
│  │  (B-Tree)   │    │  Lists      │    │  (Optional) │     │
│  └─────────────┘    └─────────────┘    └─────────────┘     │
│        ↑                                                    │
│        └────────────────────────────────────────┐           │
└─────────────────────────────────────────────────┼───────────┘
                                                  │
                        ┌─────────────────────────┘
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    FORWARD INDEX                             │
│  Document ID → [Term:TF, Term:TF, ...] + Metadata           │
│  (Used for updates, deletions, snippets)                    │
└─────────────────────────────────────────────────────────────┘
```

---

## **Key Takeaways**

1. **Inverted index inverts the document-term relationship** for efficient lookup
2. **Postings lists** contain document IDs, term frequencies, and optionally positions
3. **Query processing** retrieves posting lists, computes partial scores, and aggregates
4. **Boolean operations** are implemented as set intersections, unions, and differences
5. **Query optimization** processes most selective terms first to minimize intermediate results
6. **Position indexes** enable phrase and proximity search
7. **Forward indexes** support document maintenance and metadata access
8. **Integer ID mapping** reduces storage and improves performance


 I'll create a comprehensive lecture on the trade-offs, advanced features, and practical considerations of inverted indexes in information retrieval systems.

---

# **Inverted Indexes: Trade-offs, Advanced Features, and Practical Considerations**

## **Part 1: The Trade-off Spectrum**

### **Advantages vs. Disadvantages Summary**

| Aspect | Advantage | Disadvantage |
|--------|-----------|--------------|
| **Query Speed** | Retrieve matching documents instantly | — |
| **Flexibility** | Store various information types (DF, TF, positions) | — |
| **Complex Operations** | Support phrase search, proximity, structured retrieval | — |
| **Storage** | — | Large overhead (30-100%+ of text size) |
| **Maintenance** | — | High update/delete cost |
| **Query Complexity** | — | Processing cost increases with Boolean operators |

---

## **Part 2: Storage Overhead Deep Dive**

### **Why Storage Overhead is Inevitable**

**The Fundamental Problem:**

Consider a web-scale search engine:
- **Documents:** 10 billion web pages
- **Average page size:** 10 KB
- **Total text size:** 100 TB
- **Unique terms:** ~100 million
- **Average posting list length:** 100-1000 documents per term

**Storage Components:**

| Component | Storage Required | Reason |
|-----------|------------------|--------|
| Document IDs | 4-8 bytes × postings | Billions of references |
| Term frequencies | 1-4 bytes × postings | Frequency counts |
| Positions | 2-4 bytes × occurrences | Word location tracking |
| Vocabulary | 100M terms × overhead | Dictionary structure |

**Total Index Size:** Often **30-100%** of original text size (sometimes higher with positions)

---

### **The 2-3% Claim Explained**

Some systems claim only **2-3% storage overhead**—this is achieved through:

**Sharding Strategy:**
```
Total Corpus: 100 billion documents
                 ↓
    ┌───────────┼───────────┐
    ↓           ↓           ↓
 Server 1   Server 2    Server N
  2% docs    2% docs     2% docs
 (2B docs)  (2B docs)   (2B docs)
    ↓           ↓           ↓
  Index 1    Index 2     Index N
  (small)    (small)     (small)
```

**Per-server perspective:** Only indexing 2% of total corpus → small index
**System-wide reality:** Still 100% indexed, just distributed

**This is distributed indexing, not reduced indexing!**

---

### **Storage Overhead Formula**

$$\text{Storage Overhead (\%)} = \frac{\text{Size of Inverted Index}}{\text{Size of Original Text}} \times 100\%$$

**Typical Values:**

| Index Type | Overhead | Use Case |
|------------|----------|----------|
| Document IDs only | 20-30% | Basic Boolean retrieval |
| + Term frequencies | 30-50% | TF-IDF scoring |
| + Word positions | 100-200% | Phrase/proximity search |
| + Sentence/paragraph IDs | 150-300% | Structural search |

---

## **Part 3: Update and Maintenance Costs**

### **The Update Problem**

**Why Updates Are Expensive:**

```
New Document Arrives: "Database systems and architecture"
↓
1. Extract terms: [database, system, architecture]
2. For EACH term:
   - Update vocabulary (if new term)
   - Append to posting list (resize if needed)
   - Update document frequency (DF)
3. Update forward index
4. Update global statistics (N, avgdl, etc.)
5. Potentially rebuild parts of index structure (B-tree, etc.)
```

**Cost Analysis:**

| Operation | Time Complexity | Why Expensive |
|-----------|----------------|---------------|
| Insert new document | $O(V_d \times \log V)$ | $V_d$ = terms in doc, update posting lists |
| Delete document | $O(V_d \times P)$ | Find and remove from $V_d$ posting lists |
| Modify document | Delete + Insert | Essentially rebuild document entry |
| Update IDF | $O(V)$ | Must touch all terms |

**Where:**
- $V$ = vocabulary size (100M+ terms)
- $V_d$ = terms in specific document (100-1000)
- $P$ = average posting list length

---

### **Web Search: The Continuous Update Challenge**

**The Web Dynamics:**
- **New pages:** Millions added daily
- **Updated pages:** Billions change content regularly
- **Deleted pages:** Link rot, 404 errors

**Strategies for Dynamic Collections:**

| Strategy | Mechanism | Trade-off |
|----------|-----------|-----------|
| **Batch rebuilding** | Rebuild index periodically | Stale results between rebuilds |
| **Incremental updates** | Add new documents to separate index, merge later | Query must search multiple indexes |
| **Log-structured merge** | Buffer updates, merge in background | Write amplification |
| **Partitioned indexes** | New documents → new partition | Query complexity increases |

---

## **Part 4: Advanced Index Structures**

### **Hierarchical Position Information**

Beyond simple word positions, we can store **structural locations**:

**Level 1: Word Position**
```
database → [Doc350: [word_pos: 23, word_pos: 45, word_pos: 89]]
```

**Level 2: Sentence + Word Position**
```
database → [Doc350: [(sent: 5, word: 3), (sent: 12, word: 1)]]
```

**Level 3: Paragraph + Sentence + Word**
```
database → [Doc350: [(para: 8, sent: 12, word: 1), (para: 9, sent: 3, word: 7)]]
```

---

### **Proximity and Phrase Queries**

**Query:** "database" within 3 words of "systems"

**With Position Information:**
```
database positions in DocX: [23, 45, 67, 89]
systems positions in DocX: [25, 46, 90]

Check differences:
|25 - 23| = 2 ≤ 3 ✓ MATCH
|46 - 45| = 1 ≤ 3 ✓ MATCH  
|90 - 89| = 1 ≤ 3 ✓ MATCH
```

**Query:** "database" and "architecture" in same sentence

```
database in Doc350: [(sent: 12, word: 1), (sent: 15, word: 5)]
architecture in Doc350: [(sent: 12, word: 8), (sent: 20, word: 3)]

Match: Both have sent: 12 ✓
```

**Cost:** Each additional constraint requires more position comparisons → slower retrieval

---

## **Part 5: Term Truncation and Query Recommendation**

### **Stemming for Query Expansion**

**The Problem:** Users search for variations of same concept
- "computer", "computing", "computation", "computational"

**Solution: Stemming + Truncated Index**

```
Original terms: computer, computing, computation, computational
Stemmed form: compute

Inverted index structure:
compute → [Doc1, Doc2, Doc3, Doc4, Doc5...]
  └─ original: [computer (Doc1, Doc2), computing (Doc3), 
                computation (Doc4), computational (Doc5)]
```

**Benefits:**
- Reduced vocabulary size
- Conflates related terms automatically
- Enables prefix-based recommendation

---

### **B-Tree Based Query Recommendation**

**Structure:** Store vocabulary in B-Tree (or Trie) for prefix search

```
B-Tree Structure:
                    [r]
                   / | \
                 [re] [ro] [ru]
                 /    |     \
              [rea] [res] [rou]
              /  |    |     |
           [read] [real] [rest] [resource] [round]

User types: "re"
↓
Traverse to [re] node
↓
Collect all leaf descendants:
- read, real, rest, resource, result, retail, etc.
```

**Recommendation Algorithm:**
```
Input: prefix string P
Output: list of suggested completions

1. Navigate B-tree to node corresponding to P
2. Perform DFS/BFS from that node
3. Collect first K leaf terms (most frequent/popular)
4. Return sorted by frequency/relevance
```

**Example Walkthrough:**

| Input | Navigation | Suggestions |
|-------|-----------|-------------|
| "re" | Node [re] | resource, result, retail, return |
| "res" | Node [res] | resource, result, research, reserve |
| "reso" | Node [reso] | resource, resolution, resort |

---

### **The Prefix Limitation**

**Critical Constraint:** Tree traversal requires **prefix knowledge**

**Scenario:** User knows suffix but not prefix
- Looking for "*sultation" (consultation, resultation?)
- Tree structure cannot help—don't know where to start

**Solutions:**

| Approach | Mechanism | Cost |
|----------|-----------|------|
| **Suffix array** | Build separate index on reversed terms | Double storage |
| **N-gram index** | Index all character sequences | High storage |
| **Fuzzy search** | Edit distance matching | High computation |
| **Machine learning** | Predict intended term from context | Training required |

---

## **Part 6: The Cost-Information Trade-off**

### **The Spectrum of Index Enrichment**

```
Minimal Index                    Rich Index
─────────────────────────────────────────────────────
[Doc IDs only]              →    [Doc IDs + TF + Positions]
     ↓                                ↓
  Fast retrieval                   Slower retrieval
  Small storage (20%)              Large storage (200%)
  Simple scoring                   Complex scoring
  No phrase search                 Proximity, phrase, structure
  Easy updates                     Expensive updates
```

**Decision Framework:**

| If you need... | Store... | Accept... |
|----------------|----------|-----------|
| Basic keyword search | Doc IDs only | Limited ranking |
| TF-IDF ranking | + Term frequencies | 30-50% overhead |
| Phrase queries | + Word positions | 100%+ overhead |
| Proximity search | + Positions | Slower processing |
| Structural search | + Sentence/para IDs | 200%+ overhead |
| Real-time updates | Minimal structure | Reduced features |

---

## **Part 7: When Inverted Indexes Work Best**

### **Ideal Use Cases**

| Characteristic | Why It Fits Inverted Indexes |
|----------------|------------------------------|
| **Read-heavy, write-light** | Query speed justifies update cost |
| **Static or batch-updated collections** | Rebuilding is acceptable |
| **Structured queries** | Boolean logic maps to set operations |
| **Exact term matching** | Direct lookup is optimal |
| **Ranking required** | TF/IDF easily computed from postings |

### **Challenging Scenarios**

| Scenario | Problem | Alternative Approach |
|----------|---------|---------------------|
| **Real-time social media** | Continuous updates | Log-structured merge trees |
| **Semantic search** | Synonyms, concepts | Vector embeddings, neural IR |
| **Fuzzy matching** | Typos, variations | Levenshtein automata, phonetic indexing |
| **Very short documents** | High overhead ratio | Forward index, brute force |
| **Multimedia search** | Non-text content | Feature vectors, approximate nearest neighbors |

---

## **Part 8: Modern Optimizations and Variants**

### **Compression Techniques**

**Goal:** Reduce storage without sacrificing speed

| Technique | Method | Savings |
|-----------|--------|---------|
| **Variable-byte encoding** | Shorter codes for small numbers | 50% |
| **Gamma/delta encoding** | Optimal for power-law distributions | 60-70% |
| **PForDelta** | Fast decompression, good compression | 40-50% |
| **Elias-Fano** | Succinct representation | 30-40% |

**Example:** Document IDs [107, 108, 110, 115] stored as gaps [107, 1, 2, 5] → smaller numbers → less bits

---

### **Tiered Indexes**

**Hot/Cold Separation:**
```
Tier 1 (Hot): Frequently accessed terms → Memory resident
Tier 2 (Warm): Common terms → SSD storage  
Tier 3 (Cold): Rare terms → Disk storage
```

**Benefit:** Fast access to common queries, cheap storage for rare terms

---

## **Summary: The Inverted Index Design Space**

```
┌─────────────────────────────────────────────────────────────┐
│                    INVERTED INDEX DESIGN                     │
├─────────────────────────────────────────────────────────────┤
│  STORAGE ←──────→ SPEED ←──────→ FUNCTIONALITY              │
│    ↑               ↑                ↑                        │
│  More info      More processing   More features              │
│  = Larger       = Slower queries  = Better results           │
│  = Slower       = Higher latency  = Complex ranking          │
│    updates                                               │
├─────────────────────────────────────────────────────────────┤
│  KEY DECISIONS:                                              │
│  • What to store? (IDs, TF, positions, structure?)          │
│  • How to compress? (speed vs. space trade-off)             │
│  • How to update? (batch vs. incremental)                   │
│  • How to distribute? (sharding, replication)               │
│  • What index structures? (B-tree, hash, trie)              │
└─────────────────────────────────────────────────────────────┘
```

---

## **Key Takeaways**

1. **Storage overhead is inherent**—inverted indexes typically require 30-100%+ of original text size
2. **Update costs are high**—every document change requires modifying multiple posting lists
3. **Flexibility comes at a price**—storing positions enables phrase/proximity search but doubles storage
4. **Query recommendation requires prefix knowledge**—B-trees/Tries cannot suggest without knowing start of term
5. **Inverted indexes excel in static, read-heavy environments**—web search, digital libraries, archives
6. **The cost-information trade-off is fundamental**—more sophisticated retrieval requires more storage and processing
7. **Modern systems use compression and tiering**—to manage the storage-performance balance



 I'll create a comprehensive lecture on insertion, deletion, and maintenance strategies for inverted indexes, covering algorithms, trade-offs, and practical implementation considerations.

---

# **Inverted Index Maintenance: Insertion, Deletion, and Batch Processing**

## **Part 1: Insertion into Inverted Indexes**

### **The Basic Insertion Problem**

When adding a new document with $n$ unique terms to an existing inverted index, we must update $n$ separate posting lists.

**Scenario:**
- New document: Doc 349
- Terms: ["database", "system", "architecture", "retrieval"]
- Existing index has sorted posting lists

---

### **Case 1: Unsorted Posting Lists**

**Algorithm:**
```
For each term t in new document:
    Append (DocID, TF) to end of t's posting list
```

**Example:**
```
Before insertion:
database → [Doc345, Doc348]

After inserting Doc349:
database → [Doc345, Doc348, Doc349]  // Simply appended
```

**Advantages:**
- $O(1)$ append operation per term
- No data movement required
- Fast insertion

**Disadvantages:**
- Retrieval requires sorting or scanning entire list
- Merges during query processing are slower
- Not suitable for large-scale production systems

---

### **Case 2: Sorted Posting Lists**

**The Challenge:** Maintain sorted order when inserting arbitrary DocID

**Example:**
```
Before insertion:
database → [Doc345, Doc348, Doc350]  // Sorted

Insert: Doc349 (belongs between Doc348 and Doc350)
```

**Algorithm:**
```
For each term t in new document:
    1. Binary search posting list to find insertion point
    2. If space available: shift elements right, insert
    3. If no space: allocate new block, split/merge lists
```

**Visual Representation:**

```
Initial state:
[Doc345][Doc348][Doc350][empty][empty]
   0      1      2      3      4
   
Insert Doc349 at position 2:
[Doc345][Doc348][    ][Doc350][empty]
              ↑
         Shift Doc350 right
         
Final state:
[Doc345][Doc348][Doc349][Doc350][empty]
   0      1       2       3      4
```

**Cost Analysis:**

| Operation | Time Complexity | Disk I/O |
|-----------|----------------|----------|
| Find insertion point | $O(\log P)$ | 1-2 reads |
| Shift elements | $O(P)$ | Multiple writes |
| Update pointers | $O(1)$ | 1 write |

Where $P$ = posting list length

**Problem:** Shifting elements in large lists is expensive!

---

## **Part 2: B-Tree Based Vocabulary Management**

### **B-Tree Structure for Terms**

For indexes with 200,000+ terms, B-trees provide efficient lookup and insertion.

**B-Tree Properties:**
- Fan-out ($f$) = 10 (example)
- Each node holds $f-1$ to $2f-1$ keys
- Tree height = $O(\log_f V)$ where $V$ = vocabulary size

**Example Calculation:**
- $V$ = 200,000 terms
- $f$ = 10
- Height = $\lceil \log_{10}(200,000) \rceil \approx 6$ levels

**Operations:**
| Operation | Complexity | Disk Accesses |
|-----------|-----------|---------------|
| Lookup term | $O(\log_f V)$ | ~6 reads |
| Insert new term | $O(\log_f V)$ | ~6 reads + writes |

---

### **Posting List Storage in Pages**

**Page-Based Organization:**
- Page size: 512 bytes (typical UNIX)
- Posting entry size: 8 bytes (DocID: 4 bytes + TF: 4 bytes)
- Entries per page: $512 / 8 = 64$

**Large Posting List Example:**
- Term "database" appears in 6,400 documents
- Pages required: $6,400 / 64 = 100$ pages

**Storage Structure:**
```
database → [Page 1] → [Page 2] → ... → [Page 100]
            ↓           ↓              ↓
         [Doc1-64]   [Doc65-128]    [Doc6337-6400]
```

---

## **Part 3: Deletion Strategies**

### **The Deletion Problem**

When deleting a document, we must remove its ID from all term posting lists where it appears.

**Example:**
```
Delete: Document 1

Inverted Index:
computer    → [Doc1, Doc3, Doc7, Doc12]
database    → [Doc1, Doc8, Doc15]
retrieval   → [Doc1, Doc2, Doc9]

Must remove Doc1 from 3 posting lists
```

**Naive Approach:**
```
For each term t in deleted document:
    Load posting list for t
    Find and remove Doc1 from list
    Compact list (shift elements left)
    Write updated list back to disk
```

**Cost:** $O(V_d \times P)$ where $V_d$ = terms in document, $P$ = average posting list size

---

### **Optimized Deletion: The Forward Index**

**Forward Index Structure:**
```
DocID → [term1, term2, term3, ...]

Example:
Doc1 → [computer, database, retrieval]
Doc2 → [retrieval, system, network]
Doc3 → [computer, algorithm, data]
```

**Deletion Algorithm with Forward Index:**
```
1. Look up deleted DocID in forward index
   → Get list of terms: [computer, database, retrieval]
   
2. For each term in list:
   - Load posting list
   - Remove DocID
   - Write back
   
3. Remove DocID from forward index
```

**Advantage:** Avoids scanning entire vocabulary to find affected posting lists

---

### **Lazy Deletion: The Tombstone Approach**

**Problem:** Immediate deletion is too expensive for large documents

**Solution:** Mark documents as deleted, clean up later

**Implementation:**
```
1. Maintain "deleted documents" table:
   DeletedDocs = {Doc1, Doc45, Doc892, ...}

2. During query processing:
   Retrieve candidate documents from inverted index
   Filter out DocIDs in DeletedDocs table
   Return only active documents

3. Periodic reorganization:
   Rebuild index excluding deleted documents
   Clear DeletedDocs table
```

**Trade-offs:**

| Aspect | Immediate Deletion | Lazy Deletion |
|--------|-------------------|---------------|
| Query accuracy | Always correct | May return deleted docs if filter fails |
| Space usage | Immediate reclaim | Wasted space until rebuild |
| Update speed | Slow | Fast (just add to table) |
| Complexity | Simple | Requires filtering logic |

---

## **Part 4: Batch Processing and Index Merging**

### **The Batch Insertion Strategy**

**Motivation:** Avoid per-document update costs

**Architecture:**
```
New Documents          Master Index (Large)
     ↓                      ↑
[Doc A, Doc B, ...]    [Billions of docs]
     ↓                      ↑
   Batch Index (Small) ─────┘
   [Thousands of docs]
```

**Process:**
1. **Accumulation:** Collect new documents in buffer
2. **Batch Build:** Create separate inverted index for new documents
3. **Query Time:** Search both indexes, merge results
4. **Merge:** When batch reaches threshold, merge into master index

---

### **Index Merging Algorithm**

**Simple Merge (similar to merge sort):**

```
Master Index:     database → [Doc10, Doc50, Doc100]
Batch Index:      database → [Doc5, Doc75]

Merged Result:    database → [Doc5, Doc10, Doc50, Doc75, Doc100]
                  (interleave sorted lists)
```

**Complexity:** $O(M + B)$ where $M$ = master size, $B$ = batch size

**Disk-Based Merge:**
```
While both lists have entries:
    Read page from master index
    Read page from batch index
    Merge in memory
    Write merged page to new file
    
Replace old master with new merged file
```

---

### **Log-Structured Merge (LSM) Trees**

**Used in:** Elasticsearch, Solr, modern search engines

**Structure:**
```
Level 0 (Memory):    MemTable (active writes)
                     ↓ (flush when full)
Level 1 (Disk):      Small sorted files (SSTables)
                     ↓ (compaction)
Level 2 (Disk):      Larger sorted files
                     ↓ (compaction)
Level N (Disk):      Very large files
```

**Operations:**

| Operation | Process |
|-----------|---------|
| **Insert** | Write to in-memory MemTable |
| **Query** | Check MemTable + all disk levels, merge results |
| **Compaction** | Merge small files into larger files periodically |

**Advantage:** Converts random writes to sequential writes (disk-friendly)

---

## **Part 5: Complete Search Engine Index Architecture**

### **Index Ecosystem**

A production search engine maintains multiple specialized indexes:

| Index Type | Purpose | Content |
|-----------|---------|---------|
| **Mapping Index** | Term/Doc ID translation | Word → ID, ID → Word |
| **Inverted Index** | Core retrieval | Term → Documents (with TF, positions) |
| **Forward Index** | Update support, snippets | Doc → Terms |
| **Page Properties** | Result display | DocID → URL, title, date, size |
| **Link Index** | PageRank, navigation | Parent/child/sibling relationships |

---

### **Update Coordination Challenge**

**Problem:** Updating all indexes atomically is expensive

**Scenario:** New document arrives
```
1. Mapping Index: Add new terms, get IDs
2. Inverted Index: Update posting lists
3. Forward Index: Add Doc → Terms mapping
4. Page Properties: Add metadata
5. Link Index: Add link relationships
```

**Consistency Options:**

| Strategy | Mechanism | Trade-off |
|----------|-----------|-----------|
| **Atomic update** | Transaction across all indexes | Slow, complex |
| **Eventual consistency** | Update indexes asynchronously | Temporary inconsistency |
| **Versioning** | Keep multiple index versions | Space overhead |

---

## **Part 6: Memory Optimization Strategies**

### **The Memory-Disk Hierarchy**

```
Speed:    Fast ←────────────────────────→ Slow
          L1 Cache  L2 Cache  RAM    SSD    HDD
Size:     64KB      256KB     32GB   1TB    10TB
Cost:     $$$$$     $$$$      $$$    $$     $

Strategy: Keep hot data in RAM, cold data on disk
```

---

### **Caching Strategies**

| Cache Type | Content | Benefit |
|-----------|---------|---------|
| **Term cache** | Frequently queried terms' posting lists | Avoid disk reads for hot terms |
| **Document cache** | Recently accessed documents | Fast snippet generation |
| **Query cache** | Results of common queries | Eliminate repeated computation |
| **Filter cache** | Deleted documents, access control lists | Fast filtering |

---

### **Tiered Storage**

```
Hot Terms (Top 1000) → RAM
Warm Terms (Next 10K) → SSD  
Cold Terms (Rest) → HDD

Query "the" → RAM (instant)
Query "retrieval" → SSD (fast)
Query "obscure_term_39284" → HDD (slower)
```

---

## **Part 7: Practical Trade-offs Summary**

### **Insertion Strategy Decision Matrix**

| Scenario | Recommended Approach | Rationale |
|----------|---------------------|-----------|
| Small index, rare updates | Immediate sorted insertion | Simplicity, always consistent |
| Large index, batch updates | Batch index + periodic merge | Amortize merge cost |
| Real-time requirements | LSM tree or lazy deletion | Fast writes, acceptable read cost |
| Read-heavy, write-light | Rebuild from scratch | Optimize for queries |
| Distributed system | Partition by document ID | Parallel processing |

---

### **Cost Summary Table**

| Operation | Cost Driver | Optimization |
|-----------|-------------|--------------|
| **Insertion** | Posting list updates | Batch processing, unsorted buffers |
| **Deletion** | Multiple posting list updates | Forward index, lazy deletion |
| **Query** | Disk I/O for posting lists | Caching, tiered storage |
| **Merge** | Sequential disk reads/writes | LSM trees, background compaction |
| **Vocabulary lookup** | B-tree height | High fan-out, caching |

---

## **Key Takeaways**

1. **Sorted posting lists enable fast retrieval but expensive insertion**—unsorted lists trade query speed for update speed

2. **B-trees provide logarithmic vocabulary lookup**—essential for large-scale indexes with 100K+ terms

3. **Forward indexes enable efficient deletion**—avoid scanning entire vocabulary to find affected posting lists

4. **Lazy deletion with tombstone tables** trades space for speed—periodic cleanup maintains efficiency

5. **Batch processing amortizes update costs**—accumulate changes and merge periodically rather than immediate updates

6. **Multiple specialized indexes serve different purposes**—mapping, inverted, forward, and property indexes work together

7. **Memory hierarchy exploitation is critical**—cache hot data, tier storage by access frequency

8. **No single approach is optimal**—design depends on read/write ratio, latency requirements, and consistency needs



 I'll create a comprehensive lecture on scaling inverted indexes through partitioning and distributed architectures, addressing the challenges of large-scale search.

---

# **Scaling Inverted Indexes: Partitioning and Distributed Search**

## **Part 1: The Scalability Challenge**

### **The Growth Dynamics of Inverted Indexes**

As document collections grow, inverted indexes face two distinct scaling pressures:

| Growth Factor | Growth Rate | Impact |
|--------------|-------------|---------|
| **Vocabulary size** | Slow (sub-linear) | New unique terms appear occasionally |
| **Posting list length** | Linear | Each new document adds entries to existing lists |

**Visual Representation:**

```
Documents:     1K     10K     100K     1M      10M      100M
              │       │        │        │        │        │
Vocabulary:   │       │        │        │        │        │
  ~10K terms  ├───────┼────────┼────────┼────────┼────────┤
  (slow       │       │        │        │        │        │
   growth)    │       │        │        │        │        │
              │       │        │        │        │        │
Posting List: │       │        │        │        │        │
  Length      █       ██       ███      ████     █████    ██████
  (linear     │       │        │        │        │        │
   growth)    │       │        │        │        │        │
```

**The Bottleneck:** While vocabulary grows slowly (Zipf's law—most documents use common words), posting lists grow linearly with collection size.

---

### **When Single Index Fails**

**Symptoms of an oversized inverted index:**

| Problem | Cause | Consequence |
|---------|-------|-------------|
| Slow query response | Long posting lists require extensive scanning | User waits seconds for results |
| Memory pressure | Index exceeds RAM capacity | Excessive disk I/O, thrashing |
| Update latency | Modifying huge posting lists | Stale index, delayed new content |
| Recovery time | Rebuilding massive index | Hours/days of downtime |

**Thresholds (approximate):**
- **1-10 million documents:** Single server manageable
- **100+ million documents:** Partitioning becomes necessary
- **1+ billion documents:** Distributed architecture mandatory

---

## **Part 2: Document Partitioning Strategy**

### **The Core Idea: Divide and Conquer**

Instead of one massive index, partition the collection:

```
Collection: 30,000 documents
                ↓
    ┌───────────┼───────────┐
    ↓           ↓           ↓
 Partition 1  Partition 2  Partition 3
  (Doc 1-10K) (Doc 10K-20K) (Doc 20K-30K)
      ↓           ↓           ↓
  Server 1    Server 2    Server 3
      ↓           ↓           ↓
 Index 1      Index 2      Index 3
 (10K docs)  (10K docs)   (10K docs)
```

**Each partition:**
- Contains a **subset** of documents
- Has its own **complete inverted index**
- Resides on a **separate server** (or core)
- Handles **1/N** of the query load

---

### **Query Processing in Partitioned Architecture**

**Query Flow:**

```
User Query: "database systems"
              ↓
         Query Router
              ↓
    ┌─────────┼─────────┐
    ↓         ↓         ↓
 Server 1  Server 2  Server 3
 (Index 1) (Index 2) (Index 3)
    ↓         ↓         ↓
  Results   Results   Results
  [D1,D5]   [D12]     [D23,D34]
    ↓         ↓         ↓
    └─────────┴─────────┘
              ↓
       Result Integrator
              ↓
    Merged & Ranked Results
    [D1, D5, D12, D23, D34]
              ↓
         User Interface
```

**Key Steps:**
1. **Broadcast:** Send query to all partition servers
2. **Local search:** Each server searches its local index
3. **Retrieve:** Each server returns top-K local results
4. **Merge:** Combine results from all partitions
5. **Re-rank:** Global ranking based on combined scores

---

### **Partitioning Schemes**

| Strategy | Method | Best For |
|----------|--------|----------|
| **Range partitioning** | DocID 1-10K, 10K-20K, etc. | Uniform document sizes |
| **Hash partitioning** | hash(DocID) % N | Load balancing |
| **Time-based** | Documents by date/crawl time | Temporal queries |
| **Source-based** | By website/domain | Source-specific filtering |
| **Topic-based** | By content category | Vertical search |

---

## **Part 3: Result Merging and Global Ranking**

### **The Challenge: Local vs. Global Statistics**

**Problem:** Each partition has **local** statistics, but ranking requires **global** context.

**Example:**

```
Partition 1 (Docs 1-10,000):
  "database": DF=500, IDF=log(10000/500)=3.0
  
Partition 2 (Docs 10,001-20,000):
  "database": DF=600, IDF=log(10000/600)=2.8

Global Collection (Docs 1-20,000):
  "database": DF=1100, IDF=log(20000/1100)=2.9
```

**Issue:** Document in Partition 1 scored with IDF=3.0, but should use IDF=2.9 for fair comparison.

---

### **Solutions for Global Statistics**

| Approach | Mechanism | Trade-off |
|----------|-----------|-----------|
| **Replicated global stats** | Each partition has copy of global DF/IDF | Stale statistics, extra storage |
| **Centralized statistics** | Query router maintains global stats | Single point of contention |
| **Approximate statistics** | Use sampled/estimated global values | Slightly inaccurate ranking |
| **Two-phase scoring** | Local scoring → fetch global stats → re-score | Additional network round-trip |

---

### **Result Merging Algorithms**

**Approach 1: Simple Merge (K-best from each)**

```
From each partition, take top-K results
Merge into single list
Re-sort by global score
Return top-K overall
```

**Problem:** Best global result might be rank (K+1) in its local partition—missed!

---

**Approach 2: Score Thresholding**

```
Set global threshold T
From each partition, take all results with score ≥ T
If insufficient results, lower T and re-query
Merge and return top-K
```

**Problem:** Requires multiple rounds if thresholds misestimated.

---

**Approach 3: Document-at-a-Time (DAAT) with Global Queue**

```
Initialize max-heap (priority queue) for global top-K
For each partition:
    Stream results in decreasing score order
    Insert into global heap
    If heap size > K, remove lowest
Return heap contents
```

**Advantage:** Guaranteed to find true global top-K.

---

## **Part 4: Practical Distributed Architecture**

### **System Components**

```
┌─────────────────────────────────────────────────────────────┐
│                         CLIENT                              │
│                    (User Query Input)                       │
└───────────────────────┬─────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                    QUERY ROUTER                             │
│         (Parse, broadcast, merge, return)                   │
└───────────────────────┬─────────────────────────────────────┘
                        ↓
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   INDEX      │ │   INDEX      │ │   INDEX      │
│   NODE 1     │ │   NODE 2     │ │   NODE N     │
│  (Partition) │ │  (Partition) │ │  (Partition) │
│              │ │              │ │              │
│ • Local Inv. │ │ • Local Inv. │ │ • Local Inv. │
│   Index      │ │   Index      │ │   Index      │
│ • Local Doc  │ │ • Local Doc  │ │ • Local Doc  │
│   Store      │ │   Store      │ │   Store      │
└──────────────┘ └──────────────┘ └──────────────┘
        ↑               ↑               ↑
        └───────────────┴───────────────┘
                        ↑
┌─────────────────────────────────────────────────────────────┐
│              SHARED STORAGE / COORDINATOR                   │
│         (Global stats, configuration, monitoring)           │
└─────────────────────────────────────────────────────────────┘
```

---

### **Handling Index Updates**

**Scenario:** New documents arrive continuously

**Strategy 1: Static Partitions with Overflow**

```
Initial: 3 partitions, 10K docs each
New docs → Partition 4 (new server)
When Partition 4 fills → Partition 5, etc.
```

**Problem:** Uneven query load (newer partitions queried more?)

---

**Strategy 2: Rebalancing Partitions**

```
When partition grows too large:
1. Split partition into two halves
2. Migrate one half to new server
3. Update routing table
4. Continue operation
```

**Challenge:** Migration requires downtime or complex coordination.

---

**Strategy 3: Dynamic Partitioning (Consistent Hashing)**

```
Map documents to a ring (0 to 2^32-1)
Each server owns a range on the ring
Add server: Split existing range, migrate only affected docs
Remove server: Merge ranges, redistribute docs
```

**Advantage:** Minimal data movement when scaling.

---

## **Part 5: Advanced Scaling Techniques**

### **Replication for Read Scaling**

```
Partition 1:        Partition 2:        Partition 3:
┌─────────┐         ┌─────────┐         ┌─────────┐
│ Primary │         │ Primary │         │ Primary │
│ Index 1 │         │ Index 2 │         │ Index 3 │
└────┬────┘         └────┬────┘         └────┬────┘
     │                   │                   │
┌────┴────┐         ┌────┴────┐         ┌────┴────┐
│Replica  │         │Replica  │         │Replica  │
│Index 1  │         │Index 2  │         │Index 3  │
└─────────┘         └─────────┘         └─────────┘

Query load distributed across replicas
Writes go to primary, propagate to replicas
```

**Benefit:** 3× query throughput with 3 replicas per partition.

---

### **Tiered Index Architecture**

```
Hot Index (Memory):     Recent/Popular documents
                        ↓
Warm Index (SSD):       Moderately accessed documents  
                        ↓
Cold Index (HDD/Cloud): Archive, rarely accessed documents

Query flows: Hot → Warm → Cold (as needed)
```

**Benefit:** Fast response for common queries, cheap storage for rare content.

---

## **Part 6: Summary of Key Concepts**

### **Inverted Index Components (Review)**

| Component | Function | Implementation |
|-----------|----------|----------------|
| **Keyword Index** | Find terms quickly | B-tree, Hash table, Trie |
| **Posting List** | Map term → documents | Sorted array, skip lists |
| **Auxiliary Data** | Positions, frequencies | Inline or separate file |

---

### **Scaling Strategies Comparison**

| Strategy | Query Speed | Update Speed | Complexity | Best For |
|----------|-------------|--------------|------------|----------|
| **Single Index** | Fastest | Slow | Simple | < 10M docs |
| **Static Partition** | Fast | Medium | Medium | 10M-100M docs |
| **Dynamic Partition** | Fast | Fast | High | 100M+ docs |
| **Replicated Partition** | Very Fast | Slow | High | Read-heavy workloads |

---

### **The Fundamental Trade-off**

```
Scalability ←────────────────→ Consistency/Complexity
     ↑                              ↑
  More partitions               Harder to maintain
  Better throughput             Global ranking harder
  Easier updates                More network traffic
```

---

## **Key Takeaways**

1. **Posting lists grow linearly with documents**—the primary scalability bottleneck
2. **Vocabulary grows slowly**—not the limiting factor
3. **Document partitioning** splits collection across servers, each with local index
4. **Query broadcasting** sends queries to all partitions; results are merged
5. **Global ranking requires global statistics**—either replicated, centralized, or approximated
6. **Partitioning enables horizontal scaling**—add servers as collection grows
7. **Replication increases read throughput**—multiple copies serve queries
8. **Consistent hashing minimizes rebalancing**—efficient for dynamic growth