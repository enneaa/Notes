# 2-filter

```python
def is_odd(n):
    return n%2==1
list(filter(is_odd,[1,2,4,5,6,9,10,15]))
# 结果[1,5,9,15]
```

```python
# 删除序列中的空字符
def not_empty(s):
    return s and s.strip()
list(filter(not_empty,['A','B',None,'C',' ']))
```

### 用 Filter 求素数

```python
# 构造从3开始的记述序列
def _odd_iter():
    n=1
    while True:
        n=n+2
        yield n
# 定义一个筛选函数
def _not_divisible(n):
    return lambda x:x%n>0
# 定义一个生成器,返回下一个素数
def primes():
    yield 2
    it=_odd_iter()
    while true:
        n=next(it)
        yield n
        it=filter(_not_divisible(n),it)
# 打印1000以内的素数
for n in primes():
    if n<1000:
        print(n)
    else:
        break
```
