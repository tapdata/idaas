# iDaaS Admin API & Client Reference

## Index
- Overview
- Connection & Auth
- Shell utility commands

<!--Jobs & Pipelines-->  
- [Jobs](#Jobs)
- [Pipelines](#Pipelines)

<!--Database & Tables --> 
- DatabaseGroups   
- [Databases/Connections](#Databases)  
- Tables 
- Models
- REST API
- Tags

 
<!--System -->  
- Authentication
- Users & Roles
- System Settings
- Cluster & Instances
- Scheduled Tasks

## Overview

You may use one of the following methods to interact with iDaaS:

- iDaaS Shell: a command line interface to manage.  
- iDaaS Web/Cloud: GUI interface to manage/interact iDaaS. 

For integration purpose, we also provide Client SDKs to allow developers to create/manage pipelines from their respective applications or workflows. 

- Open API:  via Swagger standard to interact with iDaaS
- Python SDK: A set of Python APIs allows you to connect to iDaaS from Python program

@discuss:  

- shell, use Python or JS?  1) Audiences 2) UDF 3) Time to Market
- OpenAPI first or Python SDK first?
- Web: Enterprise only?

## Connectivity & Authentication

### iDaaS Shell

Example:

	# ishell  -h localhost -u admin@example.com -p
	Enter password: ******
	You are successfullly logged onto localhost:3030 as admin@example.com
	Tapdata iDaaS v2.0.123
	> 

ishell command line arguments:
	
	-h, --host:  iDaaS server host or IP, default to localhost
	-P, --port:  iDaaS Server port, default to 3030
	-u, --user: usernmae 
	-p, --password: password
	-t, --token: access token (used in place of username/password)

### Python SDK

	# pip3 install -r requirements.txt
	# python3
	> from tap import *
	> init(server, token)
	>  p = Pipeline()
	 
## Shell Commands

#### desc 

Describe the specified object/entity

Examples:

	> desc mysql_db
	alias: mysql_db
	host: demodb.tapdata.net
	port: 3306
	version:	5.8
	created by: John Doe
	created at: 2022.01.02 12:00:03
	
	> desc mysql_db.Customer
	db: mysql_db
	fields:
		CUSTOMER_ID VARCHAR
		NAME	VARCHAR
		PHONE	VARCHAR
		ADDRESS VARCHAR	
	indexes: 
		CUSTOMER_ID PK UNIQUE
	sample data:
	| CUSTOMER_ID  | NAME | PHONE    |ADDRESS    |
	| 123          | TJ   | 555-1234 | 1 Main St |
	
	
		

## Jobs
### Schema
### CRUD APIs 
### Object Level Methods
	
	start()
	stop()
	reset()
	stats()
	getConfig()
	setConfig(config_spec)
	addPipeline(pipeline_id)
	removePpeline(pipeline_id)
	listPipelines()
	 
## Pipelines
### Schema
### CRUD APIs 
	
	createPipeline
	updatePipeline
	deletePipeline
	getPipeline
	listPipelines
	
### Object Level Methods

#### start()
This is a sugarcoating method. It is equivalent of following:

* 	find the Job that this pipeline belongs to.
* 	If such job not already exists, create one, naming it as $pipeline_name + "_job"
* 	call the start() method on that job.  

#### stop()
Sugarcoating method, will call enclosing job's stop method. 

#### reset()
Sugarcoating method, will call enclosing job's reset method. 

#### stats()
Sugarcoating method, will call enclosing job's stats method. 
	
#### Additional Stage & Processor APIs

For pipeline stages, processors APIs, please see comprehensive details on this page: [Pipeline API](pipeline-api.md)

## Database Groups
### Schema
### CRUD APIs 
### Object Level Methods
	 
## Databases

### Schema/Type

 Source: [https://open-metadata.org/schema/type/databaseConnectionConfig.json ](https://open-metadata.org/schema/type/databaseConnectionConfig.json)
 
	{
		database_name: 
		database_uid: unique identifier for database,
		host:xxx,
		port:xxx,
		username:xxx,
		password:xxx		
	}

### CRUD Compatible APIs
	
#### createDatabase( database_spec, options )
Creates a database entry in iDaaS for management. Note this **does not** actually create a physical database in database server. 

Parameters:

	database_spec: JSON,  see schema definition for attributes
	options:  JSON format, specifying zero or more options 
		validate: true | false, default to true, whether to connect to database server & perform validation
		discover: true | false, default to true, whether to discover table schema
	
Return:
	OK | ERROR_CODE
#### updateDatabase(database_uid, update_spec)
update the properties of the managed database

Parameters:

	database_uid: database uid
	database_spec: JSON, the properties to be used to merge/overwrite the existing properties

Return:

	OK | ERROR_CODE
	
#### deleteDatabase(database_alias, cascading=false | true)
Remove database from iDaaS management. This **does not** actually delete database from source db server. 

Parameters:

	database_alias: database alias
	cascading: delete all the tables/models in this database

Return: 
	
	OK | ERROR_CODE
	
#### getDatabase(database_alias)
Gets the full properties of the database

Parameters:

	database_alias: database uid

Return:

	Object 

#### listDatabases(filter_spec)
Get a list of databases, filtered by the parameters.
Parameters:

	filter_spec: Mongo query syntax, in JSON.

#### validateDatabase(database_alias)

Attempt to connect to the database using the credentias and perform permission check. 

Return:

	RESULT | ERROR_CODE

### Object Level Methods 

#### getFeatures()

list supported features

Return:
	Array of strings

#### checkFeatureSupport(feature_name)

Return:
	String,  YES |  NO

#### discover()

Discover all table schemas



## Tables
A table is a metadata entity in iDaaS, it represents a table from a data source. Typically this is created by the PDK or iDaaS applications, rather than added by user.  Hence the create/update/delete API should not be published. 

### Schema
### CRUD APIs 

	createTable	Internal Only
	updateTable	Internal Only
	deleteTable	Internal Only
	getTable
	listTables

### Object Level Methods

Refer to [Table and Model API](table-model-api.md) for in-depth details	

## IncrementalModel
iModel is a special type of Table. Under the hood it is a regular table in DaaS data store. The key difference is an iModel is always backed by one or more other tables/imodels. 

### Schema
### CRUD APIs 

	createModel
	updateModel
	deleteModel
	getModel
	listModel

### Object Level Methods

	startSync()			// start or resume sync
	stopSync()			// stop sync
	resetSyncState()		// Reset sync state
	truncate()			// remove data  from iModel


## RestAPI
### Schema

### CRUD APIs 

	createAPI
	updateAPI
	deleteAPI
	getAPI
	listAPIs
	
### Object Level Methods

	publish()
	unpublish()
	stats()	// list API invokation stats
	

## Tags
### ---Schema---
### ---CRUD APIs--- 

	createTag
	updateTag
	deleteTag
	getTag
	listTags

#### assignTag(tag, target_id)

#### removeTag(tag, target_id)
	
### ---Object Level Methods---
None



## Users

## Roles

## System Settings

## Scheduled Tasks

## Reference Data


#### listDatabaseTypes(filter)
list all supported(or partially supported database types)

Parameters:

	filter: JSON
		metatype: database | file | httpapi | messaging 
