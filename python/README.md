# python knowledge
## 1 常用函数 
```
1.1 input函数   
<变量> = input(<提示性文字>)
返回一个字符串。
1.2 eval函数
eval(<字符串>)
以python表达式的方式解析并执行字符串，并结果输出
1.3 abs(x)
返回x的绝对值
1.4 divmod(x, y)
输入二元组形式(x//y, x%y)
1.5 round(x[, ndigits])
对x四舍五入，保留ndigits位小数
1.6 complex(re[, im])
生成一个复数，re表示实数部分，im表示虚数部分
1.7 len(x)
返回字符串x的长度，也可以返回其他组合数据类型元素的个数
1.8 strip()
返回的是字符串的副本，并删除前导和后缀字符，例如：b = a.strip()
1.9 split()
str.split(str="", num=string.count(str))
str -- 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等。
num -- 分割次数。默认为 -1, 即分隔所有。
1.10 vars([object])
返回对象object的属性和属性值的字典对象
```
## 2 numpy模块   
```
import numpy as np
2.1 np.arange(n)
返回0~(n-1)数字组成的数组。
2.2 np.where()
返回输入数组中满足给定条件的元素的索引；数组可以使用索引来提取满足条件的元素。
2.3 np.extract()
根据某个条件从数组中抽取元素，返回满足条件的元素。
2.4 np.argmax(), np.argmin()
分别沿指定轴返回最大元素，最小元素的索引。
其中，axis = 0，表示按列索引，axis = 1，表示按行索引。
2.5 np.argsort(a, axis=-1, kind=’quicksort’, order=None)
返回矩阵a按照axis指定的维度进行排序后的下标（注：a不发生改变）
2.6 np.concatenate((a1, a2, …), axis=0)
用于沿指定轴连接相同形状的两个或多个数组，未添加新维度。
2.7 np.stack(arrays, axis)
用于沿指定轴堆叠数组序列，相当于添加一个维度。
2.8 np.split(array, indices_or_sections, axis)
沿指定轴将数组分割为子数组，等分或者在指定位置处分割。
```
## 3 文件路径操作
3.1 glob模块  
返回指定目录下指定格式的所有文件路径组成的列表  
```python
from glob import glob
files_path = glob(os.path.join(dir, '*.npz')
```
3.2 os.path.basename  
返回指定文件路径中的文件名  
```python
import os
files_name = os.path.basename(file_path)
```
3.3 os.listdir  
返回指定目录下的所有文件的文件名组成的列表  
```python
import os
files_name = os.listdir(dir)
```
3.4 正则表达式  
返回文件名中前缀（即文件名中除去文件类型的部分）  
```python
import re
file_name_prefix = re.findall(r'(.+?)\.', basename)[0]
```
# 4 with语句
with语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源。比如文件使用后自动关闭，
线程中锁的自动获取和释放等。   
下面以文件操作为例进行说明：      
```python
try:
    f = open('xxx')
except:
    print('fail to open')
    exit(-1)
try:
    do something
except:
    do something
finally:
f.close()
```
上面的代码与下面的代码等价：   
```python
with open('xxx') as file:
    data = file.read()
do something
```
显然with语句能够减少冗长，还可以自动处理上下文环境产生的异常。 
## 5 数据的存取
5.1 .txt文件    
```
（1）存储  
 with open(filename.txt, 'w') as f:
    print(data, file=f)
 (2) 读取  
1）方式1：
data = np.loadtxt(filename.txt)
2）方式2：
with open('odom.txt', 'r') as f:
    lines = f.readlines()  # txt中所有字符串读入data
    for line in lines:
        temp = line.split()  # 将单个数据分隔开存好
        data = list(map(float, temp))  # 转化为浮点数 
```
5.2 .npz文件  
```
（1）常用属性
1）files
列出对.npz文件中，对各类数据的索引。
2）f
可以作为一种可选的方式执行Npzfile实例的索引。对象.f.索引<==>对象[‘索引’]
（2）存储
np.savez(filename, attr_name1=data1， attr_name2=data2...) 
（3）读取
data = np.load(filename)
data1 = data['attr_name1']
data2 = data['attr_name2']
```
5.3 .json文件  
```
# json的引入
import json
（1）存储
with oepn('filename.json', 'w'):
    json.dump(data, f)
（2）读取
with open('filename.json', 'r'):
    data = json.load(f)
```
注：json文件只能存取字典，列表等序列化数据，但是不能存取矩阵类型的数据。

## 6 字符串类型的格式化   
```
<模板字符串>.format(<逗号分隔的参数>)   
其中，模板字符串由一系列槽组成，用来控制修改字符串中嵌入值出现的位置。   
槽用大括号{}表示，槽的内部样式是{<参数序号>: <格式控制标记>}。格式控制标记包括填充、对齐、，、精度和类型6个字段，
用来控制参数显示时的格式。   
其中，精度表示两个含义，由小数点开头，对于浮点数，精度表示小数部分输出的有效位数，对于字符串，精度表示输出的最大长度。
类型表示输出整数和浮点数类型的格式规则，对于整数类型，o输出整数的八进制方式，x输出整数的小写十六进制方式，X输出整数的
大写十六进制方式；对于 浮点数，e/E输出科学计数法的形式，f输出浮点数的标准浮点形式，%输出浮点数的百分形式。
```
## 7 lambda函数（匿名函数）
```
<函数名> = lambda<参数列表> : <表达式>
Lambda函数与正常的函数一样，等价于下面形式：   
def <函数名>(<参数列表>):
	return <表达式>
如下例所示：   
>>> f = lambda x, y : x + y
>>> type(f)
<class ‘function’>
>>> f(10, 12)
22
```
## 8 随机数的生成
```python
import random
t = random.randint(a, b)  # 生成a~b之间的随机整数
t = random.sample(range(start, end), k)	# 生成start~end之间不重复的k个随机数
```
## 9 自定义包的引入
python默认在（当前.py文件所在的目录下）和（anaconda3/lib/下的几个文件夹下）搜索要引入的包，可以通过  
```python
import sys
print(sys.path)
```
来查看。  
如果要引入新的自定义的包，可以通过代码sys.path.append(path)将自定义包所在的路径加入到sys.path中，这样便能直接import了。 
## 10 星号的使用
10.1 单星号（*）：*args，有两个用法。  
（1）用法一：将参数以元组的形式导入。例如：   
```python
>>>def foo(param1, *param2):
	print(param1)
	print(param2)
>>>foo(1, 2, 3, 4, 5)
1
(2, 3, 4, 5)
```
（2） 用法二：元组/列表前加星号，解压参数列表。例如：   
```python
>>>def foo(bar, lee):
	print(bar, lee)
>>>l = [1, 2]
>>>foo(*l)
1 2
```
10.2 双星号（**）：**args   
将参数以字典的形式导入。例如：   
```python
>>>def foo(param1, **param2):
	print(param1)
	print(param2)
>>>foo(1, a=2, b=3)
1
{'a': 2, 'b': 3}
```
## 11 heapq模块(最大堆)   
```python
import heapq
data = []
# 将item推入到data里
heapq.heappush(data, item)
# 将最小元素从data中取出
heapq.heappop(data)
# 将item插入到data中,同时弹出data中的最小值，即取出n+1个元素中最小的那个元素
heapq.heappushpop(data, item)
```  
## 12 字符串转化成常量
今天，在处理SALICON数据集时，我发现导出fixation是'list'的形式，一时不知道该怎么处理。   
查阅资料了解到，使用eval()函数，即fixation=eval(fixation)即可得到list类型的数据。   
  
## 13 codecs：自然语言编码转换
```python
import codecs
# 用codecs提供的open方法来指定打开的文件的语言编码，它会在读取的时候自动转换为内部unicode
with codecs.open(file, 'r', encode_type)
```
## 14 字符串前加r
r是防止字符转义的 如果路径中出现'\t'的话 不加r的话\t就会被转义 而加了'r'之后'\t'就能保留原有的样子。  
在字符串赋值的时候 前面加r可以防止字符串在时候的时候不被转义，原理是在转义字符前加'\'  
例子：  
```
>>> s0 = r'\tt'  
>>> print(s0)  
\tt  
>>> s1 = '\tt'  
>>> print(s1)  
       t  
```
## 15 字典数据排序  
```python
d = {'lilee':25, 'wangyan':21, 'liqun':32, 'lidaming':19}
print(d)
# 按键排序
d1 = dict(sorted(d.items(), key=lambda item: item[0]))
print(d1)
# 按值排序
d2 = dict(sorted(d.items(), key=lambda item: item[1]))
print(d2)
```
## 16 matplotlib.pylot  
（1）显示图片和文本  
```python 
import matplotlib.pylot as plt
from PIL import Image  
image = Image.open('file_path')
captions = ['caption1', 'caption2', 'caption3', 'caption4', 'caption5']
plt.imshow(image)
# 先设置显示坐标轴，大致确定显示文本的位置，然后再关闭坐标轴
plt.axis('off')    # plt.axis('on')
for i in range(len(captions)):
    plt.text(x_init, y_init + i*space, captions[i])
plt.show()
```
(2) 绘制散点图  
```python 
import matplotlib.pylot as plt
import numpy as np
N  = 100
x = np.random.rand(N)
y = np.random.rand(N)

x1 = np.random.rand(N)
y1 = np.random.rand(N)

plt.scatter(x, y, c='green', alpha=0.6)     # 透明度设置为0.6（这样颜色浅一点，比较好看）
plt.scatter(x1, y1, c='blue', alpha=0.6)
plt.show()
```
## 17 修改字典中的键名  
```python
# 将字典中的键名 a 改成 c
dict={'a':1, 'b':2}
dict["c"] = dict.pop("a")
```
## 18 使用python读取matlab生成的.mat类型的数据
（1）普通版本的.mat文件，使用sicpy.io读取  
```python
import scipy.io
data = scipy.io.loadmat('filename.mat') 	# data是一个字典类型的数据
print(data.keys())	# 查看有哪些key，这些key是保存.mat数据时，这个文件包含的matlab变量名
# 接下来，用访问字典的方式根据需要提取需要的数据
```
（2）v7.3版本.mat文件（较大的文件），使用h5py读取  
```python
import h5py
f = h5py.File('filename.mat')
data = f['key']	# key是保存文件时里面的变量名
data1 = [item[0] for item in data]	# data1中的数据都是HDF5 object reference，用于在f中索引
data2 = [f[item][:] for item in data1]	# data2是所需要的array类型的数据
```




```
