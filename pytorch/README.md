# pytorch knowledge
## 1 可视化
（1）使用tensorboardX可视化   
参考：[https://www.jianshu.com/p/46eb3004beca]  
安装：pip install tensorboardX  
代码：  
```python
from tensorboardX import SummaryWriter
writer = SummaryWriter(log_path)
writer.add_scalar('data/scalar1', data[0], step)
writer.add_scalars(
    'data/scalar_group', 
    {
        'xsinx': n_iter * np.sin(n_iter),
        'xcosx': n_iter * np.cos(n_iter),
        'arctanx': np.arctan(n_iter)
    },
    n_iter
)
writer.add_image('image/image1', x, n_iter)
writer.add_images('image/image_group', batch_x, n_iter)
writer.add_text('Text', 'text logged at step:' + str(n_iter), n_iter)

for name, param in resnet18.named_parameters():
    writer.add_histogram(name, param.clone().cpu().data.numpy(), n_iter)

writer.close()
```
监听：在命令行，输入 tensorboard --logdir log_path --port xxx  
（2）使用visdom可视化
安装：pip install visdom  
代码：  
```python
from visdom import Visdom
viz = Visdom()
# 画一条曲线
# 初始化线，第一个值为Y，第二个值为X，现在初始化为0，win作为这条线的唯一标识符，也可以设置envs来管理win，opts是额外的配置信息
viz.line([0.], [0.], win='train_loss', opts=dict(title='train_loss'))
# 更新曲线
viz.line([loss.item()], [global_step], win='train_loss', update='append')

# 画多条曲线，初始化和更新时，设置多个Y的信息，还要注意配置信息，设置lengend做区分
viz.line([[0., 0.0]], [0.], win='test', opts=dict(title='test loss&acc.', legend=['loss', 'acc.']))
viz.line([[loss.item(), correct / len(test_loader.dataset)]], [global_step], win='test', update='append')

# 显示图片
viz.images(data.view(-1, 1, 28, 28), win='x')
# 显示文本
viz.text(str(pred.detach().cpu().numpy()), win='pred', opts=dict(title='pred'))
```
监听：在命令行，输入 python -m visdom.server  
注：可视化图片是，tensorboardX需要接收numpy()类型的数据，visdom可以直接接收tensor类型的数据。
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


