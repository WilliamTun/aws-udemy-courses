
# AMAZON RDS
- AWS RDS is a hosted relational database
- suitable for small data 
- not suitable for big data (use Redshift instead)

- faster than MySQL and postgreSQL
- 1/10th cost of other commercial databases
- up to 64TB per database instances
- can continious backup to S3
- can replicate across availability zones

# Database types:
1. AWS Aurora
2. MySQL
3. PostgreSQL
4. MariaDB
5. Oracle
6. SQL server


# ACID compliance
all RDS databases are ACID compliant
- atomicity: 
  ensures transaction is fully executed
  if part of transaction fails, the whole transaction is discarded
- consistency
  ensures data written to database adheres to rules that are set up. Eg. UNIQUE CONSTRAINTS
- isolation
  ensures each transaction is independent...
  which permits concurrent transactions
- durability
  ensures that the changes made to DB are permanent after a transaction is successfully completed. 


# SECURITY
- VPC network isolation
- can do at-rest encryption of data via KMS
- can encrypt data in transit via SSL 


