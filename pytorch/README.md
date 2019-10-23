# pytorch knowledge
## 1 使用tensorboardX可视化   
[https://www.jianshu.com/p/46eb3004beca]
## 2 F.cross_entropy(input, target)  
其中，input为模型输出的logits，即模型的最后一层没有加softmax激活函数；  
target为ground-truth labels，不需要转化为one-hot的形式。  
## 3 one-hot实现  
在pytorch中没有封装的one-hot方法，使用时需要自己定义。   
```python
def one_hot(label, depth=10):
    out = torch.zeros(label.size(0), depth).long()
    idx = torch.unsqueeze(label, dim=1)
    out.scatter_(dim=1, index=idx, value=1)
    return out.float()
```
