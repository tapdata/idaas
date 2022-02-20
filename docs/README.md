# Tapdata Incremental DaaS 

## DISCLAIMER

This document is in pre-alpha stage. Any part of the documentation may be changed without advanced notice. 

## What is iDaaS

Data as a service (DaaS) is a data management strategy that uses the cloud(or centralized data platform) to deliver data storage, integration, processing, and servicing capabilities in an on demand fashion. 

Tapdata iDaaS, or Incremental Data as a Service,  is an industry first open source implementation of the DaaS architecture.  Compared to conventional DaaS implementation which is primarily designed for analytical use case, iDaaS is designed to provide an enterprise operational data layer to serve web, mobile and backend applications, in addition to the analytical use cases (BI, reports etc). 



can automatically connect dsiparate data sources and replicate data to centralized data store to provide a coherent and up-to-date enterprise data layer  to serve the  operational and analytical applications. 

![image](https://user-images.githubusercontent.com/1950232/152077313-a006a176-a5a1-4cf4-b739-826a77fe77ab.png)


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

The name "Incremental" is inspired Delta Lake. However, unlike the data lake or data warehouses, which typically loads the data  in batch mode, data in iDaaS is incrementally inserted/updated/deleted on a row by row level, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the  **Incremental Engine**, enables users to build mission critical operational applications, including web, mobile and backend applications. 

Read more on [Architecture & Fundamentals](fundamentals.md) page	

## How It Works

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

See [full list of supported data sources & targets](supported-databases.md)


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

[iDaaS Overview](fundamentals.md)

DaaS Data Storage Engine @Berry 
 
Observability @Aplomb

iDaaS Consistency Model

### Incremental Engine  

- Incremental Engine Architecture  

- [Open CDC Standard](open-cdc.md)	@Berry

- Shared Log Mining 

- Incremental Verification   

### Working with iDaaS

- [iDaaS API References](open-api.md)

- [Pipeline API](pipeline-api.md)

- [Working with Metadata](metadata.md)

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
	- [Heterogeneous Data Replication from MySQL to MongoDB](tutorial-mysql-mongodb.md)


## Contribute
How to get involved

[Todo Projects List](todo-list.md)

## References

[Open Metadata] (https://docs.open-metadata.org/openmetadata/schemas/overview)

[Open Lineage]


