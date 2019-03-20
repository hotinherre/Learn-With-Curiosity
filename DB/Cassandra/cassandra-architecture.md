# Cassandra Architecture 

## Replication

- Replica factor is the copy of the data. 
- Replica placement is how to store the replica data. Usually using simple strategy, where save replica in continuous nodes.

## Consistency

cassandra provide tunnable `consistency level`. It means before send response back to client, how many acknowledgement from node coordinator should wait. Three common `consistency level`: all, quorum, 1

When client request a write or read, it choose a random server as coordinator, coordinator find the right nodes to work with, as soon as it gets enough response, it answer back to client.

Node repair(remove entropy). Reparing node make node to sync up with other node holding same replica and update the stale data.

## Multi Data Center

Multi data center Cassandra is a bunch of independent clusters deployed in different data center that keep synchronized and consistent with one another.  
For write request, select one node of one datacenter as coordinator, beside communicate and write to nodes of local datacenter, coordinator also talk to other cluster and find a remote coordinator to do the write remotely.

## reference
- https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html
- https://learning.oreilly.com/videos/mastering-cassandra-essentials/9781491994122/9781491994122-video314884