---
Created: 2023-09-17T12:36
Reviewed: false
---
We spend most of our time online. Literally, we generate millions of data every day. We also generate data through data collection.

  

# What Data to Collect

- How the data will be collected
- Choose data sources
    - First-party
        - Data collected by an individual or group using their own resources
        - By you or your own company
    - Second-party
        - Data collected by a group directly from its audience and then sold
        - Another company’s direct data
    - Third-party
        - Data collected from outside sources who did not collect it directly
- Decide what data to use
- How much data to collect
    - Population
        - All possible data values in a certain dataset
    - Sample
        - A part of a population that is representative of the population
    - Sometimes it is challenging to collect data from the population, therefore we can infer from the sample
- Select the right data type
- Determine the time frame

  

  

# Data Formats and Structures

  

## Quantitative

- discrete
    - data that is counted and has a limited number of values
- continuous
    - data that is measured and can have almost any numerical value

  

## Qualitative

- categorical, name, description, string
- nominal
    - does not have any order
- ordinal
    - has a set order or scale

  

## Internal Data

- data that lives within a company’s own systems
- usually more reliable and easier to collect

  

## External Data

- data generated outside the organization

  

## Structured Data

- data organized ni a certain format such as rows and columns
- spreadsheets and RDBMS are examples
- structured data works nicely with a data model and databases
- Data model
    - a model that is used for organizing data elements and how they relate to one another
    - Data elements are pieces of information

  

  

## Unstructured Data

- data that is not organized in any easily identifiable manner
- audio and video files
- it might have internal structure but they don’t fit in rows and columns
- most of data generated today are unstructured

![[/Untitled 23.png|Untitled 23.png]]

  

# Data Modeling

- is the process of creating diagrams that visually represent how data is organized and structured. These are data models. They are the blueprint of how data is represented.

![[/Untitled 1 12.png|Untitled 1 12.png]]

  

1. Conceptual data modeling
    1. high-level overview of the data structure such as how data tneracts across an organization.
2. Logical data modeling
    1. focuses on the technical details of database such as relationships, attributes and entities
3. Physical data modeling
    1. depicts how a database operates. A physical datam model defines all entities and attributes used.

  

Entity-Relationship Diagrams or ERDs and the Unified Modeling Language or UML are the most widely techniques used to create data models.

  

  

# Data types

- a specific kind of data attribute that tells what kind of value the data is.

  

In spreadsheets, there are three data types:

- Number
- Strings
- Boolean

  

  

# Data tables or Tabular data

- a type of data structure that is arranged in rows and column. The rows are called as records and columns are fields.

  

# Wide and Long data

## Wide data

- data in which every data subject has a single row with multiple columns to hold the values of various attributes of the subject
- basically how most spreadsheets work
- wide data is preferred when
    - creating tables and charts with a few variables about each data point
    - comparing straightforward line graphs

![[/Untitled 2 8.png|Untitled 2 8.png]]

## Long data

- data in which each row is one time point per subject, so each subject will have data in multiple rows

![[/Untitled 3 8.png|Untitled 3 8.png]]

  

  

  

# Bias, Ethics, Privacy, and Credibility

  

  

## Bias

- a preference in favor of or against a person, group of people, or thing. It can be conscious or unconscious.

  

## Data bias

- a type of error that systematically skews results in a certain direction
- the way you collect data can also introduce data bias or skew the data.

  

## Sampling Bias

- a sample that is not representative of the population as a whole

  

## Observer Bias (Experimenter bias / researcher bias)

- the tendency for different people to observe things differently

  

## Interpretation Bias

- the tendency to always interpret ambiguous situations in a positive or negative way

  

  

## Confirmation bias

- the tendency to search for or interpret information in a way that confirms pre-existing beliefs
- people see what they want to see

  

  

# Good Data

  

- We need a process to identify good data sources

  

ROCCC

- Reliable
- Original
- Comprehensive
- Current
- Cited

It ROCCCs!

# Bad Data

- Everything that does not ROCCCs.

  

  

# Ethics of Data

  

  

### Data Ethics

- Well-founded standards of right and wrong that dictate how data is collected, shared, and used

  

## Aspects of data ethics

- Ownership
    - Who owns data?
    - Individuals own the raw data they provide and they have primary control over its usage, how it’s processed, and how it’s shared
- Transaction transparency
    - all data-processing activities and algorithms should be completely explainable and understood by the individual who provides their data
- Consent
    - An individual’s right to know explicit details about how and why their data will be used before agreeing to provide it
- Currency
    - individuals should be aware of financial transactions resulting from the use of their personal data and the scale of these transactions
- Privacy
    - privacy is personal
    - preserving a data subject’s information and activity any time a data transaction occurs
    - data protection or information protection
    - Protection from unauthorized access to our private data
    - Freedom from inappropriate use of our data
    - The right to inspect, update, or correct our data
    - Ability to give consent to use our data
    - Legal rights to access the data
- Openness
    
    - free access, usage, and sharing of data
    - open-sourcing
    
    for data to be considered open, it has to:
    
    - Be available and accessible to the public as a complete dataset
    - Be provided under terms that allow it to be reused and redistributed
    - Allow universal participation so that anyone can use, reuse, and redistribute the data
    
      
    
      
    
    ### Metadata
    
    - data about data

  

  

# Benefits of Organizing Data

- makes it easier to find and use
- helps you avoid making mistakes during your analysis
- helps to protect your data

  

## Naming Conventions

- consistent guidelines that describe the content, date, or version of a file in its name
- use logical and descriptive names for your files to make them easier to find

  

## Best Practices

Organize your files into folders (foldering)

Break folders down into subfolders

Move old projects to a separate location to create an archive and cut down on clutter

Align your naming and storage practices with your team

Develop metadata practices

  

## Best Practices for naming files

- file names should include:
    - the project’s name
    - the file creation date
    - revision version
    - consistent style and order
- file-naming conventions should act as quick reference points so they should short and straightforward.

Example:

- SalesReport_20231125_v02

  

  

# Securing Data

  

### Data security

- protecting data from unauthorized access or corruption by adopting safety measures.

  

Common spreadsheet programs have their own security features.