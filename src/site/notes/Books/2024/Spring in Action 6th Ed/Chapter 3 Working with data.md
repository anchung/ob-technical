---
{"dg-publish":true,"permalink":"/books/2024/spring-in-action-6th-ed/chapter-3-working-with-data/","tags":["spring","spring-boot","jdbc","jpa"]}
---

> If there’s a file named `schema.sql` in the root of the application’s classpath, then the SQL in that file will be executed against the database when the application starts.

`src/main/resources/schema.sql`

> Preload data should be put in the `data.sql`

`src/main/resources/data.sql`

## JdbcTemplate

## Spring Data

**Spring Data JDBC** JDBC persistence against a relational database
**Spring Data JPA** JPA persistence against a relational database
**Spring Data MongoDB** Persistence to Mongo document database
**Spring Data Neo4j** Persistence to Neo4j graph database
**Spring Data Redis** Persistence to Redis key-value store
**Spring data Cassandra** Persistence to a Cassandra column store database

> One of the most interesting and useful features provided by Spring Data for all of these projects is the ability to automatically create repositories, based on a repository specification interface


