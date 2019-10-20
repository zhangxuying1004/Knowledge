# Knowledge
## 1、卷积神经网络的可视化  
[https://cbovar.github.io/ConvNetDraw/]  
## 2、image caption使用的COCO数据集中的标注
**（1）两个文件**  
训练集对应的标注：captions_train201x.json   
测试集对应的标注：captions_val201x.json  

**（2）文件内容的格式** 
```python
{
    "info": info,
    "licenses": [license],
    "images": [image],
    "annotations": [annotation]
}
其中：
image{
    "id": int,
    "width": int,
    "height": int,
    "file_name": str,
    "license": int,
    "flickr_url": str,
    "coco_url": str,
    "date_captured": datetime,
}

annotation{
    "id": int,
    "image_id": int,
    "caption": str
}
```
注：在COCO数据集中，每个图片至少有5个描述语句（部分图片可能更多）。     
## 3、image caption中常用的评价指标介绍  
**（1）BLEU得分**    
通常需要计算BLEU-1，BLEU-2，BLEU-3，BLEU-4，后面的数字k表示k元组。  

**（2）METEOR得分**  

**（3）ROUGE-L得分**   

**（4）CIDEr得分**   

**（5）SPICE得分**   

## 4、nltk的使用
（1）使用nltk进行分词
```python
from nltk import word_tokenize   
string = "All work and no play makes jack a dull boy, all work and no play"
print(word_tokenize(string))
运行结果：
['All', 'work', 'and', 'no', 'play', 'makes', 'jack', 'a', 'dull', 'boy', ',', 'all', 'work', 'and', 'no', 'play']
```
（2）使用nltk分句  
```python 
from nltk.tokenize import sent_tokenize
data = "All work and no play makes jack dull boy. All work and no play makes jack a dull boy."
print(sent_tokenize(data))
运行结果：
['All work and no play makes jack dull boy.', 'All work and no play makes jack a dull boy.']
```
注：NLTK 同样不支持对中文的分词和分句，具体支持哪些语言的分句，可以参考文件夹nltk_data --> tokenizers --> punkt。  
（3）使用nltk计算BLEU得分
参考[https://coladrill.github.io/2018/10/20/%E6%B5%85%E8%B0%88BLEU%E8%AF%84%E5%88%86/]
1）语句BLEU分数
NLTK提供了sentence_bleu()函数，用于根据一个或多个参考语句来评估候选语句。   
参考语句必须作为语句列表来提供，其中每个语句是一个记号列表。候选语句作为一个记号列表被提供。   
```python
from nltk.translate.bleu_score import sentence_bleu
reference = [['this', 'is', 'a', 'test'], ['this', 'is' 'test']]
candidate = ['this', 'is', 'a', 'test']
score = sentence_bleu(reference, candidate)
print(score)
```
2）语料库BLEU得分
NLTK提供了一个称为corpus_bleu()的函数来计算多个句子（如段落或文档）的BLEU分数。  
参考文本必须被指定为文档列表，其中每个文档是一个参考语句列表，并且每个可替换的参考语句也是记号列表，也就是说文档列表是记号列表的列表的列表。  
候选文档必须被指定为列表，其中每个文件是一个记号列表，也就是说候选文档是记号列表的列表。
```python
from nltk.translate.bleu_score import corpus_bleu
references = [[['this', 'is', 'a', 'test'], ['this', 'is' 'test']]]
candidates = [['this', 'is', 'a', 'test']]
score = corpus_bleu(references, candidates)
print(score)
```
3）单独的N-Gram分数  
单独的N-gram分数是对特定顺序的匹配n元组的评分，例如单个单词（称为1-gram）或单词对（称为2-gram或bigram）。  
权重被指定为一个数组，其中每个索引对应相应次序的n元组。
仅要计算1-gram匹配的BLEU分数，指定参数weights为（1,0,0,0）。  
仅要计算2-gram匹配的BLEU分数，指定参数weights为（0,1,0,0）。  
仅要计算3-gram匹配的BLEU分数，指定参数weights为（0,0,1,0）。  
仅要计算4-gram匹配的BLEU分数，指定参数weights为（0,0,0,1）。  
```python
from nltk.translate.bleu_score import sentence_bleu
reference = [['this', 'is', 'small', 'test']]
candidate = ['this', 'is', 'a', 'test']
score = sentence_bleu(reference, candidate, weights=(1, 0, 0, 0))
print(score)
```
4）累加的N-Gram分数  
累加分数是指对从1到n的所有单独n-gram分数的计算，通过计算加权几何平均值来对它们进行加权计算。  
默认情况下，sentence_bleu（）和corpus_bleu（）分数计算累加的4元组BLEU分数，也称为BLEU-4分数。  
BLEU-4对1元组，2元组，3元组和4元组分数的权重为1/4（25％）或0.25。 
```python
from nltk.translate.bleu_score import sentence_bleu

reference = [['this', 'is', 'small', 'test']]
candidate = ['this', 'is', 'a', 'test']

score = sentence_bleu(reference, candidate, weights=(0.25, 0.25, 0.25, 0.25))
print(score) 
```
## 5 数据集的保存和获取  
```python
import pandas as pd
# 保存，image_ids，image_files和captions都是list
saved_dataset = pd.DataFrame({
        'image_id': image_ids,
        'image_file': image_files,
        'caption': captions
})
saved_dataset.to_csv(saved_file)
# 读取
saved_dataset = pd.read_csv(config.temp_annotation_file)
captions = saved_dataset['caption'].values
image_ids = saved_dataset['image_id'].values
image_files = saved_dataset['image_file'].values
```
## 6 jupyter notebook常用的快捷键
在编辑模式下：  
Tab：代码补全或缩进  
Ctrl+Enter：执行当前的单元格   
Shift+Enter：执行当前的单元格并进入下一个单元格 
Alt-Enter : 运行本单元，在下面插入一单元  
Ctrl-Home : 跳到单元开头  
Ctrl-Up : 跳到单元开头  
Ctrl-End : 跳到单元末尾  
Ctrl-Down : 跳到单元末尾  
Ctrl-Left : 跳到左边一个字首  
Ctrl-Right : 跳到右边一个字首  
## 7 matplotlib.pylot  
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
## 8 数据存取遇到的坑
保存三个数据，img_files, captions和sililarity_values，其中img_files,sililarity_values为一维列表，captions是由字符串组成的二维列表   
最初使用pandas进行存取  
```python
# 保存
import pandas as pd
saved_dataset = pd.DataFrame({
    'image_file'=img_files, 
    'caption'=captions, 
    'score;=similarity_values
})
saved_dataset.to_csv('filename.csv')
```
遇到的问题是，以captions为例进行说明。  
captions原本是一个由n个子列表构成的列表，每个子列表由多个个字符串语句构成。  
pandas进行保存时，每一列只能存储一维信息，故pandas实际上先将每个子列表中的多个字符串语句打平成一个字符串，然后将这个二维列表转化为一个一维数组，存储到'caption'列下。这样显然损失了数据的维度信息。
```python
# 读取
saved_dataset = pd.read_csv('filename.csv')
captions = saved_dataset['caption']
```
此时，读取的captions的数据类型是ndarray，维度为(n,)，每个元素是由原子列表打平而成的字符串，   
但是，如果print(captions[i])，发现打印出的内容与一个列表无异，但是print(captions[i])或者print(len(captions[i]))，会发现captions[i]实际上是一个字符串！这是一个很难发觉的BUG！    
解决方案：  
首先，将每张图片的caption数目截断成5个，即每个子列表中都有5个字符串语句，然后用np.savez()将数据保存成.npz文件
```python
# 保存
np.savez(filename.npz, image_file=img_files, caption=captions, score=similarity_values)
# 读取
saved_dataset = np.load(data_file)
img_files = saved_dataset['image_file'].tolist()
captions = saved_dataset['caption'].tolist()
similarity_values = saved_dataset['score'].tolist()
```
这样获得的是列表，而且不会损失维度信息。
## 9 jupyter远程连接服务器
参考链接[https://blog.csdn.net/luo3300612/article/details/90344634]   
（1）服务器端设置  
 第一步：安装jupyter notebook   
 ```
 pip install jupyter notebook
 ```
 第二步：设置密码  
 ```
 命令：python 
 from notebook.auth import passwd
 passwd()
 ```
 输入并确认密码，赋值并保存输出的sha1:...  
 第三步：生成jupyter配置文件  
 ```
 jupyter notebook --generate-config
 ```
 第四步：修改配置文件  
 ```
 c.NotebookApp.ip = '*'
 c.NotebookApp.password = u'刚才保存的sha1:'
 c.NotebookApp.port = 8000 # 随意
 # c.NotebookApp.notebook_dir = "" # 修改jupyter启动目录，如有需要，则改成自己想要的目录，否则就不用改
 c.NotebookApp.open_browser = False
 ```
（2）Windows设置   
 第一步：下载安装XShell  
 第二步：再XShell的SSH/隧道选项中添加：   
    类型：本地（拨出）  
    源主机：localhost  
    帧听端口：8000（随意，只要是不被占用的本地端口）  
    目标主机：目标主机的内网ip，可以通过在服务器输入ifconfig看到，如果这个命令没安装，则使用sudo apt-get install net-tools安装  
    目标端口：刚刚填写的c.NotebookApp.port  
    
（3）在服务器上开一个后台，输入jupyter-notebook --allow-root，然后在Google浏览器上输入localhost：之前输入的侦听端口即可。  
