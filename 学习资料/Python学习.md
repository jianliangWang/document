# Python学习

## 1、什么是Python？优缺点？

    Python是一个脚本语言。 yPython是一个叫龟叔的荷兰老头写的（89年）

    擅长：

        1、爬虫

        2、自动化

        3、科学计算

        4、人工智能

## 2、环境搭建

1. python 官网 http://python.org

2. python解释器
   
   - CPython【主流】，底层是由C语言开发出来的Python解释器。
   
   - Jython是由Java语言开发出来的python解析器，钢鞭与让Python和Java代码做集成
   
   - IronPython是基于C#语言开发出来的Python解释器，方便与让Python和C#代码集成
   
   - RubyPython是基于Ruby语言开发出来的Python计时器
   
   - PyPy是对CPython的优化，他的执行效率提高了，引入编译器的功能。本质上将Python进行编译，再去执行编译后的代码。
   
   - ...

3. CPython解释器的版本
   
   1. CPython的解释器主要有两大版本：2.x和3.x

4. 安装Python解释器
   
   - 下载
   
   - 安装
     
     - 默认安装目录：/Library/Frameworks/Python.framework/Version
   
   - 使用Python编写HelloWord
     
     ```python
     name=input("请输入用户名")
     print("环境使用NB系统:", name)
     ```

5. 安装Pycharm编辑器
   
   下载地址： https://www.jetbrains.com/pycharm/download/download-thanks.html?platform=mac

## 3、基础

### 3.1 输出

---

将结果或内容呈现给用户

print("输出内容")

关于输出

- 默认print在尾部会加换行符

- 如果不想换行，则可以加end

print("输出内容",end="")

```py
print("看着风景美如画")
print("本想吟诗增填写")

print("Jay wang")

print("春眠不觉晓", end="")
print("处处闻啼鸟", end="")
print("")
print("夜来风雨声", end=",")
print("花落知多少", end=".")



 输出：

看着风景美如画
本想吟诗增填写
Jay wang
春眠不觉晓处处闻啼你
夜来风雨声,花落知多少.
```

### 3.2数据类型

- int 整数类型

- bool 布尔类型

- str 字符串类型

- list 列表类型

- tuple 元组类型

- dict 字典类型

- set 集合类型

- float 浮点类型

#### 3.2.1整型（int）

- 整型定义

```py
int
a = 1
print(a)

print(2 * 2)

print(2 ** 4)
```

- 转换成整型
  
  bool类型转整型
  
  n1 = int(True)
  
  字符串转整型
  
  v1 = int("186", base=10)  把字符串堪称十进制的值，然后再转换为十进制
  
  v1 = int("0b1001", base=2) 把字符串看成二进制的值，然后在转换成十进制
  
  v1 = int("0o144", base=8) 把8进制的字符串转换成十进制的值
  
  v1 = int("0x59", base=16) 把16进制字符串转换成十进制的值

#### 3.2.2布尔类型（bool）

- 定义
  
  data = False

- 转换
  
  整型只有0、空字符串、空列表、空元组、空字典转换为bool为False
  
  其他值均为True

- 其他
  
  如果在if、while条件后面写一个值当作条件时，他会默认转换为布尔类型，然后再做条件判断

#### 3.2.3字符串（str）

- 单行字符串
  
  ```py
  str
  aa = "ZhangSan"
  print(aa)
  print('单引号也是可以的')
  ```

- 多行字符串
  
  ```python
  print("""
  三引号是支持格式的
  这里换行了
  """)
  
  print('''三个单引号也是可以的
  和三个双引号是一样的效果
  ''')
  ```

- 拼接字符串+
  
  ```python
  print("Hi" + aa)
  
   输出结果：
  HiZhangSan
  ```

- 字符串支持乘
  
  ```python
  str
  aa = "ZhangSan"
  print(3 * aa)
  ```
  
         输出结果：
         ZhangSanZhangSanZhangSan

#### 3.2.4 列表（list）

- 定义
  
  user_list = []

- 获取数据，可以通过下标，user_list[0] 这是从左向右，从0开始递增，如果是从右向左取值，是从-1开始递减
  
- 列表嵌套，列表名称 = [[元素],[元素],元素]，取值，列表名称\[索引][索引]

- 追加

  user_list.append()

- 批量追加

  user_list.extend( [] )

- 插入 在原列表的指定索引位置插入值

  user_list.insert(插入位置,插入值)

- 删除值，在原列表中的根据值删除【慎用，里面没有会报错】

  user_list.remove(值)

- 删除值第二种方式

  del user_list[索引]

- 在原列表中根据索引踢出某个元素

  user_list.pop()#在user_list列表中删除最后一个，将踢出的值返回

  user_list.pop(2)#在user_list列表中删除索引为2的值，将踢出的值返回

- 清空原列表

  user_list.clear()

- 根据值获取索引【慎用，找不到会报错】

  user_list.index(值)

- 列表元素排序

  user_list.sort()# 从小到大排序

  user_list.sort(reverse=Ture) #list从大到小排序

- 反转原列表

  user_list.reverse()

- 统计某个元素有多少

  列表名.count(元素值)

#### 3.2.5 元组（tuple）

- 定义，元组是一个有序且不可变大容器，在里面可以存放多个不同类型的元素。
  
  v1= (1,2,3,4)
  
  注意：如果元组只有一个元素的时候需要在末尾加逗号

  元组内容不可以修改所以常用的方法只有三个
  
- 常用方法只有三个

  - index(元素)
  - count(元素)
  - len(元组名)


#### 3.2.6 集合（set）

集合是一个无序、可变、不允许数据重复的容器

- 定义
  
  定义及合适，只能使用v=set() 不能使用v=()(这样是定义一个空字典)

- 添加元素
  
  data = set()
  
  data.add("张三")

- 删除元素
  
  data.discard("李四")

- 交集
  
  data.intersections(data1)
  
  或者
  
  data & data1

- 并集
  
  data1.union(data)
  
  或者
  
  data | data1

- 差集
  
  data.difference(data1) #data中有，data1中没有的
  
  或者
  
  data-data1

- 集合的元素只能是int、bool、str、tuple 

#### 3.2.7 字典（dict）

字典是无序、健不重复且元素只能是健值对的可变的容器 

- 定义
  
  data = {} #定义一个空字典
  
  data = dict()
  
  data = {"name":"zhangsan", "age":13}

- 获取值
  
  data.get(key) 如果健不存在则返回None

- 获取所有健
  
  data.keys

- 获取所有值
  
  data.values

- 所有的键值
  
  data.items

- 设置值
  
  data.setdefault("age",18) #不存在新增，如果存在则什么都不做

- 更新值
  
  data.update( {"age":18, "name":"zhangsan"} )

- 移除指定键值对
  
  data.pop("age") #如果健不存在报错

- 按照顺序移除（后进先出）
  
  data.popitem()

- 求并集
  
  data ｜ data1

- 是否包含
  
  "age" in data
  
  "age" in data.keys()
  
  "age" in data.values()
  
  ("age", 12) in data.items()

- 索引
  
  data["age"]
  
  data["name"]
  
  如果健不存在报错

- 根据索引健修改值删除值添加值
  
  data[key]=value 如果已经存在更新，如果不存在新增
  
  del data[key] 删除，如果健不存在则报错

- 转换
  
  data = dict([key:value], [key1:value1])
  
  转换成
  
  data = dict(key:value, key1:value1)
  
  ![容器](/Users/jay/xmindworkspace/容器.png)

#### 3.2.8 浮点型（float）

表示小数

### 3 .3变量

变量名规范

- 变量名只能由字母、数字、下划线组成

- 不能以数字开头

- 不能用python的内置关键字

### 3.4 输入

    输入可以实现程序和用户之间的交互 
    name = input("请输入用户名")

### 3.5 条件语句

- 定义方式
  
  if 条件 : 
        条件成立 
  else: 
        条件不成立执行代码 
  if 条件A: 
    条件成立执行 
  elif 条件B: 
    条件B成立执行 
  elif 条件C: 
    条件C成立执行 
  else: 
    上述条件都不成立执行

### 3.6循环语句

- while循环
  
  ```python
  while 条件:
  条件成立，执行
  ```

- for循环

- 变量名只能由字母、数字、下划线组成
  
  - 不能以数字开头

- 不能用python的内置关键字
  
  ### 3.4 输入

输入可以实现程序和用户之间的交互 
name = input("请输入用户名")

### 3.5 条件语句

if 条件 : 
      条件成立 
else: 
      条件不成立执行代码 
if 条件A: 
  条件成立执行 
elif 条件B: 
  条件B成立执行 
elif 条件C: 
  条件C成立执行 
else: 
  上述条件都不成立执行

### 3.6循环语句

- while循环
  
  ```py
  while 条件:
  条件成立，执行
  ```

- for循环

### 3.7字符串格式化

- %
  
  name="aa"
  
  text = "我叫%s，今年18岁" %name
  
  name="aa"
  
  age=18
  
  text = "我叫%s，今年%d岁" %(name, age)
  
  message = "%(name)s，你好" %{"name":"张三"}
  
  百分比
  
  text = "s%，这个片我已经下载了90%%了，居然断网了" %"兄弟"
  
  一旦字符串格式化中出现百分比的现实，请一定要加%%以输出%

- format(推荐)
  
  ```python
  text = "我叫{0},今年18岁".format("zhangsan")
  text = "我叫{0},今年{1}岁".format("张三"，18)
  text = "我叫{0},今年{1}岁，真实姓名是{0}".format("张三", 18)
  text = "我叫{},今年18岁".format("zhangsan")
   text = "我叫{},今年{}岁".format("张三"，18)
   text = "我叫{},今年{}岁，真实姓名是{}".format("张三", 18, "张三")
  
  text = "我叫{n1},今年18岁".format(n1 = "zhangsan")
   text = "我叫{n1},今年{age}岁".format(n1 = "张三"，age = 18)
   text = "我叫{n1},今年{age}岁，真实姓名是{n1}".format(n1="张三", age = 18)
  ```
  
  ```python
  action = "跑步"
  text = f"张三喜欢{action}，跑完之后满身大汗"
  print(text)
  
  name="喵喵"
  age=19
  text = f"小猫的名字叫{name},今年{age}岁"
  print(text)
  
  
  #进制转换
  v = f"21的2进制表示方式{21:#b}"
  print(v)
  
  v = f"21的8进制表示方式{21:#o}"
  print(v)
  
  v = f"21的16进制表示方式{21:#x}"
  print(v)
  ```

### 3.8运算符

- 算数运算符

| 运算符 | 描述                      | 实例           |
| --- | ----------------------- | ------------ |
| +   | 加-两个对象相加                | a+b输出结果30    |
| -   | 减-得到负数或是一个数减去另一个数       | a-b输出结果-10   |
| *   | 乘-两个数相乘或是返回一个被重复若干次的字符串 | a*b输出结果200   |
| /   | 除-x除以y                  | a/b输出结果2     |
| %   | 取模-返回除法的余数              | a%b输出结果0     |
| **  | 幂-返回x的y次幂               | a**b为10的20次方 |
| //  | 取整除-返回商的整数部分            | 9/2输出结果4     |

- 比较运算符

| 运算符 | 描述                          | 实例   |
| --- | --------------------------- | ---- |
| ==  | 等于                          | a==b |
| !=  | 不等于，比较两个对象是否不相等             | a!=b |
| <>  | 不等于-比较两个对象是否不相等 python3中不支持 | a<>b |
| >   | 大于-返回x是否大于y                 | a>b  |
| <   | 小于                          | a<b  |
| >=  | 大于等于                        | a>=b |
| <=  | 小于等于                        | a<=b |

- 复制运算符

| 运算符 | 描述       | 实例  |
| --- | -------- | --- |
| =   | 简单的赋值运算符 |     |
| +=  | 加法赋值运算符  |     |
| -=  | 减法赋值运算符  |     |
| *=  | 乘法赋值运算符  |     |
| /=  | 除法赋值运算符  |     |
| %=  | 取模赋值运算符  |     |
| **= | 幂赋值运算符   |     |
| //= | 取整除赋值运算符 |     |

- 成员运算符

| 运算符    | 描述                             | 实例                |
| ------ | ------------------------------ | ----------------- |
| in     | 如果在指定的序列中找到值返回True，否则返回false   | "字符串" in text     |
| not in | 如果在指定的序列中没有找到值返回True，否则返回false | "字符串" not in text |

- 逻辑运算符

| 运算符 | 描述  | 实例               |
| --- | --- | ---------------- |
| and | 布尔与 | a and b返回true    |
| or  | 布尔或 | a or b 返回true    |
| not | 布尔非 | not(true)返回false |

> 运算符优先级
> 
> 加减乘除 > 比较 > not and or

## 4、函数 & 模块

![](/Users/jay/Library/Application%20Support/marktext/images/2022-10-28-14-20-41-image.png)

### 4.1 文件操作

##### 4.1.1 读取文件

- 读文本文件
  
  - 打开文件  path文件路径，可以是相对路径可以是绝对路径，mode是rb，r是读，b是二进制binary。如果直接想读文本内容，mode=rt, encoding=utf-8
    
    file_object = open(path, mode='rb')
  
  - 读取文件
    
    data = file_object.read()
  
  - 关闭文件
    
    file_object.close()

- 读图片等飞吻吧内容文件
  
  和以上代码一样，把路径换成图片路径

##### 4.1.2 写文件

- 写文本文件
  
  打开文件 如果直接想写文本内容，mode=wt
  
  file_object = open(path, mode='wb')
  
  写入内容
  
  file_object.write("zhangsan".encode("utf-8")
  
  关闭
  
  file_object.close()

- 写图片等文件
  
  f1 = open(图片路径， mode='rb')
  
  content = f1.read()
  
  f1.close()
  
  f2 = open(图片路径, mode="wb")
  
  f2.write(content)
  
  f2.close()

- 文件打开模式
  
  关于文件的打开模式常见应用有：
  
  - 只读：r、rt、rb
    
    - 存在，读
    
    - 不存在，报错
  
  - 只写：w、wt、wb
    
    - 存在，清空再写
    
    - 不存在，创建再写
  
  - 只写：x、xt、xb
    
    - 存在，报错
    
    - 不存在，创建再写
  
  - 只写：a、at、ab
    
    - 存在，尾部追加
    
    - 不存在，创建再写
  
  - 读写，可以读也可以写
    
    - r+、rt+、rb+， 默认光标位置：起始位置
    
    - w+、wt+、wb+，默认光标位置：起始位置（清空文件）
    
    - x+、xt+、xb+，默认光标位置：起始位置（新文件）
    
    - a+、at+、ab+，默认光标位置：末尾

#### 4.1.3 with

- 上下文管理
  
  with open(path, mode='r') as f:
  
      pass
  
  执行完后会自动关闭

#### 4.1.3 Excel格式文件

- 安装第三方
  
  pip install  openpyxl

- 读取Excel
  
  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook("files/test.xlsx")
  ```

#### 获取xlsx文件中的所有sheet名称

  workbook.sheetnames

#### 选中某一个sheet

  sheet = wrokbook["sheet名称"]

#### 读取单元格

  cell = sheet.cell(1,1) 

  #获取第一行的所有单元格
  print(sheet[1])

#### 获取素有行的数据

  for row in sheet.rows:
      print(row[0].value,row[1].value)

  #获取所有列的数据
  for cols in sheet.colums:
      print(cols[1].value)

- 修改Excel

- 原Excel文件基础上写内容
  
  ```python
  from openpyxl import load_workbook
   wb = load_workbook('files/p1.xlsx')
   sheet = wb.woksheets[0]
   cell = sheet.cell[1]
   # 写入excel
   cell.value = "姓名"
   workbook.save("files/test.xlsx")
   # 创建sheet
   sheet1 = workbook.create_sheet("工作计划")
   sheet1.sheet_properties.tabColor = "123213"
   workbook.save("files/test.xlsx")
   #拷贝一个sheet
   new_sheet = workbook.copy_worksheet(workbook["数据"])
   new_sheet.title = "新的工作计划"
   workbook.save("files/test.xlsx")
   #删除一个sheet
   del.workbook["数据"]
   workbook.save("files/test.xlsx")
  ```

- 新创建Excel文件写内容

#### 4.2.1压缩文件

```python
import shutil 
# 压缩文件
shutil.make_archive(base_name=r"", format="zip", root_dir=r"") 
# 解压文件
shutil.unpack_archive(filename="files.zip", extract_dir="file", format="zip")
```

### 4.2 函数

- 定义函数
  
  - def 函数名:
    
    方法 

- 例子
  
  ```python
  def test(param1, param2):
        print(param1)
        print(param2)
  ```
  
  test(1, 2)
  
  参数说明

param1, param2是形参

1和2是实参

- 参数名传参
  
  ```python
  def test_param_name(username, password):
   print("用户名是：" + username)
   print("密码是：" + password)
  
  test_param_name(password="1111", username="zhangsan")
  ```

- 默认参数
  
  如果参数不传使用默认值
  
  ```python
  # 默认参数
  def test_param_default(username, password, default="男"):
      print("用户名是：" + username)
      print("密码是：" + password)
      print("默认参数：" + default)
  ```

- 动态参数 
  
  - *
    
    只能按照位置传参数，按照元组类型来接收
    
    ```python
    def test_param_dynamic(*args):
        print(args)
    ```
    
    ```
    test_param_dynamic(1, 2, 3, 4, 5, 6)
    ```

- **
  
  只能按关键字传参，字典类型
  
  ```python
  def test_param_dynamic_kw(**kwargs):
      print(kwargs)
  test_param_dynamic_kw(name="zhangsan", age="12")
  ```
  
   

- *,**
  
  以上两种的结合
  
  ```python
  def test_param_dynamic_all(*args, **kwargs):
      print(args)
      print(kwargs)
  
  test_param_dynamic_all(1, 2, 3, name="username", password="123456")
  ```
  
    
  
  

- 返回值
  
  函数执行完之后将结果返回
  
  ```python
  def test_return():
      return False
  
  print(test_return())
  ```

### 4.3 闭包

- 闭包简而言之就是将数据封装在一个包（区域）中，使用时再去里面取

### 4.4 装饰器

- 装饰器代码

```python
def outer(orgin):
    def inner(*args, **kwargs):
        print("before")
        result = orgin(*args, **kwargs)
        print("after")
        return result
    
    return innter


@outer
def fun():
    pass

fun()
```

### 4.5匿名函数和三元运算

- 匿名函数
  
  lambda 参数：函数体
  
  **注意：** 匿名函数只能有一行函数题

- 三元运算
  
  结果 = 条件成立时 if 条件 else 不成立

### 4.6内置函数

![](/Users/jay/Library/Application%20Support/marktext/images/2022-11-04-10-45-41-image.png)

- 第一组（7个）
  
  - abs，绝对值
  
  - pow，指数
    
    pow(2,5) #2的5次方
  
  - sum，求和
  
  - divmod，求商和余数
    
    v1,v2 = divmod(9,2)
    
    v1是商4 v2是余数1
  
  - round，四舍五入

- 第二组（4个）
  
  - min最小值
  
  - max最大值
  
  - all是否全部为True
  
  - any是否存在True

- 第三组（3个）
  
  - bin十进制转二进制
  
  - oct十进制转八进制
  
  - hex十进制转十六进制

- 第四组（2个）
  
  - ord 获取自负对应的unicode码点（十进制）
  
  - chr根据码点（十进制）获取对应字符

- 第五组（9个）
  
  - int
  
  - float
  
  - str
  
  - bytes
  
  - bool
  
  - list
  
  - dict
  
  - tuple
  
  - set

- 第六组（10个）
  
  - len
  
  - print
  
  - input
  
  - open
  
  - type
  
  - range
  
  - enumerate
  
  - id 获取内存地址
  
  - hash
  
  - help
  
  - zip
  
  - callable 是否可被执行
  
  - sorted排序

### 4.7导入

import 应用场景：项目根目录的包模块级别的导入

from 应用场景：成员、嵌套的包模块级别的导入

### 4.8 内置模块

os

shutil

sys

random

hashlib

configpager

xml

### 4.9正则表达式

#### 4.9.1 字符相关

- [a, b, c] 匹配a或b或c

- [^a,b,c] 匹配除a,b或c以外的字符

- [a-z]匹配a-z的任意字符

- . 代指除换行符以外的任意字符

- \w 代指字母或数字或下划线(或汉子)

- \d 代指数字

- \s代指任意的空白字符，包括空格、制表符 

#### 4.9.2数量相关

- *重复0次或多次

- +重复1次或更多次

- ？重复0次或1次

- {n} 重复n次

- {n,} 重复n次回哦更多次

- {n,m}重复n到m次

#### 4.9.3括号（分组）

- 提取数据区域()

- 获取制定区域+或条件

#### 4.9.4 re模块

import re

findall，获取匹配到的宿友数据

match，从开始位置开始匹配，匹配成功返回一个对象，未匹配成功返回None

search，浏览整个字符串去匹配第一个，为匹配成功返回None

sub，替换匹配成功的位置

split，根据匹配成功的位置分隔

finditer，和findall功能基本相同，只不过返回的是迭代器

#### 4.10 练习 

62进制的练习题

```python
import itertools
import string

# 获取1-9A-z的数字列表
data = list(itertools.chain(string.digits, string.ascii_uppercase, string.ascii_lowercase))


def string62encode(num):
    str_encode_list = []
    # 先获取列表的长度，决定进制
    data_length = len(data)
    while num >= data_length:
        num, mod = divmod(num, data_length)
        str_encode_list.insert(0, data[mod])

    str_encode_list.insert(0, data[num])
    return "".join(str_encode_list)


print(string62encode(6200))

```

一行代码输出九九乘法口诀

```python
data = "\n".join([" ".join(["{}*{}={}".format(j, i, j * i) for j in range(1, i + 1)]) for i in range(1, 10)])
print(data)
```



## 5、异常处理

**语法** 

try:

​	可能发生错误的代码

except:

​	如果出现异常执行的代码



**捕获特殊异常**

try:

​	可能发生错误的代码

except NameError as e:

​	处理异常

**捕获多个异常**

try:

​	可能发生异常的代码

except(NameError, ZeroDivisionError) as e:

​	处理异常

**eles，没有异常的时候执行的代码**

try: 

​	可能产生异常的代码

exept:

​	处理异常

else:

​	没有异常的时候执行

**finally 无论有无异常都会执行** 

try:

​	可能产生异常的代码

except:

​	处理异常

finally:

​	有没有异常都会执行

## 6、实战（网络爬虫）

### 6.1 准备工作

安装requests

Pip install requests

指定源下载

pip install -i 国内源url requests

### 6.2get请求

```python
import requests

url = "https://sogou.com/web?query=%E5%BC%A0%E9%BE%99"

headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
}

resp = requests.get(url, headers=headers)

print(resp.text)

resp.close()

```



### 6.2 豆瓣电影爬取

``` python
# 豆瓣排行榜电影
import requests
import re
import csv


class DouBan:
    url = "https://book.douban.com/top250"

    headers = {
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
    }

    def __init__(self, url):
        self.url = url

    def get_books(self):

        resp = requests.get(self.url, headers=self.headers)

        page_content = resp.text
        obj = re.compile(r'<div class="pl2">.*?<a href=.*? onclick=.*? title=.*?>(?P<book_name>.*?)</a>.*?</div>',
                         re.S)
        result = obj.finditer(page_content)
        for book in result:
            print(''.join(book.group("book_name").replace('<span style="font-size:12px;">', "").replace("</span>", "")
                  .split()))

        resp.close()

    def get_movies(self, start_num):
        # url
        url = "https://movie.douban.com/top250"

        headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
        }

        params = {
            "start": start_num
        }
        resp = requests.get(url, headers=headers, params=params)

        # 获取到返回到页面
        page_context = resp.text
        # print(page_context)

        # 正则表达式获取需要到内容
        obj = re.compile(r'<li>.*?<div class="item">.*?<span class="title">(?P<name>.*?)</span>'
                         r'.*?<span class="rating_num" property="v:average">(?P<rating_num>.*?)</span>'
                         r'.*?<span>(?P<count>.*?)</span>', re.S)

        result = obj.finditer(page_context)

        file_write = open("top_movies.cvs", mode="a", encoding="utf-8")
        csvwriter = csv.writer(file_write)
        for it in result:
            dict_data = it.groupdict()
            csvwriter.writerow(dict_data.values())

        print("over")
        resp.close()


DouBan("https://book.douban.com/top250").get_books()

```

### 6.3 bs4使用

- 安装bs4

  pip install bs4

- 导入 bs4

  from bs4 import BeautifulSoup

- BeautifulSoup(rest.text, "html.parser")

- 获取电影天堂最新电影

  ```Python
  url = "https://www.dygod.net/html/gndy/china/index.html"
  
      headers = {
          "Accept-Language": "zh-CN,zh;q=0.9",
          "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36"
      }
  
      resp = requests.get(url, headers)
      resp.encoding = "gb2312"
  
      html = BeautifulSoup(resp.text, "html.parser")
      div = html.find("div", attrs={"class": "co_content8"})
      tables = div.find_all("table", attrs={
          "border": "0",
          "cellpadding": "0",
          "cellspacing": "0",
          "class": "tbspan",
          "style": "margin-top:6px",
          "width": "100%"
      })
      for table in tables:
          trs = table.find_all("tr")
          tds = trs[1].find_all("td")
  
          movie_name = tds[1].find_all("a", attrs={
              "class": "ulink"
          })
          print(movie_name[1].text)

### 6.5 xpath

- 安装lxml

- 导入

  from lxml import etree

### 6.6 代理

``` python
import requests

proxies = {
  "https":"https://ip:port"
  
}

resp = requests.get(url, proxies=proxies)

resp.encoding = "utf-8"
print(resp.text)
```

## 7、多线程的&多进程

### 7.1多线程定义

- 第一种写法

```python
from threading import Thread

def func():
  print("123")
  
if __name__ =="main":
  t.Thread(target=func)
  t.start()
  for i in range(1000):
    print("main",i)
```

- 第二种写法

```python
class MyThread(Thread):
  def run(self):
    for i in range(100):
      print("子线程",i)
  
if __name__=='main':
  t = MyThread()
  t.start()
  
  for i in range(1000):
    print("主线程", i)
```

### 7.2 多进程

第一种写法

```python
from multiprocessing import Process

def func:
  for i in range(1000):
    print("子进程", i  )
    
if __name__=='main':
  p = Process(target=func)
  p.start()
  for i in range(1000):
    print("main", i)
```

第二种写法



### 7.3 线程池

```python
from concurrent.futures import ThreadPoolExecutor,ProcessPoolExecutor

def fn(name):
  for i in range(1000):
    print(name, i)
    
if __name__ in '__main__':
  with TheadPoolExecutor(50) as t:
    for i in range(100):
      t.submit(fn, name=f"线程{i}")
      
  print("Over")
```

### 7.4 协程

```python
import asyncio
async def func():
  print()
  await asynicio.sleep(4)
async def func1():
  await asynicio.sleep(3)
  
async def func2():
  await asynicio.sleep(2)

async def main():
  tasks = [
    asyncio.crate(func())
    asyncio.crate(func1())
    asyncio.crate(func2())
  ]
  
  await asyncio.wait(tasks)
  
if __name__=='__main__':
  g = func
  asyncio.run(main() )

```

aiohttp

## 8 selenium

1. 安装pip install selenium
2. 下载浏览器驱动： https://registry.npmmirror.com/binary.html?path=chromedriver/&spm=a2c6h.24755359.0.0.6d444dccDqLGnz
3. 解压下载的浏览器驱动chromedriver 放到python解释器所在的文件夹
4.  **/Library/Frameworks/Python.framework/Versions/3.10/bin->** 

```python
from selenium.webdriver import Chrome 
#创建浏览器对象
web = Chrome()
#打开一个网址
web.get("https://baidu.com")
```



python 清华源: https://pypi.tuna.tsinghua.edu.cn/simple 
