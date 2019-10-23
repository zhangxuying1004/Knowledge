# pytorch knowledge
## 1 使用tensorboardX可视化   
[https://www.jianshu.com/p/46eb3004beca]
## 2 F.cross_entropy(input, target)  
其中，input为模型输出的logits，即模型的最后一层没有加softmax激活函数；  
target为ground-truth labels，不需要转化为one-hot的形式。  
