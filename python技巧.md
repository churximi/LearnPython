## 一些python技巧
#### 交换变量
```python
x, y = 1, 2
print u"原来：x=%s,y=%s" % (x, y)
x, y = y, x
print u"之后：x=%s,y=%s" % (x, y)
```

#### if 语句在行内
```python
x, y = 1, 2
print "Yes" if x == 1 else "No"
print "Yes" if y == 1 else "No"
```

#### 连接
- 列表“相加”

```python
list1 = ["a", "b"]
list2 = ["c", "d"]
print list1 + list2
```

- 字符串“相加”

```python
print str(1) + "f"
```

- 不同类型放在一起

```python
print list1, 1, "e"
```

#### 数值比较
```python
x = 2
if 3 > x > 1:
    print x
if 1 < x > 0:
    print x
```

#### 同时迭代两个列表
```python
list1 = ["a", "b"]
list2 = ["c", "d"]
for item1, item2 in zip(list1, list2):
    print item1 + "★" + item2
```

#### 带索引的列表迭代
```python
list1 = ["a", "b", "c", "d"]
for index, item in enumerate(list1):
    print index, item
```

#### 列表推导式
```python
numbers = [1, 2, 3, 4, 5, 6]
even = [number for number in numbers if number % 2 == 0]
print even
```
