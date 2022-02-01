# Tapdata Incremental DaaS 

## What is iDaaS

iDaaS, or Incremental Data as a Service, is an open source data platform that integrates enterprise data silos in real time and provide a complete and consistent data layer to serve the operational applications or analytical systems. 


#### Key Difference Compared to Data Lake/Data Warehouse
In short, iDaaS is aimed to serve operational/transactional workload while data lake/data warehouses are exclusivelly designed to serve analytical workload. 

Unlike the data lake or data warehouses, which typically loads the data via nightly or hourly batch, data in iDaaS is incrementally inserted/updated/deleted, mirroring the changes in source systems as those changes occur.  The freshness of data, and the correctness of the data guaranteed by the industry first Incremental Engine, enables users to build mission critical operational applications, including web, mobile and backend applications. 



## Use Cases 

When and Where iDaaS can be used?

- Real time heterogenous database replication
	- MySQL to MySQL
	- PostgreSQL to MongoDB	
	- Oracle to MySQL	
	- MySQL to Redis
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

## Typical Workflow

### Heterogeneous Database Replication: MySQL to MongoDB

This is the fastest way to experience iDaaS platform with least setup & resources. 

	# install tapdata agent / idaas agent
	./tapdata-agent -i		# --interactive, enter into shell prompt
	> login('https://cloud.tapdata.net',  'demo@tapdata.net', 'tapdata');
	   OK
	> createConnection( {
				alias: "mysql_demo_db",
				ip: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: demo123,
				db: "insurance" 			
			});
  	  OK
  	>  createConnection( {
				alias: "mongodb_demo_db",
				ip: 'demodb.tapdata.net,
				port: 27000,
				user: demo
				password: demo123			
				db: "insurance"				
			}); 
   	 OK
	> createPipeline("demo_flow").readFrom(mysq_demo_db).
	
#### Real Time Dashboard
@todo

####  Microservices CRUD API
@todo

## Quick Start using iDaaS CLI + Cloud Console


	
## Key Features 

#### Incrementally Updated

All changes, including insert/update/delete as well as DDL changes, are captured and replicated to the iDaaS to ensure the data platform is incrementally updated and sync-ed with the source systems.  

#### Correctness Guarantee

Count, row level, field level, incremental verification methods

#### Low Latency Replication

Changes in source systems typically take less than one second to be reflected in the iDaaS platform

#### Causal Consistency Guarantee(*)

Provide "Read your writes" as well as "Causal Consistency" guarantees under circumstances where stronger consistency is required to ensure user experience. 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

#### Pluggable architecture allows easy extension

All major components, including Source, Processor and Target, are designed with extensibility in mind. One can easily follow the tutorial or documentation to create custom source, target or processors. 

#### Comprehensive Data Source Support

Support most common databases and messaging systems including but not limited to Oracle, MySQL, SQLServer, PostgreSQL,  MongoDB, DB2, Sybase, Kafka, MQ etc. 

#### Cloud Native

Scalable architecture, docker compatible, can be easily deployed on-prem or on any of the major cloud providers.  

## Architecutral Diagram

 iDaaS conssits of following major components:
 
 - Incremental Engine
 - Flow Engine
 - Data Store
 - Service Engine

 
![image](https://user-images.githubusercontent.com/1950232/151819404-c7045673-119d-496e-9326-0436e00c1a59.png)



## Documentation(Learn More)

### Installation 
- Test


### Fundamentals & Concepts
- DaaS & Components
- CDC vs. ETL
- Open CDC Standard
- Incremental Engine
- Flow Engine
- Service Engine
- Causal Consistency

### Tutorials & How To Guides

###  Plugin Development Kit

### iDaaS CLI

### Reference APIs



## References



