# Tapdata Incremental DaaS 

## Change Log



## What is iDaaS

iDaaS, or Incremental Data as a Service, is an open source data platform that integrates enterprise data silos in real time and provide a complete and unified data layer to serve the operational applications or analytical systems. 

![image](https://user-images.githubusercontent.com/1950232/151978968-68c35570-fa0f-4350-bedf-5154ed2038a8.png)


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

The name "Incremental" is inspired Delta Lake. However, unlike the data lake or data warehouses, which typically loads the data  in batch mode, data in iDaaS is incrementally inserted/updated/deleted on a row by row level, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the industry first **Incremental Engine**, enables users to build mission critical operational applications, including web, mobile and backend applications. 


## How It Works

iDaaS can be used in two main scenarios: 
- As a centralized data platform, similar to a data lake
- As a real time data integration platform, similar to a modern ELT+L tool.

Typical workflow when used  as a **Data Integration Platform**:

- Configure source connection, enable CDC in source database
- Configure target connection
- Create pipelines to replicate data from source to target
- Add stateless or stateful transformation processors in the pipeline when needed
- Run the data pipeline

Typical workflow when used  as a **Enterprise Data Platform**:

- Confirm data requirements from business users
- Plan & Design the data architecture in DaaS Store, including Data Models and Data API 
- Configure source connection, enable CDC in source database
- Create models in DaaS(iModel), configure model's backing source table, and activate the model
- Create & Publish APIs backed by the iModel
- Start consuming data from the API




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


## Core Capabilities
	
#### Incrementally Updated Data Platform

All changes, including insert/update/delete as well as DDL changes, are captured and replicated to the iDaaS to ensure the data platform is incrementally updated and sync-ed with the source systems.  

#### Correctness Guarantee

Count, row level, field level, incremental verification methods

#### Low Latency Replication, Mesurable 

Changes in source systems typically take less than one second to be reflected in the iDaaS platform.  The  replication delay can be accurately measured to allow user be aware 

#### Consistency Guarantee(*)

Provide "Read your writes" as well as "Causal Consistency" guarantees under circumstances where stronger consistency is required to ensure user experience. 

	
## More Key Features 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

#### Pluggable Architecture allows easy extension

iDaaS is designed with extension in mind. All major components, including Source, Processor and Target, are designed with extensibility in mind. One can easily follow the tutorial or documentation to create custom source, target or processors. 

#### Third Party CDC Integration

Already setup OGG, Attunity, HVR, Canal ? No problem, you can connect your CDC tool to iDaaS to enjoy the flow engine and data api capability.

#### Interactive Shell

Explore, search, create, manage data models, create and run data pipelines
 
#### Open API  & Client SDK

All functionalities can be accessed via Open API for easy integration.  

#### Cloud Native

Scalable architecture, docker compatible, can be easily deployed on-prem or on any of the major cloud providers.  

## Who Are Using iDaaS

SANY Heavy Industry

Chowsangsang

First Auto

ChangAn Auto

China Eastern Airlines


 
## Documentation

### Installation

-  [Install using Docker ](docs/installation.md)
- Install from source
- Install from Tapdata Cloud

### Tutorials & How To
- Working with iDaaS & iModel
	- Create a simple iModel
	- Publish a Data API 
	- Create a complex iModel backed by more than one source
- Working with Data Pipelines
	- [Heterogeneous Data Replication from MySQL to MongoDB](docs/tutorial-mysql-mongodb.md)

### Fundamentals & Concepts
[Basic Concepts](docs/fundamentals.md)

- iModel / IncrementalModel
- iDatabase / IncrementalDatabase
- Data API / Model backed RESTful/GraphQL or Streaming API
- Job
- Pipeline
- Connection
- Table



[ Consistency Model](docs/consistency-model.md)

### Technical Architecture

[iDaaS Overview](docs/architecture-overview.md)

[iDaaS Components](docs/components.md)

Incremental Engine 

- Incremental Engine Architecture
- [Open CDC Standard](docs/open-cdc.md)
- [Third Party CDC Integration](docs/third-party-cdc.md)
- Incremental Verification: mechanisms and caveats
- Shared Log Mining

Flow Engine

- Flow Engine Architecture 
- Stream Join 
- Streaming Aggregation 
- Using IMDG for Caching

Consistency & Correctness Control

- Provide causal consistency 
- 

[Metadata Management](docs/metadata.md)

[Observability](docs/observability.md)

Reverse ETL

### iDaaS Open API References 

- [Open API References](docs/open-api.md)
- Python SDK

### iDaaS Shell Reference

- Overview
- Explore / Search data
- Working with iModels
- Working with Pipelines
- Working with Metadata
- Working with Data APIs
- Working with Data Sources & Targets
 
###  Plugin Development Kit - Extending iDaaS

- iDaaS Pluggable Architecture 
- PDK Introduction 
- Tutorial: Create & Test a custom database  source 
- Tutorial: Create & Test a custom data target
- Tutorial: Create & Test a custom SaaS source
- Tutorial: Create & Test a custom processor
- Process for submitting plugin for certification review
- Plugin SPI Reference



## References

[Open Metadata] (https://docs.open-metadata.org/openmetadata/schemas/overview)


