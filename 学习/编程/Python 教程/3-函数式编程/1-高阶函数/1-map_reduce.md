# 1-map_reduce
### map() 函数
map() 函数接收两个参数,一个是函数,一个是 `lterable`,`map` 将传入的函数依次作用到序列的每一个元素,并把结果作为新的 `lterablor` 返回

```python
def f(x):
    return x*x
r=map(f,[1,2,3,4,5,6,7,8,9])
list(r)
```

### reduce()

```python
from functools import reduce
def add(x,y):
    return x+y
reduce(add,[1,3,5,7,9])
```
