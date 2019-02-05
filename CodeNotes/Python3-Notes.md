---
title: Python3学习碎片
date: 2018-08-18 12:00:00
tags: [Python]
category: [编程]
---

<pre><code>《Programing in Python3》
# Question:
# Solution:
# Answer:
</code></pre>

---
### sys.argv[]

sys.argv[] 从外部获取参数，得到一个列表  
sys.argv[0] 代码本身文件路径  
sys.argv[1] 命令行中输入的第一个参数  
sys.argv[2] 命令行中输入的第二个参数  
以此类推...  
sys.argv[2:] 可以用分片操作来得到所有输入的参数
 <!-- more -->
---
### zip()

用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。  
我们可以使用 list() 转换来输出列表。  
如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同。  
利用 * 号操作符，可以将元组解压为列表。

```
>>> a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)     # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)  # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]

>>> a1, a2 = zip(*zip(a,b))          # 与 zip 相反，*zip 可理解为解压，返回二维矩阵式

>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
>>>
```

> Reference：http://www.runoob.com/python3/python3-func-zip.html

---
### sort 与 sorted 区别：

sort 是应用在 list 上的方法，  
sorted 可以对所有可迭代的对象进行排序操作。

list 的 sort 方法返回的是对已经存在的列表进行操作，
而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

> Reference：http://www.runoob.com/python/python3-func-sorted.html

---
### sys.stdout和sys.stderr 

- 定义：
标准输出和标准错误（通常缩写为 stdout 和 stderr）是建立在每个UNIX系统内的管道(pipe)。  
当你 print 某东西时，结果输出到 stdout 管道中；当你的程序崩溃并打印出调试信息时（像Python中的错误跟踪），结果输出到 stderr 管道中。  
通常这两个管道只与你正在工作的终端窗口相联，所以当一个程序打印输出时，你可以看到输出，并且当一个程序崩溃时，你可以看到调试信息。（如果你在一个基于窗口的Python IDE系统上工作，stdout 和 stderr 缺省为“交互窗口”。） 

- 使用：
stdout 和 stderr 都是类文件对象，就象我们在提取输入源中所讨论的一样，但它们都是只写的。它们没有 read 方法，只有 write。  
然而，它们的确是类文件对象，并且你可以将任意文件对象或类文件对象赋给它们来重定向输出。

- 缓冲：
sys.stdout是有缓冲区的，解决缓冲有两种方式：  
    - print() 或者sys.stdout.write()后加sys.stdout.flush()
    - 执行python脚本时增加-u 参数，即 python -u XXX.py

### sys.stdout与print
在python中调用print时，事实上调用了sys.stdout.write(obj+'\n')  
print 将需要的内容打印到控制台，然后追加一个换行符，以下两行代码等价：
```
sys.stdout.write('hello' + '\n')
print('hello')
```

### sys.stdin与input

sys.stdin.readline()会将标准输入全部获取，
包括末尾的'\n'，
因此用len计算长度时是把换行符'\n'算进去了的，
但是input()获取输入时返回的结果是不包含末尾的换行符'\n'的。

因此如果在平时使用sys.stdin.readline()获取输入的话，
不要忘了去掉末尾的换行符，
可以用strip()函数，有以下两种方法去掉换行。  
<pre><code>sys.stdin.readline().strip('\n')
sys.stdin.readline( )[:-1]</code></pre>

> Reference：https://www.cnblogs.com/keye/p/7859181.html
> Reference：https://www.cnblogs.com/silvi/p/7260506.html

---

### optparse.OptionParser
```
from optparse import OptionParser
parser = OptionParser(usage=usage) #传入值在被调用时打印
parser.add_option(opt_str, ..., attr=value, ...)
parser.set_defaults(dest=..., ...)
options, args = parser.parse_args()
```

- add_option中最重要的四个option的属性是：
action,type,dest(destination),help。
    - action:存储方式，分为三种store、store_false、store_true
        - store: 如果输入参数（如'-v'）存在，则变量被赋值为输入参数后的内容；如果输入参数不存在，则变量为None
        - store_true: 如果输入参数存在（如'-v'），则变量被赋值为True
        - store_false: 如果输入参数存在（如'-v'），则变量被赋值为False
        - 注：如果action="store_true"时，变量的值只与是否'-v'有关。即为True或False，不会受输入参数后内容的影响。
    - dest:存储的变量
    - type:指定变量类型（int/float/string）
    - default:默认值
    - help:帮助信息
    
- set_defaults()为变量设置默认值  
- parse_args()返回两个值，一个是选项options（如：-f）,另一个是参数args,即除选项options以外的值（如：test.txt）

---
### datetime
<pre><code>import datetime, os
t = os.path.getmtime(filename) # 返回文件最后修改的时间戳 1535512877.73176340
tt = datetime.datetime.fromtimestamp(t) # 返回文件最后修改的时间 2018-08-29 11:21:17.731763
ttt = tt.strftime('%Y-%m-%d %H:%M:%S') # 格式化时间 2018-08-29 11:21:17
ttt = strftime('%Y-%m-%d %H:%M:%S', tt) #另一种形式</code></pre>

---
### enumerate()
