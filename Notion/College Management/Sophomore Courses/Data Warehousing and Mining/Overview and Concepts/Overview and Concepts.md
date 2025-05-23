Companies starting from the 1960s started to depend on computer systems.

In the 1990s, as globalization become more prominent, businesses became more complex, competitive, and so their needs also evolved to stay competitive. business executives become hungry for information.

  

# Escalating Need for Strategic Information

### Who needs strategic information in an enterprise?

- the executives and managers who are responsible for keeping the enterprise competitive

### What is strategic information?

- combined essential information needed to make decisions in the formulation and execution of business strategies and objectives

  

# Strategic Information

- strategic information is not for running the day-to-day operations of the business
- strategic information is far more important for the continued health and survival of the corporation
- critical business decisions depend on the availability of proper strategic information in an enterprise.

## Characteristics

- Integrated
    - must have a single, enterprise-wide view
- Data Integrity
    - information must be accurate and must conform to business rules
- Accessible
    - easily accessible with intuitive access paths, and responsive for analysis
- Credible
    - every business factor must have one and only one value
    - basically consistent
- Timely
    - information must be available within the stipulated time frame

# Information Crisis

Whatever the size of the company is, we are faced with two facts:

- Organizations have lots of data
- Information technology resources and systems are not effective at turning all that data into useful strategic information

![[/image 14.png|image 14.png]]

### Inability to provide information

- User needs information
- user requests reports from IT
- IT places request on backlog
- IT creates ad hoc queries
- IT sends requested reports
- user hopes to find the right answers
- user needs information (loops back)

![[/image 1 10.png|image 1 10.png]]

### Operational vs. Decision-Support Systems

|   |   |
|---|---|
|Operational|Decision-Support Systems|
|making the wheels of business turn|watching the wheels of business turn|
|take an order|show me the top-selling products|
|process a claim|show me the problem regions|
|make a shipment|tell my why (drill down)|
|generate an invoice|let me see other data (drill across)|
|receive cash|show the highest margins|
|reserve an airline seat|alert me when a district sells below target|

  

## The Need for Informational Systems

As businesses evolve, continuing traditional past practices will only lead to disaster. We need to design and build informational systems

- that serve different purposes
- whose scopes are different
- whose data content is different
- where the data usage patterns are different
- where the data access types are different

  

## Inability to Provide Information

![[/image 2 8.png|image 2 8.png]]

  

  

![[/image 3 7.png|image 3 7.png]]

- The type of information needed for strategic information making is different from operational systems. We need a new type of system environment. The desired features are:
    - Database designed for analytical tasks
    - Data from multiple applications
    - Easy to use and conducive to long interactive sessions by users
    - Read-intensive data usag
    - Direct interaction with the system by the users without IT assistance
    - content updated periodically and stable
    - content to include current and historical data
    - ability for users to run queries and get results online
    - ability for users to initiate reports

# Data Warehouse

> [!important] A physical repository where relational data (current and historical) are specially organized to provide enterprise-wide, cleansed data in a standardized format.

The data warehouse is an informational environment that:

- Provides an integrated and total view of the enterprise
- makes the enterprise’s current and historical information easily available for strategic decision-making
- makes decision-support transactions possible without hindering operational systems
- renders the organization’s information consistent
- presents a flexible and interactive source of strategic information

  

The data warehouse is born out of the need for strategic information and is the result of the search for a new way to provide such information. The new concept is not to generate fresh data, but to make use of the large volumes of existing data and to transform it into forms suitable for providing strategic information. **Take all the data you already have in the organization, clean and transform it, and then provide useful strategic information.**

  

A data warehouse is not a single software or hardware product. It is a computing environment where users can find strategic information, an environment where users are put directly in touch with the data they need to make better decisions. it is a user-centric environment.

  

![[/image 4 6.png|image 4 6.png]]

  

# OLTP vs OLAP

|   |   |   |
|---|---|---|
|Feature|OLTP (e.g., Django DB)|OLAP (Data Warehouse)|
|Purpose|Daily operations (CRUD)|Analytics, reporting|
|Schema|Highly normalized (3NF)|De-normalized (star/snowflake)|
|Query types|Short, transactional queries|Long-running aggregations, trends|
|Data volume|Small to medium|Very large (TBs to PBs)|
|Examples|User registration, orders|"Average scores by department/year"|

# Evolution of BI

Companies began to perceive that the goal of decision-support systems is twofold

- transformation of data to information
- derivation of knowledge from information.

Therefore, BI for an organization requires two environments:

- one to concentrate on transformation of data into information
- another to deal with transformation of information into knowledge

  

Business Intelligence, therefore, is a broad group of applications and technologies.

1. the term refers to the systems and technologies for gathering, cleansing, consolidating, and storing corporate data.
2. the term also relates to the tools, techniques, and applications for analyzing the stored data.

  

## BI: Two Environments

- Data to Information
    - in this environment data from multiple operational systems are extracted, integrated, cleansed, transformed, and stored as information in specially designed repositories
- Information to Knowledge
    - in this environment analytical tools are made available to users to access and analyze the information content in the specially designed repositories and turn information into knowledge.

![[/image 5 5.png|image 5 5.png]]

  

Bill Inmon considered to be the father of data warehousing provides the following definition:

  

> [!important] A Data Warehouse is a subject-oriented, integrated, nonolatile, and time variant collection of data in support of management’s decisions.

### Defining Features

- Subject oriented
    - data are organized by detailed subject containing only information relevant for decision support. it provides a more comprehensive view of the organization.
    - the data in a data warehouse is organized in such a way that all the data sets relating to the same real-world business subject or event is tied together. We have Invoices, Customer, Payment, basically entities or tables in an RDBMS.
    - in a data warehouse, there is no application flavor. the data in a data warehouse cuts across applications.

![[/image 6 3.png|image 6 3.png]]

- Integrated
    
    - data warehouses must place data from different sources into a consistent format
    - since data warehouses would need to get data from various operational systems, there might be inconsistencies in:
        - naming conventions
        - codes
        - data attributes
        - measurements
    
    ![[/image 7 3.png|image 7 3.png]]
    
- Time variant (time series)
    - it contains historical (daily, weekly, and monthly) in addition to current data (real-time)
    - for an operational system, the stored data contains the current values. of course, some past transactions are also stored in operational systems to support every day operations. on the other hand, data in a data warehouse is meant for analysis and decision making.
    - a data warehouse, because of the very nature of its purpose, has to contain historical data, not just current values. data is stored as snapshots over past and current periods. changes to data are tracked and recorded. every data structure in the data warehouse contains the time element. this allows
        - analysis of the past
        - relating information to the present
        - forecasts for the future
- Non-volatile
    
    - data can not be changed or updated after it had been entered into the data warehouse. In some cases, obsolete or old data are discarded, and changes are recorded as new data.
    - in the data warehouse, you keep the extracted stock status data as snapshots over time. you do not update the data warehouse every time you process a single order.
    - data from the operational systems are moved into the data warehouse at specific intervals. depending on the requirements of the business, these data movements take place at different frequencies.
    - usually the data in the data warehouse is not updated or deleted
    
    ![[/image 8 3.png|image 8 3.png]]
    

  

In a data warehouse, you find it efficient to keep data summarized at different levels. Data granularity in a data warehouse refers to the level of detail. Depending on the requirements, multiple levels of detail may be present. Many data warehouses have at least dual levels of granularity.

### More features for a data warehouse

- Web based
    - designed for web-based applications
- Relational/multidimensional
    - its structure is either relational or multidimensional
- Uses client server
    - for easy access
- Real-time
    - characteristic for new data warehouses
- Include metadata
    - data about data

  

# Definitions and Concepts

## Operational Data Stores

- a type of db often used as an interim (temporary) area for a data warehouse, especially for customer information files

## Oper marts

- an operational data mart. An oper mart is a small-scale data mart typically used by a single department or functional area in an organization when they need to analyze operational data

## Enterprise data warehouse (EDW)

- a technology that provides a vehicle for pushing data from source systems into a data warehouse that is used across the enterprise for decision support

## Metadata

- data about data. in a data warehouse, metadata describes the contents of a data warehouse and the manner of its use.

  

### **ETL (Extract, Transform, Load)**

This is the **pipeline** that moves data from source systems (like your Django PostgreSQL DB) into the warehouse.

- **Extract** → Pull data from sources (DBs, APIs, CSVs)
- **Transform** → Clean, aggregate, reformat (e.g., convert `score` to percentile)
- **Load** → Store in the DW in a format suitable for analysis

> ✅ Modern stack tip: ETL is often now ELT (transform after loading), especially in tools like BigQuery or Snowflake.

# Data Warehousing Process

- organizations continuously collect data, information, and knowledge at an increasingly accelerated rate and store them in computerized systems
- the number of users needing the access the information continues to increase as a result of improved reliability and availability of network access, especially the internet.

![[/image 9 3.png|image 9 3.png]]

# Data Mart

> [!important] a logical and physical subset of a data warehouse; in its most simplistic form, represents data from a single business process (e.g., retail sales, retail inventory, purchase orders). Basically, a departmental data warehouse.

- Dependent data mart
    - a subset that is created directly from a data warehouse
- Independent data mart
    - a small data warehouse designed for a strategic business unit (SBU) or a department and its source is not the EDW (Enterprise Data Warehouse)

![[/image 10 3.png|image 10 3.png]]

Bill Inmon said:

> The single most important issue the IT manager this year is whether to build the data warehouse first or the data mart first.

  

Top-down or bottom-up approach?

1. overall data warehouse feeding dependent data marts
2. several departmental or local data marts combining into a data warehouse.

  

Bill Inmon is one of the leading proponents of the top-down approach. He has defined a data warehouse as a centralized repository for the entire enterprise. In the Inmon vision the data warehouse is at the center of the “Corporate Information Factory” (CIF) prividing the logical framework for delivering business intelligence to the enterprise.

Advantages are:

- a truly corporate effort, an enterprise view of data
- inherently architected, not a union of disparate data marts
- single, central storage of data about the content
- centralized rules and control
- may see quick results if implemented with iterations

Disadvantages

- takes longer to build even with an iterative method
- high exposure to risk of failure
- needs high level of cross-functional skills
- high outlay without proof of concept

This is the big-picture approach in which you build the overall, big, enterprise-wide data warehouse. you do not have fragmented islands of information. but takes longer to build and has a high risk of failure if you have an inexperienced team of IT professionals therefore is harder to sell to executives since they will not see results soon enough.

  

Ralph Kimball is a proponent of the bottom-up approach. He envisions the corporate data warehouse as a collection of conformed data marts. The key consideration is the conforming of the dimensions among the separate data marts. you would build you departmental data marts one by one.

  

### A practical approach

1. Plan and define requirements at the overall corporate level
2. create a surrounding architecture for a complete warehouse
3. conform and standardize the data content. you determine the data content for each supermart.
4. implement the data warehouse as a series of supermarts, one at a time

This is a combined top-down and bottom-up approach.

  

A data warehouse, therefore, is a conformed union of all data marts. Individual data marts are targeted to particular business groups in the enterprise, but the collection of all the data marts form an integrated whole, called the enterprise data warehouse.

  

## Architectural Types

![[/image 11 3.png|image 11 3.png]]

### Centralized Data Warehouse

- takes into account the enterprise-level information requirements.
- an overall infrastructure is established.
- Atomic level normalized data at the lowest level of granularity is stored in the third normal form
- occasionally, some summarized data is included
- queries and applications access the normalized data in the central data warehouse
- there are no separate data marts.

  

### Independent Data Marts

- organizational units develop their own data marts for their own specific purposes.
- these separate data marts do not provide “a single version of the truth”
- the data marts are independent of one another. as a result, they are likely to have inconsistent data definitions and standards.

  

### Federated

- data may be physically or logically integrated through shared key fields, overall global metadata, distributed queries, and such other methods because there is an existing legacy of an assortment of decision-support structures in the form of operational systems, extracted datasets, primitive data marts, and so on.

  

### Hub-and-Spoke

- Inmon Corporate Information Factory approach.
- atomic data in the third normal form is stored in the centralized data warehouse.
- there is presence of dependent data marts which obtain data from the centralized data warehouse.
- the centralized data warehouse forms the hub to feed data to the data marts on the spokes.
- top-down approach to data warehouse deployment

  

### Data-Mart Bus

- Kimbal conformed supermarts approach.
- you build the first data mart (supermart) using business dimensions and metrics
- these business dimensions will be shared in the future data marts
- the principal notion is that by conforming dimensions among the various data marts, the result would be logically integrated supermarts that will provide an enterprise view of the data.
- the data marts contain atomic data organized as a dimensional data model.

  

# Overview of Components

For an operational system, you would have:

- GUI
- SQL
- Views
- Network interface

Architecture is the proper arrangement of the components. you build a data warehouse with software and hardware components.

  

## Data Warehouse Components

![[/image 12 2.png|image 12 2.png]]

### Source Data Component

may be grouped into four broad categories:

  

Production Data

- comes from the various operational systems of the enterprise.
    - financial systems, manufacturing systems, etc…
- there are variations in the data formats and data resides on different hardware platforms. data is supported by different database systems and operating systems. this is data from many vertical applications.
- the significant and disturbing characteristic of production data is disparity. the great challenge is to standardize and transform the disparate data from the various production systems, convert the data, and integrate it into pieces that are useful for storage in the data warehouse.

  

Internal Data

- private data from the organization. spreadsheets, documents, customer profiles, etc…

  

Archived Data

- in every operational system, you periodically take the old data and store it in archived files.

  

External Data

- most executives depend on data from external sources for a high percentage of information they use. they use statistics relating to their industry produced by external agencies and national statistical offices. they use market share data of competitors, etc…
- in order to spot industry trends and compare performance against other organizations, you need data from external sources.

  

### Data Staging Component

The extracted data coming from several sources need to be changed, converted and made ready in a format for data warehousing.

  

Data needs to be:

- Extracted using custom-written or commercial software called ETL
- Transformed, cleansed, and preprocessedj
- Loaded into the data warehouse storage

These functions take place in a staging area because we are querying from different operational systems, therefore, there is a necessity in preparing data.

  

Extraction

- you would have to employ the appropriate technique for each data source because source data may be from different source machine in diverse data formats.

Transformation

1. you clean the data extracted from each source.
2. standardization of data
3. sorting and merging data

Loading

1. When you complete the design of a data warehouse, you load initial data and large volumes of it into the data warehouse.
2. as the data warehouse starts functioning, you continue to extract changes coming from source data.

  

  

### Data Storage Component

- the data storage for the data warehouse is a separate repository.
- the operational systems use online transaction processing applications and they can use RDBMs.
- in a data warehouse, you have to keep data in structures suitable for analysis, and not for quick retrieval of individual pieces of information. therefore, the data storage for the data warehouse is kept separate from data storage of operational systems.
- in operational systems, DBs can be updating at any one time and can be in the middle of updating records. analysts need to know that the data is stable and that it represents snapshots at specified periods. for this reason, data warehouses are read-only data repositories.
- generally, the db in your data warehouse must be open. the data warehouse must be open to different tools. most of the data warehouses employ RDBMSs
- many data warehouses also employ multidimensional database management systems.

  

### Information Delivery Component

- different users have different technical expertise and different informational needs. you must have different methods of information delivery.
- Ad hoc reports
    - predefined reports primarily meant for novice and casual users
- Complex queries, multidimensional analysis, and statistical analysis cater to the needs of the business analysts and power users.
- Information fed into executive information systems or EIS is meant for senior executives and high-level managers.
- Data mining applications
    - knowledge discovery systems where the mining algorithms help discover trends and patterns from the usage of data.

![[/image 13 2.png|image 13 2.png]]

  

### Metadata Component

- metadata is the data about the data in the data warehouse.

  

### Management and Control Component

- sits on top of all the other components
- coordinates the services and activities within the data warehouse.
- controls the data transformation and the data transfer into the data warehouse storage.
- moderates the information delivery to the users
- monitors everything