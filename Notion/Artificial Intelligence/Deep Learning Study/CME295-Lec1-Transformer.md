## 🏛️ Welcome to the Lecture Hall: Transformers and Large Language Models (CME 295)

Welcome to the lecture hall. With five decades in this field, I assure you, we will cover this subject thoroughly, moving from the foundational history to the most complex applications. The field of Natural Language Processing (NLP) and Large Language Models (LLMs) is one of the most exciting and rapidly evolving areas of computer science today.

You have requested a deep-dive lecture based on the introductory material presented for CME 295: **Transformers and Large Language Models**, covering the foundational concepts of NLP, tokenization, word representation, Recurrent Neural Networks (RNNs), and the core **Transformer architecture**.

Based on the material provided, I will proceed with a **Novice to Intermediate** approach, ensuring we establish firm foundations before delving into the technical specifics of the Transformer.

Let us commence our deep dive!

---

## 1. Natural Language Processing (NLP) Overview

Natural Language Processing (NLP) is the field of study focused on the interactions between computers and human language. The goal is to program computers to process and analyze large amounts of natural language data.

### 1.1 The Three Core Categories of NLP Tasks

As outlined, NLP tasks can be broadly categorized into three buckets:

| Category | Description | Examples | Evaluation Metrics |
| :--- | :--- | :--- | :--- |
| **Classification** | Input text maps to a single, discrete output category. | Sentiment Analysis, Intent Detection ("Create an alarm"), Language Detection, Topic Modeling. | Accuracy, **Precision, Recall, F1-Score**. |
| **Multi-Classification** | Input text maps to multiple discrete labels, often applied at the token/word level. | **Named Entity Recognition (NER)** (identifying locations, people, organizations), Part-of-Speech (POS) Tagging. | Token-level or Entity-level Precision, Recall, F1-Score. |
| **Generation** | Input text maps to a variable-length output text sequence. | **Machine Translation**, Question Answering, Summarization, Code Generation, Conversational AI. | **BLEU**, **ROUGE**, **Perplexity** (PPL), Human Evaluation. |

#### **A Deeper Look at Evaluation Metrics**

For classification, the distinction between Accuracy, Precision, and Recall is critical, especially in imbalanced datasets (e.g., 99% positive reviews, 1% negative reviews).

* $$\text{Precision} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}}$$ (Out of all I *said* were positive, how many *were* positive?)
* $$\text{Recall} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}}$$ (Out of all that *are* positive, how many did I *find*?)
* $$\text{F1-Score} = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$$ (The harmonic mean, offering a single balanced measure).

For generation tasks, metrics are complex:

* **BLEU (Bilingual Evaluation Understudy):** A precision-focused metric that counts the number of matches between the model's output and a set of reference translations, with a brevity penalty. A higher BLEU score generally means a "better" translation.
* **ROUGE (Recall-Oriented Understudy for Gisting Evaluation):** A recall-focused metric primarily used for summarization, measuring the overlap in *n*-grams (sequences of $n$ words) between the generated text and the reference text.
* **Perplexity (PPL):** A measure of how well a probability distribution or language model predicts a sample. It is the exponentiated average negative log-likelihood of the probability assigned to each word.
    $$\text{PPL} = 2^{H(p)}$$ where $H(p)$ is the cross-entropy. **Lower perplexity is better**, as it means the model is less "surprised" by the sequence it generates or sees.

### 1.2 A Brief History of Language Models

The shift from the 1980s to the 2020s highlights the critical role of data and compute power.

| Era | Key Methodologies | Limiting Factors |
| :--- | :--- | :--- |
| **1980s - 2000s** | Rule-based systems, Simple Neural Networks (RNNs, LSTMs). | Lack of large, digitized corpora (datasets) and limited computational power. |
| **2010s** | **Word2vec** (2013), Gated Architectures (LSTMs, GRUs). | Sequential nature of RNNs led to the **vanishing gradient problem** and slow training. |
| **2017 - Present** | **Transformer** (2017), Scaling (BERT, GPT, LLMs). | **Attention Mechanism** solved the long-range dependency problem and enabled massive parallelization using GPUs. |

---

## 2. Preparing Text for the Model: Tokenization

The fundamental challenge is that models understand numbers (vectors), not raw text. **Tokenization** is the process of splitting text into elementary units called **tokens**.

### 2.1 Tokenization Strategies and Trade-offs

| Strategy | Unit | Pros | Cons/Key Issue |
| :--- | :--- | :--- | :--- |
| **Word-Level** | Complete words. | Simple, human-intuitive. | **High OOV (Out-Of-Vocabulary) risk**; does not leverage word roots (e.g., "bear" and "bears" are separate). |
| **Subword-Level** | Morphemes/common character sequences (e.g., "read-ing"). | **Low OOV risk**; leverages shared word roots; good balance of vocabulary size and sequence length. | Sequences are longer than word-level, increasing computational cost (e.g., "reading" becomes "read" + "ing"). |
| **Character-Level** | Individual characters. | **Zero OOV risk**; robust to misspellings/typos. | **Very long sequence lengths**, dramatically increasing computation and inference time; embeddings are less meaningful. |

The **Subword-Level Tokenizer** is the standard approach in modern LLMs (e.g., Byte Pair Encoding - BPE, WordPiece). For a language like English, a vocabulary size might be in the tens of thousands, while a multilingual model might exceed a hundred thousand tokens to account for different scripts and languages.

---

## 3. Representing Tokens: Word Embeddings

Once we have tokens, we need to convert them into a numerical format, or **word representation**, that captures their semantic meaning.

### 3.1 From One-Hot Encoding to Continuous Embeddings

1.  **One-Hot Encoding (OHE):** Assigns a vector of size $V$ (vocabulary size) to each word, where only one dimension is '1' and all others are '0'.
    * **Problem:** All OHE vectors are mathematically **orthogonal** (perpendicular) to each other, meaning their dot product (and cosine similarity) is zero. This fails to capture that "King" and "Queen" are more similar than "King" and "Apple."
2.  **Continuous Embeddings (Word2vec):** The breakthrough was learning a dense, lower-dimensional vector representation (e.g., dimension $D=768$) where geometric distance reflects semantic meaning.
    * **Method:** Word2vec uses a **proxy task** (e.g., Continuous Bag-of-Words or Skip-gram) to learn these embeddings.
        * **CBOW:** Predict the current word based on its surrounding context words.
        * **Skip-gram:** Predict the surrounding context words based on the current word.
    * **Intuition:** If two words frequently appear in similar contexts, their learned embeddings will be close in the vector space. This allows for the famous analogy: **$$\text{vector}(\text{'King'}) - \text{vector}(\text{'Man'}) + \text{vector}(\text{'Woman'}) \approx \text{vector}(\text{'Queen'})$$**

**The Word2vec Network:** By training a simple shallow neural network to predict a word from its context (or vice versa), the weights of the connection between the input layer and the hidden layer form the embedding matrix. These hidden layer vectors are the desired learned embeddings.

---

## 4. Modeling Sequences: RNNs and Long-Range Dependencies

The previous method (Word2vec) gives us a single static embedding for a word, regardless of its context in a sentence. To capture sequence and context, we introduced **Recurrent Neural Networks (RNNs)**.

### 4.1 Recurrent Neural Networks (RNNs)

RNNs process sequence data by maintaining a **Hidden State** ($h_t$) that summarizes the information seen up to time step $t$.

* $$h_t = f(W_{hh}h_{t-1} + W_{xh}x_t + b_h)$$

**The Problem: Long-Range Dependencies**

The key limitation of simple RNNs is the **vanishing gradient problem**. As the sequence length grows, the influence of earlier inputs on the current hidden state is gradually "washed out" because the gradient (used for updating weights during backpropagation) shrinks exponentially. This makes it impossible for the model to "remember" information from the beginning of a long text (e.g., the subject of a very long sentence needed to correctly conjugate the verb at the end).

* **LSTMs (Long Short-Term Memory):** Introduced a separate **Cell State** ($C_t$) with special **gates** (input, forget, output) to regulate the flow of information, mitigating the vanishing gradient problem, but not solving the core issue of sequential processing.

---

## 5. The Revolution: The Attention Mechanism

To solve the problem of long-range dependencies and slow sequential computation, the **Attention Mechanism** was introduced, culminating in the **Transformer** architecture.

### 5.1 The Core Concept: Self-Attention

Attention allows a model to look at the entire input sequence at once and assign different weight (or **attention**) to different words when processing a specific word.

* **Contextual Embeddings:** This directly solves the ambiguity problem. The representation of the word 'bank' will now be different in the context of 'river bank' versus 'robbing a bank' because the attention mechanism focuses on different surrounding words for each instance.
* **Parallelization:** Since the calculation for each token can be done simultaneously (all at once) instead of sequentially, the process can be efficiently parallelized on modern hardware like GPUs, making the training of massive models (LLMs) possible.

### 5.2 The Query, Key, and Value (Q, K, V) Paradigm

The self-attention calculation is based on three learned projection matrices:

| Concept | Symbol | Role | Source in Self-Attention |
| :--- | :--- | :--- | :--- |
| **Query** | $Q$ | The token we are currently processing. **What am I looking for?** | Input Embedding $\times$ $W_Q$ |
| **Key** | $K$ | The tokens being evaluated for relevance to the Query. **What do I have?** | Input Embedding $\times$ $W_K$ |
| **Value** | $V$ | The actual information from the relevant tokens used to compute the new representation. | Input Embedding $\times$ $W_V$ |

The output of the attention mechanism (the new, contextualized representation) is a **weighted sum of the Value vectors**, where the weights are determined by the **similarity between the Query and Key vectors**.

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

1.  **$$QK^T$$:** Calculates the score (dot product) of how well the Query aligns with each Key.
2.  **$$\text{Scaling by } \sqrt{d_k}$$:** Divides by the square root of the key dimension to prevent the dot products from growing too large and pushing the softmax function into regions where gradients are tiny (i.e., making the model too confident).
3.  **$$\text{Softmax}$$:** Converts the scores into a probability distribution (weights), ensuring they sum to 1. This is the **attention weight**.
4.  **$$\times V$$:** Multiplies the weights by the actual Value vectors and sums them up. The result is a highly contextualized output vector.

### 5.3 Multi-Head Attention

Instead of performing the attention calculation once (one "head"), **Multi-Head Attention** performs the calculation $h$ times in parallel, using $h$ independent sets of $W_Q, W_K, \text{ and } W_V$ projection matrices.

* **Benefit:** Each "head" learns to focus on different types of relationships. For example, one head might attend to syntactic relationships (subject-verb agreement), while another might attend to semantic relationships (e.g., 'cute' and 'teddy bear'). The outputs of all heads are then concatenated and projected back to the original dimension using a final learned matrix, $W_O$.

---

## 6. The Transformer Architecture (Encoder-Decoder)

The Transformer, introduced in 2017, fully abandons recurrence and convolution, relying entirely on the Attention mechanism. 

### 6.1 Input Preparation

1.  **Tokenization:** Convert text into tokens (e.g., subwords).
2.  **Input Embedding:** Convert tokens into their learned $D$-dimensional word embeddings.
3.  **Positional Encoding (PE):** Since the Attention mechanism has no inherent concept of word order, fixed sine and cosine functions are used to create a unique vector for each position, which is *additively* combined with the word embedding. This is crucial for maintaining the sequential information lost by parallelizing.

### 6.2 The Encoder Stack

The Encoder is tasked with processing the input sequence and producing rich, context-aware representations (embeddings).

* **Components:** $N$ identical layers, each containing:
    1.  **Multi-Head Self-Attention (MHA):** All tokens attend to *all other tokens* in the input sequence to create a new contextualized vector for each.
    2.  **Feed-Forward Network (FFN):** A simple two-layer neural network applied independently to each position. This provides non-linearity and allows the model to learn additional, complex projections.
    3.  **Residual Connections and Layer Normalization:** (Not discussed in detail, but vital) A residual connection (skip connection) is added around each sub-layer, followed by layer normalization.

### 6.3 The Decoder Stack

The Decoder is tasked with taking the Encoder's output and generating the target sequence one token at a time (autoregressively).

* **Components:** $N$ identical layers, each containing:
    1.  **Masked Multi-Head Self-Attention:** This is self-attention, but with a crucial modification for the decoding process: a **causal mask**. When predicting token $t$, the decoder is only allowed to attend to tokens *before* $t$ (i.e., $t-1, t-2, \dots, \text{BOS}$). It cannot see the future tokens of the sequence it is generating.
    2.  **Cross-Attention:** This layer connects the Decoder to the Encoder's output.
        * **Query (Q):** Comes from the *output* of the masked self-attention layer (the current state of the translation).
        * **Key (K) & Value (V):** Come from the final *output* of the Encoder stack (the final, context-aware representation of the source sentence).
        * **Role:** Allows the decoder to selectively focus on the most relevant parts of the *source* sentence at each generation step.
    3.  **Feed-Forward Network (FFN):** Same as in the Encoder.

### 6.4 Output and Generation

1.  The output of the final decoder layer is projected by a **Linear Layer** to a vector of size $V$ (vocabulary size).
2.  A **Softmax Layer** converts this vector into a probability distribution over the vocabulary. The token with the highest probability is selected as the next word.
3.  This newly generated token is then fed back into the decoder's input for the next time step, continuing the process until the End-of-Sequence (EOS) token is generated.

### 6.5 Label Smoothing

A valuable technique used during training, **Label Smoothing**, modifies the one-hot target label. Instead of providing the model with a hard target of $[0, 1, 0, \dots]$, it is "smoothed" to be a slightly more uncertain target, such as $[0.01, 0.97, 0.01, \dots]$.

* **Effect:** This prevents the model from becoming overconfident in its predictions, which acts as a form of regularization. Empirically, it often improves generalization and overall performance metrics like BLEU score.

---

### Concluding Remarks

We have now established the full pipeline from raw text to a model's output: from the critical decision of **tokenization** to the invention of **contextual embeddings**, the challenges of **RNNs**, and the final, pivotal invention of the **Attention Mechanism** that underpins the **Transformer architecture**.

This foundational understanding is essential, as nearly all state-of-the-art Large Language Models (LLMs) today—from BERT to GPT to Gemini—are direct descendants or variations of the original Transformer model. The field has since moved largely to **Decoder-Only** or **Encoder-Only** variations, but the core mechanism of self-attention remains the heart of the revolution.

Do you have any questions on this initial lecture, perhaps specifically on the mathematical operation of the $\text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ formula, or would you like to move on to the next segment covering Encoder-Only and Decoder-Only LLMs?