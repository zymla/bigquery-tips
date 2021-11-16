# bigquery-tips
```
auth
gcloud auth login
gcloud auth application-default login
```
## Print cost of last query
```
from google.cloud import bigquery
client = bigquery.Client()

print(round(list(client.list_jobs(max_results=1))[0].total_bytes_billed/1e12*5, 2), '$')
```
## Select all columns except some
`SELECT * EXCEPT(col1, col2)`

## Max of LIST-column
```
SELECT (SELECT MAX(x) FROM UNNEST(col_array) AS x) AS col_array_max
FROM some_table
```

## First row in GROUP
Watch out for duplicates as `RANK()` gives same value to ties
```
SELECT * EXCEPT(rk) FROM (
  SELECT partition_key, ordering_column, other_col_1, other_col_2, 
    RANK() OVER(PARTITION BY partition_key ORDER BY ordering_column, other_col_1,other_col_2) AS rk
  FROM my_table
)
WHERE rk = 1
```
