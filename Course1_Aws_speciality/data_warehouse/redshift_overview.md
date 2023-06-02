# Redshift
- Fully managed petabyte-scale dataware house
- 10x better performance than other DW's
  for ML, massive parallel query execution (MPP)
- use column storage
- SQL interface available!
- can scale up or down on demand
- built in replication and backup
- monitored with cloudwatch / cloud trail
  eg. which user querys are consuming high resources

# OLAP vs OLTP
- OLAP is optimized for complex data analysis and reporting
- OLTP is optimized for transactional processing and real-time updates.
- Redshift is OLAP! 


# Redshift use case
- accelerate ML workloads
- unified datawarehouse / lake

# Example use cases
- stock trade data
- analyse ad clicks
- aggregate gamng data
- analyse social trends

# DO NOT USE REDSHIFT FOR
- small datasets (Use RDS instead!)
- OLTP - fast transactions (Use RDS or dynamoDB instead!)
- Unstructured data. Transform unstructured data with EMR etc first before loading the pre-processed structured data into redshift
- BLOB data - i.e large binary files, images etc (Use S3 instead!)

# Redshift architecture
- It is a big cluster
- Leader node + many compute nodes
- compute nodes contain many node slices
- user / external clients communicate with leader node via SQL query interface

# Compute node types:
1. Dense Storage (DS) 
   ^ optimised for storage capacity
   uses HDDs for low pricing
   available in: 
   XL - 3HDD, 2TB of storage, 31Gb of ram
   8XL - 24HDD, 16TB of storage, 244Gb of ram 
   
2. Dense Compute (DC)
   ^ Optimised for compute and querying
   High performance, many CPUs and lots of RAM. 
   SSD (solid state disks)


# ============================================

# Redshift Spectrum 
- Query exabytes of unstructured data in S3 
  * without the need to load data into redshift cluster itself
  * without the need to transform the data
- you can have tables for S3 datalake
  and tables for data actually in redshift
- limitless concurrency
- can horizontally scale out to thousands of instances so queries can run quickly
- storage and compute are separated, so both aspects can be scaled independently of each other.
- supports Gzip and snappy compression
- supports many data formats (AVRO, CSV, JSON, ORC, PARQUET, TXT, TSV)



# ============================================

# Redshift durability

- replicates the data for you automatically
- data automatically & continuously backed up to S3
- THREE copies of data are maintained:
  1) on original
  2) on replica on compute nodes
  3) on periodic backup in S3
  ^ Great for disaster recovery

- If "retention" config is set to 0, automated backups will be turned OFF.


# Scaling redshift
- supports vertical and horizontal scaling
- during scaling, the old cluster remains for querying and reads ... while new cluster is being creatd. 
- data moved in parallel to new compute nodes. 


# Redshift distribution style

data in table is distributed across many compute nodes
and the slices within each compute nodes.

There are several ways to distribute data to these nodes/ slices:

1. AUTO (default)
   redshift figures out the optimal distribution style
   based on how big your table data is.
   AWS will choose EVEN / KEY / ALL for you...
2. EVEN
   regardless of values in each column
   we walk across each node/slice and sprinkles a little data in each slice, and continues circulating again and again.

   * Appropriate when data table does not participate in JOINs
   * or when there is no clear choice between KEY / ALL

3. KEY 
    rows are distributed in accordance to value in a specific column. leader node will place matching values (eg. from same column of table) on same node slice.
    * Appropriate when we are querying based on values in a column
    * makes sure all data associated with specific key value are located on same compute node / slice. Great for dictionary look ups

4. ALL
    multiplies storage required by number of nodes in cluster
    ... so it takes longer to load / update / insert data into multiple tables. 
    * appropraite for tables that are not updated often. 
    * small dimensional tables are not appropriate for ALL distribution. 

   


The ultimate goal is to distribute data uniformly across all nodes / slices + minimize data movement during query execution



# Redshift Sort keys

- similar to indexes in relational database systems
- when creating a table, you can define one or more of the columns as "SORT-KEYS"
- when data is loaded into an empty table, the rows can be stored in a sorted order, and all the rows with the same key can be stored close to each other. 
- sorting makes for FAST queries.

How to choose sort key?
1. Queries based on recency - used a time based sort key
2. Filtering on one column? - specify that column as sort key
3. Will this table be frequently joined? - specify the column that contains the IDs to be joined as the distribution key.


Sort key types:
1. Single column - appropriate if querying by a single column... eg. time dates

2. Compound sort key [default] - compound key specify multiple columns to filter data by... with speed priviledges given in the ORDER that each of the columns are listed by. 
- Useful when query applies CONDITIONS (eg. filter / joins)
- We have primarily, secondary columns, n columns. The primary column is the one we will query the most!

3. Interleaved sort keys
   Gives equal priviledge weight to each column in the sort key list. 
   - useful if multiple queries use different columns for filtering


