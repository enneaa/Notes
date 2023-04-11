# 6- 使用 Dict 和 Set
### Dict

```python
d={'Michael':95,'Bob':75,'Tracy':85}
d['Michael']
d['Bob']=67 #通过 key 放入value
```

```python
>>> 'Thomas' in d
False
>>> d.get('Thomas')
>>> d.get('Thomes',-1)
d.pop('Bob') #删除一个 key
```

> dict 的 key 必须是 **不可变对象**

### Set

```
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

```
s.add(4)
s.remove(4)
```

```
>>> s1=set([1,2,3])
>>> s2=set([2,3,4])
>>> s1&s2
{2,3}
>>> s1|s2
{1,2,3,4}
```
