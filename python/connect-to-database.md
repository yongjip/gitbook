# Connect to Database

PostgreSQL/Redshift

```python
import psycopg2
import pandas as pd
 
 
output_name = 'your excel file name.xlsx'
 
user = "{USER_NAME}"
password = "{PASSWORD}"
dbname = "DATABASE"
host = "HOST"
port = 5439 # your port numbr
conn_str = "dbname='{}' user='{}' host='{}' password='{}' port={}".format(dbname, user, host, password, port)
 
conn = psycopg2.connect(conn_str)
cursor = conn.cursor()
 
 
q = """
SELECT *
FROM schema.table_name
LIMIT 300
;
"""
 
cursor.execute(q)
 
output = cursor.fetchall()
col_names = [desc[0] for desc in cursor.description]
 
df = pd.DataFrame(output, columns=col_names)
 
print(df)
 
df.to_excel(output_name, index=False)

```

PyHive - Hive

```python
from pyhive import hive


hive_conn = hive.Connection(host="{HOST}",
                            port=10000, # your port number
                            username="{USER_NAME}",
                            )
cursor = hive_conn.cursor()
cursor.execute("SELECT * FROM {TABLE}")
output = cursor.fetchall()
    
```

PyHive - Presto

```python
from pyhive import presto


q = """
SET hive.execution.engine = tez;
SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;
SET hive.strict.checks.large.query = false;
SET hive.mapred.mode = nonstrict;
SET hive.vectorized.execution.reduce.enabled = true;
SET hive.vectorized.execution.enabled = true;
SET mapreduce.map.memory.mb = 18000;
SET mapreduce.map.java.opts = -Xmx14400m;
SET mapreduce.reduce.memory.mb = 18000;
SET mapreduce.reduce.java.opts = -Xmx14400m;


SELECT *
FROM {TABLE}
;
"""

hive_conn = presto.Connection(host="{HOST}",
                              port=8889, # your port numer
                              username="{USER_NAME}",
                           )

cursor = hive_conn.cursor()
cursor.execute(q)
output = cursor.fetchall()

```

Hive - pyhs2

```python
import pyhs2


conn = pyhs2.connect(host='{host}',
                     port=10000,
                     authMechanism="PLAIN",
                     user='{username}',
                     password='{password}',
                     database='default'
                     )
cursor = conn.cursor()

print(cursor.getDatabases())

cursor.execute("SELECT * FROM {TABLE_NAME} LIMIT 10")

print(cursor.getSchema())
cursor.close()
conn.close()

```

Hive - Run queries in parallel using multiprocessing

```python
from pyhive import hive
from pyhive import presto
import tqdm
from multiprocessing.dummy import Pool


num_connection = 10
threadPool = Pool(num_connection)


def execute_hive_query(query_list):
    hive_conn = hive.Connection(host="{HOST}",
                                port=10000, # your port number
                                username="{USER_NAME}",
                                )
    cursor = hive_conn.cursor()
    for q in query_list:
        cursor.execute(q[:-1])
    try:
        output = cursor.fetchall()
    except:
        output = None
    return output


q = """
set hive.execution.engine = tez;
SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;
SET hive.strict.checks.large.query = false;
SET hive.mapred.mode = nonstrict;
SET hive.vectorized.execution.reduce.enabled = true;
SET hive.vectorized.execution.enabled = true;
SET mapreduce.map.memory.mb = 18000;
SET mapreduce.map.java.opts = -Xmx14400m;
SET mapreduce.reduce.memory.mb = 18000;
SET mapreduce.reduce.java.opts = -Xmx14400m;

SELECT *
FROM schema.table_name
;
"""

query_list = [[q], [q], [q], [q]]

list(tqdm.tqdm(threadPool.imap(execute_hive_query, query_list), total=len(query_list)))

```

