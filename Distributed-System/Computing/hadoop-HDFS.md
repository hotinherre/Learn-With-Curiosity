# Hadoop

## What is Apache Hadoop?

Hadoop is an open-source framework designed for distributed storage and huge data set computing.
Writ

## Hadoop component

- HDFS - hadoop distributed file system. inspired by GFS.
- Yarn for job scheduling and cluster resource management.
- MapReduce for parallel processing
- Common libraries 


## HDFS

hdfs has five services as follows:
1. name node
2. secondary name node
3. data node
4. job tracker
6. task tracker

### Name Node(Master Node)

Name Node holds all files' metadata, number of blocks, data is store on which data node, where is replication  and other details. There is only one Name Node.

### Secondary Name Node

take care of checkpoints of file system metadata. It is known as checkpoint Node, helperNode for Name Node.

### Data Node(Slave Node)

stores actually data. Every data node send heartbeat to Name node every 3s.

### Job Tracker

usually runs in a separate node other than Name Node and Data Node. it is Daemon for map-reduce job. If job tracker is down, map-reduce work is halted

#### handle map reduce job
1. receive request from client
2. ask metadata and data location from name node
3. Find appropriate Task Tracker node to execute the map reduce job based on data locality.
4. monitor individual taskTrackers and send progress back to client

### Task Tracker

Task Tracker runs on data node. it execute map reduce job assigned by Job Tracker. It constantly send progress back to Job Tracker. If task tracker is down, job is assigned to other node.

