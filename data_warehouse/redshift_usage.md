# Importing and exporting data

# COPY command [data into redshift]
- most efficient way to load data
- can be parallalized 
- can read from multiple data sources SIMULTANEOUSLY
  * S3 (requires manifest file + IAM role)
  * EMR
  * DyanmoDB
  * remote hosts
- copy can automatically decrypt data from S3 encyption at rest objects

# INSERT command [If data already in redshift table]
'''
INSERT INTO ... SELECT
'''


# UNLOAD command [data out of redshift]
- unload from redshift tables into S3

# Enhanced VPC routing
- force dataflow for copy and unload through AWS VPC
- if this is not set up, traffic flows through internet which is security risk

# AUTO-COPY from S3 / Aurora 
- set this up to automatically monitor an S3 bucket / Aurora database
- as data moves into source (S3/aurora), it will auto copy to redshift
# Streaming ingestion
- can take streaming data from kinesis data streams into redshift...


# DBLINK
- use connect Redshift to a postgreSQL service (eg. AWS RDS)
- use to COPY and SYNC between redshift and postgreSQL

