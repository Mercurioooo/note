## Python 标识符

- 在 Python 里，标识符由字母、数字、下划线组成。但不能以数字开头,区分大小写的。

- 以下划线开头的标识符是有特殊意义的:
  - 以单下划线开头 **_foo** 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 **from xxx import \*** 而导入。
  - 以双下划线开头的 **__foo** 代表类的私有成员，以双下划线开头和结尾的 **__foo__** 代表 Python 里特殊方法专用的标识，如 **__init__()** 代表类的构造函数。

Python 可以同一行显示多条语句，方法是用分号 **;** 分开，如：

```
>>> print ('hello');print ('runoob');
hello
runoob
```

## 基本语法

严格缩进就完事儿了,相关的两个错误:

```
IndentationError: unindent does not match any outer indentation level
错误表明，你使用的缩进方式不一致，有的是 tab 键缩进，有的是空格缩进，改为一致即可。

IndentationError: unexpected indent
则 python 编译器是在告诉你"Hi，老兄，你的文件里格式不对了，可能是tab和空格没对齐的问题**"
```

1. 多行 用\分隔, 语句中包含 [], {} 或 () 括号就不需要使用多行连接符
2. 引号 单,双,三(可以由多行组成)
3. 注释 #,多行注释使用三个单引号(''')或三个双引号(""")。
4. 空行:习惯:函数和函数,函数和类,类和类之间空一行为了好理解
5. 输入:`raw_input`
6. 输出默认换行,要不换就在末尾加上`,`

## 变量

1. 赋值,就=,可以连着好几个

2. 标准数据类型:Numbers,String,List,Tuple,Dictionary

   - Numbers
     - 不可变,变值就要创建新的对象
     - 删除:`del var1,var2...`
     - 支持:int,long,float,complex

   - String

     - ```
        S t r i n g
        0 1 2 3 4 5
       -6 5 4 3 2 1 (都是负的)
       ```

     - **[头下标:尾下标]** 获取的子字符串包含头下标的字符，但不包含尾下标的字符。

     - ```python
       str = 'Hello World!'
        
       print str           # 输出完整字符串
       print str[0]        # 输出字符串中的第一个字符
       print str[2:5]      # 输出字符串中第三个至第六个之间的字符串
       print str[2:]       # 输出从第三个字符开始的字符串
       print str * 2       # 输出字符串两次
       print str + "TEST"  # 输出连接的字符串
       ```

   - 列表[,  ,   ,  ,]

     ```python
     #!/usr/bin/python
     # -*- coding: UTF-8 -*-
      
     list = [ 'string', 786 , 2.23, 'str', 70.2 ]
     tinylist = [123, 'john']
      
     print list               # 输出完整列表
     print list[0]            # 输出列表的第一个元素
     print list[1:3]          # 输出第二个至第三个元素 
     print list[2:]           # 输出从第三个开始至列表末尾的所有元素
     print tinylist * 2       # 输出列表两次
     print list + tinylist    # 打印组合的列表
     ```

   - 元组:(),只读类型的列表

     ```python
     tuple = ( 'runoob', 786 , 2.23, 'john', 70.2 )
     ```

   - 字典

     ```python
     dict = {}
     dict['one'] = "This is one"
     dict[2] = "This is two"
      
     tinydict = {'name': 'john','code':6734, 'dept': 'sales'}
      
      
     print dict['one']          # 输出键为'one' 的值
     print dict[2]              # 输出键为 2 的值
     print tinydict             # 输出完整的字典
     print tinydict.keys()      # 输出所有键
     print tinydict.values()    # 输出所有值
     ```

3. 类型转换

   | 函数                     | 描述                                                |
   | :----------------------- | :-------------------------------------------------- |
   | [int(x [,base\])]        | 将x转换为一个整数                                   |
   | [long(x [,base\] )]      | 将x转换为一个长整数                                 |
   | [float(x)]               | 将x转换到一个浮点数                                 |
   | [complex(real [,imag\])] | 创建一个复数                                        |
   | [str(x)]                 | 将对象 x 转换为字符串                               |
   | [repr(x)]                | 将对象 x 转换为表达式字符串                         |
   | [eval(str)]              | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
   | [tuple(s)]               | 将序列 s 转换为一个元组                             |
   | [list(s)]                | 将序列 s 转换为一个列表                             |
   | [set(s)]                 | 转换为可变集合                                      |
   | [dict(d)]                | 创建一个字典。d 必须是一个序列 (key,value)元组。    |
   | [frozenset(s)]           | 转换为不可变集合                                    |
   | [chr(x)]                 | 将一个整数转换为一个字符                            |
   | [unichr(x)]              | 将一个整数转换为Unicode字符                         |
   | [ord(x)]                 | 将一个字符转换为它的整数值                          |
   | [hex(x)]                 | 将一个整数转换为一个十六进制字符串                  |
   | [oct(x)]                 | 将一个整数转换为一个八进制字符串                    |

## 算数运算符

1. 特别的:

| **   | 幂 - 返回x的y次幂                         | a**b 为10的20次方， 输出结果 100000000000000000000 |
| ---- | ----------------------------------------- | -------------------------------------------------- |
| //   | 取整除 - 返回商的整数部分（**向下取整**） | `>>> 9//2 4 >>> -9//2 -5`                          |

2. 逻辑运算符

   Python语言支持逻辑运算符，以下假设变量 a 为 10, b为 20:

   | 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
   | :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
   | and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 | (a and b) 返回 20。     |
   | or     | x or y     | 布尔"或" - 如果 x 是非 0，它返回 x 的值，否则它返回 y 的计算值。 | (a or b) 返回 10。      |
   | not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |

3. 身份运算符 : 用于比较两个对象的存储单元

   | 运算符 | 描述                                        | 实例                                                         |
   | :----- | :------------------------------------------ | :----------------------------------------------------------- |
   | is     | is 是判断两个标识符是不是引用自一个对象     | **x is y**, 类似 **id(x) == id(y)** , 如果引用的是同一个对象则返回 True，否则返回 False |
   | is not | is not 是判断两个标识符是不是引用自不同对象 | **x is not y** ， 类似 **id(a) != id(b)**。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

   is 与 == 区别：

   is 用于判断两个变量引用对象是否为同一个(同一块内存空间)， == 用于判断引用变量的值是否相等。

## 循环运算符

 比较特别的:

1. While else

```python
while :...
else : ...
```

2. for in

```python
for letter in 'Python'
	print '当前字母:',letter
for index in range(len(fruits)):
   print '当前水果 :', fruits[index]
```

3. for … else 表示这样的意思，for 中的语句和普通的没有区别，else 中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行，while … else 也是一样。

4. pass 一般用于占位置。

   在 Python 中有时候会看到一个 def 函数:

   ```python
   def sample(n_samples):
       pass
   ```

   该处的 pass 便是占据一个位置，因为如果定义一个空函数程序会报错，当你没有想好函数的内容是可以用 pass 填充，使程序可以正常运行。

## Number

``` python
>>> import math
>>> dir(cmath)
['__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atanh', 'cos', 'cosh', 'e', 'exp', 'inf', 'infj', 'isclose', 'isfinite', 'isinf', 'isnan', 'log', 'log10', 'nan', 'nanj', 'phase', 'pi', 'polar', 'rect', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau']
```

数学函数

| 函数                                                         | 返回值 ( 描述 )                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [abs(x)](https://www.runoob.com/python/func-number-abs.html) | 返回数字的绝对值，如abs(-10) 返回 10                         |
| [ceil(x)](https://www.runoob.com/python/func-number-ceil.html) | 返回数字的上入整数，如math.ceil(4.1) 返回 5                  |
| [cmp(x, y)](https://www.runoob.com/python/func-number-cmp.html) | 如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1    |
| [exp(x)](https://www.runoob.com/python/func-number-exp.html) | 返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045         |
| [fabs(x)](https://www.runoob.com/python/func-number-fabs.html) | 返回数字的绝对值，如math.fabs(-10) 返回10.0                  |
| [floor(x)](https://www.runoob.com/python/func-number-floor.html) | 返回数字的下舍整数，如math.floor(4.9)返回 4                  |
| [log(x)](https://www.runoob.com/python/func-number-log.html) | 如math.log(math.e)返回1.0,math.log(100,10)返回2.0            |
| [log10(x)](https://www.runoob.com/python/func-number-log10.html) | 返回以10为基数的x的对数，如math.log10(100)返回 2.0           |
| [max(x1, x2,...)](https://www.runoob.com/python/func-number-max.html)!!! | 返回给定参数的最大值，参数可以为序列。                       |
| [min(x1, x2,...)](https://www.runoob.com/python/func-number-min.html)!!! | 返回给定参数的最小值，参数可以为序列。                       |
| [modf(x)](https://www.runoob.com/python/func-number-modf.html) | 返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示。 |
| [pow(x, y)](https://www.runoob.com/python/func-number-pow.html) | x**y 运算后的值。                                            |
| [round(x [,n\])](https://www.runoob.com/python/func-number-round.html) | 返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。 |
| [sqrt(x)](https://www.runoob.com/python/func-number-sqrt.html) | 返回数字x的平方根                                            |

随机数函数

| 函数                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [choice(seq)](https://www.runoob.com/python/func-number-choice.html) | 从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数。 |
| [randrange ([start,\] stop [,step])](https://www.runoob.com/python/func-number-randrange.html) | 从指定范围内，按指定基数递增的集合中获取一个随机数，基数默认值为 1 |
| [random()](https://www.runoob.com/python/func-number-random.html) | 随机生成下一个实数，它在[0,1)范围内。                        |
| [seed([x\])](https://www.runoob.com/python/func-number-seed.html) | 改变随机数生成器的种子seed。如果你不了解其原理，你不必特别去设定seed，Python会帮你选择seed。 |
| [shuffle(lst)](https://www.runoob.com/python/func-number-shuffle.html) | 将序列的所有元素随机排序                                     |
| [uniform(x, y)](https://www.runoob.com/python/func-number-uniform.html) | 随机生成下一个实数，它在[x,y]范围内。                        |

Python三角函数：

| 函数                                                         | 描述                                              |
| :----------------------------------------------------------- | :------------------------------------------------ |
| [acos(x)](https://www.runoob.com/python/func-number-acos.html) | 返回x的反余弦弧度值。                             |
| [asin(x)](https://www.runoob.com/python/func-number-asin.html) | 返回x的反正弦弧度值。                             |
| [atan(x)](https://www.runoob.com/python/func-number-atan.html) | 返回x的反正切弧度值。                             |
| [atan2(y, x)](https://www.runoob.com/python/func-number-atan2.html) | 返回给定的 X 及 Y 坐标值的反正切值。              |
| [cos(x)](https://www.runoob.com/python/func-number-cos.html) | 返回x的弧度的余弦值。                             |
| [hypot(x, y)](https://www.runoob.com/python/func-number-hypot.html) | 返回欧几里德范数 sqrt(x*x + y*y)。                |
| [sin(x)](https://www.runoob.com/python/func-number-sin.html) | 返回的x弧度的正弦值。                             |
| [tan(x)](https://www.runoob.com/python/func-number-tan.html) | 返回x弧度的正切值。                               |
| [degrees(x)](https://www.runoob.com/python/func-number-degrees.html) | 将弧度转换为角度,如degrees(math.pi/2) ， 返回90.0 |
| [radians(x)](https://www.runoob.com/python/func-number-radians.html) | 将角度转换为弧度                                  |

数学常量

| 常量 | 描述                                  |
| :--- | :------------------------------------ |
| pi   | 数学常量 pi（圆周率，一般以π来表示）  |
| e    | 数学常量 e，e即自然常数（自然常数）。 |

## 字符串

1. 不支持单字符类型

2. 字符串连接用+

3. 转义字符

4. 字符串运算符

   - *重复输出字符串

   - `"H" in a` a是字符串,如果包含返回true,还有`not in`

5. 字符串格式化

   `print "My name is %s and weight is %d kg!" % ('Zara', 21) `

   | 符  号 | 描述                                 |
   | :----- | :----------------------------------- |
   | %c     | 格式化字符及其ASCII码                |
   | %s     | 格式化字符串                         |
   | %d     | 格式化整数                           |
   | %u     | 格式化无符号整型                     |
   | %o     | 格式化无符号八进制数                 |
   | %x     | 格式化无符号十六进制数               |
   | %X     | 格式化无符号十六进制数（大写）       |
   | %f     | 格式化浮点数字，可指定小数点后的精度 |
   | %e     | 用科学计数法格式化浮点数             |
   | %E     | 作用同%e，用科学计数法格式化浮点数   |
   | %g     | %f和%e的简写                         |
   | %G     | %F 和 %E 的简写                      |
   | %p     | 用十六进制数格式化变量的地址         |

6. 三引号 用于多行

7. Unicode  创建一个unicode字符串

   ```python
   >>> u 'Hello World'
   ```

8. 常用函数

   | **方法**                                                     | **描述**                                                     |
   | :----------------------------------------------------------- | :----------------------------------------------------------- |
   | [capitalize()](https://www.runoob.com/python/att-string-capitalize.html) | 把字符串的第一个字符大写                                     |
   | [center(width)](https://www.runoob.com/python/att-string-center.html) | 返回一个原字符串居中,并使用空格填充至长度 width 的新字符串   |
   | **[string.count(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-count.html)** | 返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数 |
   | [string.decode(encoding='UTF-8', errors='strict')](https://www.runoob.com/python/att-string-decode.html) | 以 encoding 指定的编码格式解码 string，如果出错默认报一个 ValueError 的 异 常 ， 除非 errors 指 定 的 是 'ignore' 或 者'replace' |
   | [string.encode(encoding='UTF-8', errors='strict')](https://www.runoob.com/python/att-string-encode.html) | 以 encoding 指定的编码格式编码 string，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace' |
   | **[string.endswith(obj, beg=0, end=len(string))](https://www.runoob.com/python/att-string-endswith.html)** | 检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False. |
   | [string.expandtabs(tabsize=8)](https://www.runoob.com/python/att-string-expandtabs.html) | 把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8。 |
   | **[string.find(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-find.html)** | 检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1 |
   | **[string.format()](https://www.runoob.com/python/att-string-format.html)** | 格式化字符串                                                 |
   | **[string.index(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-index.html)** | 跟find()方法一样，只不过如果str不在 string中会报一个异常.    |
   | [string.isalnum()](https://www.runoob.com/python/att-string-isalnum.html) | 如果 string 至少有一个字符并且所有字符都是字母或数字则返回 True,否则返回 False |
   | [string.isalpha()](https://www.runoob.com/python/att-string-isalpha.html) | 如果 string 至少有一个字符并且所有字符都是字母则返回 True,否则返回 False |
   | [string.isdecimal()](https://www.runoob.com/python/att-string-isdecimal.html) | 如果 string 只包含十进制数字则返回 True 否则返回 False.      |
   | [string.isdigit()](https://www.runoob.com/python/att-string-isdigit.html) | 如果 string 只包含数字则返回 True 否则返回 False.            |
   | [string.islower()](https://www.runoob.com/python/att-string-islower.html) | 如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False |
   | [string.isnumeric()](https://www.runoob.com/python/att-string-isnumeric.html) | 如果 string 中只包含数字字符，则返回 True，否则返回 False    |
   | [string.isspace()](https://www.runoob.com/python/att-string-isspace.html) | 如果 string 中只包含空格，则返回 True，否则返回 False.       |
   | [string.istitle()](https://www.runoob.com/python/att-string-istitle.html) | 如果 string 是标题化的(见 title())则返回 True，否则返回 False |
   | [string.isupper()](https://www.runoob.com/python/att-string-isupper.html) | 如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False |
   | **[string.join(seq)](https://www.runoob.com/python/att-string-join.html)** | 以 string 作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 |
   | [string.ljust(width)](https://www.runoob.com/python/att-string-ljust.html) | 返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串 |
   | [string.lower()](https://www.runoob.com/python/att-string-lower.html) | 转换 string 中所有大写字符为小写.                            |
   | [string.lstrip()](https://www.runoob.com/python/att-string-lstrip.html) | 截掉 string 左边的空格                                       |
   | [string.maketrans(intab, outtab\])](https://www.runoob.com/python/att-string-maketrans.html) | maketrans() 方法用于创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。 |
   | [max(str)](https://www.runoob.com/python/att-string-max.html) | 返回字符串 *str* 中最大的字母。                              |
   | [min(str)](https://www.runoob.com/python/att-string-min.html) | 返回字符串 *str* 中最小的字母。                              |
   | **[string.partition(str)](https://www.runoob.com/python/att-string-partition.html)** | 有点像 find()和 split()的结合体,从 str 出现的第一个位置起,把 字 符 串 string 分 成 一 个 3 元 素 的 元 组 (string_pre_str,str,string_post_str),如果 string 中不包含str 则 string_pre_str == string. |
   | **[string.replace(str1, str2, num=string.count(str1))](https://www.runoob.com/python/att-string-replace.html)** | 把 string 中的 str1 替换成 str2,如果 num 指定，则替换不超过 num 次. |
   | [string.rfind(str, beg=0,end=len(string) )](https://www.runoob.com/python/att-string-rfind.html) | 类似于 find()函数，不过是从右边开始查找.                     |
   | [string.rindex( str, beg=0,end=len(string))](https://www.runoob.com/python/att-string-rindex.html) | 类似于 index()，不过是从右边开始.                            |
   | [string.rjust(width)](https://www.runoob.com/python/att-string-rjust.html) | 返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串 |
   | [string.rpartition(str)](https://www.runoob.com/python/att-string-rpartition.html) | 类似于 partition()函数,不过是从右边开始查找                  |
   | [string.rstrip()](https://www.runoob.com/python/att-string-rstrip.html) | 删除 string 字符串末尾的空格.                                |
   | **[string.split(str="", num=string.count(str))](https://www.runoob.com/python/att-string-split.html)** | 以 str 为分隔符切片 string，如果 num 有指定值，则仅分隔 num+ 个子字符串 |
   | [string.splitlines([keepends\])](https://www.runoob.com/python/att-string-splitlines.html) | 按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。 |
   | [string.startswith(obj, beg=0,end=len(string))](https://www.runoob.com/python/att-string-startswith.html) | 检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查. |
   | **[string.strip([obj\])](https://www.runoob.com/python/att-string-strip.html)** | 在 string 上执行 lstrip()和 rstrip()                         |
   | [string.swapcase()](https://www.runoob.com/python/att-string-swapcase.html) | 翻转 string 中的大小写                                       |
   | [string.title()](https://www.runoob.com/python/att-string-title.html) | 返回"标题化"的 string,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle()) |
   | **[string.translate(str, del="")](https://www.runoob.com/python/att-string-translate.html)** | 根据 str 给出的表(包含 256 个字符)转换 string 的字符,要过滤掉的字符放到 del 参数中 |
   | [string.upper()](https://www.runoob.com/python/att-string-upper.html) | 转换 string 中的小写字母为大写                               |
   | [string.zfill(width)](https://www.runoob.com/python/att-string-zfill.html) | 返回长度为 width 的字符串，原字符串 string 右对齐，前面填充0 |

   

## 列表

1. 访问列表中的值[]

2. 更新列表:`append   del `

3. 脚本操作符号:+ *

4. 函数:`cmp(list1,list2),len(list),max(list),min(list),list(seq)`

5. 方法:

   ```python
   append(obj)
   count(obj)
   entend(seq)
   index(obj)
   insert(index,obj)
   pop
   remove(obj)
   reverse()
   sort(cmp = None,key = None,reverse = False)
   ```

## 元组

类似于列表,但是不能修改

1. 访问[i]

2. 修改:不能改,只能+

3. 删除,不允许,但可以删除整个 `del tuple`

4. 运算符

   ```python
   len((1,2,3))
   (1,2,3)+(4,5,6)
   ("hi",)*4
   3 in (1,2,3)
   for x in (1,2,3): print x,
   ```

5. 索引[ ]

6. 无关闭分隔符

   ```python
   print 'abc', -4.24e93, 18+6.6j, 'xyz'
   x, y = 1, 2
   print "Value of x , y : ", x,y
   ```

7. 函数

   ```python
   cmp(tuple1, tuple2)   #比较两个元组元素。
   len(tuple) 		  		 	#计算元组元素个数。
   max(tuple)						#返回元组中元素最大值。
   min(tuple)	  				#返回元组中元素最小值。
   tuple(seq)						#将列表转换为元组。
   ```

## 字典

1. 概述: 

   - `d = {key1 : value1, key2 : value2 }`
   - 键是唯一的,如果重复最后的一个键值对会替换前面的，值不需要唯一。
   - 键必须是不可变的,字符串/数字/元组

2. 访问`dict[key]`,如果没有这个key,就会有KeyError

3.  删除

   ```python
   del dict['key']   # 删除键是'key'的条目
   dict.clear()      # 清空字典所有条目
   del dict          # 删除字典
   ```

4. 函数

   ```python
   cmp(dict1, dict2)  #比较两个字典元素。
   len(dict)				   #计算字典元素个数，即键的总数。
   str(dict)					 #输出字典可打印的字符串表示。
   type(variable)		 #返回输入的变量类型，如果变量是字典就返回字典类型。
   ```

5. 方法

   ```python
   dict.clear()								#删除字典内所有元素
   dict.copy()									#返回一个字典的浅复制
   dict.fromkeys(seq[, val])		#创建一个新字典，以序列 seq 中元素做字典的键，val为默认初始值
   dict.get(key, default=None) #返回指定键的值，如果值不在字典中返回default值
   dict.has_key(key)						#如果键在字典dict里返回true，否则返回false
   dict.items()								#以列表返回可遍历的(键, 值) 元组数组
   dict.keys()									#以列表返回一个字典所有的键
   dict.update(dict2)					#把字典dict2的键值对更新到dict里
   dict.values()								#以列表返回字典中的所有值
   pop(key[,default])					#删除字典给定键 key 所对应的值，返回值为被删除的值。
   popitem()										#返回并删除字典中的最后一对键和值。
   dict.setdefault(key, default=None)  #和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default
   ```

## [日期和时间](https://www.runoob.com/python/python-date-time.html)

## 函数

1. 定义一个函数

   ```python
   def functionname( parameters ):
      "函数_文档字符串"
      function_suite
      return [expression]
   ```

   ```python
   def printme( str ):
      "打印传入的字符串到标准显示设备上"
      print str
      return
   ```

2. 调用的时候直接用就可以

3. 对象:

   **类型属于对象,变量是没有类型的**

   如下,**"Runoob"** 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是 List 类型对象，也可以指向 String 类型对象。

   ```python
   a=[1,2,3]
   a="Runoob"
   ```

   可更改:list,dict,不可更改:strings, tuples, 和 numbers

   可变类型:类似引用传递,会变;

   不可变类型,类似值传递,不变

4. 参数:

   - 必备参数

   - 关键字参数:允许顺序不一致,能够用参数名匹配参数值

     ```python
     def printinfo( name, age ):
        "打印任何传入的字符串"
        print "Name: ", name
        print "Age ", age
        return
     #调用printinfo函数
     printinfo( age=50, name="miki" )
     ```

   - 默认参数:在定义函数的时候就说明

     ```python
     def printinfo( name, age = 35 ):
        "打印任何传入的字符串"
        print "Name: ", name
        print "Age ", age
        return
      
     #调用printinfo函数
     printinfo( age=50, name="miki" )
     printinfo( name="miki" )
     ```

   - 不定长参数:

     ```python
     def printinfo( arg1, *vartuple ): #用这个*就可以
        "打印任何传入的参数"
        print "输出: "
        print arg1
        for var in vartuple:
           print var
        return
      
     # 调用printinfo 函数
     printinfo( 10 )
     printinfo( 70, 60, 50 )
     ```

5. 匿名函数:lambda表达式

   python 使用 lambda 来创建匿名函数。

   - lambda只是一个表达式，函数体比def简单很多。
   - lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
   - lambda函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
   - 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。

   lambda函数的语法只包含一个语句，如下：

   ```python
   lambda [arg1 [,arg2,.....argn]]:expression
   ```

   如下实例：

   ```python
   sum = lambda arg1, arg2: arg1 + arg2  
   
   # 调用sum函数 
   print "相加后的值为 : ", sum( 10, 20 ) 
   print "相加后的值为 : ", sum( 20, 20 )
   ```

   以上实例输出结果：

   ```
   相加后的值为 :  30
   相加后的值为 :  40
   ```

6. return 语句 跟别的语言一样
7. 全局变量和局部变量 跟别的语言一样

## 模块

```python
import filename   #不写.py
```

```python
from fib import fibonacci  #从fib.py中导入 fibonacci 函数
```

```python
from math import * 				#全部导入
```

1. **搜索路径**

当前目录 -> shell 变量 PYTHONPATH下的每个目录 -> 默认路径。UNIX下，默认路径一般为/usr/local/lib/python/。

2. **PYTHONPATH:**

在 UNIX 系统，典型的 PYTHONPATH 如下：

```python
set PYTHONPATH=/usr/local/lib/python
```

如果要给函数内的全局变量赋值，必须使用 global 语句

```python
#Python 会假定 Money 是一个局部变量。然而，我们并没有在访问前声明一个局部变量 Money，结果就是会出现一个 UnboundLocalError 的错误。
Money = 2000
def AddMoney():
   # 想改正代码就取消以下注释:
   # global Money
   Money = Money + 1
 
print Money
AddMoney()
print Money
```

3. dir函数:一个排好序的字符串列表，内容是一个模块里定义过的名字。

   ```python
   import math
   content = dir(math)
   print content;
   
   #输出
   ['__doc__', '__file__', '__name__', 'acos', 'asin', 'atan', 
   'atan2', 'ceil', 'cos', 'cosh', 'degrees', 'e', 'exp', 
   'fabs', 'floor', 'fmod', 'frexp', 'hypot', 'ldexp', 'log',
   'log10', 'modf', 'pi', 'pow', 'radians', 'sin', 'sinh', 
   'sqrt', 'tan', 'tanh']
   
   ```

4. globals() 和 locals() 函数可被用来返回全局和局部命名空间里的名字。

   如果在函数内部调用 locals()，返回的是所有能在该函数里访问的命名。

   如果在函数内部调用 globals()，返回的是所有在该函数里能访问的全局名字。

   两个函数的返回类型都是字典。所以名字们能用 keys() 函数摘取。

5. reload

6. 包,就是有`__init__.py`的文件夹

## 文件IO

1. 打印到屏幕:print

2. 读取输入:raw_input&input,input可以接受Python表达式

3. 打开文件:`open(file_name [, access_mode][, buffering])`

   File对象的属性:closed,mode,name,softspace

4. 关闭文件:close()

5. 写入文件:`fileObject.write(string)`

6. 读取文件:`fileObject.read([count])`

7. 文件定位:`fileObject.tell`

8. 目录: 

   ```python
   os.mkdir("test")
   os.chdir("/home/newdir")
   os.getcwd()
   os.rmdir('dirname')
   ```

## [File方法](https://www.runoob.com/python/file-methods.html)

## 异常处理

```python
try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print "Error: 没有找到文件或读取文件失败"
else:
    print "内容写入文件成功"
    fh.close()
    
 #######################
try:
    #正常的操作
   	#......................
except  xxError:
    #发生异常，执行这块代码
    #......................
else:
    #如果没有异常执行这块代码
```

```python
try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
finally:
    print "Error: 没有找到文件或读取文件失败"
#######################
try:
	<语句>
finally:
	<语句>    #退出try时总会执行
raise
```

触发异常:

```python
raise [Exception [, args [, traceback]]]
#######################
def functionName( level ):
    if level < 1:
        raise Exception("Invalid level!", level)
        # 触发异常后，后面的代码就不会再执行
```

自定义异常:

```python
class Networkerror(RuntimeError):
    def __init__(self, arg):
        self.args = arg
```



## [os文件/目录方法](https://www.runoob.com/python/os-file-methods.html)

## [内置函数](https://www.runoob.com/python/python-built-in-functions.html)

# 高级

## 面向对象

1. 创建类:

   ```python
   class ClassName:
     '类的帮助信息' #类文档字符串
     class_suite
   ```

2. 构造方法:` __init__()` 是一种特殊的方法

3. Self: 类的实例,定义类的方法的时候是必须要有的,代表当前对象的地址,`self.__class__`指向类

​		 它不是python关键字,换成啥都可以运行

4. 创建直接调用构造函数就可以

5. 访问属性用`.`

6. 内置类属性:

   - __dict__ : 类的属性（包含一个字典，由类的数据属性组成）
   - __doc__ :类的文档字符串
   - __name__: 类名
   - __module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod）
   - __bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）

7. 垃圾回收:引用计数

8. 继承:`class 派生类名(基类名)`

   - 在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 self 参数

   - 先在本类中查找调用的方法，找不到才去基类中找）

   - 可以继承多个类

     ```python
     class A:        # 定义类 A
     .....
     
     class B:         # 定义类 B
     .....
     
     class C(A, B):   # 继承类 A 和 B
     .....
     ```

9. 私有属性`__attribute` & 私有方法`__method`
10. - `__foo__` 特殊方法,系统定义的名字
    - `_foo`  是protected类型的变量
    - `_foo` private类型的变量



## [正则表达式](https://www.runoob.com/python/python-reg-expressions.html)

## CGI编程

### 定义

通用网管接口,运行在服务器上如：HTTP 服务器，提供同客户端 HTML 页面的接口。

1. 使用你的浏览器访问 URL 并连接到 HTTP web 服务器。

2. Web 服务器接收到请求信息后会解析 URL，并查找访问的文件在服务器上是否存在，如果存在返回文件的内容，否则返回错误信息。

3. 浏览器从服务器上接收信息，并显示接收的文件或者错误信息

![image-20191222205344326](/Users/dylan/Library/Application Support/typora-user-images/image-20191222205344326.png)

web服务器及配置











