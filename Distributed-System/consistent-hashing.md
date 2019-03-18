# Consistent Hashing

consistent hashing enable elastic scaling for cluster of cache or databases.

## Why use consistent Hashing

Tradition hashing determines node ID by `hash(key) % N`. Where N is number of partition node(slot). When adding or removing a new node to cluster, entire cluster need to suffer downtime to do data migration and do remapping work for every objects(key).

With consistent hashing, add/remove new nodes only impact `K/N` objects. K is total number of object(key), N is total number of partition node(slot).

### TLDR
- uniformly distribute request
- dynamically remove/add server, minimal amount of data get impacted
 
## Example of use
- CouchBase
- DynamoDB
- Cassandra
- openstack swift storage

## How does it work

1. create the hash key space. 0 ~ X
2. form hash space as a ring.
3. place server in the ring. Node ID with hash(server_ip)
4. Insert a new record
   1. key = hash(record)
   2. start at key in the node ring, search clockwise to find first node. 
   3. insert record in the first node
![](https://i2.wp.com/www.acodersjourney.com/wp-content/uploads/2017/08/Key-Placement-on-Hash-Ring.jpg?w=966&ssl=1)

5. Add a new server between node 3, node 4
   1. key between node 3 and node 4 need to be remapped new node.
   2. on average, only k/n keys. k - total object, n - node number
6.  Remove server node 2.
    1.  key between node 1 and node 2 need to be remapped to node 3

## non-unifrom distribution

Originally, data are distributed across node uniformly. But after removing nodes, some servers may additional data from the removed nodes. After adding node, some node may has less data since some data remapped into new node.

### solution - virtual node

When adding one server, instead of only assigning one node 1 with fixed position, assigning mutiple node as A1,A2,A3,A4... and across entire hash ring. If some server is more powerful, we can assign more nodes to it. 

#### What's the benift of virtual node
Without virtual node, removing a server B, we need to map all data from Node B to Node C

With virtual node, to remove server B, we actually removing node B0,B1,B2.... Then all data of server B is randomly mapped to other virtual nodes to ensure the uniform distribution.

![](https://uploads.toptal.io/blog/image/129309/toptal-blog-image-1551794743400-9a6fd84dca83745f8b6ca95a2890cdc2.png)

## Reference

- https://www.acodersjourney.com/system-design-interview-consistent-hashing/
- https://en.wikipedia.org/wiki/Consistent_hashing
- https://learning.oreilly.com/videos/distributed-systems-in/9781491924914/9781491924914-video215269
- http://www.tom-e-white.com/2007/11/consistent-hashing.html
- https://www.toptal.com/big-data/consistent-hashing