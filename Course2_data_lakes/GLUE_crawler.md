# GLUE and ATHENA

GLUE CRAWLER:
- can discover metadata from complex folder structures
  and create tables

ATHENA:
- expects data to be similar in a particular S3 path.
- DON'T mix files of different schemas in the same folder


# ORGANISING DATA BY SCHEMA

GLUE CRAWLER Example:
                          |--- US 
           |---- Sales ---|
S3 bucket -|              |___ ASIA
           |
           |              |--- employees
           |____ HR ------|
                          |___ training


Sales folder contains subfolders of same schema format
so glue data crawler can create a single "Sales" table
and partition by region (US vs asia)

HR folder contains data with schema format X in employees and schema format Y in training subfolders.
Thus glue will create two SEPARATE tables for these. 

conclusion:
GLUE crawler can create multiple tables for a given path. 



HOWEVER, A GLUE crawler that puts data into ATHENA CANNOT infer schemas automatically and automatically create tables. You have to SPECIFY specific paths for specific tables.
In this example, you would specify:
1. sales:        S3://bucket/sales
2. HR-employees: S3://bucket/HR/employees
3. HR-training:  S3://bucket/HR/training


# =================================================

# ORGANISING DATA BY SECURITY

1. Tier 1: Protected data  
   ... for internal use cross teams
2. Tier 2: Restricted data 
   ... for internal use for specific teams
3. Tier 3: strategic data 
   ... trade secrets, whose leakage cause legal / financial trouble

Security best practise:
1. least priviledge
2. separate buckets based on security tier


# =============================================

# ORGANISING DATA BY TIME

                          |--- SpringSummer
           |---- 2020 ----|
Sales -----|              |___ AutumnWinter
           |
           |              |--- SpringSummer
           |____ 2021 ----|
                          |___ AutumnWinter

You can make queries in athena to search for specific time points. 


You can create folders via KEY-VALUE pairs



                                |--- SEASON: SpringSummer
           |---- YEAR: 2020 ----|
Sales -----|                    |___ SEASON: AutumnWinter
           |
           |                    |--- SEASON: SpringSummer
           |____ YEAR: 2021 ----|
                                |___ SEASON: AutumnWinter



# ==============================

