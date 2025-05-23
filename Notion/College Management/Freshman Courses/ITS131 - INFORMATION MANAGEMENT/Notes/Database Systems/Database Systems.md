Database Systems

  

# Why Databases?

  

### Characteristics of Modern Data

- **Ubiquitous** (abundant, global, and everywhere)
- **Pervasive** (unescapable, prevalent, and persistent)
- We always consume and produce data, from our birth certificates to death certificates (and even beyond!)

  

Databases are specialized structures that allow computer-based systems to store, manage, and retrieve data very quickly.

- Databases make data persistent and shareable in a secure way.

# Data Vs. Information

### Data

- Not yet processed to reveal meaning to the end user
- Consists of **raw facts**
- Building blocks of information

### Information

- Results from processing **raw data**
- Requires **context**
    - is this in celcius or fahrenheit?
- Can be used as the foundation for **decision making**
- Raw data must be properly _formatted_ for storage, processing, and presentation.
    - dates might be stored in Julian
    - Boolean choices might be converted to 0/1
- Bedrock of knowledge
    - knowledge is the body of information and facts about a specific subject.
    - A key characteristic of knowledge is that “new” knowledge can be derived from “old” knowledge.
- Should be accurate, relevant, and timely
    - In this “information age”, this is the key to good decision making. In turn, good decision making is the key to business survival in a global market. We are now said to be entering the “knowledge age”.

  

  

### Database Management

  

Like any basic resource, the data environment must be managed carefully.

  

> [!important] **Database management is a discipline that focused on the proper generation, storage, and retrieval of data.**

  

  

# Introducing the Database

  

> [!important] A database is a shared, integrated computer structure that stores a collection of:
> 
>   
>   
> -  
> **End user-data** — raw facts of interest to the end user  
> -  
> **Metadata** — or data about data, through which the end user data is integrated and managed; data about data characterstics and relationships.

  

The metadata provides information that complements and expands the value and use of the data. In short, metadata presents a more complete picture of the data in the database.

- If I may offer an analogy, it’s like having an instant background check on a person.
- Given the characteristics of metadata, you might hear a database described as a **“collection of self-describing data”.**

  

### Database Management System or DBMS

- is a collection of programs that manages the database structure and controls access to the data stored in the database.
- In a sense, a database resembles a very-well organized electronic filing cabinet in which powerful software (the DBMS) helps manage the cabinet’s contents.

  

## Role and Advantages of the DBMS

  

The DBMS serves as the intermediary between the user and the database.

- The database structure itself is stored as a collection of files, and the only way to access the data in those files is through the DBMS.
- The DBMS presents the end user (or application program) with a single, integrated view of the data in the database.
- The DBMS receives all application requests and translates them into the complex operations required to fulfill those requests.
- The DBMS hides much of the database’s internal complexity, introducing an interface or an abstraction layer for an end user or application program to communicate with.

  

Having a DBMS between the end user’s applications and the database offers some important advantages.

- The DBMS enables the data in the database _to be shared among multiple applications or users._
- The DBMS integrates the many different users’ view of the data into a single all-encompassing data repository.

  

The DBMS helps make data management more efficient and effective.

- Improved data sharing.
- Improved data security. A DBMS provides a framework for better enforcement of data privacy and security policies.
- Better data integration
- Minimized data inconsistency.
    - Data inconsistency exists when different versions of the same data appear in different places. The probability of data inconsistency is greatly reduced in a properly designed database
- Improved data access
    - The DBMS makes it possible to produce quick answers to ad hoc queries.
    - From a database perspective, a query is a specific request issues to the DBMS for data manipulation.
- Improved decision making
- Increased end user productivity.

  

  

## Types of Databases by Number of Users

  

Single-user Database: supports one user at a time

- Desktop database: single-user database on a personal computer

  

Multiuser Database: supports multiple users at the same time

- Workgroup databases - supports a small number of users or a specific department
- Enterprise database - supports many users across many departments

  

## Types of Databases by Location

- Centralized Database - data located at a single site
- Distributed (Decentralized) Database - data distributed across different sites

Both centralized and distributed databases require a well-defined infrastructure to implement and operate the database. Typically, this is owned and maintained by the organization or company. Basically, in-house infrastructure.

  

- Cloud Database
    
    - created and maintained using cloud data services that provide defined performance measures (data storage capacity, required throughput, and availability )for the database, but do not necessarily specify the underlying infrastructure to implement it.
    - The data owners do not have to know, or be concerned about, what hardware and software are being used to support their databases. The performance capabilities can be renegotiated with the cloud provider as the business demenads on the database change.
    
      
    

## Type of Databases by Data Type

  

- General-purpose Database
    - contains a wide variety of data used in multiple disciplines
- Discipline-specific Database
    - contains data focused on specific subject areas
    - the data in this type of database is used mainly for academic or research purposes within a small set of disciplines
- Operational Database
    
    - designed to support a company’s day-to-day operations
    
      
    

## Types of Database based on How the Data is Used and Time Sensitivity

  

- Operational Database
    - designed to support a company’s day-to-day operations
    - For example, transactions such as product or service sales, payments, and supply purchases reflect critical day-to-day operations. Such transactions must be recorded accurately and immediately.
    - also known as online transaction processing or OLTP database, transactional database, or production database
- Analytical Database
    
    - focuses primarily on historical data and business metrics used exclusively for tactical or strategic decision making. Such analysis requires extensive “data massaging” (data manipulation) to produce information on which to base pricing decisions, sales forecasts, market strategies, and so on.
    - Analytical databases allow the end user to perform advanced analysis of business data using sophisticated tools.
    - Typically, analytical databases comprise two main components:
        - A data warehouse - is a specialized database that stores data in a format optimized for decision support. It contains historical data obtained from the operational databases as well as data from other external sources.
        - Online analytical processing or OLAP - is a set of tools that work together to provide an advanced data analysis environment for retrieving, processing, and modeling data from the data warehouse.
    - Business intelligence - describes a comprehensive approach to capture and processing business data with the purpose of generating information to support business decision making.
    
    ![[/Untitled 28.png|Untitled 28.png]]
    

  

## Types of Databases based on Data Structure

  

Unstructured Data

- exists in its original (raw) state

  

Structured Data

- results from formatting
- Structure is applied based on type of processing to be performed
    - 80923 can be a zip code but if used a string, we cannot do mathematical operations on it.

  

Semistructured Data

- processed to some extent (usually using formatting tags or some markup language) but does not conform to the strict tabular format typically of the relational model.
- most of the data we interact with in the internet is usually semistructured.

  

  

XML

- Unstructured and semistructured data storage and management needs are being addressed through a new generation of databases known as XML databases.
- not a database per se, but a way for data to travel within systems in text format.
- With the emergence of social media, there has been an enormous volume of data influx. As a result, MySQL database Twitter was using to store user content was frequently overloaded by demand. Same with Facebook.
- Over the past few years, this new breed of database is known as NoSQL database. The term NoSQL (Not only SQL) is generally used to describe a new generation of DBMS that is not based on the traditional relational database model. NoSQL databases are designed to handle the unprecedented volume of data, variety of data types and structures, and velocity of data operations that characteristics of these new business requirements.

## Why Database Design is Important?

  

Database design refers to the activities that focus on the design of the database structure that will be used to store and manage end-user data.

  

Well-designed database

- facilitates data management and generates accurate and valuable information

  

Poorly-designed Database

- causes difficult-to-trace errors that may lead to poor decision making.

  

  

## Evolution of File System Data Processing

  

1. Manual File Systems
    - accomplished through a system of file folders and filing cabinets
2. Computerized File Systems
    - Generating reports from manual file systems was slow and cumbersome.
    - Data processing or DP specialist created a computer-based system to track data and produce required reports
    - Problem with data consistency among systems
3. File System Redux
    
    - Includes spreadsheet programs such as MS Excel.
    
      
    

### Basic File Terminology

- Data - raw facts
- Field - used to define and store data
- Record - a logically connected set of one or more fields
- File - a collection of related records

  

  

## Problems with File System Data Processing

- the file system was a definite improvement over the manual system. But there are critiques.
    
    - Lengthy development times
        - even the simplest data-retrieval task requires extensive programming
    - Difficulty of getting quick answers
        - the need to write programs to produce even the simplest reports makes ad hoc queries impossible.
    - Complex system administration
        - system administration becomes more difficult as the number of files in the system expands
    - Lack of security and limited data sharing.
    - Extensive programming
        - making changes to an existing file structure can be difficult
            1. Reads a record from the original file
            2. transforms the original data to conform to the new structure’s storage requirements
            3. writes the transformed data into the new file structure, and
            4. repeats the preceding steps for each record in the original file
    
      
    
      
    

## Structural and Data Dependence

  

A file system exhibits **structural dependence**, which means that access to a file is dependent on its structure. It refers to the situation where the application programs that access data are dependent on the specific physical structure of the data. This means that any change in the physical structure of the data (such as adding or removing a field, changing the order of fields, or modifying the record structure) would require modifying all the programs that access that data.

- Example: if you want to add a new field for email address, this change would require updating all the programs that access the customer file to reflect the new structure, including the order of fields, field definitions, and record specifications.
- Structural Independence - file structure is changed without affecting the application’s ability to access the data

  

Data dependence - data access changes when data storage characteristics change. It refers to the situation where the application programs that access data are dependent on the specific data type or formats used to store the data. Changing any data type or formats would require modifying all the programs that access that data.

- Data independence - data storage characteristics are changed without affecting the program’s ability to access the data.
- Practical significance of data dependence is the difference between logical and physical format.

  

## Data Redundancy

- the file system’s structure makes it difficult to combine data from multiple sources, snd its lack of security renders the file system vulnerable to security breaches.
- **unnecessarily storing the same data at different places**
    - database professionals call scattered and isolated data locations as **islands of information.**
- increases the probability of having different version of the same data.
- Possible results of uncontrolled data redundancy
    
    - Poor data security
        - having multiple copies of data increases the chances for a copy of the data to be susceptible to unauthorized access.
    - Data inconsistency
    - Data-entry errors
        - are more likely to occur when complex entries are made in several different files or recur frequently in one or more files.
    - Data integrity problems
    
      
    

Note

- Data that displays data inconsistency is also referred to as data that lacks data integrity. Data integrity is defined as the condition in which the data in the database is consistent with the real-world events and conditions.
    
    - Data is accurate - there are no data inconsistencies
    - Data is verifiable - the data will always yield consistent results.
    
      
    

## Data Anomalies

- a data anomaly develops when not all of the required changes in the redundant data are made successfully.
    - Update anomalies
        - If you update something, you need to update it everywhere
    - Insertion anomalies
        - If you insert something, you need to insert it everywhere
    - Deletion anomalies
        - If you delete something, you need to delete within every system

  

  

## Database Systems

- the problems inherent in file systems make using a database system very desirable. Unlike the file system, the database system consists of logically related data stored in a single logical data repository (The “logical” label reflects the fact that the data repository appears to be a single unit to the end user, even though data might be physically distributed among multiple storage facilities and locations.)
- DBMS eliminates most of the problems in file systems
- The current generation of DBMS software stores not only the data structures but also the relationships between those structures and the access paths to those structures—all in a central location. they also take care of defining, storing, managing all required access paths to those components.

![[/Untitled 1 15.png|Untitled 1 15.png]]

  

## The Database System Environment

> [!important] Database System - an organization of components that defines and regulates the collection, storage, management, and use of data in a database environment.

  

Remember that the DBMS is just one of several crucial components of a database system. The DBMS may even be referred to as the database system’s heart.

  

The database system environment is composed of five major parts

![[/Untitled 2 11.png|Untitled 2 11.png]]

  

- Hardware
- Software
    - OS
    - DBMS Software
    - Application programs and utility software are used to access and manipulate data in the DBMS and to manage the computer environment in which data access and manipulation take place
- People or Human Elements
    - System administrators oversee the database system’s general operations
    - Database administrators known as DBAs, manage the DBMS and ensure that the database is functioning properly.
    - Database designers design the database structure. They are the database architects.
    - System analysts and programmers design and implement the application programs. They design and create the data-entry screens, reports, and procedures through which endu sers access and manipulate the database’s data.
    - End users: sales clerks, supervisors, managers, and directors are all classified as end users.
- Procedures
    - are the instructions and rules that govern the design and use of the database system.
- Data
    - covers the collection of facts stored in the database.

  

  

## DBMS Functions

  

- Data dictionary management
    
    - The DBMS stores definitions of the data elements and their relationships (metadata) in a data dictionary, also known as a metadata repository. The DBMS uses the data dictionary to look up the required data components structures and relationships, thus relieving you to code such complex relationships in each program.
    - It serves as a comprehensive reference for the structure, characteristics, and usage of data elements in the database
    - Additionally, any changes made in a database structure are automatically recorded in the data dictionary
    - In other words, the DBMS provides data abstraction, and it removes structural and data dependence from the system.
    
      
    
- Data storage management
    - The DBMS creates and manages the complex structures required for data storage, thus relieving you from the difficult task of defining and programming the physical data characteristics.
    - It is important for performance tuning
    - Performance tuning relates to the activities that make the database perform more efficiently in terms of storage and access speed. Although the user sees the database as a single data storage unit, the DBMS actually stores the database in multiple physical data files.
- Data transformation and presentation
    - The DBMS transforms entered data to conform to required data structures. The DBMS relieves you of the chore of distinguishing between the logical data format and the physical data format.
        - The logical data format is the way the data is perceived and viewed by the end users or application programs. It represents the conceptual or abstract view of the data, without considering the physical storage details.
            - For example, for a date field, the logical data format might be “DD/MM/YYYY” for users in one country, and another format in another. The logical data format is how the data is presented to the users and how they expect to interact with it.
        - Physical data format refers to the way the data isactually stored and represented in the database system’s storage structures. The physical data format is determined by the DBMS and is optimized for efficient storage and retrieval of data. Details such as storage optimization, platform-specific requirements, or internal representation schemes used by the DBMS are considered.
            
            - For example, while the logical data format might be “DD/MM/YYYY”, the physical data format in the database could be a numeric representation
            
              
            
- Security Management
    - The DBMS creates a security system that enforces user security and data privacy.
- Multiuser access control
    - the DBMS uses algorithms to ensure multiple users can concurrently access the database without compromising its integrity.
- Backup and recovery management
    - to ensure data safety and integrity. Current DBMS systems provide special utilities that allow the DBA to perform routine and special backup and restore procedures.
- Data integrity management
- Database access language and APIs
    - The DBMS provides data access through a query language. A query language is a nonprocedural language—one that lets the user specify what must be done without having to specify how.
    - SQL is the de facto query language.
    - the DBMS also provides APIs to procedural programming languages such as Java, C, C++, etc…
- Database communication interfaces
    
    - A current-generation DBMS accepts end-user requests via multiple communication interfaces.
    
      
    

  

## Challenges of Database Systems

- Increased costs
    - require sophisticated hardware and software and highly skilled personnel
- Management complexity
    - technological complexity
- Maintaining currency
    - making the system current. applying latest patches and security measures to all components.
- Vendor dependence
    - given the heavy investment in technology and personnel training, companies might be reluctant to change database vendors.
- Frequent upgrade/replacement cycles

  

  

  

## Transaction Management

- When several users access and possibly modify a database concurrently, the DBMS must order their requests carefully to avoid conflicts.
- A transaction is any one execution of a user program in a DBMS (Executing the same program several times will generate several transactions.) This is the basic unit of change as seen by the DBMS. Partial transactions are not allowed and the effect of a group of transactions is equivalent to some serial execution of all transactions.
    - This means that transactions are all or nothing. It can either succeed or fail (entire transaction is cancelled).

### Concurrent Execution of Transactions

- an important task of a DBMS is to schedule concurrent accesses to data so that each user can safely ignore the fact that others are accessing the data concurrently.
- A DBMS allows the users to think of their programs as if they were executing in isolation, one after the other in some order chosen by the DBMS. For example, if a program that deposits cash is submitted at the same time as another program that debits money from the same account, either of these programs could be run first by the DBMS, but their steps will not be interleaved in such a way that they interfere with each other.
- A locking protocol is a set of rules to be followed by each transaction (and enforced by the DBMS) to ensure that, even though actions of several transactions might be interleaved, the net effect is identical to executing all transactions in some serial order.
    - A lock is a mechanism used to control access to database objects.
    - Shared locks on an object can be held by two different transactions at the same time
    - An exclusive lock on an object ensures that no other transactions hold any lock on this object.
        - Suppose that the following locking protocol is followed: Every transaction begins by obtaining a shared lock on each data object that it needs to read and an exclusive lock on each data object that it needs to modify, then releases all its locks after completing all actions. Consider two transactions T1 and T2 such that T1 wants to modify a data object and T2 wants to read the same object. Intuitively, if T1’s request for an exclusive lock on the object is granted first, T2 cannot proceed until T1 releases this lock. Thus, all T1’s actions will be completed first.
- This is managed by the concurrency control manager

  

### Incomplete Transactions and System Crashes

- transactions can be interrupted before running to completion for a variety of reasons like a system crash. A DBMS must ensure that the changes made by such incomplete transactions are removed from the database.
    - For example, if the DBMS is in the middle of transferring money from account A to account B and has debited the first account but not yet credited the second when the crash cccurs, the money debited from account A must be restored when the system comes back up after the crash.
    - To do so, the DBMS maintains a log of all writes to the database. A crucial property of the logic is that each write action must be recorded in the log (on disk) before the corresponding change is reflected in the database itself—otherwie, the system crashes just after making the change in the database but before the change is recorded in the log. The DBMS would be unable to detect and undo this change. THis property is called Write-Ahead Log or WAL. To ensure this property, the DBMs must be able to selectively force a page in memory to disk.
    - The log is also used to ensure that the changes made by successfully completed transactions are not lost due to a system crash. Bringing the database to a consistent state after a system crash can be a slow process, since the DBMs must ensure that the effects of all transactions completed prior to the crash are restored, and that the effects of incomplete transactions are undone. The time required to recover from a crash can be reduced by periodically forcing some information to disk; this periodic operation is called a checkpoint.
- This is managed by the recovery manager.

  

  

## Non-Procedural Access

- is a way for users to retrieve data from a database without having to write complex procedures or code.
- It allows users to specify what data they want to retrieve, rather than how to retrieve it. This makes it easier for people with limited computing skills to work with databases.
- It can greatly reduce the amount of code needed for software development. This can make it productive.
- To access databases using non-procedural access, Structured Query Language or SQL is used.
- Query formulation is a fundamental skill for IT professionals.

  

## Database Programming Languages

- While non-procedural access is convenient and powerful, it may not be suitable for all types of applications. For more complex tasks like developing web pages or searching large databases, a combination of SQL and programming languages must be used.
- Used for big data tasks and customization of applications