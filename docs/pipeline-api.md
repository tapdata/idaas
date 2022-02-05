# Pipeline API Reference

## Index

- Overview
- Source & Sinks/Targets
- Stateless Processors
- Stateful Processors

## Overview

iDaaS's data collect/process/output tasks are organized in a "Job".   A data pipeline is defined as a connected DAG, with at least one source stages,s zero or more processing stages and at least one sink/target stages.  A job may contain one or more pipelines. 

Depends on the language and client, you may use one of the following two methods to construct a pipeline:

- Fluent API
- Pipeline Config

A fluent API looks like this: 

	createPipeline("my pipeline").readFrom(src_stage).filter(filter_stage_func).writeTo(target_stage)
	
While an equivalent pipeline config JSON would look like this:

	{
		pipeline_name: "my pipeline", 
		readFrom: "database.table",
		filter: {
				return event.gender == 'M';   // filter out male 
		},
		writeTo: "database.table"
	}
	
iDaaS Pipeline engine is built on top of [Hazelcast's Jet streaming engine](http://jet-start.sh). Many of the Jet stage processors are directly supported in iDaaS pipeline. 	
	
In iDaaS, a pipeline must belong to a Job, for which  lifecycle can be managed. For instance, you may create a job, start/stop & reset the job.  All the pipelines will be start/stop/reset all together as a group. 



## Source & Sink Stages

All the database tables(including imodels) are automatically available as source or target stage. You may reference it using "DatabaseGroup.Database.Table" syntax.  DatabaseGroup can be omitted. 

For example, assuming we have a MySQL database "mysql_insurance", which contains 3 tables Customer, Policy, Claim.  Then you could reference these table as follows:

	pipeline.readFrom(mysql_insurance.Customer)

##  Stateless Processors

### 

rename(current_field_name, new_field_name)
renameAll(regex)


### Inspired by Mongo's Aggregation Framework
[https://docs.mongodb.com/manual/meta/aggregation-quick-reference/](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/)

- match
- project
- lookup (lookup from any arbitary managed table)
- redact
- limit (useful for testing)
- addFields
- replaceWith
- sample
- skip
- unwind

### Inherited from Jet Framework

* map
* filter
* flatMap
* merge
* mapUsingService
* mapUsingServiceAsync

#### UDF 
#### mapWithPython
#### mapWithJS


## Stateful Processors

### join
 
### aggregate

### rollingAggregate
 
### mapStateful