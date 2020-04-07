---
layout: post
title: "[TIL] Migrate MySQL to PostgreSQL in Docker"
description: ""
category: 
- Today_I_Learn
tags: ["docker", "postgre", "mysql", "TIL"]

---

#### Run Docker Postgres
```
docker run --name some-postgres -p 5432:5432 -v $PWD/psql-data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=mysecretpassword -d postgres 
```

#### Find docker IP
```
docker inspect <CONTAINER_ID> | grep -w "IPAddress" | awk '{ print $2 }' | head -n 1 | cut -d "," -f1
```

#### PSQL commands 

Check [here](http://jazstudios.blogspot.tw/2010/06/postgresql-login-commands.html)

Useful commands here.

- `\q` quit psql
- `\d` list all instances
- `\dt` list all tables
- `\connect Db_NAME`
- `\dl` list all databases


#### psql connect to DB
```
psql -U postgres -h <IP>
```

#### Dump MySQL to SQL first
 ```
 mysqldump --compatible=postgresql --default-character-set=utf8 -r DB.mysql -u root -p DB
 ```
 
#### Migragte from sql -> psql by mysql-postgresql-converter

```
git clone https://github.com/lanyrd/mysql-postgresql-converter
cd mysql-postgresql-converter
python db_converter.py databasename.mysql databasename.psql
```

#### Create DB in psql first
`CREATE DATABASE DB;`

#### Refine PSQL
- remove all "COMMENT 'KB'" in PSQL
- remove all default timestamp '"0000-00-00 00:00:00"' 
- modify "mediumint" to int4

#### About phpPgAdmin
 - Check php path on config  /etc/apache2/conf-enabled  add one here refer phpmyadmin
 - restart apache2 `service apache2 restart`
 - However it still need php to build with psql -> change to Windows UI [PgAdmin](http://www.pgadmin.org/)