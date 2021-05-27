## 1 opencv安装
```
CUDA version: 9.0.176，CUDNN version: 7.4.2，pyhton==3.6.9环境下，opencv的安装：
pip install opencv-python==3.3.0.10
pip install opencv-contrib-python==3.3.0.10
```
## 2 size and 坐标
```
size = (height, width, channel)
坐标 = (w_coord, h_coord)
即坐标的第二维对应size的高度，坐标的第一维对应size的宽度。
