# What is TD-Hive
a java project,read from TDengine on Hive.
# Building 
## Install build dependencies
To install openjdk-8 and maven:
```
sudo apt-get install -y openjdk-8-jdk maven
```
To install docker-compose:
```
sudo apt-get install -y docker-compose
```
## Build
```
cd jdbc-handler
mvn install -DskipTests
```
# Run
1. start Hive
```
cd docker-Hive
docker-compose up
cp hive-jdbc-handler-2.3.2.jar into `hive-server` /opt/hive/lib
cp taos-jdbcdriver-2.0.36-dist.jar into `hive-server` /opt/hive/lib
```
2. create table 
* TDengine
```
create database test;
use test;
create table t1 (ts timestamp, i int);
insert into t1 values(now, 11)
```
  * Hive
```
hive
CREATE EXTERNAL TABLE t1
(
ts TIMESTAMP,
vol INT  
)
STORED BY 'org.apache.hive.storage.jdbc.JdbcStorageHandler'
TBLPROPERTIES (
    "hive.sql.database.type" = "TDENGINE","hive.sql.jdbc.driver" = "com.taosdata.jdbc.rs.RestfulDriver","hive.sql.jdbc.url" = "jdbc:TAOS-RS://$FQDN:6041/test?user=root&password=taosdata","hive.sql.dbcp.username" = "root","hive.sql.dbcp.password" = "taosdata","hive.sql.query"="select ts,vt1 from t1","hive.sql.dbcp.maxActive" = "1"
);
```
3. query
```
hive
select * from t1;
```

