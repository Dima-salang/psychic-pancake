  

# Data Warehouse Requirements

- In a data warehouse, the requirements help define the business goal you will achieve with your design
- if you don’t have business requirements, don’t build the data warehouse because it won’t be right.

  

Themes >> Critical Success Factor >> Business Operations

### Theme

- a central goal you are trying to achieve
    - to grow sales across all market segments and product lines

### Critical Success Factor

- a group of data elements that are central to achieving that goal
    - analyze sales volume, price and cost trends for PH stores over time

### Business Question

- a specific question that can be tied to data to identify if the critical success factor is being met or not
    - How may men statement shirts were sold in the Philippines during NCAA semifinals 2016 and what was the total net sales?

![[/image 15.png|image 15.png]]

# Dimensional Modeling Process

1. Select the business process to model
2. Declare the grain of the business process
3. Choose the dimensions that apply to each fact table row
4. Identify the facts

  

Dimensional modeling is part science and part art.

  

Example

1. Select the business process to model
    - requires an understanding of both business requirements and available data
    - management wants to better understand customer purchases as captured by the POS system
    - The business process CMI will model is POS retail sales
2. Declare the grain of the business process:
    
    - specify exactly what an individual fact table row represents—the grain conveys the level of detail associated with fact table measurements
    - it is highly recommended to choose the most granular or atomic information captured by the business process. Why?
    - the grain for CMI is SKU (Stock Keeping Unit), Data, and Store
    
      
    
    1. Choose the dimensions
        - determine the ways the data will be aggregated or filtered
        - identify the level of hierarchy associated with each part of the grain
    
      
    
    1. Identify Facts
        - determine the measurements that are available at the chosen grain
        - identify any consolidations, calculations, or conversions to be done
    
      
    
      
    
    ### Business Process
    
    - are the operational activities performed by your organization, such as taking an order, processing an insurance claim, etc…
    - Business process events generate or capture performance metrics that translate into facts in a fact table.
    - Most fact tables focus on the results of a single business process.
    
      
    
    ### Grain
    
    - Declaring the grain is the pivotal step in a dimensional design.
    - it establishes exactly what a single fact table row represents. the grain declaration becomes a binding contract on the design.
    - the grain must be declared before choosing dimensions or facts because every candidate dimensions or fact must be consistent with the grain.
    - Atomic grain refers to the lowest level at which data is captured by a given business process.
    - Each propose fact table grain results in a separate physical table
    - different grains must not be mixed in the same fact table
    - It asks “At what level am I recording the facts (measure)?”
    
      
    
    ---
    
    ## ✅ What is the _Grain_?
    
    **The grain defines exactly what one row in the fact table represents.**
    
    Think of it as the **finest level of detail** captured in your fact table.
    
    > 🧠 It answers the question:
    > 
    > “**At what level** am I recording the facts (measures)?”
    
    ---
    
    ## 🔍 Why is Grain Important?
    
    - It determines what **facts/measures** you can store.
    - It determines what **dimensions** you need to connect.
    - It ensures you don’t mix inconsistent levels of detail (which leads to messy or inaccurate analytics).
    
    ---
    
    ## 🧾 Examples of Grain
    
    Let’s look at a few examples to make this clear.
    
    ### Example 1: Sales
    
    |   |   |
    |---|---|
    |Grain|Meaning|
    |One row per **product per sale transaction**|You’re recording each product in a receipt.|
    |One row per **receipt**|You’re recording total sale per customer order.|
    |One row per **day per product**|You’re aggregating daily product sales (less granular).|
    
    So:
    
    ```Plain
    Grain: One row per product sold in a transaction
    ```
    
    Then your fact table might look like:
    
    |   |   |   |   |   |
    |---|---|---|---|---|
    |Transaction ID|Product ID|Quantity|Price|Date|
    |TXN001|PROD01|2|10.00|2025-05-14|
    |TXN001|PROD02|1|15.00|2025-05-14|
    
    ---
    
    ### Example 2: Education System
    
    You are building a fact table for students' test performance.
    
    ### Option A (Fine Grain):
    
    ```Plain
    Grain: One row per student per subject per exam
    ```
    
    |   |   |   |   |   |
    |---|---|---|---|---|
    |Student|Subject|Exam Date|Raw Score|Grade|
    |Alice|Math|2025-03-12|78|B|
    |Alice|English|2025-03-12|82|A-|
    
    ### Option B (Coarser Grain):
    
    ```Plain
    Grain: One row per student per semester
    ```
    
    |   |   |   |   |
    |---|---|---|---|
    |Student|Semester|Avg Score|GPA|
    |Alice|2nd 2025|80.3|3.6|
    
    🚨 **Changing the grain changes what kind of analysis you can do.**
    
    ---
    
    ## ⚙️ How to Define Grain (in Practice)
    
    Before building your fact table, ask:
    
    1. 🔍 What does one row in this table represent?
    2. 📐 Is it consistent across all rows?
    3. 🧾 Can I describe it precisely with dimension keys?
    
    > Always define the grain before modeling the schema.
    
    ---
    
    ## 🛠️ Sample Fact Table Design (with Grain)
    
    ```Python
    # Grain: One row per student per subject per test
    class ScoreFact(models.Model):
        student = models.ForeignKey(Student, on_delete=models.CASCADE)
        subject = models.ForeignKey(Subject, on_delete=models.CASCADE)
        test = models.ForeignKey(Test, on_delete=models.CASCADE)
    
        raw_score = models.FloatField()
        is_passed = models.BooleanField()
    ```
    
    ---
    
    ## ❌ Common Mistake: Mixed Grain
    
    Imagine a fact table like this:
    
    |   |   |   |   |
    |---|---|---|---|
    |Student|Subject|Score|GPA|
    |Alice|Math|90|3.8|
    |Alice|English|88|3.8|
    
    ⚠️ GPA is at the **student-semester** level, but the row is **student-subject** level. That’s a mixed grain and will lead to incorrect analysis.
    
    ---
    
    ## 🧠 Summary: Key Takeaways
    
    |   |   |
    |---|---|
    |Term|Meaning|
    |Grain|The level of detail (one row = ?) in the fact table|
    |Fine Grain|More detailed (e.g., per transaction, per test)|
    |Coarse Grain|More summarized (e.g., per day, per student-semester)|
    |Must be consistent|All rows should follow the same grain|
    
    ---
    
    Would you like to walk through designing a sample fact table **step by step**, choosing the right grain based on what you want to analyze?
    
    ---
    
    ## 🔷 What Is Dimensional Modeling?
    
    **Dimensional modeling** is a **design technique** optimized for **data retrieval**, especially in **data warehouses** and **OLAP (Online Analytical Processing)** systems.
    
      
    
    > ✅ It's all about structuring your data so non-technical users can easily understand and query it for reporting and insights.
    
    It was popularized by **Ralph Kimball**, a pioneer in data warehousing.
    
    ---
    
    ## 🔶 Core Concepts: Fact + Dimensions
    
    At the heart of dimensional modeling are two types of tables:
    
    ### 1. 🧮 **Fact Tables**
    
    - Store **quantitative data** — the **measurable** stuff.
    - Primary table which stores the performance measurements of the business
    - The term fact refers to a business measure
    - Each row in a fact table corresponds to a specific measurement
    - Each measurement is taken at the intersection of all the relevant dimensions (e.g., day, product, and store)—this list of dimensions defines the “grain” of the fact table
    - All measurements in a fact table must be at the same grain
    - Facts are either additive, semiadditive, or nonadditive—most are numeric
    - Contains two or more foreign keys to dimension tables
    - Expresses the many-to-many relationships between dimensions in dimensional models
    - These are the **center** of analysis.
    - A single fact table row has a one-to-one relationship to a measurement event as described by the fact table’s grain. Thus a fact table corresponds to a physical observable event, and not to the demands of a particular report.
    - Examples: test scores, sales amount, click counts.
    
    |   |   |   |   |   |
    |---|---|---|---|---|
    |student_id|test_id|subject_id|score|time_spent|
    
    ### 2. 🧾 **Dimension Tables**
    
    - Store **descriptive attributes** — used to slice, filter, and group facts.
    - Contains the textual descriptors of the business
    - Usually low in cardinality, but very wide (50-100 attributes not uncommon)
    - Dimension attributes used as query constraints, groupings, and report labels
    - Often contain hierarchical relationships (city⇒state⇒region)
    - The more descriptive the dimension attributes, the better
    - The analytic power of the DW/BI environment is directly proportional to the quality and depth of the dimension attributes.
    - it is sometimes called the soul of the data warehouse system because they are the drivers of the user’s BI experience.
    - Think: who, what, where, when, how.
    
    Examples:
    
    - `StudentDim` → name, gender, age
    - `SubjectDim` → name, grade level, department
    - `DateDim` → day, week, month, year
    
    ---
    
      
    
    Fact Table + Dimensional Tables = Dimensional Model (Star Schema)
    
    Benefits of a dimensional model:
    
    - Simplicity
        - easy for business users to understand
        - improved query performance
    - Extensibility
        - easily accommodates change (but not that easily!)
    
      
    
    ## 🔷 Schema Types
    
    ### 1. ⭐ Star Schema (Most Common)
    
    - One central **fact table** linked to **dimension tables** directly.
    
    ```Plain
                    +-------------+
                    |  Date Dim   |
                    +-------------+
                          |
                          v
    +-------------+   +----------------+   +----------------+
    | Student Dim |<--|  Scores Fact   |-->| Subject Dim    |
    +-------------+   +----------------+   +----------------+
    ```
    
    ✅ **Simple, fast**, and intuitive. Great for most reporting needs.
    
    ---
    
    ### 2. ❄️ Snowflake Schema
    
    - Dimension tables are **normalized**, forming a “snowflake” structure.
    
    ```Plain
    +---------------+
    | DepartmentDim |
    +---------------+
            ^
            |
    +--------------+
    | SubjectDim   |
    +--------------+
            ^
            |
    +--------------+        +-------------+
    | Scores Fact  |<------>| Student Dim |
    +--------------+        +-------------+
    ```
    
    ✅ Slightly more complex, but reduces redundancy in dimensions.
    
    ---
    
    ## 🔷 Fact Table Types
    
    |   |   |
    |---|---|
    |Type|Example|
    |**Transactional**|One row per event (test taken, login)|
    |**Snapshot**|Summary at regular intervals (daily avg)|
    |**Accumulating**|Tracks progress (e.g. application stage)|
    
    ---
    
    ## 🔷 Measures vs Attributes
    
    - **Measures** → Numeric data in fact tables: `score`, `time_spent`, `attempt_count`
    - **Attributes** → Descriptors in dimension tables: `gender`, `subject_name`, `test_difficulty`
    
    ---
    
    ## 🔷 Step-by-Step: Dimensional Modeling for You
    
    Suppose you're building a **student assessment analytics app**.
    
    ### Step 1: Identify Business Process
    
    - Student takes a test → stores scores and metadata.
    
    ### Step 2: Declare Grain
    
    - One row = one student’s performance on one test.
    
    ### Step 3: Identify Dimensions
    
    - **Student**: ID, name, gender, age
    - **Test**: ID, subject, type (quiz/final), difficulty
    - **Date**: day, week, month, year
    
    ### Step 4: Identify Facts
    
    - **Score**
    - **Time taken**
    - **Attempts**
    - **Predicted score** (from your ML model)
    
    ### Step 5: Design Schema
    
    **Fact Table:** `**StudentPerformanceFact**`
    
    | student_id | test_id | date_id | raw_score | time_spent | predicted_score |
    
    **Dimension Tables:**
    
    - `StudentDim(student_id, name, gender, age, school_id)`
    - `TestDim(test_id, subject, difficulty, type)`
    - `DateDim(date_id, day, week, month, year)`
    
    ---
    
    ## 🔷 Benefits of Dimensional Modeling
    
    ✅ Easy for non-technical users to understand (BI, dashboards)
    
    ✅ Fast aggregations (optimized for GROUP BY queries)
    
    ✅ Works well with OLAP engines
    
    ✅ Scales across many subject areas (data marts)
    
    ---
    
    ## 🔧 Tools That Use Dimensional Modeling
    
    |   |   |
    |---|---|
    |Tool|Purpose|
    |DBT|Define models in SQL (ELT)|
    |Airflow|ETL orchestration|
    |Superset|Visualize dimensional data|
    |Power BI|Drag-and-drop dimension slices|
    |Django ORM|You can design dim models via models + queries|
    |BigQuery|Star schema works great here|
    
    ---
    
    ## 🧠 Final Takeaways
    
    - **Dimensional modeling** is NOT about transactional systems — it’s for **insights**.
    - Focus on **fact tables for measurable data**, and **dimension tables for filters**.
    - Choose **star schema** for simplicity unless normalization gives strong benefits.
    - Integrate with **ETL pipelines** and **dashboards** for a complete system.
    
    ---
    

  

# Examples of Facts and Dimensions

## 🔷 1. **Education Analytics System** (e.g., Student Performance Dashboard)

|   |   |
|---|---|
|**Fact Table**|**Dimension Tables**|
|`StudentPerformanceFact`|`StudentDim`: name, gender, birthdate|
|• raw_score|`TestDim`: subject, test_type, difficulty|
|• predicted_score|`DateDim`: day, week, month, year|
|• time_spent|`InstructorDim`: name, department, experience|
|• attempt_count||

✅ **Facts**: numerical/measurable → scores, time, attempts

✅ **Dimensions**: descriptors → student info, test details, dates

---

## 🔷 2. **E-commerce System**

|   |   |
|---|---|
|**Fact Table**|**Dimension Tables**|
|`SalesFact`|`CustomerDim`: name, gender, location|
|• quantity_sold|`ProductDim`: category, brand, price|
|• total_price|`DateDim`: day, month, year|
|• discount_applied|`StoreDim`: location, manager, region|

✅ You can now ask:

> “How many iPhones did women in New York buy in Q1 2024?”

---

## 🔷 3. **Hospital or Health System**

|   |   |
|---|---|
|**Fact Table**|**Dimension Tables**|
|`PatientVisitFact`|`PatientDim`: age, gender, insurance type|
|• visit_cost|`DoctorDim`: specialty, years_experience|
|• days_admitted|`HospitalDim`: name, type, region|
|• medication_count|`DateDim`: admission/discharge dates|

✅ Enables analyses like:

> “Average cost of hospitalization by doctor specialty and region.”

---

## 🔷 4. **Online Learning Platform (e.g., Coursera, Udemy)**

|   |   |
|---|---|
|**Fact Table**|**Dimension Tables**|
|`CourseEngagementFact`|`UserDim`: age, country, education level|
|• videos_watched|`CourseDim`: subject, difficulty, instructor|
|• quiz_score|`DateDim`: when activity occurred|
|• completion_percent|`DeviceDim`: mobile, desktop, tablet|

---

## 🔷 5. **Mobile App Analytics**

|   |   |
|---|---|
|**Fact Table**|**Dimension Tables**|
|`UserEventFact`|`UserDim`: device_id, OS, user_type|
|• session_duration|`EventDim`: event_type, page, action_name|
|• actions_taken|`DateDim`: session_start_date|

---

## ✅ Summary Table

|   |   |
|---|---|
|**Fact (Measurable)**|**Dimension (Descriptive)**|
|Sales amount|Product name, brand, category|
|Test score|Subject, difficulty, student gender|
|Calories burned|Exercise type, user weight|
|Page views|Page type, user location, device type|
|Medication cost|Patient age group, illness, doctor|

---

## 🔍 Pro Tip for Developers

In Django, facts would live in **models with numeric fields**, and dimensions would be models with **categorical/textual metadata**.

Here’s a quick Django-style example for an education app:

```Python
class Student(models.Model):
    name = models.CharField(...)
    gender = models.CharField(...)
    age = models.IntegerField(...)

class Test(models.Model):
    subject = models.CharField(...)
    difficulty = models.CharField(...)

class PerformanceFact(models.Model):
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    test = models.ForeignKey(Test, on_delete=models.CASCADE)
    score = models.FloatField()
    time_spent = models.IntegerField()
    date_taken = models.DateField()
```

Would you like a full ERD (Entity Relationship Diagram) or SQL schema next?

  

  

Great questions! Let’s dive into three key **foundations of data warehousing**:

---

## 🔹 1. **Additivity in Facts**

Understanding additivity is essential when designing **fact tables** because it tells you **how facts can be aggregated** across dimensions.

### ✅ Types of Additivity:

|   |   |
|---|---|
|Type|Description|
|**Additive**|Can be summed across all dimensions.|
|**Semi-additive**|Can be summed across some dimensions, but not all.|
|**Non-additive**|Cannot be summed across any dimension.|

### 🔸 Examples:

|   |   |   |
|---|---|---|
|Metric|Additivity Type|Why?|
|`total_sales`|Additive|You can add sales across time, stores, products.|
|`account_balance`|Semi-additive|You can sum across accounts, but not time (balance at a point in time).|
|`percentage_score`|Non-additive|You can't sum percentages meaningfully across students or tests.|

### 🧠 Best Practice:

- Choose **fact metrics** that are **additive** whenever possible for easier reporting.
- Use **averages, ratios, or medians** as **derived facts**, not raw stored facts.

---

## 🔹 2. **Surrogate Keys**

### ❓ What Are They?

Surrogate keys are **artificial primary keys** (usually integers) used in dimension tables instead of using business or natural keys.

### ✅ Why Use Surrogate Keys?

|   |   |
|---|---|
|Reason|Explanation|
|**Immutability**|Business keys can change (e.g., a product code might be updated).|
|**Efficiency**|Integers are smaller and faster to join and index.|
|**Support Slowly Changing Dimensions**|You can track historical versions using surrogate keys.|

### 🧱 Example:

Instead of using a `student_id = '2025-CS-00123'` as a foreign key:

```Python
student_sk = 12345  # surrogate key
```

- This `student_sk` maps internally to the business key.
- Great for tracking history when a student changes departments or names.

---

## 🔹 3. **Data Warehouse Architectures**

These define **how data flows** from source systems to the final reporting or analytical systems.

One can distinguish among them by dividing the data warehouse system into three parts:

- The data warehouse itself that contains the data and associated software
- Data acquisition (back-end) software that extracts data from different source systems
- Client (front-end) software that allows users to access and analyze data from the warehouse.

  

![[/image 1 11.png|image 1 11.png]]

---

### 🔸 A. **One-Tier Architecture**

### 📦 What:

- Combines **data source**, **warehouse**, and **presentation** in a single tier.

### ✅ Pros:

- Simpler design.
- Minimal latency.

### ❌ Cons:

- Poor scalability.
- Not modular; hard to maintain or optimize.

### ⚙️ Used in:

- Small organizations or prototypes.

---

### 🔸 B. **Two-Tier Architecture**

### 📦 What:

- Separates the **data warehouse layer** from the **presentation layer**.
- ETL is still often tightly integrated.

### ✅ Pros:

- Somewhat scalable.
- Separation of logic and presentation.

### ❌ Cons:

- Integration and performance bottlenecks.
- May still lack flexibility.

![[/image 2 9.png|image 2 9.png]]

---

### 🔸 C. **Three-Tier Architecture** (most common in modern data warehousing)

### 📦 Layers:

1. **Bottom Tier (Data Sources):**
    - OLTP databases, logs, files, APIs.
2. **Middle Tier (Data Warehouse Server):**
    - ETL tools (e.g., Apache Airflow, Talend)
    - Central data warehouse (e.g., Snowflake, PostgreSQL, Redshift)
    - Dimensional modeling (star/snowflake schemas)
3. **Top Tier (Presentation Layer):**
    - BI tools (e.g., Tableau, Power BI, Metabase)
    - Reports, dashboards, analytics interfaces

![[/image 3 8.png|image 3 8.png]]

### ✅ Pros:

- Modular, scalable, and maintainable.
- Ideal for large-scale, multi-source, real-time analytics.

### ❌ Cons:

- Complexity.
- Longer development/setup time.

---

## 🔄 Summary Table

|   |   |
|---|---|
|Concept|Key Idea|
|**Additive Fact**|Can be summed across all dimensions|
|**Semi-additive Fact**|Can be summed over some, not all dimensions|
|**Surrogate Key**|System-generated unique key for stable joins|
|**One-Tier DW**|All-in-one; fast but limited|
|**Two-Tier DW**|Some separation; better for medium complexity|
|**Three-Tier DW**|Best practice for scale, maintainability, analytics|

---

  

## Ten Factors

Ten factors that potentially affect the architecture selection decision:

1. Information interdependence between organizational units
2. upper management’s information needs
3. urgency of need for a data warehouse
4. nature of end-user tasks
5. constraints on resources
6. strategic view of the data warehouse prior to implementation
7. compatibility with existing systems
8. perceived ability of the in-house IT staff
9. technical issues
10. social/political factors

  

  

  

### Nulls in Fact Tables

Nulls must be avoided in the fact table’s foreign keys because these nulls would automatically cause a referential integrity violation. Rather than a null foreign key, the associated dimension table must have a default row (and surrogate key) representing the unknown or not applicable condition.

  

---

## ✅ **1. Nulls in Fact Tables (Foreign Keys)**

**Why nulls are a problem:**

- Fact tables have **foreign keys pointing to dimension tables**.
- If a foreign key is null, it means there’s no corresponding dimension row, which **breaks referential integrity**.

**Solution:**

- Instead of nulls, dimension tables must contain a **default “Unknown” or “Not Applicable” row**, e.g.:
    - `CustomerID = -1, Name = 'Unknown Customer'`
- The fact table then refers to this row rather than being null.

**Why?**

- This makes queries simpler and allows consistent reporting (e.g., “sales with unknown customer”).

---

## 🔁 **2. Conformed Facts**

These are **metrics (measures) that are defined consistently across fact tables and subject areas**.

- E.g., `Total Sales`, `Profit`, `Order Count`
- Used **across different fact tables** — say, in both `OnlineSales` and `RetailSales` — with the **same meaning, logic, and granularity**.

**Why conformed facts matter:**

- They ensure **consistency** in analytics and **trust in numbers** across dashboards and departments.

---

## 📥 **3. Transaction Fact Tables**

These record **each business transaction as it happens**, with the **most granular level**.

**Example:**

- Every individual purchase in an online store.
- Columns: `TransactionID`, `CustomerID`, `ProductID`, `Quantity`, `Price`, `DateKey`, etc.

**Use case:**

- Good for detailed analysis (drill down).
- Always grows — never updated.

---

## 📸 **4. Periodic Snapshot Fact Tables**

These record **summaries of performance at regular intervals** (daily, weekly, monthly).

**Example:**

- Daily inventory levels of all products.
- Columns: `ProductID`, `DateKey`, `WarehouseID`, `QuantityOnHand`, `QuantityOnOrder`

**Use case:**

- Used for **trend analysis** over time.
- Data is captured on a schedule.

---

## 📚 **5. Accumulating Snapshot Fact Tables**

These track **the lifecycle of a business process** — one row per entity (e.g., an order), and updates happen as it moves through stages.

**Example:**

- Order fulfillment process:
    - `OrderPlacedDate`, `OrderShippedDate`, `OrderDeliveredDate`
- One row per order, updating each date as it progresses.

**Use case:**

- **Process-oriented** analysis.
- Ideal for **SLA tracking**, bottlenecks, or funnel analysis.

---

## ❌ **6. Factless Fact Tables**

These contain **only foreign keys** — no numeric facts.

### Two types:

- **Event tracking**: "A student attended a class"
- **Coverage**: "This product is sold in this store"

**Why?**

- Great for **counting events**, answering “did it happen?” and **many-to-many relationships**.

---

## 📊 **7. Aggregate Fact Tables**

These store **pre-aggregated data** at a higher level than the base transaction data.

**Example:**

- Instead of storing every sale, an aggregate fact table stores:
    - `DailyTotalSales` per `Product`, `Store`, `Date`

**Why?**

- **Improves performance** for summary queries.
- Useful when queries don’t need transaction-level detail.

---

## 🧩 **8. Consolidated Fact Tables**

These **combine facts from multiple processes** into a single table, usually for ease of reporting or performance.

**Example:**

- Combining sales and returns into one fact table with a `TransactionType` column.

**Tradeoffs:**

- Easier for reporting.
- More complex ETL and more columns.

---

### 🧠 Summary Table:

|   |   |
|---|---|
|Fact Table Type|Purpose / Use Case|
|Transaction Fact Table|Captures atomic events (e.g., individual sales)|
|Periodic Snapshot|Tracks regular intervals (e.g., daily inventory)|
|Accumulating Snapshot|Tracks progress through stages (e.g., order lifecycle)|
|Factless Fact Table|Records events/relationships without measures|
|Aggregate Fact Table|Pre-summarized for performance|
|Consolidated Fact Table|Combines multiple fact types for unified view|

---

## 🧰 What to Use When

|   |   |
|---|---|
|Scenario|Best Fact Table Type|
|Real-time purchase tracking|Transaction Fact Table|
|Weekly student test scores|Periodic Snapshot|
|Student enrollment lifecycle|Accumulating Snapshot|
|Students attending a seminar|Factless Fact Table|
|Fast dashboards with sales by category|Aggregate Fact Table|
|Unified report for all academic events|Consolidated Fact Table|

---

  

---

## 🔹 1. **Transaction Fact Table**

### 📌 Purpose: Capture a single event or transaction.

### ✅ Example: `fact_test_scores`

|   |   |   |   |
|---|---|---|---|
|student_id|subject_id|test_date|score|
|1001|MATH101|2024-12-05|88|
|1002|ENG201|2024-12-05|76|
|1001|ENG201|2024-12-10|81|

- Every row is a test attempt by a student.
- Granularity = one row per student per subject per test date.

---

## 🔹 2. **Periodic Snapshot Fact Table**

### 📌 Purpose: Capture state of data at regular intervals.

### ✅ Example: `fact_daily_student_performance_snapshot`

|   |   |   |   |
|---|---|---|---|
|student_id|snapshot_date|cumulative_average|total_tests_taken|
|1001|2024-12-01|82.5|6|
|1001|2024-12-08|83.1|7|
|1002|2024-12-08|76.0|3|

- A new row is added weekly or daily to capture current performance stats.
- Great for trend analysis.

---

## 🔹 3. **Accumulating Snapshot Fact Table**

### 📌 Purpose: Track the lifecycle of a process.

### ✅ Example: `fact_enrollment_lifecycle`

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|enrollment_id|student_id|course_id|enrolled_date|dropped_date|completed_date|
|3001|1001|CS101|2024-09-01|NULL|2024-12-15|
|3002|1002|CS101|2024-09-01|2024-10-01|NULL|

- One row per student-course enrollment.
- Dates are updated as student progresses through stages.

---

## 🔹 4. **Factless Fact Table (Event Tracking)**

### 📌 Purpose: Track occurrences without numeric measures.

### ✅ Example: `fact_class_attendance`

|   |   |   |
|---|---|---|
|student_id|class_id|date|
|1001|ENG101|2024-11-15|
|1002|MATH101|2024-11-15|
|1001|MATH101|2024-11-16|

- No numeric values — just tracking **who attended what and when**.
- You can still **count** events in queries (e.g., attendance rate).

---

## 🔹 5. **Aggregate Fact Table**

### 📌 Purpose: Pre-summarize data to improve performance.

### ✅ Example: `fact_weekly_average_scores`

|   |   |   |   |
|---|---|---|---|
|subject_id|week_start|avg_score|test_count|
|MATH101|2024-12-01|82.1|132|
|ENG201|2024-12-01|77.4|98|

- This is **summarized from** `**fact_test_scores**`.
- Instead of querying 100,000 rows, you use 100 for reporting.

---

## 🔹 6. **Consolidated Fact Table**

### 📌 Purpose: Combine multiple processes into one fact table.

### ✅ Example: `fact_student_activity_log`

|   |   |   |   |
|---|---|---|---|
|student_id|event_type|event_date|details|
|1001|TEST_COMPLETED|2024-12-05|score=88|
|1001|CLASS_ATTENDED|2024-12-06|class_id=MATH101|
|1001|ASSIGNMENT_UPLOADED|2024-12-07|file=sub1.pdf|

- Combines multiple event types: tests, attendance, submissions.
- Used in **dashboards** showing a unified timeline of student activity.

---

## 🔹 7. **Nulls in Fact Table FK Example (Solved via Default Row)**

### ✅ Problem Table (Bad):

|   |   |   |
|---|---|---|
|student_id|course_id|score|
|NULL|CS101|80|

→ Bad foreign key: breaks referential integrity.

### ✅ Fixed Table with Default Dimension Row:

**In** `**dim_student**`**:**

|   |   |
|---|---|
|student_id|name|
|-1|Unknown Student|

**In** `**fact_test_scores**`**:**

|   |   |   |
|---|---|---|
|student_id|course_id|score|
|-1|CS101|80|

---

### ⚒️ Next Steps to Practice

If you want to build and practice these:

1. **Create dimension and fact tables in PostgreSQL or SQLite.**
2. Load data via Python (Pandas/SQLAlchemy).
3. Use Django ORM to model fact/dimension schemas and seed sample data.
4. Visualize it in Superset or Metabase.

  

  

Here are simple examples and explanations for each **Basic Dimension Table Technique** from Kimball’s dimensional modeling approach. These examples use a **Retail Sales** context for clarity.

---

### 1. **Dimension Table Structure**

**Concept**: Dimension tables have a single primary key and are denormalized. Dimension tables are usually wide, flat denormalized tables with many low-cardinality text attributes.

**Example**:

```SQL
Customer Dimension:
+-------------+------------+-------------+------------+
| CustomerKey | Name       | Gender      | Region     |
+-------------+------------+-------------+------------+
| 101         | Alice Wong | Female      | West       |
| 102         | John Smith | Male        | East       |
```

- `CustomerKey` is a surrogate primary key.
- Attributes like `Name`, `Gender`, `Region` are textual and used in reports.

---

### 2. **Dimension Surrogate Keys**

**Concept**: Use anonymous surrogate keys instead of natural keys. A dimension table is designed with one column serving as a unique PK. This PK cannot be the operational system’s natural key because there will be multiple dimension rows for that natural key when changes are tracked over time.

**Example**:

- Natural key: `Cust_ID = 'A123'` (from CRM)
- Surrogate key: `CustomerKey = 101` (generated by DW)

---

### 3. **Natural, Durable, and Supernatural Keys**

**Concept**: Durable keys never change and survive natural key changes. Natural keys created by operational source systems are subject to business rules outside the control of the DW system. the best durable keys have a format that is independent of the original business process and thus should be simple integers.

**Example**:

- `EmployeeDurableKey = 9001` stays constant
- Natural key: `Emp_ID = 'E123'` (may change if rehired)

---

### 4. **Drilling Down**

**Concept**: Add dimension attributes to GROUP BY for more granularity.

**Example**:

```SQL
SELECT Region, Gender, SUM(Sales)
FROM SalesFact
JOIN CustomerDim ON SalesFact.CustomerKey = CustomerDim.CustomerKey
GROUP BY Region, Gender;
```

---

### 5. **Degenerate Dimensions**

**Concept**: Dimension value (like InvoiceNumber) exists in the fact table but has no dimension table.

**Example**:

```SQL
SalesFact:
+-------------+---------------+--------+
| InvoiceNo   | ProductKey    | Sales  |
+-------------+---------------+--------+
| INV001      | 2001          | 100.00 |
```

---

### 6. **Denormalized Flattened Dimensions**

**Concept**: Don’t normalize; store all useful attributes in one wide table.

**Example**:

```SQL
ProductDim:
+------------+------------+-----------+--------+
| ProductKey | Category   | SubCat    | Brand  |
+------------+------------+-----------+--------+
| 2001       | Electronics| Audio     | Sony   |
```

- Instead of separate `Category`, `Brand` tables.

---

### 7. **Multiple Hierarchies in Dimensions**

**Concept**: Support more than one hierarchy in the same dimension.

**Example**:

```SQL
DateDim:
+----------+--------+------------+-------------+
| DateKey  | Month  | FiscalQtr  | CalendarQtr |
+----------+--------+------------+-------------+
| 20240510 | May    | Q2         | Q2          |
```

---

### 8. **Flags and Indicators as Textual Attributes**

**Concept**: Avoid cryptic codes; use readable text.

**Example**:

```SQL
CustomerDim:
+-------------+------------+------------------+
| CustomerKey | IsActive   | IsLoyaltyMember  |
+-------------+------------+------------------+
| 101         | Active     | Yes              |
```

---

### 9. **Null Attributes in Dimensions**

**Concept**: Replace nulls with descriptive placeholders.

**Example**:

```SQL
ProductDim:
+----------+-----------+
| Product  | Color     |
+----------+-----------+
| Widget A | Unknown   |
```

---

### 10. **Calendar Date Dimensions**

**Concept**: Use rich date dimension, possibly with meaningful primary key.

**Example**:

```SQL
DateDim:
+----------+----------+-----------+-----------------+
| DateKey  | Date     | Weekday   | IsHoliday       |
+----------+----------+-----------+-----------------+
| 20240510 | 2025-05-10| Saturday | No              |
```

---

### 11. **Role-Playing Dimensions**

**Concept**: Same physical dimension plays different roles.

**Example**:

```SQL
SalesFact:
+------------+------------+------------+
| OrderDate  | ShipDate   | ReturnDate |
| (FK to DateDim with aliases)         |
```

---

### 12. **Junk Dimensions**

**Concept**: Combine miscellaneous flags into one dimension.

**Example**:

```SQL
TransactionProfileDim:
+------------+-----------+-------------+--------------+
| ProfileKey | IsPromo   | PaymentType | IsWebOrder   |
+------------+-----------+-------------+--------------+
| 1          | Yes       | Credit Card | No           |
```

---

### 13. **Snowflaked Dimensions**

**Concept**: Normalize dimension (not recommended for BI).

**Example**:

```Plain
ProductDim --> joins to BrandDim
          --> joins to CategoryDim
```

---

### 14. **Outrigger Dimensions**

**Concept**: Dimension references another dimension (use sparingly).

**Example**:

```SQL
AccountDim:
+-------------+-------------+---------------+
| AccountKey  | Type        | OpenDateKey   | -- FK to DateDim
+-------------+-------------+---------------+
```

---

### 15. **Conformed Dimensions**

**Concept**: Reused, consistent dimensions across fact tables.

**Example**:

- `ProductDim` used in both `SalesFact` and `InventoryFact` with identical structure and contents.

---

### 16. **Shrunken Dimensions**

**Concept**: Aggregated version of a dimension for summary fact tables.

**Example**:

```SQL
ProductBrandDim:
+------------+--------+
| BrandKey   | Brand  |
+------------+--------+
| 101        | Nike   |
```

---

### 17. **Drilling Across**

**Concept**: Use conformed dimensions to query multiple fact tables.

**Example**:

```SQL
-- Sales and Returns, grouped by Product
SELECT P.Brand, SUM(S.SalesAmount), SUM(R.ReturnAmount)
FROM SalesFact S
JOIN ReturnsFact R ON S.ProductKey = R.ProductKey
JOIN ProductDim P ON S.ProductKey = P.ProductKey
GROUP BY P.Brand;
```

---

### 18. **Value Chain**

**Concept**: Multiple processes in order (e.g., Purchasing → Inventory → Sales)

**Example**:

- `PurchaseFact`, `InventoryFact`, and `SalesFact` each have their own schema but share conformed dimensions.

---

### 19. **Enterprise Data Warehouse Bus Architecture**

**Concept**: Build incrementally using business processes, integrate via conformed dimensions.

**Example**:

- Start with Sales → add Inventory → add Returns
- Each has fact tables; all use conformed `CustomerDim`, `ProductDim`, `DateDim`.

---

Would you like these summarized into a PDF or flashcard format for studying?

  

  

Dealing with **Slowly Changing Dimensions (SCDs)** is exactly what Kimball’s dimension techniques (Types 0–6) are designed to address. SCDs are dimension attributes (e.g., customer address, employee title, product category) that **change over time**, but not frequently — hence “slowly changing.”

Let’s go deeper into:

---

## 🧠 What are Slowly Changing Dimensions?

- In a **data warehouse**, dimensions like `Customer`, `Product`, or `Employee` describe the "who" and "what" of your facts.
- Over time, these attributes may **change slowly**: e.g.,
    - A customer moves to a new address.
    - An employee changes job roles.
    - A product changes category or branding.

The challenge is deciding how to **handle these changes** without:

- Losing historical accuracy
- Misleading reports (e.g., attributing sales to the wrong region)

---

## 🛠️ How to Handle SCDs: Choosing the Right Technique

The technique you use depends on your **business needs** and how much historical context you want to keep.

---

### ✅ **If you don't care about history (e.g., fix typos)** → Use **Type 1**

- **Overwrites** the old value
- Good for: minor fixes, non-analytical attributes
- ⚠️ Bad for: historical tracking

**Example:**

```SQL
UPDATE customer_dim
SET address = '456 Oak St'
WHERE customer_id = 123;
```

---

### ✅ **If you must preserve history fully** → Use **Type 2**

- **Adds a new row** for each change, with a new surrogate key
- Best for: any attribute where historical context matters
- Enables point-in-time reporting (e.g., “How many customers lived in Region A in 2019?”)

**Example Structure:**

```SQL
| cust_sk | cust_id | address     | valid_from | valid_to   | is_current |
|---------|---------|-------------|------------|------------|------------|
| 1001    | 123     | 123 Elm St  | 2020-01-01 | 2022-05-01 | N          |
| 1054    | 123     | 456 Oak St  | 2022-05-01 | NULL       | Y          |
```

---

### ✅ **If you want limited history (e.g., only current vs previous)** → Use **Type 3**

- Adds a `previous_value` column
- Good for: simple comparison use cases, e.g., previous department, region
- ⚠️ Only stores **1 level of history**

**Example Columns:**

```SQL
| cust_id | current_region | previous_region |
|---------|----------------|-----------------|
| 123     | West           | East            |
```

---

### ✅ **If you want full history but with a clean current-state table** → Use **Type 4**

- Separate historical and current dimension tables
- Good for: very large dimensions, compliance/audit logging

---

### ✅ **If some attributes change often and others rarely** → Use **Type 5 (Mini-Dimension)**

- Store fast-changing attributes in a separate mini-dimension
- Avoid bloating the main dimension with frequent SCD Type 2 changes

---

### ✅ **If you want the best of all worlds (complex but flexible)** → Use **Type 6 (Hybrid)**

- Combines Type 1 + 2 + 3
- Useful when you need:
    - Full row history (Type 2)
    - Easy access to current and previous states (Type 3)
    - Overwrite fixable attributes (Type 1)

---

## 🧩 Example Scenario: Customer Address Change

|   |   |
|---|---|
|Business Need|Recommended SCD Type|
|Don't care about past addresses|Type 1|
|Need to analyze customers by past addresses|Type 2|
|Only need to show current and previous address|Type 3|
|Need to audit every change separately|Type 4|
|Only birthdate changes often|Type 5 (Mini-dimension for age group)|
|Need full row history + previous state columns|Type 6|

---

## 📈 Real-World Tips

1. **Track attribute volatility**: Split your dimension into volatile vs stable attributes.
2. **Automate SCD logic**:
    - Use ETL tools like **dbt**, **Airflow**, or **Informatica** to automate comparison logic.
    - Use `MD5` hashes to detect changes.
3. **Use surrogate keys**, not natural keys, in fact tables — this allows changes without breaking historical facts.
4. **Add metadata**: `is_current`, `effective_date`, `expiration_date` for easier querying.

---

## 🔍 Summary Table

|   |   |   |   |   |
|---|---|---|---|---|
|SCD Type|Description|Preserves History?|Complexity|Use Case|
|Type 0|Retain original|✅|Low|Immutable data|
|Type 1|Overwrite|❌|Low|Correct errors, non-analytical data|
|Type 2|Add new row|✅✅✅|High|Time-based analysis, compliance|
|Type 3|Add new attribute|✅ (1 step)|Medium|Track current and previous only|
|Type 4|Separate history table|✅✅✅|High|Big data volumes, clean current dimension|
|Type 5|Mini-dimension + Type 1|✅ (mini only)|Medium|Frequently changing attributes|
|Type 6|Type 1 + 2 + 3 hybrid|✅✅✅|Very High|Complex reporting, analytics, point-in-time|

---

Would you like a **visual example**, or to see **ETL logic in SQL or Python** to implement SCD handling in practice?