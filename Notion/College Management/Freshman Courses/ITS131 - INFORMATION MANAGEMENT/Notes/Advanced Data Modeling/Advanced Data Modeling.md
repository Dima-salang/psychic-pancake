  

# Extended Entity Relationship Model

- referred to as the enhanced entity relationship model, is the result of adding more semantic contructs to the original ER model.
- A diagram that uses the EERM is called an EERD.

  

  

## Entity Supertypes and Subtypes

- We need to find a way to group.
- A PILOT entity stores only attributes that are unique to pilots and that the EMPLOYEE entity stores attributes that are common to all employees.
    - PILOT is a subtype of EMPLOYEE and that EMPLOYEE is the supertype of PILOT
- In modeling terms, an entity supertype is a generic type that is related to one or more entity subtypes. The enity supertype contains common characteristics, and the entity subtypes each contain their own unique characteristics.
- Two criteria help the designer
    - There must be different, identifiable kinds or types of the entity in the user’s environement
    - The different kinds or types of instance should each have one or more attributres that are unique to that kind or type of instance.

  

## Specialization Hierarchy

- Entity supertypes and subtypes are organized in a specialization hierarchy, which depicts the arrangement of higher-level supertypes (parent entities) and lower-level entity subtypes (child entities).

  

  

### Inheritcance

- enables an entity subtype to inherit the attributes and relationships of the supertype.
- entity subtypes inherit all relationships in which the supertype entity participates.

  

### Subtype Discriminator

- is the attribute in the supertype entity that determines to which subtype the supertype occurrence is related.

  

### Disjoin and Overlapping Constraints

- also known as nonoverlapping subtypes are subtypes that contain a unique subset of the supertype entity set; in other words, each entity instance of the supertype can appear in only one of the subtypes.
- If the business rule specifies that employees can have multiple classifications, the EMPLOYEE supertype may contain overlapping job classification subtypes. Overlapping subtypes are subtypes that contain nonunique subsets of the supertype entity.
    
    - A person may be an employee, a student, or both. STUDENT and EMPLOYEE are overlapping subtypes of the supertype PERSON.
    
      
    
      
    

### Completeness Constraint

- specifies whether each entity supertype occurrence must also be a member of at least one subtype. The completeness constraint can be partial or total.
- Partial completeness means that not every supertype occurrence is a member of a subtype.
- Total completeness means that every supertype occurrence must be a member of at least one subtype

  

  

### Specialization and Generalization

- Specifalization is the top-down process of identifying lower-level, more specific entity subtypes from a higher-level supertype.
- Specialization is based on grouping the unique characteristics and relationships of the subtypes.
- Generalization is the bottom-up process of identifying a higher-level, more generic entity supertype from lower-level entity subtypes. Generalization is based on grouping the common characteristics and relationships of the subtypes.

  

  

## Entity Clustering

- is a virtual entity type used to represent multiple entities and relationships in the ERD. An entity cluster is formed by combining multiple interrelated entities into a single, abstract entity object.

  

## Entity Integrity: Selecting Primary Keys

- The PK’s function is to guarantee entity integrity.

  

### Natural Keys and PKs

- A natural key or natural identifier is a real-world, generally accepted identifier used to uniquely identify an entity. It is familiar to end users and forms part of their day-to-day vocab.
- Usually, if an entity has a natural identifier, a data modeler uses it as the PK of the entity being modeled.

  

### PK Guidelines

- The main function is to uniquely identify an entity instance or row within a table. In particular, given a PK value—that is, the determinant, the relational model can determine the value of all dependent attributes that describe the entity. The function of the PK is to guarantee entity integrity, not to describe the entity.
- Database applications should let the end user choose among multiple descriptive narratives of different objects, while using PK values behind the scenes.

![[/Untitled 25.png|Untitled 25.png]]

  

### When to User Composite PKs

- as identifiers of composite entities, in which each PK combination is allowed only once in the M:N relationship
- as identifiers of weak entities, in which the weak entity has a strong identifying relationships with the parent entity.

  

### When to use Surrogate PKs

- A natural key might not exist. In these cases, it is standard practice to create a surrogate key.
- A surrogate key is a PK created by the database designer to simplify the identification of entity instances. the surrogate key has no meaning in the user’s environmen—it exists only to distinguish one entity instance from another.
- One practical advantage of a surrogate key is that because it has no intrinsic meaning, values for it can be generated by the DBMS to ensure that unique values are always provided.