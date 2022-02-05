# iDaaS Shell

Maintainer: @TJ
## Overview

Daas shell is a command line utility. You can use it in following scenarios:

- Explore & Manage iDaaS Data Store 
- Explore & Manage iModels 
- Author, test, run & monitor the Jobs & Pipeliens

## Running Shell


## Command Reference 

### System Commands


### Admin API Objects

- Connections
- Models
- Jobs
- Pipelines
- Tags
- Users




### Top Level Objects
	
	daas: 常用api 功能入口
	db:		数据库连接入口
	
	# DaaS 基本数据对象
	Flow
	Connection
	TestUtil
	
	# 待实现
	Metadata
	Interface
	DaasUtil
	Stage
	

### Top Level Functions
	
	addDB
	listDB
	removeDB
	listFlow


### daas

> daas.addDB(properties) 
	
	> daas.addDB( {
				name: "Alpha DB",
				database_alias: "oracle_alpha",
				ip:
				port:
				user:
				password:			
			});
  	  OK
	> 

> daas.listDB()

List all connections

> daas.removeDB(dbo, cascade)

Delete connection.

	dbo: DB/Connection object
	cascade: true/false, whether delete flows/apis that are referencing (Not supported)

 
> daas.listFlow()

List all flows



## db

## Flow

> new Flow(name)

> from()
> 
> to()
> 
> map()
> 
> save()
> 
> submit()
> 
> stats()
> 
> status()
> 

## 5. Data Access API 

### Table / Collection API
		

**count**

Count of the rows in table/collection
	
	> db.oms.ORDER.count()
	10392

	> db.oms.ORDER.count({CUSTOMER_ID:100})
	12
	
**findOne**

Display the first item for quick check of the table.

	> ext.oms.ORDER.findOne()
	{
		_id: xxx,
		ORDER_ID: 101,
		ORDER_DATE: '2020-02-02',
		ORDER_TITLE: 'Cabrinha Kite 12m'
		ORDER_STATUS: 'A'
	}

**find**

Find items from the table

	> db.oms.ORDER.find(
			{ORDER_ID: 101}, 
			{CUSTOMER_ID:1, STATUS:1, ORDER_DATE:1}, 
			{limit:5} );

**sql**
	
	> db.alpha.sql("select * from customers where city='shenzhen'");

	
 
 
 
## Fluent DAG API

- Flow ~= DAG
- Flow 可以包含多个Pipeline
- Pipeline 如何定义


### Pipeline API

真正的API方法执行在Pipeline Stage上面。
	  
	.from(source)
	
	.to( sink )			// sink:  table, collection, file?
	
	.mapWithCache(cache, keyFn, valFn)	// cache, IMAP

	.match()			// 字段过滤
	
	.project()		// 字段处理器
	
	.mapWithJS()		// JS
		
	.merge()			// Jet API
	
	.group()
	
	.addFields()		 
	
	.limit()
	
	.skip()
	
	.lookup()
	
	.unwind()

	.filterTable()		//表过滤器

#### Example:
 
 		p = new Pipeline();
 		p.from().map().to(); 		
 		submit(p );
 		

### Flow API 

	tapdata.makeFlow()
	
	flow.newPipeline();
	flow.addPipeline(pipeline);
	flow.preview(NUMBR_OF_ROWS); // submit for preview
	flow.submit(); // submit for actual execution
	flow.reset()
	flow.setMode( INITIAL | CDC )
	flow.setSchedule({
		startTime: '2020-02-02 10:10:00",
		repeatable: {
		},
		xxxs
	})
	flow.getStats()
	flow.getStatus()
	
	
### Flow Example
	
 		p1 = new Pipeline();
 		p1.from().map().to(); 		

 		p2 = new Pipeline();
 		p2.from().map().to(); 		
 		
 		flow = makeFlow();
 		flow.addPipleline(p1,p2);
 		flow.name="my flow"
 
 		submit(flow);
 		



## Sample Scenarios

### Add Data Source/Sink



		addDB({alias:"insurance_db", database_type:"MySQL", type:"SOURCE", host:"x.x.x.x"});
		addDB({alias:"daas_db", database_type:"MongoDB", type:"TARGET", host:"x.x.x.x"});	
		

###  1. DB Clone task 
 

	var flow = tapdata.makeFlow({ name: 'demo flow' });	var pipeline = flow.newPipeline();
	pipeline.from( ext.product_db )
		.filterTable({ includes: ['PRODUCT','PROD_ATTR'] } })
		.to(fdm.product_db );		
			
	flow.execute();
	

###  2. Oracle - Mongo single table CDC 
 
	var flow = tapdata.makeFlow({ name: 'demo flow 2' });
	var pipeline = flow.newPipeline();
	pipeline.from(ext.product_db.PRODUCT )
			.map( prod -> { prod.NAME = prod.LONG_NAME; return prod })
			.to( fdm.product_db.PRODUCT1 ).to(fdm.product_db.PRODUCT2) 
	flow.submit();
	
	
###  3. Two FDM collection union
 
	var flow = tapdata.makeFlow({ name: 'demo flow 3' });
	var pipeline = flow.newPipeline();
	pipeline.from( fdm.alpha.sys_iv.CSS_MODEL, );
	pipeline.from( 
	fdm.alpha.sys_iv.CSS_MODEL.merge(fdm.shpd.sys_iv.CSS_MODEL)
		.to( mdm.pim.CSS_MODEL );	

	flow.submit();
	
### 4. With Union & Lookup

	var flow = tapdata.makeFlow({ name: 'Demo5 - Union and Lookup ' });
	var modelCache = tapdata.makeCache('modelCache', mdm.pim.CSS_MODEL);
	
	fdm.alpha.sys_iv.IV_INVNT_INDEX.addField("SRC_DB", "ALPHA")
		.mapWithCache(cache, 
						invnt->{ return invnt.MODEL_SEQ_NBR } , 
						(invnt,model) -> { invnt.MODEL_NBR = model.MODEL_NBR; return invnt} ).
		.to

	fdm.shpd.sys_iv.IV_INVNT_INDEX.addField("SRC_DB", "SHPD")
		.mapWithCache(cache, 
						invnt->{ return invnt.MODEL_SEQ_NBR } , 
						(invnt,model) -> { invnt.MODEL_NBR = model.MODEL_NBR; return invnt} );

	flow.add([ 	fdm.alpha.sys_iv.IV_INVNT_INDEX,  fdm.shpd.sys_iv.IV_INVNT_INDEX ])
		
		
	flow.submit();






## Todo
#### HIGH 
- Improve download page 
- Host SDK jar on tapdata.net website

#### MEDIUM
- a
- b



	- 每个数据库有一条并且仅有一条database元数据
	- 每个database 元数据有个alias 字段: 不超过10个字符，英文开头，可以有数字，下划线
	- 每个外部库的表自动实现  Source 类型
	- 每个中台库的表自动实现  SOurce / SInk 类型
	- 每个目标库的表自动实现 Sink类型




**list** 

List children elements

	> daas
	fdm		dir
	mdm	 	dir
	adm		dir
	ext		dir
	
	> ext
	pim		db
	crm		db
	oms		db

	> ext.oms.ORDER.count()
	102

	> fdm
	pim		db
	crm		db
	oms		db
	
	> fdm.oms

	ORDER				table
	ORDER_DETAIL		table 
	ADDRESS			table
	CUSTOMER_INFO		table
		
	>fdm.oms.ORDER.count()
	102

	> fdm.oms.ORDER.findOne()
	{
		_id: xxx,
		ORDER_ID: 101,
		ORDER_DATE: '2020-02-02',
		ORDER_TITLE: 'Cabrinha Kite 12m'
		ORDER_STATUS: 'A'
	}

**find**
Find items from the table

	> fdm.oms.ORDER.find()
	> fdm.oms.ORDER.find().limit(5)
	> fdm.oms.ORDER.find().sort({ORDER_DATE:1});
	> fdm.oms.ORDER.find({ORDER_ID: 101}, {CUSTOMER_ID:1, STATUS:1, ORDER_DATE:1} );

**sql**
	
	> ext.alpha.sql("select * from customers where city='xitang'");

	
