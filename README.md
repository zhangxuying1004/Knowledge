# Knowledge
## 1、卷积神经网络的可视化  
[https://cbovar.github.io/ConvNetDraw/]  
## 2、image caption使用的COCO数据集中的标注
**（1）两个文件**  
训练集对应的标注：captions_train201x.json   
测试集对应的标注：captions_val201x.json  

**（2）文件内容的格式** 
```python
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


