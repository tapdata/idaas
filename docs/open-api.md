# Open API

## Index
- Auth
- Jobs
- [Connections](#Connections)
- [Pipelines](#Pipelines)
- Users



## Connectivity & Authentication

How to Connect & Authenticate 

## Connections

### Functions

	GET    listConnection
	POST createConnection 
	PATCH updateConnection
	DELETE deleteConnection

### Connection Methods

- getConnectionAttribute
- List connections (by status, owner, type, )
- Get Connection by ID
- Create Connection
- Update Connection Attributes
- Delete Connection 
- Verify Connection

## Jobs

Functions:

	GET    listJob
	POST createJob
	PATCH updateJob
	DELETE deleteJob

Job Methods:
	
	addPipeline
	removePpeline
	start
	stop
	reset
	status()
	getConfig()
	 

## Pipelines

Functions]=
	GET    listPipeline
	POST createPipeline
	PATCH updatePipeline
	DELETE deletePipeline

Pipeline Methods:
	
	start
	stop
	reset
	status()
	getConfig()
	  
## IncrementalModel

iModel

- List models
- Get model detail
- Create IncrementalModel
- Update IncrementalModel
- Delete IncrementalModel

## Database

List Databases
Get  Database by ID

## Table

List Tables (by database Id)
Get Table

## Tags
List Tags
Create Tag
Remove Tag



