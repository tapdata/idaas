# Tapdata Incremental DaaS 

## Change Log



## What is iDaaS

iDaaS, or Incremental Data as a Service, is an open source data platform that integrates enterprise data silos in real time and provide a complete and unified data layer to serve the operational applications or analytical systems. 

![image](https://user-images.githubusercontent.com/1950232/152077313-a006a176-a5a1-4cf4-b739-826a77fe77ab.png)


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

The name "Incremental" is inspired Delta Lake. However, unlike the data lake or data warehouses, which typically loads the data  in batch mode, data in iDaaS is incrementally inserted/updated/deleted on a row by row level, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the industry first **Incremental Engine**, enables users to build mission critical operational applications, including web, mobile and backend applications. 


## How It Works

iDaaS can be used in two main scenarios: 

- Scenario A: As a real time data integration platform, building data pipelines
- Scenario B:  As a centralized data platform, similar to a data lake but with operational/transactional capability, creating data models and APIs to be consumed by downstream applications

###  Scenario A. Modern Data Integration Platform

Let's assume we have a MySQL database holds the CRM tables and we would like to replicate the customer data to another mysql instance dedicated for analytical purpose. 

#### 1. Use iDaaS Shell or Client DDK(Python or JS)
#### 2. Configure data sources in DaaS, and given each data source an unique name		
	> createConnection( {
				alias: "mysql_crm_db",
				ip: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: "xxxxxx",
				db: "crm" 			
			});  	
			`
	`> createConnection( {
				alias: "mysql_analytical_db",
				ip: 'demodb.tapdata.net,
				port: 3307,
				user: "demo"
				password: "xxxxxx"			
				db: "analyticaldb"				
			}); 
	    `	

#### 3. Create a data integration pipeline:

	> create_pipeline({alias: "my_pipeline"})
		.readFrom( mysql_crm_db.Customer)
		.writeTo(mysql_analytical_db.CRM_Customer,  {AutoCreate:true} )
		.start();

iDaaS will perform an initial load of the whole Customer table to mysql_analytical_db, then start CDC replication between two tables. 

#### 4. Check the running status 
	
	> my_pipeline.stats()

	Status: running
	Input Total: 2400
	Output Total: 2300
	Throughput:  500 events/second
	Last Input:   2022.02.01 15:00:03.203
	Last Output:  2022.02.01 15:00:03.829	




###  Scenario B. Enterprise DaaS Platform

We would like to build a centralized data platform to hold a copy of the master data that are currently scattered in data silos. We would like to use this data platform to serve many of the data requirements requested by different BU or application team.

- Confirm data requirements from business users
- Plan & Design the data architecture in DaaS Store, including Data Models and Data API 
- Configure source connection, enable CDC in source database
- Create models in DaaS(iModel), configure model's backing source table, and activate the model
- Create & Publish APIs backed by the iModel
- Start consuming data from the API

#### 1. Use iDaaS Shell or Client DDK(Python or JS)

#### 2. Configure data sources & daas db, and given each data source an unique name

	> createConnection( {
				alias: "mysql_insurance_db",
				ip: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: "xxxxxx",
				db: "insurance" 			
			});  	
			`
	> createConnection( {
				alias: "daas_db",
				ip: 'demodb.tapdata.net,
				port: 27000,
				user: "demo"
				password: "xxxxxxx"
				db: "daas_db"   				
			}, { DaaSDB:true});    # Note: DaaSDB flag indicates this is db used by the iDaaS


#### 3. Create a  data model in DaaS DB, backed by a source table:

	> createModel({  db_alias: "daas_db",name: "OmniCustomer" })
		.readFrom( mysql_insurance_db.Customer) 
		.startSync();

iDaaS will perform an initial load of the whole Customer table to MongoDB, then enter into real time sync mode. 

#### 4. Create RESTful API to allow application to query the Customer Data

	> createREST({  	group: "crm_api",
					name: "OmniCustomer" ,
					method: "GET",
					path: "/OmniCustomer",
					allowedParameters: ["type", "gender", "zipcode"],
					model: daas_db.OmniCustomer
				}).publish()

	# curl -H "auth:xxxx" http://daas_server:3030/daas/crm_api/OmniCustomer?gender=M
	
#### 5. Check Model's Status

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

#### DaaS Development Kit

Development Kit including easy to use APIs and commands to facilitate rapid data development activities. 

	
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

[DaaS Data Architecture](docs/daas-data-architecture.md)

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

### iDaaS Open API References & DDK

- [Open API References](docs/open-api.md)
- Python DDK
- Javascript DDK

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


