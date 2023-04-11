# 2- 使用@property

```python
# @property 装饰器把一个方法变成属性调用
class Student(object):
    @property
    def score(self):
		return self._score
    @soir.setter
    def score(self,value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
