# iDaaS Admin API & Client Reference

## Index
- Overview
- Connection & Auth


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
	 

## Jobs
### Schema
### CRUD APIs 
### Object Level Methods
	
	addPipeline(pipeline_id)
	removePpeline(pipeline_id)
	start()
	stop()
	reset()
	status()
	config( config_spec)
	 
## Pipelines
### Schema
### CRUD APIs 
	
	createPipeline
	updatePipeline
	deletePipeline
	getPipeline
	listPipelines
	
### Object Level Methods

See detail on this page: [Pipeline API](pipeline-api.md)

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
	
#### createDatabase( database_spec )
Creates a database entry in iDaaS for management. Note this **does not** actually create a physical database in database server. 

Parameters:

	database_spec: JSON, provide the properties
	
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

None


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


## RestAPI
### Schema

### CRUD APIs 

	createAPI
	updateAPI
	deleteAPI
	getAPI
	listAPIs

### Object Level Methods



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
