# Tapdata iDaaS 

## DISCLAIMER

This document is in alpha stage. Changes will be made without advanced notice. 

## What is iDaaS

Tapdata iDaaS, or Incremental Data as a Service, is an open source implementation of the DaaS architecture. 
 
Data as a service (DaaS) is a data management strategy that uses the public cloud or private cloud to deliver data silo integration, processing, and data serving capabilities in an on demand fashion. 

Tapdata iDaaS is next generation real time data platform,  designed to provide an enterprise data serving layer to serve web, mobile and backend applications,as well as analytical use cases such as BI and reporting.  

<img width="603" alt="image" src="https://user-images.githubusercontent.com/1950232/159158412-88a1ad20-be53-4d10-bcd3-a312668f796d.png">

iDaaS features following capabilities:

- One place, one method to access enterprise core data assets
- Connect and replicate data from disparate databases to data platform or 
- Automatic data APIs, can be created on demand
- Modern, intuitive GUI for general purpose data processing and integration
- Programmable APIs to facilitate large, complex data processing jobs. 


## Comparison between Kafka based Solution & iDaaS

## How it Works
 
The main use case for iDaaS is to quickly make the data asset available to downstream applications. 

There are three main activities could happen in iDaaS:

#### Step 1: Catalog all your databases, tables, views, APIs that you may need

		> createConnection( {
				alias: "mysql_demo",
				host: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: "demo123",
				db: "insurance"
			});
		> createConnection( {
				alias: "mdm_db",
				host: 'demodb.tapdata.net,
				port: 27000,
				user: "demo",
				password: "demo123",			
				db: "mdm_db"				
			}); 
		
#### Step 2:  Build data pipelines with fluent API,  from data sources to targets, to data store, or to cloud 

	> createPipeline("my_pipeline")
		.readFrom( mysql_demo.CUSTOMER )
		.writeTo(mdm_db.OmniCustomer)
		.start()
		
	> my_pipline.status()           
        Status: running
        Input Total: 2400
        Output Total: 2300
        Throughput:  500 events/second
        Last Input:   2022.02.01 15:00:03.203
        Last Output:  2022.02.01 15:00:03.829  
	        
####  Step 3. Serve fresh data to your applications, via auto Data API or reverse sync. 

	> createREST({  	group: "crm_api",
					name: "OmniCustomer" ,
					method: "GET",
					path: "/OmniCustomer",
					allowedParameters: ["type", "gender", "zipcode"],
					model: mdm_db.OmniCustomer
				}).publish()

Step 3 is optional, you can use iDaaS purely for a data integration and data development purpose. 




## Who & When Can Use iDaaS

#### Application Develoepers
iDaaS can be used by Application developers in following use cases:

- Automatica API backend (Backend as a service) for data CRUD operations
- Code-less CQRS implementation
- Code-less RDBMS caching solution
- Code-less Producer / Consumer for Kafka 
- Mainframe offloading
- Implement CQRS pattern
- Full text search / Graph search 

#### Data Engineers or Data Analysts

For data engineers or data analysts,  iDaaS can be used as a modern, general purpose, low code ETL platform for various data sync, processing or data modeling activities.

- Data extract / transform / load
- Data processing for data warehouse
- Data modeling
- Kafka-based data integration alternative
- Event streaming platform

#### DBAs / System Engineers

iDaaS can be used by DBAs in following use cases:

- Heterogeneous database replication
- Real time backup
- Database HA
- Database clustering 
- Disaster Recovery strategy
- Sync to cloud or cross cloud data replication

#### Data Steward

iDaaS can be used by data stewards in following possible scenarios:

- Build an enterprise master data management platform, either as a hub or transactional type
- As a metadata management solution
- As a data as a service platform to facilitate fast data distribution to BUs
 

  


## Core Capabilities
	
#### CDC based database event capture 

#### Incrementally Updated Data Platform

All changes, including insert/update/delete as well as DDL changes, are captured and replicated to the iDaaS to ensure the data platform is incrementally updated and sync-ed with the source systems.  

#### Correctness Guarantee

Count, row level, field level, incremental verification methods

#### Low Latency Replication, Mesurable 

Changes in source systems typically take less than one second to be reflected in the iDaaS platform.  The  replication delay can be accurately measured to allow user be aware 

#### Consistency Guarantee(*)

Provide "Read your writes" as well as "Causal Consistency" guarantees under circumstances where stronger consistency is required to ensure user experience. 

####  State-of-the-Art Fluent Pipeline API 

Full suite of Pipeline APIs allows following benefits:

- Easy to develop
- Code versioning
- Quickly build/rebuild entire data platform
	 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

See [full list of supported data sources & targets](supported-databases.md)


#### Pluggable Architecture allows easy extension

iDaaS is designed with extension in mind. All major components, including Source, Processor and Target, are designed with extensibility in mind. One can easily follow the tutorial or documentation to create custom source, target or processors with the help of Plugin Develeopment Kit

#### Third Party CDC Integration

Already setup OGG, Attunity, HVR, Canal ? No problem, you can connect your CDC tool to iDaaS to enjoy the flow engine and data api capability.

#### Interactive Shell

Explore, search, create, manage data models, create and run data pipelines
 
#### Python SDK

All functionalities can be accessed via Open API for easy integration.  

#### Cloud Native

Scalable architecture, docker compatible, can be easily deployed on-prem or on any of the major cloud providers.  


## Roadmap
 
 Roadmap
 
## Documentation

### Quick Start @TJ

-  [Install iDaaS ](installation.md)
	- Install using Docker 
	- Install from source
	- Install from Tapdata Cloud
	
- [Quick Start](quick-start.md)
	- Setup Connections(Data Sources)
	- Create a Table to Table replication
	- Create a materialized view(wide table)
	- Publish a Data API

###  Concepts & Architecture

- [iDaaS Overview](fundamentals.md)

- Incremental Engine: Architecture  

- [Open CDC Standard](open-cdc.md)	

- Incremental Verification   

### Working with iDaaS

- [iDaaS API References](open-api.md)

- [Pipeline API](pipeline-api.md)

- [Working with Metadata](metadata.md)

- iDaaS Shell

- Python SDK
 
###  Plugin Development Kit - Extending iDaaS

- iDaaS Pluggable Architecture 
- PDK Introduction 
- Tutorial: Create & Test a custom database  source 
- Tutorial: Create & Test a custom data target
- Tutorial: Create & Test a custom SaaS source
- Tutorial: Create & Test a custom processor
- Plugin API Reference

### Tutorials & How To

- Working with iDaaS & iModel
	- Create a simple iModel
	- Publish a Data API 
	- Create a complex iModel backed by more than one source

- Working with Data Pipelines
	- [Heterogeneous Data Replication from MySQL to MongoDB](tutorial-mysql-mongodb.md)


## Contribute

How to get involved

## References

[Open Metadata] (https://docs.open-metadata.org/openmetadata/schemas/overview)

[Open Lineage]

