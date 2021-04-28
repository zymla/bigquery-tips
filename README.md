# bigquery-tips
## Print cost of last query
```
from google.cloud import bigquery
client = bigquery.Client()

print(round(list(client.list_jobs(max_results=1))[0].total_bytes_billed/1e12*5, 2), '$')
```
## Select all columns except some
`SELECT * EXCEPT(col1, col2)`
