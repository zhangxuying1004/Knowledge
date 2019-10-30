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
## 4 tensor与numpy之间的转换
```python
# tensor to numpy
tensor_var.numpy()

# numpy to tensor
torch.from_numpy(numpy_var)
```
## 5 GPU计算
5.1 计算设备  
（1）查看显卡信息  
命令：nvidia-smi或者gpustat  
（2）用代码查看GPU信息  
```python
torch.cuda.is_available()       # 查看GPU是否可用，正常情况下输出为True
torch.cuda.device_count()       # 查看GPU数目
torch.cuda.current_device()     # 查看当前GPU索引号，索引号从0开始
torch.cuda.get_device_name(i)   # 根据索引号查看GPU名字
```
（3）将对象放到GPU上  
方式一：.cuda()  
如果只有一张卡，对象.cuda()  
如果有多张卡，对象.cuda(i)，表示第i(从0开始)张GPU和相应的显存，且cuda(0)和cuda等价  
方式二：.to(device)  
device = torch.device('cuda:i')  
对象.to(device)  
（4）多GPU计算  
使用torch.nn.DataParallel将模型wrap一下：net = torch.nn.DataParallel(net, device_ids=None)  
默认所有的GPU都会被使用，若要使用特定的GPU，可以在device_ids列表中指定对应的索引号。  
（5）多GPU模型的保存与加载  
方式一：  
```python
# 保存模型
torch.save(net.state_dict(), 'model_name.pkl')
# 加载模型
net = model_class()
net = torch.nn.DataParallel(net)
net.load_state_dict(torch.load('model_name.pkl'))
```
方式二：  
```python
# 保存模型
torch.save(net.module.state_dict(), 'model_name.pkl')
# 加载模型
net = model_class()
net.load_state_dict(torch.load('model_name.pkl'))
```
推荐使用方式二。  


