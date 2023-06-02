# Glue Catalog Management

- When building a datalake, you have to prepare for changes to structure of data. 

Common ways data change:
- columns renamed
- new columns / deleted columns
- columns reordered
- data type changes

How to minimise impact on systems when data structure changes?

GLUE can handle data changes fine.
1. GLUE catalog has a schedule crawler to run periodically for automated updates
2. manually update data catalogue with schema changes

ATHENA and other data systems (EMR, Redshift spectrum) may not handle schema changes well. 


# ACCESS methods - index vs name
Athena can access columns via numeric index or col name. 
- File formats that support index based access:
  1. csv
  2. tsv
  3. ORC 
  4. parquet
pros of index approach:
  a) you can rename the columns in the catalogue
  b) you can add data at the END 
cons of index approach:
  a) you cannot insert columns at START or MIDDLE

- File formats that support name based access:
  3. ORC
  4. parquet



# Data type comparison
### CSV:
works well if order of existing columns is preserved
and new columns are added at the END. 

### Parquet:

PROS:
1. COLUMN NAME ACCESS. 
default access method is by COLUMN NAME, not INDEX.  Athen will refer to columns in parquet format by NAME. If column order is shuffled, that is fine! 
Column order does not matter when using parquet format.. so we can insert new columns in the MIDDLE of the data.
2. Parquet is EFFICIENT and COST-EFFECTIVE. 
   Great for BIG DATA. 


CONS: 
1. we cannot rename column names.





