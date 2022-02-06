# Metadata Management
 
Maintainer: @TJ

## Overview

Metadata Management provides these capabilities for iDaaS:

- Explore & Search data asset
- Manage user defined metadata for entities
- Organize tables & models 
- Manage auto-tagging rules

## Explore & Search

#### search(keyword, scope)

Search in iDaaS managed tables & table schemas, including :
	Table Name
	Table description(curated by admin)
	Column Name
	Column Description(curated by admin)

Parameters:

	keyword: keyword to search for
	scope: Tables, ALL, default to ALL
	
Return:

	A list of Tables, with matched items 
	
#### listDatabases(dbzone)

List all databases managed by iDaaS

Parameters:

	dbzone: specify DBZONE tag value, if used. Optional

Example:

	> listDatabases()
	mysql_demo_db
	mongodb_demo_db
	fdm_db
	mdm_db
	
#### listTables(db, tag)
	
List all the tables, including models 

Parameters:
	db:	only list tables belong to this database 
	tag:     only list tables with specified tag

Example

	> list tables --db mysql_demo_db 
	mysql_demo_db:
		Customer
		Policy
		Claim
	Total: 3
	
## Organize 
 

#### Add tag to a table

	> assignTag({PRIVACY_LEVEL: "PI"}, mysql_db.Customer);

#### Remove tag from a table

	> removeTag({PRIVACY_LEVEL: "PI"}, mysql_db.Customer);

#### Special tag: DBZONE

A set of related tables that serve an use case typically are organized / created within one same database. 
Sometimes we may have a set of databases that may be related, for instance, a complext application/service may have tables created in multiple database. To represent this relationship, you may use a special tag "dbzone" to group one or more related databases. 

	> assignTag({DBZONE: "external"}, mysql_demo_db);
	> assignTag({DBZONE: "fdm"}, mongodb_demo_db);
	

## Auto Tag Rules

You may define auto tag rules to assign/remove tags automatically when certain events occur.

@todo

## User Defined Entity Metadata

Privileged user can add/modify/delete custom attributes for following entities:

	Database
	Table / Model
	Column
	Job
	
	
#### setAttribute(entity, attributes)

set attribute to an entity

Parameters:

	entity: DB, Table or Column
	attributes: JSON, key value pairs
	
Example:

	setAttribute(mysql_db.Customer,  {"DC": "DC1", "CONTACT": "Steven"}）

#### removeAttribute(entity, attributes)
	
remove attribute from an entity

Parameters:

	entity: DB, Table or Column
	attributes: key/value to remove, value can be anything. 
	
Example:

	removeAttribute(mysql_db.Customer,  {"DC":1}）




