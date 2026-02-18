  *rubs hands together with visible excitement*

Oh, this is delicious! Now we're getting into the real meat of the problem. You've been asking about phrases, n-grams, collocations—this is where information retrieval stops being about pushing symbols around and starts being about **understanding the structure of language itself**. And let me tell you, the gap between "words" and "meaning" is where all the interesting problems live!

## The Fundamental Tension: Precision vs. Flexibility

*walks to the board and draws two boxes*

Let me set up the central conflict for you. On one side, we have **precision**—we want "Hong Kong University" to mean exactly that institution, not "a university in Hong Kong" generically. On the other side, we have **flexibility**—we want to catch "University of Hong Kong" and "HKU" and "the university located in Hong Kong" as well.

Your slides start with this beautiful observation: if we encode "apple juice" as a single atomic term "apple_juice," we solve the precision problem but create a flexibility nightmare. Search for "juice" and you miss it. Search for "how to extract juice from apple" and you miss it. The user has to guess exactly how we tokenized the phrase!

This is the **representation problem** in its purest form. How do we represent meaning so that:
1. "Apple juice" is recognized as a unit (semantic coherence)
2. "Apple" and "juice" are still accessible individually (compositional flexibility)
3. "Juice of apple" and "apple-derived juice" are somehow connected (semantic variation)

The answer, as your slides suggest, is **multi-level indexing**. Index it as "apple_juice" AND "apple" AND "juice". But now you've tripled your index size! And with longer phrases—"Hong Kong University of Science and Technology"—this explodes combinatorially.

## N-grams: The "Good Enough" Engineering Solution

*chuckles*

Now, here comes the beautiful hack. Someone said, "You know what? Finding 'real' phrases with NLP is hard. It's slow, it's error-prone, and linguists can't even agree on what constitutes a phrase. Let's just take **every consecutive sequence of N words** and call it a day!"

This is n-gram indexing. For "hong kong university of science and technology":

- **Unigrams**: hong, kong, university, of, science, and, technology
- **Bigrams**: hong kong, kong university, university of, of science, science and, and technology  
- **Trigrams**: hong kong university, kong university of, university of science, science and technology

*counts on fingers*

For a text of length L words, you get L unigrams, L-1 bigrams, L-2 trigrams... The index grows fast! But here's the genius: **you don't need to be perfect, you just need to be useful**.

Look at your example with the query "hong kong university":
- If you indexed only trigrams, you match exactly "hong kong university" in one lookup. Fast! But you miss "university of hong kong."
- If you indexed bigrams, you match "hong kong" + "kong university." This catches "hong kong polytechnic university" and "university of hong kong" too—higher recall, but now you've matched documents that might not be about HKU specifically!

This is the **precision-recall tradeoff** manifesting in the indexing structure itself. Bigrams and trigrams are the sweet spot for English because:
- Unigrams lose word order (bag-of-words)
- 4-grams+ are too sparse (how many times does a specific 4-word sequence repeat?)
- Bigrams/trigrams capture most meaningful phrases while staying frequent enough to be useful

## The Stop Word Problem: Semantic vs. Syntactic Glue

*leans on the podium*

Now, here's where it gets subtle. Look at these bigrams from your slides:
- "of the" (frequency: 80,871)
- "in the" (frequency: 58,841)
- "to the" (frequency: 26,430)

These are **syntactic glue**. They carry almost no semantic content. But "New York" (11,428) and "Saudi Arabia" (3,191)—these are **semantic units**!

The challenge is telling them apart automatically. Your slides suggest **stop word filtering**: throw away any n-gram containing "the," "of," "in," etc.

But wait! *raises finger* What about "University of Science"? The "of" seems like glue, but "University of Science" is a proper name! Or "Man of Steel"? "Beauty and the Beast"?

This is the **stop word paradox**. Words like "of" and "the" are usually noise, but sometimes they're load-bearing elements of proper nouns. Throw them out blindly and you destroy meaning. Keep them and you drown in garbage n-grams.

## Grammatical Filtering: Bringing Linguistics Back In

*draws part-of-speech tags on the board*

So we try a different filter. Instead of looking at which words are in the n-gram, we look at **what kinds of words** they are:

- **Adjective-Noun**: "relational database," "distributed system"
- **Noun-Noun**: "database system," "Los Angeles"  
- **Adjective-Adjective-Noun**: "distributed computing system"
- **Noun-Preposition-Noun**: "degrees of freedom"

This is much better! "of the" is Preposition-Article—filtered out. "New York" is Noun-Noun (or treated as a proper noun compound)—kept. "University of Science" is Noun-Preposition-Noun—kept!

But now you need a **part-of-speech tagger**. And POS taggers make mistakes. And they're slower than simple string matching. And they struggle with domain-specific terms ("MapReduce" is what, exactly? Noun? Proper noun?).

*grins*

This is the eternal dance in IR: **linguistic sophistication vs. computational efficiency**. The more linguistic knowledge you add, the better your precision, but the slower and more brittle your system becomes.

## Collocation: Beyond Consecutive Words

*waves hands excitedly*

Now we get to the really interesting part! N-grams assume words must be consecutive. But language doesn't work that way!

Consider:
- "dual core CPUs" vs "CPUs with dual core"
- "distributed systems" vs "distributed and reliable systems"
- "Dik Lee" vs "Dik Lun Lee" vs "Lee Dik Lun" (name order variations!)

If we only index consecutive bigrams/trigrams, we miss these variations. The user searches for "dual core" and we miss "CPUs with dual core" entirely!

**Collocation** solves this. Instead of "consecutive words," we define it as **"words that appear near each other more often than chance would predict."**

The "near" can be:
- Same document (co-occurrence)
- Same sentence
- Same paragraph  
- Within a window of K words (say, 10 words to the left or right)

*draws a text window on the board*

```
she knocked on his door
    |     |     |
    x     y     (distance = 3 words between "knocked" and "door")
```

Notice direction matters! "knocked on the door" vs "the door was knocked"—the distance from "knocked" to "door" changes sign depending on order.

## PMI: When Statistics Reveals Semantics

*settles into a chair, speaking more slowly*

Now I want to tell you about one of the most elegant ideas in all of computational linguistics: **Pointwise Mutual Information**. This is where we stop guessing about what makes words "go together" and let the math tell us.

Here's the intuition: Two words are "associated" if they appear together **more often than they would if they were independent**.

Think of a fair die. The probability of rolling a 3 is 1/6. The probability of rolling two 3s in a row is 1/36. If you observe two 3s together exactly 1/36 of the time, they're independent—no special relationship.

But words aren't dice! If "computer" appears in 3/1000 documents and "system" appears in 6/1000 documents, and they're independent, we'd expect them together in (3×6)/1,000,000 = 18/1,000,000 documents. But if they actually appear together in 2/1000 documents (that's 2000/1,000,000!), they're appearing **111 times more often than chance**!

*writes the formula large*

**PMI(x,y) = log₂ [ P(x,y) / (P(x) × P(y)) ]**

Or using frequencies:
**PMI(x,y) = log₂ [ (f(x,y) × N) / (f(x) × f(y)) ]**

Where N is the total number of word pairs observed.

Now look at your examples:

**"Toyota" and "durability":**
- f(Toyota, durability) = 1000
- f(Toyota) = 10,000  
- f(durability) = 5,000
- N = 50,000

PMI = log₂[(1000 × 50000)/(10000 × 5000)] = log₂(1) = **0**

**"Volvo" and "safety":**
- f(Volvo, safety) = 50
- f(Volvo) = 150
- f(safety) = 1,000

PMI = log₂[(50 × 50000)/(150 × 1000)] = log₂(16.67) ≈ **4.06**

*slaps the table*

See?! Even though "Toyota" and "durability" appear together 20 times more often in raw count, their association is actually **weaker**! Why? Because Toyota is mentioned everywhere for all sorts of reasons. "Durability" is just noise in the Toyota signal.

But "Volvo" and "safety"—that's a real association. Volvo is rare, safety is common, but when Volvo appears, safety often appears with it. That's brand identity captured by math!

## The Variance of Distance: Syntax as Statistics

*points to the distance table in your slides*

This is one of my favorite observations. Look at these patterns:

| Relation | Word x | Word y | Mean Distance | Variance |
|----------|--------|--------|---------------|----------|
| Fixed | bread | butter | 2.00 | 0.00 |
| Compound | computer | scientist | 1.12 | 0.10 |
| Semantic | man | woman | 1.46 | 8.07 |
| Lexical | refraining | from | 1.11 | 0.20 |

"Bread" and "butter" almost always appear exactly 2 words apart ("bread and butter"). Zero variance! It's a frozen phrase.

"Computer" and "scientist" are usually adjacent ("computer scientist"), but sometimes you get "scientist working in computer science"—small variance.

But "man" and "woman"—the mean distance is small, but the variance is huge! They appear in comparisons ("man and woman"), contrasts ("man vs woman"), parallel structures ("neither man nor woman")... They're semantically related but syntactically flexible.

This **variance of distance** tells us about the *type* of relationship! Low variance = syntactic fixedness (collocation). High variance = semantic association (co-occurrence).

## Applications: From Theory to Search Engine Features

*stands up, pacing excitedly*

So what do we do with all this? Your slides mention several applications, and I want to emphasize how powerful these simple statistical ideas are:

**1. Query Suggestion/Expansion**
User searches "Toyota." You look up high PMI words: "durability," "fuel efficiency," "Camry." Suggest these or silently expand the query. This is **pseudo-relevance feedback** without needing click data!

**2. Spam Detection**
Spam pages stuff popular keywords but lack coherent topic structure. A real page about "Toyota" will have clusters of related terms (high PMI with each other). A spam page has random popular words with no internal structure. Check for **coherence** via collocation networks!

**3. Page Summarization**
Want to summarize a page? Find the sentences that contain the highest density of **collocated terms**—words that statistically "belong together." These sentences are likely the most information-dense.

**4. Topic Classification**
A page belongs to topic X if it contains enough terms that have high mutual information with the **seed terms** of that topic. No machine learning required—just statistics!

## The Synthesis: Building a Real System

*draws a flowchart on the board*

So how would Professor Feynman build a search engine index? Not with one technique, but with **layers**:

**Layer 1: Base Unigrams**
Index everything. This is your fallback.

**Layer 2: Filtered N-grams**  
- Generate all bigrams/trigrams
- Filter out stop-word-heavy ones (fast)
- Filter by grammatical patterns if you have a POS tagger (better precision)
- Keep only those with frequency > threshold AND positive PMI (semantic significance)

**Layer 3: Collocation Network**
- For high-frequency unigrams, pre-compute their top PMI associates
- Store these as "soft phrases"—not requiring consecutive appearance

**Query Processing:**
- If query contains recognized n-gram (Layer 2), boost documents containing that exact sequence
- Also boost documents containing query terms that are PMI-associated (Layer 3)
- Always fall back to unigram matching (Layer 1) for recall

*turns to face the class*

This way, "apple juice" gets exact-match boost, "apple" + "juice" individually get base scores, and documents about "cider" (high PMI with "apple") get a small semantic boost. Precision when you can, flexibility when you must!

## The Deeper Lesson: Language is Not Random

*speaks softly now*

I want to leave you with the philosophical point your slides make at the end. **Humans are not random, so their writings are not random.**

Zipf's law, PMI, collocation patterns—these all work because language is a **complex adaptive system** with deep structure. We don't choose words randomly. We choose them based on meaning, context, convention, and the Principle of Least Effort.

When we discover that "bread" and "butter" have PMI through the roof, we're not just finding a statistic. We're discovering a **cultural fact**—a convention of English-speaking cuisine. When we find that "Volvo" and "safety" are tightly coupled, we're mapping **brand identity** in vector space.

The search engine, at its best, is a **model of human knowledge and association**. Every collocation it captures is a fragment of how we understand the world. The n-gram is a crude photograph of syntax. The PMI-weighted association network is a map of semantic memory.

*adjusts spectacles*

And the beautiful thing? We built this map with nothing but counting. No neural networks (though those help now). No deep learning. Just the patient accumulation of statistics over millions of documents, revealing the hidden structure of meaning.

That's the power of information retrieval. We don't just find documents. We **model the structure of human communication itself**.

Any questions? I'm happy to dive deeper into the math of PMI, the engineering of n-gram indexes, or the linguistics of why "strong tea" is English but "powerful tea" sounds wrong—that's collocation at work!