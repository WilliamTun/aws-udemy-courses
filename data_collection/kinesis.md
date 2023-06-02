
# Kinesis data streams

- made up of many "shards"
- all_shards = [shard1, shard2, ... shard n]
- we have to provision x amount of shards prior to set up.
- Data is distributed across all the shards


### Typeical architecture:
Producers -[records]-> Kinesis -[records]-> consumers

a) kinesis is immutable: data cannot be deleted after it is inserted into kinesis
b) data is retained between 1-365 days

### Producers
"Producers" send data into kinesis data streams. 
examples of producers:
- applications
- client computers / mobile devices
- sdk
- kinesis agents 

### Records
Producers send "Records" into Kinesis streams. 
Records consist of:
- 1) partition key
- 2) data blobs - up to 1 MB
Data that shares the same partition key, goes to the same shard. 

### Consumers
Consumers can be:
- 1) applications 
- 2) lambda 
- 3) kinesis data firehose
They recieve records that consist of:
- partition keys
- sequence numbers
- data blob


# ================================

# Configuration 1: Capacity Mode 
### a) Provisioned mode
- you pick number of shards to configure kinesis with
- shards can be scaled manually or with api
- each shard can take 1 MB/s 
  or 1000 records/s
- you pay per shard provisioned per hour

### b) on-demand mode
- no need to provision shard capacity during set up
- no need to manage capacity
- by default, capacity is 4mb/second or 4000 records per second. 



