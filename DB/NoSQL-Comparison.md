# NoSQL Database comparison

## NoSQL Types

## KV store Based

There is no structure and relation. Focus on quickly store and retrieve. Easy to scale.

### When To Use

- caching
- queueing for task
- distributed information/configuration
- live infromation for pub/sub

### Example

#### Memcache

- a simple pure in-mem kv store
- only supports string -> string

#### Redis

- can simply be a in-mem kv.
- have capability to persist data to disk
- supports various value types: list, set.
- Always use redis over memcase for new project

## Column Based

Example: cassandra, HBase  
It is a two dimensional arrays, where each key mapped to a record. Each record has one or more columns which is also key/value pair.

In cassandra, if to store more complex column, you need to use UDT(user defined type)

### When To Use
- keeping unstructured, non-volatile information
- scaling

## Document Based

Example: MongoDB, CouchBase, CouchDB
Work in a simliar fashion to column-based ones, but it supports more `deeply nested and complicated` objects, like document inside another document.

### When to use
- nested information
- javascript friendly.(JSON)

## Graph 

Example: neo4j  
Use graph-like structures with nodes and edges.

### When to use

- handling complex relational information
- modelling and handling classification s

## Cassandra vs MongoDB

- cassandra performs so much better than MongoDB in large scale. Designed to be scalable easily.
- MongoDB is simple to set up and use for small scale applicaiton. 

## Reference
- https://www.digitalocean.com/community/tutorials/a-comparison-of-nosql-database-management-systems-and-models
- https://blog.pandorafms.org/nosql-databases-the-definitive-guide/
