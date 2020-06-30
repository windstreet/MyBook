# list分组

```python
def run(list_obj, step_length=4):
    return [list_obj[i:i+4] for i in xrange(0, len(list_obj), step_length)] 


# In [1]: def run(list_obj, step_length=4):
#    ...:     return [list_obj[i:i+4] for i in xrange(0, len(list_obj), step_length)]
#    ...:
# 
# In [2]: a = [1, 2, 3, 4,5, 6, 7, 8, 9, 10]
# 
# In [3]: run(a)
# Out[3]: [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10]]
```
