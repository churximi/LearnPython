## 第3章 使用字符串
#### **字符串格式化**
字符串格式化操作符：%

只有元组和字典可以格式化一个以上的值。

格式化字符串里的百分号：用%%代替
```python
print u"%s的学号是%s" % (u"张三", u"N123")
```

#### 字符串方法
- find
- join
- lower
- replace
- split
- strip

---

## 第4章 字典
映射（mapping）

字典：键，值

字典中的键是唯一的，值不一定是唯一。

dict()

在字典中检查键的成员资格比在列表中检查值的成员资格更高效，数据结构的规模越大，两者的效率差距越明显。

字典的格式化字符串

#### **字典方法**
- clear （清空）
- copy （浅复制，shallow copy）
- get
- pop （删除某项）

---

## 第5章 条件、循环和其他语句
logging模块记日志

bool()函数

#### **语句**
- **if语句**
- **else子句**
- **elif子句**

#### **运算符**
- 比较运算符
- 相等运算符 ==
- 同一性运算符 is ：判断是否是同一个对象
- 成员资格运算符 in
- 布尔运算符 and，or，not

#### *三元运算符*
```python
a if b else c
# 如果b为真，则返回a，否则返回c
```

#### **循环**
- while循环
- for循环（尽量使用for循环）

#### **range和xrange**
#### zip()函数
#### reversed()反转
#### sorted() 排序

#### range()，用于反向迭代

#### 跳出循环
- break
- continue

#### while True/break

#### **列表推导式**
利用其他列表创建新列表。
```python
[x*x for x in range(10)]
[x*x for x in range(10) if x % 3 == 0]
[(x, y) for x in range(3) for y in range(3)]
```

#### **其他**
- pass
- del
