# DZ_04
NAME: postgres25032022
ZONE: us-central1-a
MACHINE_TYPE: e2-medium
PREEMPTIBLE:
INTERNAL_IP: 10.128.0.10
EXTERNAL_IP: 34.122.251.14
STATUS: RUNNING


postgres@postgres25032022:/home/anadyrov$ psql 
psql (14.2 (Ubuntu 14.2-1.pgdg18.04+1))
Type "help" for help.

postgres=# create database testdb;
CREATE DATABASE
postgres=# \c testdb
You are now connected to database "testdb" as user "postgres".
testdb=# create schema testnm;
CREATE SCHEMA
testdb=# 
testdb=# 
testdb=# 
testdb=# 
testdb=# create table testnm.t1(c1 integer);
CREATE TABLE
testdb=# \dt
Did not find any relations.
testdb=# insert into testnm.t1 values(1);
INSERT 0 1
testdb=# select * from testnm.t1;
 c1 
----
  1
(1 row)

testdb=# create role readonly;
CREATE ROLE
testdb=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 readonly  | Cannot login                                               | {}

testdb=# grant connect on database testdb to readonly;
GRANT
testdb=# grant usage on schema testnm readonly;
ERROR:  syntax error at or near "readonly"
LINE 1: grant usage on schema testnm readonly;
                                     ^
testdb=# grant usage on schema testnm to readonly;
GRANT
testdb=# grant select on all tables in schema testnm to readonly;
GRANT
testdb=# create user testread with encrypted password 'test123';
CREATE ROLE
testdb=# grant readonly t testread;
ERROR:  syntax error at or near "t"
LINE 1: grant readonly t testread;
                       ^
testdb=# grant readonly to testread;
GRANT ROLE
testdb=# c testdb testread
testdb-# \c testdb testread
Password for user testread: 
You are now connected to database "testdb" as user "testread".
testdb-> 
testdb-> SELECT * FROM t1;
ERROR:  syntax error at or near "c"
LINE 1: c testdb testread
        ^
testdb=> SELECT * FROM t1;
ERROR:  relation "t1" does not exist
LINE 1: SELECT * FROM t1;
                      ^
testdb=> SELECT * FROM testnm.t1;
 c1 
----
  1
(1 row)

testdb=> SHOW search_path;
   search_path   
-----------------
 "$user", public
(1 row)

testdb=> SET search_path TO testnm;
SET
testdb=> SELECT * FROM t1;
 c1 
----
  1
(1 row)

testdb=> \c testdb postgres
You are now connected to database "testdb" as user "postgres".
testdb=# \c testdb testread
Password for user testread: 
You are now connected to database "testdb" as user "testread".
testdb=> 
testdb=> 
testdb=> 
testdb=> 
testdb=> select * from testnm.t1;
 c1 
----
  1
(1 row)

testdb=> create table t2(c1 integer); insert into t2 values (2);
CREATE TABLE
INSERT 0 1
testdb=> 
