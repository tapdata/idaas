# Tapdata Incremental DaaS 

## DISCLAIMER

This document is in pre-alpha stage. Any part of the documentation may be changed without advanced notice. 

## What is iDaaS

iDaaS, or Incremental Data as a Service, is an open source data platform that integrates and replicates enterprise data silos in real time,  provides a complete and unified data layer  to serve the operational applications or analytical systems. 

![image](https://user-images.githubusercontent.com/1950232/152077313-a006a176-a5a1-4cf4-b739-826a77fe77ab.png)


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

The name "Incremental" is inspired Delta Lake. However, unlike the data lake or data warehouses, which typically loads the data  in batch mode, data in iDaaS is incrementally inserted/updated/deleted on a row by row level, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the industry first **Incremental Engine**, enables users to build mission critical operational applications, including web, mobile and backend applications. 

Read more on [Architecture & Fundamentals](docs/fundamentals.md) page	

## How It Works

iDaaS can be used in two main scenarios: 

- Scenario A: As a real time data integration platform, connecting data sources to targets, building pipelines
- Scenario B:  As a centralized data platform, similar to a data lake but with operational/transactional capability, creating data models and APIs to be consumed by downstream applications

###  Scenario A. Modern Data Integration Platform

Create a data pipeline to replicate the Customer table in mysql_crm_db to mysql_analytical_db and name it as "CRM_Customer". Creates the table automatically in target database if not exists. 

Assuming the database connections were already configured, you only need to write statements like below in  iDaaS DDK/Shell:
	
		> createPipeline({alias: "my_pipeline"})
			.readFrom( mysql_crm_db.Customer)	
			.writeTo(mysql_analytical_db.CRM_Customer,  {AutoCreate:true} )
			.start();
		> my_pipline.status()			
			Status: running
			Input Total: 2400
			Output Total: 2300
			Throughput:  500 events/second
			Last Input:   2022.02.01 15:00:03.203
			Last Output:  2022.02.01 15:00:03.829	
				
	
 
###  Scenario B. Enterprise DaaS Platform

We would like to build a centralized data platform to hold a copy of the master data that are currently scattered in data silos. We would like to use this data platform to serve many of the data requirements requested by different BU or application team.
 
#### 1. Create a  data model in DaaS DB, backed by a source table:

	> createModel({  db: "daas_db",name: "OmniCustomer" })
		.readFrom( mysql_insurance_db.Customer) 
		.startSync();

iDaaS will perform an initial load of the whole Customer table to MongoDB, then enter into real time sync mode. 

#### 2. Create RESTful API to allow application to query the Customer Data

	> createREST({  	group: "crm_api",
					name: "OmniCustomer" ,
					method: "GET",
					path: "/OmniCustomer",
					allowedParameters: ["type", "gender", "zipcode"],
					model: daas_db.OmniCustomer
				}).publish()

You can verify the published API using curl:

	# curl -H "auth:xxxx" http://daas_server:3030/daas/crm_api/OmniCustomer?gender=M
	
#### 3. Check Model's Status

	> daas_db.OmniCustomer.status()
	
		iModel :			
			db: daas_db
			name: OmniCustomer
			count: 1000
			last update: 2022.02.01 12:00:02.039UTC
			replication delay: 1000ms
		Sync source: mysql_insurance_db.Customer
			state: running
			count: 1002
			last log entry: 2022.02.01 12:00:01.039UTC
			replication delay: 1000 ms
			count diff: -2
	
Interested yet? Follow this document to [Get Started](docs/quick-start.md)
  

## When to Use 

When iDaaS can be used?

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

####  State-of-the-Art Fluent Pipeline API 

Full suite of Pipeline APIs allows following benefits:

- Easy to develop
- Code versioning
- Quickly build/rebuild entire data platform
	
## More Features 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

See [full list of supported data sources & targets](docs/supported-databases.md)


#### Pluggable Architecture allows easy extension

iDaaS is designed with extension in mind. All major components, including Source, Processor and Target, are designed with extensibility in mind. One can easily follow the tutorial or documentation to create custom source, target or processors with the help of Plugin Develeopment Kit

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


## Version History & Roadmap
 
 Roadmap
 
## Documentation

### Quick Start @TJ

-  [Install iDaaS ](docs/installation.md)
	- Install using Docker 
	- Install from source
	- Install from Tapdata Cloud
- [Quick Start](docs/quick-start.md)
	- Setup Connections(Data Sources)
	- Create a Table to Table replication
	- Create a materialized view(wide table)
	- Publish a Data API

###  Concepts & Architecture

[iDaaS Overview](docs/fundamentals.md)

DaaS Data Storage Engine @Berry 
 
Observability @Aplomb

iDaaS Consistency Model

### Incremental Engine  

- Incremental Engine Architecture  

- [Open CDC Standard](docs/open-cdc.md)	@Berry

- Shared Log Mining 

- Incremental Verification   

### Working with iDaaS

- [iDaaS API References](docs/open-api.md)

- [Pipeline API](docs/pipeline-api.md)

- [Working with Metadata](docs/metadata.md)

- iDaaS Shell

- Python SDK

 
###  Plugin Development Kit - Extending iDaaS  @Aplomb

- iDaaS Pluggable Architecture 
- PDK Introduction 
- Tutorial: Create & Test a custom database  source 
- Tutorial: Create & Test a custom data target
- Tutorial: Create & Test a custom SaaS source
- Tutorial: Create & Test a custom processor
- Process for submitting plugin for certification review
- Plugin SPI Reference


### Tutorials & How To
- Working with iDaaS & iModel
	- Create a simple iModel
	- Publish a Data API 
	- Create a complex iModel backed by more than one source

- Working with Data Pipelines
	- [Heterogeneous Data Replication from MySQL to MongoDB](docs/tutorial-mysql-mongodb.md)


## Contribute
How to get involved

[Todo Projects List](docs/todo-list.md)

## References

[Open Metadata] (https://docs.open-metadata.org/openmetadata/schemas/overview)


