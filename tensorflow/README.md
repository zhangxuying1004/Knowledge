# Tensorflow knowledge   
## 1、global_step分析      
引用自博客[https://blog.csdn.net/leviopku/article/details/78508951]     
global_step在滑动平均、优化器、指数衰减学习率等方面都有用到，这个变量的实际意义非常好理解：代表全局步数，比如在多少步该进行什么操作，现在神经网络训练到多少轮等等，类似于一个钟表。
