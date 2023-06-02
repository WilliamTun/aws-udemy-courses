# Integrating redshift with other services

- S3
- DynamoDB
- EMR
- EC2
- data pipelines 
- data migration services

# Redshift workload management
- prioritise short fast queries vs long slow queries
- create query queues
- interaction via CLI or API

# Concurrency scaling
- automatically add cluster capacity 
  to handle sudden increases in concurrent read queries
  ... eg. appropriate when service recieves BURST of queries / spikey workloads 
- support unlimited concurrent user & queries
- "WorkloadManager queues" manage which queries
   are sent to concurrency scaling cluster

# Automatic workload management
- creates up to 8 queues
- default mode: 5 queues - with even memory allocation

- if we have large queries going into a queue
  eg. query with big hash joins
  concurrency is automatically lowered
- if we have small queries (eg. inserts, aggregations)
  the concurrency is automatically raised

# Short query acceleration
- can be used in place of automatic workload management
- prioritise short running queries over long queries
- ML is used under the hood to estimate queries execution time. 
- the short queries are given dedicated priority and space ... so they don't wait in line behind a long query

can be used with:
```
CREATE TABLE AS <>
```
can be used with read only queries
```
SELECT statements
```
- a parameter is available to determine how many seconds is considered a "short query" is. 


# VACUUM command
- recovers space from deleted rows
1. VACUUM FULL : resort all rows and reclaim space from deleted rows
2. VACUUM DELETE ONLY
3. VACUUM SORT ONLY
4. VACUUM REINDEX : will reanalyse distribtion values in table sort key column - and then perform vacuum 


