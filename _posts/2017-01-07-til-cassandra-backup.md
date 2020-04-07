---
layout: post
title: "[TIL] Cassandra Backup Survey"
description: ""
category: 
- TodayILearn
tags: ["cassandra", "TIL"]

---


## Preface 

Our cassandra instance sometime got crash mess amount of data. To backup those data to another storage is task need to do.


## CQLSH

### First backup canssandra schema

Export entire schema of your cassandra.

```
cqlsh -e "DESC SCHEMA" > user_schema.cql
```

Or you can specific your keyspace here.

```
cqlsh -e "DESC SCHEMA" > user_schema.cql
```

### Export and Import cassandra data


#### Export 

```
COPY <tablename> [ ( column [, ...] ) ]
     FROM ( '<filename>' | STDIN )
     [ WITH <option>='value' [AND ...] ];
```

For example 

```
cqlsh> COPY log.parts FROM STDIN;
```

Another example to transform output format

```
cqlsh> COPY log.chatlogs (ts, content, other) TO './chatlog.dat'
   ... WITH DELIMITER = '|' AND QUOTE = '''' AND ESCAPE = '''' AND NULL = '<null>';
```
   
#### Import 

```
cqlsh> COPY emp (empid,deptid,last_name,first_name) FROM 'temp.csv';
```        

## Troubleshooting

1. Cannot dump all DB because partition.
 
```
Error for (-5813055698912042437, -5769658640073958582): Failed to connect to all replicas ['10.123.456.789'] for (-5813055698912042437, -5769658640073958582), errors: ['NoHostAvailable - (\'Unable to connect to any servers\', {\'10.123.456.789\': error(None, "Tried connecting to [(\'10.123.456.789\', 9042)]. Last error: timed out")})'] (will try again later attempt 1 of 5)
```

**Ans:**

Because your cassandra has three partition, and it is in GCE. We only export one port for `cqlsh` remote connect. So, you could not get any data from your other two partition.

Try install `cqlsh` in GCE machine with `pip install cqlsh`.



2. `pip install cqlsh` has some problem with `copy`.

```
'module' object has no attribute 'parse_options'
```

**Ans: **

We could not use `cqlsh` from `pip`, suggestion use cassanra's cqlsh. Refer [jira](https://issues.apache.org/jira/browse/CASSANDRA-12284)

```
sudo docker run -it cassandra /usr/bin/cqlsh
```

## Refer:

- [stackoverflow: Import and export schema in cassandra](http://stackoverflow.com/questions/16440606/import-and-export-schema-in-cassandra)
- [Simple data importing and exporting with Cassandra](https://www.datastax.com/dev/blog/simple-data-importing-and-exporting-with-cassandra)
- [Export/Import Data in Cassandra Table](http://techie-matter.blogspot.tw/2014/02/exportimport-data-in-cassandra-table.html)
- [AttributeError: 'module' object has no attribute 'compress'](https://issues.apache.org/jira/browse/CASSANDRA-10255) 



 