# Tapdata Incremental DaaS 

## What is iDaaS

iDaaS, or Incremental Data as a Service, is an open source data platform that integrates enterprise data silos in real time and provide a complete and unified data layer to serve the operational applications or analytical systems. 


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

The name "Incremental" is inspired Delta Lake. However, unlike the data lake or data warehouses, which typically loads the data  in batch mode, data in iDaaS is incrementally inserted/updated/deleted on a row by row level, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the industry first **Incremental Engine**, enables users to build mission critical operational applications, including web, mobile and backend applications. 


## Use Cases 

When and Where iDaaS can be used?

- Real time heterogenous database replication
- Build a real time Data as a Service platform
- Data processing / data serving store for BI/Reporting application
- Mainframe offloading
- Implement CQRS pattern
- Build a read caching layer in front of RDBMS
- Real Time / Operational Dashboard
- Data prep(extract, transform, load) for Data Lake or Data Warehouse
- Building materialized view
- Customer/Product/Service 360
- Transactional MDM data platform

And many more.

## How It Works


	
## Key Features 

#### Incrementally Updated

All changes, including insert/update/delete as well as DDL changes, are captured and replicated to the iDaaS to ensure the data platform is incrementally updated and sync-ed with the source systems.  

#### Correctness Guarantee

Count, row level, field level, incremental verification methods

#### Low Latency Replication, Mesurable 

Changes in source systems typically take less than one second to be reflected in the iDaaS platform.  The  replication delay can be accurately measured to allow user be aware 

#### Causal Consistency Guarantee(*)

Provide "Read your writes" as well as "Causal Consistency" guarantees under circumstances where stronger consistency is required to ensure user experience. 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

#### Pluggable architecture allows easy extension

All major components, including Source, Processor and Target, are designed with extensibility in mind. One can easily follow the tutorial or documentation to create custom source, target or processors. 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

#### Third Party CDC Integration

Already setup OGG, Attunity, HVR, Canal ? No problem, you can connect your CDC tool to iDaaS to enjoy the flow engine and data api capability.

#### Open API  

All functionalities can be accessed via Open API for easy integration. 

#### Cloud Native

Scalable architecture, docker compatible, can be easily deployed on-prem or on any of the major cloud providers.  

## Architecutral Diagram

 iDaaS conssits of following major components:
 
 - Incremental Engine
 - Flow Engine
 - Data Store
 - Service Engine

 
![image](https://user-images.githubusercontent.com/1950232/151819404-c7045673-119d-496e-9326-0436e00c1a59.png)

## Repository Structure

- idaas: main repo
- idaas-shell: Interactive shell for using all functionality of iDaaS
- idaas-fe: flow engine
- idaas-se: service engine
- idaas-ie: incremental engine
- idaas-web: Web GUI
- idaas-crud: CRUD framework for all data sources
- idaas-open-api: Open API 
- idaas-pdk: Plugin development kit

## Documentation

### [Setup & Installation ](docs/installation.md)

-  Install using Docker
-  Install from source
-  Install from Tapdata Cloud

### Tutorials 
- iDaaS Tasks
	- Create a simple iModel
	- Publish a Data API 
	- Create a complex iModel backed by more than one source
- General Data Pipeline Tasks
	- [Heterogeneous Data Replication from MySQL to MongoDB](docs/tutorial-mysql-mongodb.md)

### Learn the Fundamentals & Concepts
[Fundamentals](docs/fundamentals.md)

- iModel / IncrementalModel
- iDatabase / IncrementalDatabase
- Data API / Model backed RESTful/GraphQL or Streaming API
- Job
- Pipeline
- Connection
- Table
- CDC vs. ETL

[Open CDC Standard](docs/open-cdc.md)

[Components](docs/components.md)

- Incremental Engine
- Flow Engine
- Service Engine

[ Consistency Model](docs/consistency-model.md)

###  Plugin Development Kit - Extend iDaaS

### iDaaS Open API References & Client SDK
- [Open API References](docs/open-api.md)
- Client SDKs

### Contribute 
- How to contribute
- Coding conventions & standards

## References

[Open Metadata] (https://docs.open-metadata.org/openmetadata/schemas/overview)


