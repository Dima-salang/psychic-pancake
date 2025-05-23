The relational model introduced by E.F. Codd in 1970 is based on predicate logic and set theory.

  

Predicate logic, used extensively in math, provides a framework in which an assertion (statement of fact) can be verified as either true or false.

  

Set theory is a mathematical science that deals with sets, or groups of things, as the basis for data manipulation in the relational model.

  

Based on these concepts, the relatoinal model has three well-defined components:

1. A logical data structure represented by relations
2. A set of integrity rules to enforce that the data is consistent and remains consistent over time.
3. A set of operations that defines how data is manipulated.

  

### Tables and Their Characteristics

- The logical view of the relational database is facilitated by the creation of data relationships based on a logical construct known as a relation.
- Because a relation is mathematical construct, end users find it easier to think of tables. A table is perceived as a two-dimensional structure composed of rows and columns. A table is also called a relation because the relational model’s creator, E.F. Codd, used the two terms as synonyms. We can think of a table as a persistent representation of a logical relation—that is, a relation whose contents can be permanently saved for future use.
- A table contains a group of related entity occurrences—that is, an entity set. For that reason, the terms entity set and table are often used interchangeably.

![[/Untitled 29.png|Untitled 29.png]]

  

  

## Keys

- in the relational model, keys are important because they are used to ensure that each row in the table is uniquely identifiable. They are also used to establish relationships among tables and to ensure the integrity of the data.
- A key consists of one or more attributes that determine other attributes.

  

### Dependencies

- the role of a key is based on the concept of determination. Determination is the state in which knowing the value of one attribute makes it possible to determine the value of another.
- If you are given a value for STU_NUM, then you can determine the value for STU_LNAME because one and only one value of STU_LNAME is associated with any given value of STU_NUM. This is functional dependence, which means that the value of one or more attributes determines the value of one or more other attributes.
- The standard notation is STU_NUM → STU_LNAME
    - In this functional dependency, the attribute whose value determines another is called the determinant or the key. The attribute whose value is determined by the other attribute is called the dependent.
    - It would be correct to say that STU_NUM is the determinant and STU_LNAME is the dependent.
    - STU_NUM → (STU_LNAME, STU_FNAME, STU_GPA)
- Now, determinants of more than one attribute require special consideration:
    - STU_NUM → STU_GPA
    - (STU_NUM, STU_LNAME) → STU_GPA
    - In the second functional dependency, the determinant includes STU_LNAME but this attribute is not necessary for the relationship since given STU_NUM, STU_GPA is already determined. It is redundant. However, the functional dependency is valid because given of pair values for the determinants, only one value would occur for STU_GPA.
    - A more specific term, full functional dependence is used to refer to functional dependencies in which the entire collection of attributes in the determinant is necessary for the relationship.

  

### Types of Keys

- Keys are determinants in functional dependencies

  

Composite key

- is a key that is composed of more than one attribute. An attribute that is a part of a key is called a key attribute.
    - (STU_LNAME, STU_FNAME, STU_INIT, STU_PHONE) → STU_HRS

  

Superkey

- is a key that can uniquely identify any row in the table. In other words, a superkey functionally determines every attribute in the row.
- Composite keys can also be superkeys.
- One specific type of superkey is called the candidate key. It is a minimal superkey—that is, a superkey without any unnecessary attributes, meaning that it has full functional dependency.
- A table can have many different candidate keys. Candidate keys are called candidates because they are the eligible options from which the designer will choose when selecting the primary key. The primary key is the candidate key chosen to be the primary means by which the rows of the table are uniquely identified.
    - all primary keys are candidate keys but not all candidate keys are chosen to be primary keys.

  

  

### Entity Integrity

- is the condition in which each row (entity instance) in the table has its own known, unique identity. To ensure integrity, the primary keys has two requirements: (1) all of the values in the primary key must be unique and (2) no key attribute in the primary key van contain a null.
- Null values are problematic in the relational model. A unll is the absence of any data value, and it is never allowed in any part of the primary key.
- Technically, a null in the database in not a value and it is not an entry—it is the absence of a value and the absence of an entity.

  

  

In addition to its role in providing a unique identity to each row in the table, the primary keys help in controlled redundancy.

  

### Foreign Keys or FK

- is the primary key of one table that has been placed into another table to create a common attribute.
- FKs are used to ensure referential integrity, the condition in which every reference to an entity instance by another entity instance is valid.
- In other words, every foreign key entry must either be null or a valid value in the primary of the related table. It is okay to have a null value in the FK as long as there is no null values in the PK in the referred table. Example, a customer does not have an assigned agent to them yet.

  

  

### Secondary Key

- is defined as a key that ised strictly for data retrieval purposes and does not require a functional dependency.
- Example the secondary is the combination of the customer’s last name and phone number. keep in mind that a secondary key does not necessarily yield a unique outcome. A customer’s last name and home number could easily yield several matches in which one family lives together and shares a phone line.
- A secondary key’s effectiveness in narrowing down a search depends on how restrictive the key is.

  

## Integrity Rules

- Relational database integrity rules are the foundation of good database design.

![[/Untitled 1 16.png|Untitled 1 16.png]]

- To avoid nulls, some designers use special codes, known as flags, to indicate the absence of some value.

![[/Untitled 2 12.png|Untitled 2 12.png]]

  

  

## Relational Algebra

- the data in relational tables is of limited value unless the data can be manipulated to generate useful information.
- Relational algebra defines the theoretical way of manipulating table contents using relational operators.

  

  

### Formal Definitions and Terminology

  

A relations is the data that you see in your tables.

A revlar or relation variable is a variable that holds a relation.

  

When you create a table, the table structure holds the table data. the structure is properly called a relvar, and the data in the structure would be a relation. The relvar is container for holding relation data, not the relation itself. The data in the table is a relation.

  

A relvar has two parts

- the relvar heading contains the names of the attributes
- the relvar body contains the relation.

  

  

### Relational Set Operators

- The relational operators have the property of closure; that is, the use of relational algebra operators on existing relations or tables produces new relations.

  

### SELECT (RESTRICT)

- is referred to as a unary operator because it only uses one table as input. It yields values for all rows found in the table that satisfy a given condition.
- SELECT can be used to list all of the rows, or it can yield only rows that match a specified criterion. In other words, SELECT yields a horizontal subset of a table. SELECT will not limit the attributes returned so all attributes of the table will be included in the result.
- Formally, SELECT is denoted by the lowercase Greek letter sigma (σ). Sigma is followed by the condition to be evaluated called a predicate as a subscript, and then the relation is listed in parentheses.

![[/Untitled 3 11.png|Untitled 3 11.png]]

- This would select all of the rows in the CUSTOMER table that have the value “10010” in the CUS_CODE attribute.

  

### PROJECT

- yields all values for selected attributes. It is also a unary operator, accepting only table as input.
- PROJECT will return only the attributes requested, int the order win which they were requested. In other words, it yields a vertical subset of a table.
- PROJECT will not limit the rows returned, so all rows of the specified attributes will be included in the result.
- Formally PROJECT is denoted by the Greek letter pi. Pi is followed by the list of attributes to be returned as subscripts and then the relation listed in parentheses.

![[/Untitled 4 8.png|Untitled 4 8.png]]

  

UNION

- combines all rows from two tables, excluding duplicate rows.
- the tables must have the same attribute characteristics, in other words, the column and domains must be compatible.
    - When two or more tables share the same number of columns and when their corresponding columns share the same or compatible domains, they are said to be union-compatible.

  

INTERSECT

- yields only the rows that appear in both tables.
- tables must be union-compatible
- for the rows to be considered the same in both table and appear in the result of the operation, the entire rows must be exact duplicates.

  

DIFFERENCE

- yields all rows in one table that are not found in the other table; that is, it subtracts one table from the other.

  

  

PRODUCT

- yields all possible pairs of rows from two tables—also known as the Cartesian product.

  

JOIN

- allows information to be intelligently combined from two or more tables.
- it is the real power behind the relational database, allowing the use of independent tables linked by common attributes or FKs

  

  

## The Data Dictionary and the System Catalog

  

The data dictionary provides a detailed description of all tables in the database created by the user and designer. Thus, the data dictionary contains at least all of the attribute names and characteristics for each table in the system. In short, the data dictionary contains metadata.

- the data dictionary is sometimes described as “the database designer’s database” because it records the design decisions about tables and their structures.

  

Like the data dictionary, the system catalog contains metadata. The system catalog can be described as a detailed system data dictionary that describes all objects within the database.

- Because the system catalog contains all required data dictionary information, the terms system catalog and data dictionary are often used interchangeably. In fact, current RDBMSs generally provide only a system catalog, from which the designer’s data dictionary information may be derived.
- The system catalog is actually a system-created database. Therefore, the system catalog tables can be queried just like any user/designer-created table.
- In effect, the system catalog automatically produces database documentation. As new tables are added, that documentation allows the RDBMS to check for homonyms and synonyms.
    - In a database context, homonyms indicate the use of the same name to label different attributes. C_NAME in CUSTOMER and C_NAME to label a consultant name attribute in CONSULTANT. Avoid homonyms.
    - Synonyms indicate the use of different names to describe the same attribute. C_NAME and CUS_NAME.

  

## Relationships within the Relational Database

- Determining which primary key to use as the foreign key to create a common attribute in the relational model is based on the classification of the relationship.
    
    - 1:M relationships is the relational modeling ideal. Therefore, this should be the norm in any relational database design.
    - The 1:1 relationship should be rare in any relational database design
    - M:N relationships cannot be implemented as such in the relational model.
    
      
    
    ### 1:M Relationship
    
    - The 1:M relationship is easily implemented in the relational model by putting the primary key of the “1” into the table of the “many” side as a foreign key.
    
      
    
    ### 1:1 Relationship
    
      
    
      
    
      
    
    ### M:N Relationship
    
    - is not supported directly in the relational environment.
    - M:N relationships can be implemented by creating a new entity in 1:M relationships with the original entities.
        
        - This is implemented by creating a composite entity. Because such a table is used to link the tables that were originally related in an M:N relationship, the composite entity structure includes—as foreign keys—at least the primary keys of the tables that are to be linked.
        
          
        
    
      
    
    ## Data Redundancy
    
    - The proper use of foreign keys is crucial to controlling data redundancy, It minimizes data redundancies and the chances that destructive data anomalies will develop.
    - The real test of redundancy is not how many copes of a given attribute are stored, but whether the elimination of an attribute will eliminate information.
    
      
    
      
    
    ## Indexes
    
    - An index is an orderly arrangement used to logically access rows in a table.
    
    ![[/Untitled 5 8.png|Untitled 5 8.png]]
    
    - From a conceptual point of view, an index is composed of an index key and a set of pointers. The index key, in effect, is the index’s reference point. More formally, an index is an ordered arrangement of keys and pointers. Each key points to the location of the data identified by the key.
    - DBMSs use indexes for many purposes. An index key can be composed of one or more attributes.
    - Indexes play an important role for the implementation of primary keys. When you define a table’s primary key, the DBMS automatically creates a unique index on the primary key column(s). In a unique index, the index key can have only one pointer value (row) associated with it.
    - A table can have many indexes, but each index is associated with only one table.
    
      
    
    ## Codd’s Relational Database Rules
    
    - In 1985, Dr. E.F. Codd published a list of 12 rules to define a relational database system.
    
    ![[/Untitled 6 8.png|Untitled 6 8.png]]
    
      
    
      
    
      
    

## Domain Constraints

- specify important conditions that we want each instance of the relation to satisfy.
- the domain of afield is essentially the type of that field
- The degree, also called arity, of a relation is the number of fields. The cardinality of a relation instance is the number of tuple in it.

  

## Integrity Constraints over Relations

- a database is only as good as the information stored in it.
- An integrity constraint or IC is a condition specified on a database schema and restricts the data that can be stored in an instance of the database.
    - If a database instance satisfies all ICs specified, it is a legal instance.
- A DBMS enforces integrity constraints, in that it permits only legal instances to be stored in the database.
- ICs are specified and enforced at different time
    
    - When the DBA or end user defines a database schema, they specify the ICs that must hold on any instance of this database
    - When a database application is run, the DBMS checks for violations and disallows changes to the data that violate the specified IC.
    
      
    

  

## Primary Key Constraints

- is a statement that a certain minimal subset of the fields of a relation is a unique identifier for a tuple.
- A set of fields that uniquely identifies a tuple according to a key constraint is called a candidate key for the relation.
    - Two distinct tuples in a legal instance cannot have identical values in all the fields of a key
    - No subset of the set of fields in a key is a unique identifier for a tuple. This means that there must be full functional dependency. If not full, then it is a superkey, which is a set of fields that contains a key.
- We can also use the UNIQUE constraint

  

## Foreign Key Constraints

  

  

  

## General Constraints

- The IC that students must be older than 16 can be thought of as an extended domain constraint since we are essentially defining the set of permissible age values more stringently than is possible by simply using a standard domain such as INT.
- Current RDBMSs support such general constraints in the form of table constraints and assertions.
    - Table constraints are associated with a single table and checked whenever that table is modified.
    - Assertions involve several tables and are checked whenever any of these tables is modified.

  

## Enforcing Integrity Constraints

- ICs are specified when a relation is created and enforced when a relation is modified. Every potential IC violation is generally checked at the end of each SQL statement execution, although it can be deferred.
- SQL provides several alternative ways to handle FK violations