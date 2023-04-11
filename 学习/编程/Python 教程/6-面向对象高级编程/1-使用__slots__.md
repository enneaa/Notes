# 1- 使用 __slots__

```python
class Student(object):
    pass
	s = Student()
    s.name = 'Michael'
    print(s.name)
```

### 使用 __slot__
限制 class 实例能添加的属性:

```python
class Student(object):
    __slots__ = ('name','age') #用tuple定义允许绑定的属性名称
```

使用 `__slots__` 要注意，`__slots__` 定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
