Below, I’ve merged the notes and explanations into a cohesive format for each section from the provided text on "Partitioning" from Chapter 6. Each section includes step-by-step examples or practical illustrations where applicable, along with additional essential information to enhance your understanding as a computer science and software engineering student.

---

### 1. Partitioning Overview

- **Merged Note & Explanation**:
    - **Definition**: Partitioning (aka sharding, regions, tablets, vnodes, vBuckets) splits large datasets into smaller, manageable pieces, each assigned to a single partition. Each record belongs to exactly one partition, acting like a mini-database, though cross-partition operations are possible.
    - **Purpose**: Scalability—distribute data and query load across nodes in a shared-nothing cluster (no shared resources between nodes). This scales storage (more disks) and throughput (more CPUs).
    - **Scalability Benefits**:
        - Small queries run independently on one partition’s node.
        - Large queries can parallelize across nodes, though this is complex.
    - **History**: Pioneered by Teradata and Tandem NonStop SQL (1980s), revived by NoSQL and Hadoop for transactional or analytical workloads.
    - **Example**: A 1TB dataset split into 10 partitions of 100GB each, spread across 10 nodes, handles 10x the queries of a single node.
- **Additional Info**:
    - **Terminology**: “Partition” is standard; other terms (e.g., shard) vary by system (MongoDB, Cassandra).
    - **Workloads**: Transactional (OLTP) tunes for low latency; analytical (OLAP) for throughput—partitioning adapts to both.
    - **Real-World**: Google BigTable partitions data for massive scale.

---

### 2. Partitioning and Replication

- **Merged Note & Explanation**:
    - **Integration**: Partitioning often pairs with replication. Each partition is replicated across multiple nodes for fault tolerance (e.g., Figure 6-1). A record stays in one partition but may exist on several nodes.
    - **Node Roles**: A node can store multiple partitions, acting as leader for some and follower for others in a leader-follower model (Chapter 5).
    - **Independence**: Partitioning scheme (how data splits) is separate from replication scheme (how copies are managed), simplifying this chapter’s focus on partitioning alone.
    - **Example**: Node 1 leads Partition 1, follows Partition 2; Node 2 leads Partition 2, follows Partition 1—writes go to leaders, reads can hit followers.
- **Additional Info**:
    - **Fault Tolerance**: Replication ensures data survives node failures; partitioning scales capacity.
    - **Practical**: Cassandra combines partitioning and leaderless replication; Figure 6-1 mirrors MySQL’s leader-follower setup.
    - **Diagram Insight**: Replication streams per partition maintain consistency (e.g., Partition 4’s leader on Node 4 syncs to followers).

---

### 3. Partitioning of Key-Value Data

- **Merged Note & Explanation**:
    - **Goal**: Evenly spread data and query load across nodes for linear scalability (e.g., 10 nodes = 10x throughput). Uneven distribution (skew) creates hot spots, degrading performance.
    - **Challenge**: Random assignment balances data but makes reads inefficient (query all nodes). Key-based partitioning is smarter.
    - **Assumption**: Data is key-value, accessed by primary key (e.g., encyclopedia lookup by title).
    - **Example**: 1000 records split across 4 nodes. Skewed: Node 1 gets 700, others 100 each. Balanced: 250 per node.
- **Additional Info**:
    - **Hot Spot**: One partition overloaded (e.g., all writes to “today’s” data) bottlenecks the system.
    - **Real-World**: Redis Cluster uses key-based partitioning for even load.
    - **Theory**: Load balancing is NP-hard; practical systems approximate fairness.

---

### 4. Partitioning by Key Range

- **Merged Note & Explanation**:
    - **Mechanism**: Assign continuous key ranges to partitions (e.g., Figure 6-2: A-B in Volume 1, T-Z in Volume 12). Boundaries adapt to data distribution, set manually or automatically.
    - **Pros**:
        - Easy lookup: Know the key, find the partition (e.g., “K” goes to Volume 6).
        - Sorted keys within partitions (e.g., SSTables) enable efficient range scans (e.g., all sensor data for April).
    - **Cons**: Hot spots if keys correlate with load (e.g., timestamps: all writes hit today’s partition).
    - **Workaround**: Prefix keys (e.g., `sensor1_timestamp`) to spread writes.
    - **Step-by-Step Example (Sensor Data)**:
        1. Partition 1: `sensor1_2023-01` to `sensor1_2023-06`.
        2. Partition 2: `sensor2_2023-01` to `sensor2_2023-06`.
        3. Write `sensor1_2023-04-03` to Partition 1.
        4. Query range `sensor1_2023-04-*` hits Partition 1 only.
    - **Usage**: BigTable, HBase, RethinkDB, early MongoDB.
- **Additional Info**:
    - **Trade-off**: Range scans vs. write skew—mitigate with composite keys.
    - **Practical**: HBase uses key ranges for time-series data, adjusting boundaries dynamically.
    - **Code Insight**: Lookup might use binary search on boundaries:
        
        ```Python
        def find_partition(key, boundaries):
            return next(i for i, b in enumerate(boundaries) if key < b)
        ```
        

---

### 5. Partitioning by Hash of Key

- **Merged Note & Explanation**:
    - **Mechanism**: Hash the key (e.g., MD5) to a number, assign ranges of hash values to partitions (Figure 6-3). Evens out skewed data (e.g., timestamps).
    - **Pros**: Reduces hot spots by randomizing key placement (e.g., consecutive timestamps spread across nodes).
    - **Cons**: Loses sort order, making range queries inefficient (query all partitions).
    - **Consistent Hashing**: Pseudo-random boundaries (Karger et al.), rarely used in DBs due to rebalancing issues—prefer “hash partitioning.”
    - **Cassandra Compromise**: Hash first column of compound key (e.g., `user_id`), sort others (e.g., `timestamp`) within partition for range scans.
    - **Example**: Hash(`"2014-04-19 17:08:10"`) = 7372 → Partition 0; Hash(`"2014-04-19 17:08:11"`) = 18805 → Partition 2.
    - **Usage**: MongoDB (hash sharding), Riak, Couchbase, Voldemort.
- **Additional Info**:
    - **Hash Function**: Non-cryptographic (e.g., FNV) suffices for distribution.
    - **Real-World**: Cassandra’s compound keys model one-to-many (e.g., user posts).
    - **Code Example**:
        
        ```Python
        def hash_partition(key, num_partitions):
            return hash(key) % num_partitions
        ```
        

---

### 6. Skewed Workloads and Relieving Hot Spots

- **Merged Note & Explanation**:
    - **Problem**: Extreme skew (e.g., celebrity’s social media key) overloads one partition despite hashing.
    - **Solution**:
        - Add random suffix to hot keys (e.g., `celeb_id_73`), splitting load across partitions (e.g., 100 keys).
        - Reads must aggregate all variants (e.g., query `celeb_id_*`).
    - **Trade-off**: Extra read work and bookkeeping (track hot keys needing splitting).
    - **Example**: `celeb_123` gets 1M writes. Split to `celeb_123_00` to `celeb_123_99`; each partition gets ~10K writes.
    - **Step-by-Step**:
        1. Detect hot key `celeb_123` (e.g., >50% load).
        2. Rewrite as `celeb_123_{rand(00-99)}`.
        3. Read: Query all 100 variants, merge results.
- **Additional Info**:
    - **Manual**: Most systems don’t auto-detect skew—app must handle it.
    - **Future**: Dynamic splitting (e.g., DynamoDB) could automate this.
    - **Practical**: Twitter sharding mitigates celebrity spikes this way.

---

### 7. Partitioning and Secondary Indexes

- **Merged Note & Explanation**:
    - **Challenge**: Secondary indexes (e.g., search by `color` not `id`) don’t map to one partition like primary keys.
    - **Document-Based Partitioning (Local Index)**:
        - **Mechanism**: Each partition indexes its own documents (e.g., Figure 6-4). Write to one partition; secondary index is local.
        - **Pros**: Simple writes (e.g., add `color:red` to Partition 0’s index).
        - **Cons**: Reads scatter/gather across all partitions (e.g., `color:red` in both Partition 0 and 1), increasing latency.
        - **Example**: Car site: Partition 0 has `id=191 (color:red)`, Partition 1 has `id=768 (color:red)`. Query `color:red` hits both, merges `[191, 768]`.
    - **Usage**: MongoDB, Riak, Cassandra, Elasticsearch, SolrCloud, VoltDB.
    - **Step-by-Step (Query)**:
        1. Client queries `color:red`.
        2. Send to all partitions (e.g., 0 and 1).
        3. Partition 0 returns `[191, 306]`, Partition 1 returns `[768]`.
        4. Client merges: `[191, 306, 768]`.
- **Additional Info**:
    - **Scatter/Gather**: Parallel queries amplify tail latency (slowest node delays all).
    - **Optimization**: Design keys to limit scatter (e.g., `user_id` as primary key for user-specific queries).
    - **DIY Risk**: App-managed indexes (e.g., value-to-ID mapping) face race conditions—use DB-native indexes.

---

### How to Use This

- **Study**: Each section covers “what” (method), “why” (scalability), and “how” (examples). Quiz yourself on key range vs. hash pros/cons or secondary index trade-offs.
- **Practice**: Simulate partitioning in Python (e.g., hash keys to 4 “nodes”) or set up MongoDB sharding locally.
- **Next Steps**: Ask about rebalancing (p. 201), transactions (Chapter 7), or code a partition lookup!

What’s next on your mind?

  

---

### What is Partitioning and Why Do We Need It?

- **Simple Explanation**: Partitioning is like splitting a huge book into smaller chapters so you can store each chapter on a different shelf. Imagine you have a giant library with millions of books, but only one librarian and one shelf. Finding a book or adding a new one would take forever because everything’s in one pile! Partitioning breaks that giant pile into smaller, organized stacks (partitions) and puts each stack on a different shelf (node/computer). Now multiple librarians (servers) can work at once, making things faster and manageable.
- **Why We Need It**:
    - **Too Much Data**: One computer can’t hold or process everything if your data grows to terabytes (e.g., social media posts, sensor readings).
    - **Too Many Queries**: One computer can’t handle thousands of users asking for data at the same time (e.g., searching a website).
    - **Scalability**: By splitting data across many computers (nodes), you can add more nodes as your data or users grow, like adding more shelves to a library.
- **Real-Life Example**: Think of a phonebook for a whole country on one computer—it’s slow to search. Split it into “A-M” on one computer and “N-Z” on another, and searches get twice as fast because two computers work together.
- **Goal**: Spread data and work evenly so no single node gets overwhelmed (a “hot spot”), and every node does its fair share.

---

### How Does Partitioning Work with Key-Value Data?

- **Key-Value Basics**: Imagine your data as a dictionary—each entry has a “key” (like a word) and a “value” (its definition). For example:
    - Key: `user123`, Value: `{name: "Alice", age: 25}`
    - Key: `user124`, Value: `{name: "Bob", age: 30}`
    - You look up data by its key (e.g., “Give me user123’s info”).
- **Partitioning Idea**: We decide which node gets each key-value pair using a rule. Each key belongs to exactly one partition, and each partition lives on one or more nodes (we’ll ignore replication for now to keep it simple).
- **Analogy**: Picture a filing cabinet with labeled drawers. Each drawer (partition) holds a specific set of files (key-value pairs). You need a system to know which drawer gets a new file and where to look for an old one.
- **Challenge**: If we split unevenly (e.g., one drawer has 90% of the files), that node gets swamped while others sit idle. We want balance!

---

### Two Main Ways to Partition Key-Value Data

Let’s explore the two primary methods: **Partitioning by Key Range** and **Partitioning by Hash of Key**. I’ll use a consistent example—a phonebook with names as keys—and explain each with steps, pros/cons, and analogies.

### 1. Partitioning by Key Range

- **What It Does**: Split keys into alphabetical or numerical ranges, like dividing a phonebook into volumes (Figure 6-2). Each partition gets a chunk of keys from a minimum to a maximum.
- **How It Works**:
    - Define boundaries (e.g., A-G, H-N, O-Z).
    - Assign each range to a node.
    - Look up a key by finding its range (e.g., “Mary” goes to H-N).
- **Step-by-Step Example (Phonebook with 3 Nodes)**:
    1. Data: Keys are names like `Alice`, `Bob`, `Charlie`, `David`, `Eve`, `Frank`, `Grace`, `Helen`, `Ivy`.
    2. Boundaries:
        - Node 1: A-E (Alice, Bob, Charlie, David, Eve)
        - Node 2: F-J (Frank, Grace, Helen, Ivy)
        - Node 3: K-Z (none yet)
    3. Write: Add `Charlie` → Node 1 (A-E).
    4. Read: Find `Grace` → Node 2 (F-J).
- **Pros**:
    - **Fast Lookups**: You know exactly which node has your key (e.g., “Helen” is in F-J on Node 2).
    - **Range Queries**: Easy to grab a group (e.g., all names A-E in one go), like flipping through a phonebook section.
    - **Sorted Order**: Keys stay sorted within each partition (e.g., Alice, Bob, Charlie), great for scans.
- **Cons**:
    - **Hot Spots**: If everyone adds names starting with “A” today (e.g., timestamp keys like `2025-04-03`), Node 1 gets overloaded while Node 3 sits empty.
- **Fixing Hot Spots**: Add a prefix (e.g., `sensor1_2025-04-03`, `sensor2_2025-04-03`) to spread writes across ranges.
- **Analogy**: Like library bookshelves labeled A-G, H-N, O-Z. Finding “Moby Dick” is quick (H-N shelf), but if everyone writes books starting with “A,” that shelf gets crowded.
- **Real-World**: HBase uses key ranges for time-series data (e.g., timestamps), adjusting boundaries to balance load.

### 2. Partitioning by Hash of Key

- **What It Does**: Run each key through a hash function (a math trick) to get a number, then assign that number to a partition. It scrambles keys to spread them evenly, like shuffling cards into piles.
- **How It Works**:
    - Hash the key (e.g., `hash("Alice") = 7372`).
    - Divide hash range into partitions (e.g., 0-20,000 on Node 1, 20,001-40,000 on Node 2).
    - Place key in its hash’s partition.
- **Step-by-Step Example (Phonebook with 3 Nodes)**:
    1. Data: Same names—`Alice`, `Bob`, `Charlie`, `David`, `Eve`, `Frank`, `Grace`, `Helen`, `Ivy`.
    2. Hash Values (simplified):
        - `hash("Alice") = 7372`, `hash("Bob") = 18805`, `hash("Charlie") = 50537`, `hash("David") = 31579`, etc.
    3. Ranges:
        - Node 1: 0-20,000 (`Alice=7372`, `Bob=18805`)
        - Node 2: 20,001-40,000 (`David=31579`)
        - Node 3: 40,001-65,535 (`Charlie=50537`)
    4. Write: Add `Eve` (`hash("Eve") = 6253`) → Node 1.
    5. Read: Find `Grace` (`hash("Grace") = 24510`) → Node 2.
- **Pros**:
    - **Even Spread**: Hashes randomize placement, avoiding hot spots (e.g., `2025-04-03` and `2025-04-04` don’t clump).
    - **Balanced Load**: Nodes share work evenly, even with skewed keys.
- **Cons**:
    - **No Range Queries**: Keys scatter (e.g., `Alice` and `Bob` on different nodes), so “A-B” requires asking all nodes.
    - **Lookup Overhead**: Must hash the key first to find its node.
- **Analogy**: Like sorting mail by a random number on each envelope into bins. It’s fair (each bin gets some), but finding all “A” names means checking every bin.
- **Real-World**: MongoDB’s hash sharding spreads user IDs evenly across servers.

---

### Comparing Key Range vs. Hash Partitioning

- **Visualize It**:
    - **Key Range**: A phonebook split into A-G, H-N, O-Z. Quick to find “Mary” (H-N), but new “A” entries flood one section.
    - **Hash**: A phonebook shuffled by a number generator into 3 piles. Evenly split, but “A-G” requires searching all piles.
- **When to Use**:
    - **Key Range**: Great for ordered data or range scans (e.g., timestamps, alphabetical lists), if you can avoid hot spots.
    - **Hash**: Best for random access (e.g., user IDs, emails) where even load matters more than order.
- **Example Scenario**:
    - Social Media Posts: Key = `userID_timestamp`.
        - Key Range: `user1_2025-01` to `user1_2025-06` on Node 1. Good for fetching a user’s posts, but all new posts hit the latest range.
        - Hash: `hash(user1_2025-01)` spreads posts across nodes. Balanced, but fetching a range needs all nodes.

---

### Extra Clarity with a Hands-On Example

Let’s partition 10 users (`user1` to `user10`) across 3 nodes:

- **Key Range**:
    - Node 1: `user1-user3`
    - Node 2: `user4-user7`
    - Node 3: `user8-user10`
    - Add `user5` → Node 2. Find `user1-user3` → Node 1 only. All `user10` writes → Node 3 (potential hot spot).
- **Hash Partitioning**:
    - Hash range 0-99:
        - Node 1: 0-33 (`hash("user1")=15`, `hash("user4")=20`)
        - Node 2: 34-66 (`hash("user5")=40`, `hash("user7")=60`)
        - Node 3: 67-99 (`hash("user10")=80`)
    - Add `user6` (`hash=45`) → Node 2. Find `user1-user3` → Query all nodes.

---

### Additional Tips to Understand

- **Think Like a Librarian**: Partitioning organizes chaos. Key range is alphabetical filing; hashing is random sorting.
- **Balance vs. Speed**: Key range speeds up specific lookups but risks imbalance; hashing balances load but slows range queries.
- **Try It Out**: Grab a list of 10 names, split them by range (A-M, N-Z) vs. hash (odd/even numbers), and see where they land!

---

### Questions to Check Your Understanding

1. Why might `timestamp` as a key cause a hot spot with key range partitioning?
2. How does hashing fix that problem?
3. If you want all posts from `user1` in April, which method is easier?

Let me know what’s still fuzzy, or if you want more examples, analogies, or a specific part (e.g., hot spots) explained further! What’s tripping you up?

  

  

  

Let’s break down the additional sections from Chapter 6 that you’ve provided, focusing on **Partitioning Secondary Indexes by Document**, **Partitioning Secondary Indexes by Term**, and **Rebalancing Partitions**. I’ll explain each concept clearly, using simple analogies, step-by-step examples, and practical insights tailored to your interest in understanding partitioning better. Since you mentioned struggling with partitioning earlier, I’ll keep it approachable and connect it to what we’ve already discussed (e.g., key-value partitioning). Ready? Let’s dive in!

---

### 1. Partitioning Secondary Indexes by Document (Local Index)

- **Simple Explanation**: Imagine you’re running a used car website with thousands of car listings. Each car has a unique ID (like a library book’s call number), and you’ve split your database into partitions based on these IDs (e.g., IDs 0-499 on Partition 0, 500-999 on Partition 1). Now, users want to search by color or make (e.g., “red cars” or “Ford cars”), not just ID. A secondary index is like a card catalog that lists all cars by color or make, but each partition keeps its own catalog only for its cars—this is called a **local index**.
- **How It Works**:
    - **Primary Key Partitioning**: Cars are split by ID (e.g., Figure 6-4: ID 191 on Partition 0, ID 768 on Partition 1).
    - **Local Index**: Each partition builds its own index for its cars’ attributes (color, make). For example:
        - Partition 0: `color:red → [191, 306]`, `make:Honda → [191]`.
        - Partition 1: `color:red → [768]`, `make:Volvo → [768]`.
    - **Writes**: Add a car (e.g., ID 191, `color:red`, `make:Honda`) to Partition 0 → update only Partition 0’s index.
    - **Reads**: Search “red cars” → ask all partitions (scatter), combine results (gather): `[191, 306, 768]`.
- **Step-by-Step Example**:
    1. Car Listings:
        - ID 191: `{color: "red", make: "Honda"}` → Partition 0 (0-499).
        - ID 306: `{color: "red", make: "Ford"}` → Partition 0.
        - ID 768: `{color: "red", make: "Volvo"}` → Partition 1 (500-999).
    2. Indexes:
        - Partition 0: `color:red → [191, 306]`.
        - Partition 1: `color:red → [768]`.
    3. Query “red cars”:
        - Ask Partition 0 → `[191, 306]`.
        - Ask Partition 1 → `[768]`.
        - Combine: `[191, 306, 768]`.
- **Pros**:
    - **Simple Writes**: Only update one partition’s index (e.g., adding ID 191 affects Partition 0 only).
    - **Independent Partitions**: No coordination needed between partitions.
- **Cons**:
    - **Scatter/Gather Reads**: Must query all partitions for “red cars” since red cars are spread out, slowing things down.
    - **Latency Issues**: If one partition is slow (tail latency), the whole query waits.
- **Analogy**: Each partition is a small library with its own card catalog. To find all “red” books, you check every library’s catalog and combine the lists—easy to update, but slow to search.
- **Real-World**: MongoDB, Cassandra, Elasticsearch use this. Tip: Design IDs to group related data (e.g., user-specific cars) to reduce scatter.

---

### 2. Partitioning Secondary Indexes by Term (Global Index)

- **Simple Explanation**: Instead of each partition having its own index, you create one big index for all cars, but split it by the search term (e.g., “color:red” or “make:Ford”) across partitions. This is a **global index** because it covers all data, not just one partition’s. Think of it as a central phonebook sorted by color, split into sections (A-R on one shelf, S-Z on another).
- **How It Works**:
    - **Term-Based Partitioning**: Index entries are split by the term (e.g., Figure 6-5):
        - Partition 0: `color:black → [214]`, `color:red → [191, 306, 768]`.
        - Partition 1: `color:silver → [515, 893]`, `color:yellow → []`.
    - **Writes**: Add a car (e.g., ID 191, `color:red`) → update the `color:red` entry in Partition 0, regardless of where ID 191 lives.
    - **Reads**: Query “red cars” → go straight to Partition 0’s `color:red` → `[191, 306, 768]`.
- **Step-by-Step Example**:
    1. Same Car Listings:
        - ID 191: `{color: "red", make: "Honda"}` (Partition 0 by ID).
        - ID 306: `{color: "red", make: "Ford"}` (Partition 0).
        - ID 768: `{color: "red", make: "Volvo"}` (Partition 1).
    2. Term Index:
        - Partition 0 (A-R): `color:red → [191, 306, 768]`.
        - Partition 1 (S-Z): `color:silver → [515, 893]`.
    3. Query “red cars”:
        - Ask Partition 0 only → `[191, 306, 768]`.
- **Pros**:
    - **Fast Reads**: One partition has all “red cars,” no scatter/gather needed.
    - **Range Scans**: If terms are sorted (e.g., prices), you can query ranges efficiently.
- **Cons**:
    - **Complex Writes**: Adding a car updates multiple index partitions (e.g., `color:red` and `make:Honda` might be on different nodes).
    - **Asynchronous Updates**: Writes often lag (e.g., DynamoDB’s global indexes update in <1s usually), risking stale reads.
- **Analogy**: A central catalog split by topic— “Red” books in one drawer, “Silver” in another. Finding “Red” is quick (one drawer), but adding a book means updating multiple drawers, which might not sync instantly.
- **Real-World**: DynamoDB, Riak Search, Oracle warehouses use this. Often async to avoid slow distributed transactions.

---

### Comparing Local vs. Global Indexes

- **Visualize It**:
    - **Local (Document-Based)**: Each library has its own “red cars” list. Search means visiting every library.
    - **Global (Term-Based)**: One master list of “red cars” in a single library. Search is direct, but updates are scattered.
- **Trade-Off**:
    - **Local**: Fast writes, slow reads (scatter/gather).
    - **Global**: Slow writes (multi-partition updates), fast reads (single partition).
- **Example Query**: “Red Fords”:
    - Local: Ask all partitions for `color:red` and `make:Ford`, intersect results (e.g., `[191, 306] ∩ [306, 515] = [306]`).
    - Global: Ask `color:red` partition, filter for `make:Ford` there—fewer steps.

---

### 3. Rebalancing Partitions

- **Simple Explanation**: Rebalancing is like rearranging bookshelves when you add or remove a shelf (node). Your data grows, or a node fails, so you move partitions around to keep the load even without downtime.
- **Why It’s Needed**:
    - More queries → add CPUs (nodes).
    - More data → add disks/RAM.
    - Node fails → redistribute its work.
- **Requirements**:
    - Fair load after rebalancing.
    - Keep accepting reads/writes during the process.
    - Minimize data movement (network cost).
- **Strategies**:
    1. **Fixed Number of Partitions**:
        - **How**: Create many partitions upfront (e.g., 1000 for 10 nodes, 100 each). Add a node → steal partitions (e.g., Figure 6-6: Node 4 takes some from Nodes 0-3).
        - **Example**:
            - Before: Node 0 has `p0, p4, p8`.
            - Add Node 4: Move `p4` to Node 4.
            - Keys stay in `p4`, only partition moves.
        - **Pros**: Simple, predictable moves.
        - **Cons**: Fixed limit (e.g., 1000 nodes max), overhead if too many partitions.
        - **Used By**: Riak, Cassandra, Elasticsearch.
    2. **Dynamic Partitioning**:
        - **How**: Split big partitions (e.g., >10GB in HBase) into two, merge small ones. Move halves to balance load.
        - **Example**:
            - Partition `A-G` (20GB) splits to `A-C` and `D-G`.
            - Move `D-G` to a new node.
        - **Pros**: Adapts to data size (small data = few partitions).
        - **Cons**: Starts with one partition (initial bottleneck), needs pre-splitting.
        - **Used By**: HBase, RethinkDB.
    3. **Avoid: Hash mod N**:
        - **Why Bad**: `hash(key) % 10` → Node 6; add a node (`% 11`) → most keys move (e.g., 123456 % 10 = 6, % 11 = 3).
        - **Fix**: Use fixed ranges or dynamic splits instead.
- **Analogy**: Fixed = pre-dividing a book into 1000 chapters, moving chapters to new shelves. Dynamic = cutting a big chapter in half when it’s too thick, handing half to a new shelf.

---

### Hands-On Example

- **Car Website (4 Cars, 2 Nodes)**:
    - Cars: ID 191 (red, Honda), 306 (red, Ford), 515 (silver, Ford), 768 (red, Volvo).
    - **Local Index**:
        - Partition 0 (0-499): `color:red → [191, 306]`.
        - Partition 1 (500-999): `color:red → [768]`.
        - Query “red”: Ask both, get `[191, 306, 768]`.
    - **Global Index**:
        - Partition 0 (A-R): `color:red → [191, 306, 768]`.
        - Partition 1 (S-Z): `color:silver → [515]`.
        - Query “red”: Ask Partition 0 only.
    - **Rebalancing**:
        - Add Node 2 (Fixed): Move Partition 1’s `p1` (e.g., 500-749) to Node 2.
        - Dynamic: Split Partition 0 (0-499, 20GB) into 0-249 and 250-499, move 250-499 to Node 2.

---

### Key Takeaways

- **Local Index**: Easy to update (one partition), slow to search (all partitions).
- **Global Index**: Hard to update (multiple partitions), fast to search (one partition).
- **Rebalancing**: Keeps load even as nodes change; fixed partitions move chunks, dynamic splits adaptively.

What’s still unclear? Want more examples, a deeper dive into rebalancing, or help connecting this to earlier partitioning concepts? Let me know!