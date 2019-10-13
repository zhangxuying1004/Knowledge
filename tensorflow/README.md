# Tensorflow knowledge   
## 1、global_step分析      
引用自博客[https://blog.csdn.net/leviopku/article/details/78508951]     
global_step在滑动平均、优化器、指数衰减学习率等方面都有用到，这个变量的实际意义非常好理解：代表全局步数，比如在多少步该进行什么操作，现在神经网络训练到多少轮等等，类似于一个钟表。   
![image](https://github.com/zhangxuying1004/Knowledge/blob/master/tensorflow/image_folder/1.PNG)
输出：
```
0.107177  
1  
0.11487  
2  
0.123114  
3  
0.131951  
4  
```
显然，global_step能够在训练的过程中自动加一。  
但是，如果把train_step = tf.train.GradientDescentOptimizer(learning_rate).minimize (loss, global_step =global_steps)后面部分的global_step=global_steps去掉，global_step的自动加一就会失效，输出如下：  
```
0.1  
0  
0.1  
0  
0.1  
0  
0.1  
0  
```
因为指数衰减的学习率是伴随global_step的变化而衰减的，所以当global_step不改变时，学习率也变成一个定值。   
综上所述：损失函数优化器的minimize()中global_step = global_steps能够提供global_step自动加一的操作。  
## 2、mnist数据集操作   
70k = 55k + 5K+ 10K, 28x28  
```python
# 读取mnist数据集，MNIST_dir为数据集保存的位置  
from tensorflow.examples.tutorials.minist import input_data
mnist = input_data.read_data_sets(MNIST_dir,one_hot=True)

# 获取全部训练数据
mnist.train.images, mnist.train.labels
# 获取全部验证数据
mnist.validation.images, mnist.validation.labels 
# 获取全部测试数据
mnist.test.images, mnist.test.labels

# 每次返回一个batch的训练数据（images+labels）
mnist.train.next(batch_size)
# 每次返回一个batch的验证数据（images+labels）
mnist.validation.next(batch_size)
每次返回一个batch的测试数据（images+labels）
mnist.test.next(batch_size)
```
## 3、tf.variable_scope函数
引用《Tensorflow实战Google深度学习框架》P108-P110   
tf.variable_scope函数通常与tf.get_variable函数配套使用，因此，先对tf.get_variable函数进行说明。
tf.get_variable函数与tf.Variable函数一样，均是创建一个新变量的函数，如下代码所示：  
```python
v = tf.get_variable('v', shape=[l], initializer=tf.constant_initializer(l.0))
v = tf.Variable(tf.constant(l.0, shape=[l]), name ='v')
```
这两个定义是等价的。   
tf.get_variable函数与tf.Variable函数最大的区别在于指定变量名称的参数。       
对于tf.Variabble函数，变量名称是一个可选的参数，通过name='v'的形式给出，      
但是对于tf.get_variable函数，变量名称是一个必填的参数。tf.get_ariable会根据这个名字去创建或者获取变量。  
在上面的例子中，tf.get_variable首先会试图去创建一个名字为v的参数，如果创建失败（比如已经有同名的参数），那么这个程序就会报错，这是为了避免无意识的变量复用造成的错误。比如在定义神经网络参数时，第一层网络的权重已经叫weights了，那么在创建第二层神经网络时，如果参数名仍叫weights，就会触发变量重用的错误，否则两层神经网络共用一个权重会出现一些难以发现的错误。   
如果需要通过tf.get_variable获取一个已经创建的变量，需要通过一个tf.variable_scope函数来生成一个上下文管理器，并明确指定在这个管理器中。如下面代码所示：      
```python
# 在名字为foo的命名空间内创建名字为v的变量
with tf.variable_scope('foo'):
     v = tf.get_variable('v', [1], initializer=tf.constant_initializer(l.0))
# 因为在foo中已经存在名字为v的变量，所以以下代码将会报错
with tf.variable_scope('foo'):
     v = tf.get_variable('v', [1])
# 在生成上下文管理器时，将参数reuse设置为True，tf.get_variable函数将直接获取已经声明的变量
with tf.variable_scope('foo'，reuse=True):
     v1 = tf.get_variable('v', [1])
     print(v==v1)  # 输出为True
     # 将参数reuse设置为True时，tf.variable_scope将只能获取已经创建过的变量
     with tf.variable_scope('bar'):
          v = tf.get_variable('v', [1])		# 这段代码会报错，因为bar中没有创建v
```
注：当出现嵌套时，若未声明，某一层的reuse参数与相邻外层相同。   
此外，tf.variable_scope函数还可以管理变量的名称，如下代码所示：   
```python
vl = tf.get_variable('v', [1])
print(vl.name)		# 输出为v:0
	
with tf.variable_scope('foo'):
     v2 = tf . get variable('v', [1])
     print(v2.name)		# 输出为foo/v:0

     with tf.variable_scope('foo'):
          with tf.variable_scope('bar'):
          v3 = tf.get_variable('v', [1])		
          print(v3.name)		# 输出为foo/bar/v:0
     v4 = tf.get_variable('v1', [1])
print(v4.name)		# 输出为foo/v1:0
```
## 4、模型的保存与加载  
```python 
saver = tf.train.Saver()
```  
（1）模型保存
saver.save(sess, model_save_path)  
（2）模型加载
saver.restore(sess, model_save_path)    
之后便可以使用sess.run()调用模型了。   
## 5、将数据转化为one-hot编码的形式   
```python
tf.one_hot(indices, depth, on_value=None, off_value=None, axis=None, name=None)
```
示例：
```python
class_num = 3
labels = tf.constant([0, 1, 2])
output = tf.one_hot(labels, class_num)
with tf.Session() as sess:
    print(output.eval())
```
输出：
```python
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
 ```
## 6、TypeError: from_generator() got an unexpected keyword argument 'args'
使用lambda关键字基于raw_data_gen定义一个匿名函数
例如：
错误：training_dataset = tf.data.Dataset.from_generator(raw_data_gen, (tf.float32, tf.uint8), ([None, 1], [None]), args=(([1])))
改正：training_dataset = tf.data.Dataset.from_generator(lambda: raw_data_gen (train_val_or_test  =1), (tf.float32, tf.uint8), ([None, 1], [None]))
其中，train_val_or_test=1是raw_data_gen函数所需要的参数。
## 7、tf.nn.embedding_lookup
```
假设：embedding：shape=[vocabulary, d_model]，x：shape=[N, len]  
执行：x_emdedding = tf.nn.embedding_lookup(embedding, x)  
结果：x_embedding：shape=[N, len, d_model]  
tf.nn.embedding_lookup是根据x对embedding执行查询操作，把查到的所有结果concat起来，作为返回值。
```
## 8、tensorboard的使用  
Tensorboard的可视化功能很丰富。  
SCALARS栏目展示各标量在训练过程中的变化趋势，如accuracy、cross entropy、learning_rate、网络各层的bias和weights等标量。  
如果输入数据中存在图片、视频，那么在IMAGES栏目和AUDIO栏目下可以看到对应格式的输入数据。在GRAPHS栏目中可以看到整个模型计算图结构。  
在HISTOGRAM栏目中可以看到各变量（如：activations、gradients，weights 等变量）随着训练轮数的数值分布，横轴上越靠前就是越新的轮数的结果。  
DISTRIBUTIONS和HISTOGRAM是两种不同形式的直方图，通过这些直方图可以看到数据整体的状况。  
PROJECTOR栏目中默认使用PCA分析方法，将高维数据投影到3D空间，从而显示数据之间的关系。  
（1）步骤：  
1）创建writer，写日志文件    
```python
writer=tf.summary.FileWriter('/path', tf.get_default_graph())  
```
2）保存日志文件  
```python
writer.close() 
```
3）运行可视化命令，启动服务  
tensorboard –logdir path  
4）打开可视化界面  
通过浏览器打开服务器访问端口http://xxx.xxx.xxx.xxx:6006  
（2）在本地使用服务器端的tensorboard  
连接服务器时使用：  
如果使用的是cmd，则ssh -L 8080:127.0.0.1:8080 username@remote_server_ip；  
如果使用的是xshell，则设置隧道，建立本地和服务器的映射。  
在服务器端使用：  
tensorboard –logdir=logs/ --port 8008  
打开谷歌浏览器：输入网址127.0.0.1:8008  
## 9、Tensorflow不同版本引起的错误  
（1）AttributeError: 'module' object has no attribute 'merge_all_summaries'  
将tf.merge_all_summaries() 改为：summary_op = tf.summary.merge_all()  
（2）AttributeError: 'module' object has no attribute 'SummaryWriter'  
将 tf.train.SummaryWriter 改为：tf.summary.FileWriter  
（3）AttributeError: 'module' object has no attribute 'scalar_summary'  
将tf.scalar_summary 改为：tf.summary.scalar  
（4）AttributeError: 'module' object has no attribute 'histogram_summary'  
将histogram_summary 改为：tf.summary.histogram  






