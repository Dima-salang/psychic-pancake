  

Extract, Transformation, and Load (ETL) (or Data Staging) takes the raw data from operational systems and prepares it for the data warehouse.

![[/image 16.png|image 16.png]]

## Extraction

- Reading and understanding source data and copying needed data into data staging area for further processing
- Implemented using gateways, standard interfaces, third-party products or custom developmen
- Two logical approaches:
    - Full
        - entire data source extracted—no need to track changes to data
    - Incremental
        - only data that has changed during specified time interval is extracted
        - more efficient, but requires identifying changes to data in operational source systems

### Extraction in DBs

- Oracle provides several techniques to track changes for incremental extraction:
    - Change Data Capture
        - captures changes made to table either synchronously (using triggers) or asynchronously (using redo log files)
    - Timestamps
        - can be used if operational systems has timestamps indicating date/time a row was last modified
    - Partitioning
        - can be used if source systems use range partitioning with a date key
    - Triggers
        - typically used in conjunction with timestamp columns—trigger updates timestamp column upon each change on the source table
- In SQL Server
    - Physical Extraction Methods
        - Online - can be done via a reference to another db or schema
        - Offline - extraction process access data in structures outside source system
            - Flat files
            - export data utility
            - backup files

  

# ETL Requirements

- need to handle different extract schedules
- need to join data from multiple sources for a single dimension
- need to determine correct source to use when multiple versions of a field are available from different sources.
- You must round up the requirements and that includes considering several factors:

  

## Business Needs

The business needs are the DW/BI system users’ information requirements. This narrowly means the information content that business users need to make informed business decisions. List KPIs

## Compliance

Changing legal and reporting requirements

## Data Quality

## Security

## Data Integration

usually takes the form of conforming dimensions and conforming facts in the data warehouse.

## Data Latency

describes how quickly source system data must be delivered to the business users via the DW/BI system.

## Archiving and Lineage

every DW needs various copies of old data, either for comparisons with new data to generate change capture records or reprocessing. It is recommended to stage the data (writing it to disk) after each major activity of the EtL pipeline.

all staged data should be archived unless a conscious decision is made that specific data sets will never be recovered in the future. each staged/archived data set should have accompanying metadata describing the origins and processing steps that produced the data.

## BI Delivery Interfaces

The ETL team must work closely with the modeling team and must take responsibility for the content and structure of the data that makes the BI applications simple and fast.

It is irresponsible to hand off data to the BI application in such a way as to increase the complexity and querying of the BI application.

  

# The 34 Subsystems of ETL

ETL has four major components