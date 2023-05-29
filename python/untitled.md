# Multiprocessing

```python
from multiprocessing.dummy import Pool


num_connection = 10
threadPool = Pool(num_connection)


def square(num):
    return num * num


input_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

outputs = threadPool.imap(square, query_list)

print(outputs)

```
