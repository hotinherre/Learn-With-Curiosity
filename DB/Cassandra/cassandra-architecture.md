# Cassandra Architecture 

## Partitioner

A partitioner determines which node will receive the first replica of a piece of data, and how to distribute other replicas across other nodes in the cluster.

## Snitch

A snitch determines which datacenters and racks nodes belong to

## Replication

- Replica factor is the copy of the data. 
- Replica placement strategy is how to store the replica data. 

### replication strategy
- SimpleStrategy: used in single datacenter. place first on a node determined by partitioner, then place additional replica on the next nodes clockwise in the ring.
- NetworkTopologyStrategy: used in multi-datacenter. Try to place replica in different racks.


## Consistency

cassandra provide tunnable `consistency level`. It means before send response back to client, how many acknowledgement from node coordinator should wait. Three common `consistency level`: all, quorum, 1

When client request a write or read, it choose a random server as coordinator, coordinator find the right nodes to work with, as soon as it gets enough response, it answer back to client.

Node repair(remove entropy). Reparing node make node to sync up with other node holding same replica and update the stale data.

## Multi Data Center

Multi data center Cassandra is a bunch of independent clusters deployed in different data center that keep synchronized and consistent with one another.  
For write request, select one node of one datacenter as coordinator, beside communicate and write to nodes of local datacenter, coordinator also talk to other cluster and find a remote coordinator to do the write remotely.

## Propagate node state information

### naive way
 
Each node periodically talk to all the other node. Expensive, and less efficient.

### gossip protocol

Gossip is a peer-to-peer communication protocol in which nodes periodically exchange state information about themselves and about other nodes they know about. Gossip runs every second and exchanges stat message up to 3 nodes in the cluster.

Cassandra use node state infomation to avoid routing request to node which are down or performing poorly.

### LSM Write in Cassandra

In cassandra, write is faster than write due to LSM - log structured merge tree.

1. Write to commited log on disk. Each node has one commit log. If node crashes before memtable is flushed, cassandra use commit log to restore last memtable.
2. Write into memtable. Each cassandra table has one memtable. Memtable contains record of that table with key ordered.
3. if memetable is full, flush memtable into SStable in disk. sstable is a file sorted by partition key. Each sstable is a copy of one memtable. SStable is immutable, to delete or update a record, just add a new record to sstable, clients only select the newest version. To delete, it mark the record as delete with tombstone.
4. Run offline compaction periodically to reduce the size of SStables.

### LSM Read in Cassandra

It is a process of assembling rows and columns we need.

1. Read from memtable. super fast, if found a complete row(it is not complete when the record is a one field update), then it must be the most recent data, since it hasn't been written into disk. Return it to client.
2. Find record in sstable. At the header of SStable, there is a `bloom filter` that can tell either a `certain negative` that key not in this table or `likely positive` that key is probably in this table.

### compaction

Any log structured merge tree storage engine needs compaction. compaction is offeline job, merge old SStable to new one. And remove old sstables. There are many strategies to do compactions. Not gonig to list any detail here.


## reference
- https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html
- https://learning.oreilly.com/videos/mastering-cassandra-essentials/9781491994122/9781491994122-video314884