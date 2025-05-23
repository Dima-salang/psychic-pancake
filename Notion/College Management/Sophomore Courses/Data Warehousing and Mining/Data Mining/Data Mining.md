> _**It is the nontrivial process of identifying valid, novel, potentially useful, and ultimately understandable patterns in data stored in structured databases (Fayyad et al., 1996)**_

  

> _**Necessity, who is the mother of invention - Plato**_

# Data Mining

- is the process of discovering meaningful new correlations, patterns, and trends by sifting through large amounts of data stored in repositories, using pattern recognition technologies as well as statistical and mathematical techniques (Gartner Group)
- is the analysis of (often large) observational data sets to find unsuspected relationships and to summarize the data in novel ways that are both understandable and useful to the data owner (Hand et al.)
- is an interdisciplinary field bringing together techniques from machine learning, pattern recognition, statistics, databases, and visualization to address the issue of information extraction from large databases (Evangelos Simoudis)
- Data mining is the process of discovering interesting patterns from massive amounts of data. As a knowledge discovery process, it typically involves:
    - data cleaning
    - data integration
    - data selection
    - data transformation
    - pattern discovery
    - pattern evaluation
    - knowledge representation

![[/image 17.png|image 17.png]]

  

A **pattern** is interesting if it s valid on test data with some degree of certainty, novel, potentially useful, and easily understood by humans. interesting patterns represent knowledge. **Measures** of pattern interestingness, either objective or subjective, can be used to guide the discovery process.

The major dimensions of data mining are **data, knowledge, technologies and applications.**

Data mining can be conducted on any kind of data as long as the data are meaningful for a target application.

![[/image 1 12.png|image 1 12.png]]

  

![[/image 2 10.png|image 2 10.png]]

![[/image 3 9.png|image 3 9.png]]

![[/image 4 7.png|image 4 7.png]]

# Cross Industry Standard Process: CRISP-DM

1. Business/Research Understanding Phase
    - define project requirements and objectives
    - translate objectives into data mining problem definition
    - prepare preliminary strategy to meet objectives
2. Data Understanding Phase
    - collect data
    - perform EDA
    - assess data quality
    - optionally, select interesting subsets
3. Data Preparation Phase
    - prepares for modeling in subsequent phases
    - select cases and variables appropriate for analysis
    - cleanse and prepare data so it is ready for modeling tools
    - perform transformation of certain variables, if needed
4. Modeling Phase
    - select and apply one or more modeling techniques
    - calibrate model settings to optimize results
    - if necessary, additional data preparation may be required for supporting a particular technique
5. Evaluation Phase
    - evaluate one or more models for effectiveness
    - determine whether defined objectives achieved
    - establish whether some important facet of the problem has not been sufficiently accounted for
    - make decision regarding data mining results before deploying to field
6. Deployment Phase
    - make use of models created

  

# Six Common Data Mining Tasks

- Description
- Estimation
- Prediction
- Classification
- Clustering
- Association

![[/image 5 6.png|image 5 6.png]]