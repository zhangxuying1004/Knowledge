## 0 Ubuntu 16.04下载镜像
中科大源  
http://mirrors.ustc.edu.cn/ubuntu-releases/16.04/  

阿里云开源镜像站  
http://mirrors.aliyun.com/ubuntu-releases/16.04/  

兰州大学开源镜像站  
http://mirror.lzu.edu.cn/ubuntu-releases/16.04/  

北京理工大学开源  
http://mirror.bit.edu.cn/ubuntu-releases/16.04/  

浙江大学  
http://mirrors.zju.edu.cn/ubuntu-releases/16.04/  

## 1 添加删除用户 
```
（1）添加用户  
sudo adduser 用户名  
（2）删除用户  
sudo userdel -r 用户名  
```
## 2 查看CUDA、CUDNN版本号 
```
（1）查看CUDA版本  
cat /usr/local/cuda/version.txt  
（2）查看CUDNN版本  
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2  
```
## 3 查看显卡信息  
```
nvidia-smi  
```
## 4 刷新命令  
```
source ~/.bashrc  
```
## 5 xxx is not in the sudoers file.This incident will be reported.的解决方法 
```
（1）切换到一个已在sudoers的用户下  
（2）输入命令：sudo vim /etc/sudoers，填写此用户的密码  
找到这行 root ALL=(ALL) ALL,在他下面添加xxx ALL=(ALL) ALL  
```
## 6 tmux常用命令  
```
（1）启动一个新会话：tmux [new -s 会话名 -n 窗口名]  
（2）列出存在的所有会话：tmux ls  
（3）恢复会话：tmux at [-t 会话名]  
（4）关闭会话：tmux kill-session -t 会话名 
（5）脱离当会话：Ctrl + b，d
（6）多窗口操作
Ctrl + b，"，水平分割当前窗格
Ctrl + b，%，垂直分割当前窗格
Ctrl + b，x，删除当前窗格
Ctrl + b，方向键，通过上下左右方向键跳转到对应的窗格
Ctrl + b，；，跳转到上次激活的窗格
Ctrl + b，o，跳转到下一个窗格
Ctrl + b，q，显示各窗口的编号，并输入编号跳转到对应的窗格
Ctrl + b，{，将当前窗格移动到最左边
Ctrl + b，}，将当前窗格移动到最右边
```
## 7 VIM配置
```
syntax on                                                                                                           
set number                                                             
set cursorline                                                                          
                                                                                                                       
set tabstop=4                                                                                                   
set softtabstop=4                                                                          
set shiftwidth=4                                                                                                      
                                                                                                                       
set autoindent                                                                                                       
                                                                                                                      
filetype on                                                                                                    
filetype plugin on  
filetype indent on                                                                                         
set completeopt=longest,menu                                                                      
                                                                                                            
set encoding=utf-8                                                                                             
set termencoding=utf-8                                                 
set clipboard+=unnamed
```
## 8 VIM常用操作
```
（1）跳转到最后一行：shift+g
（2）跳转到当前行的第一个字符：在当前行按“0”
（3）文件重新载入：e!
（4）单行复制：将光标移到复制行 按 'yy'进行复制
（5）多行复制：将光标移到复制首行 按 'nyy'进行复制 n=1.2.3.4…
（6）单行剪切：将光标移到复制行 按 'dd'进行复制
（7）多行剪切：将光标移到复制首行 按 'ndd'进行复制 n=1.2.3.4…
（8）粘贴：将光标移到粘贴行 按 'p'进行粘贴
（0）查找：/pattern Enter
```
## 9 文件夹操作
```
（1）文件夹复制
cp -r source target
（2）文件夹移动
mv source target
（3）文件夹删除
rm -rf source target
```
## 10 管道符“|”的作用
```
命令格式：命令A|命令B，即命令A的正确输出作为命令B的操作对象。
例如：ps aux | grep "test"  在 ps aux中的結果中查找test。
```
## 11 命令之间的分号，&&， ||
```
在用linux命令时候，我们经常需要同时执行多条命令，那么命令之间该如何分割呢？
分号：顺序地独立执行各条命令， 彼此之间不关心是否失败，所有命令都会执行。
&&：顺序执行各条命令， 只有当前一个执行成功时候，才执行后面的。
||：顺序执行各条命令， 只有当前面一个执行失败的时候，才执行后面的。
```
## 12 安装Java运行环境
```
sudo apt-get install default-jre
sudo apt-get install default-jdk
```
## 13 统计当前目录下的文件个数、目录个数
```
（1）查看当前目录下的文件数量（不包含子目录中的文件）
ls -l|grep "^-"| wc -l
（2）查看当前目录下的文件数量（包含子目录中的文件） 注意：R，代表子目录
ls -lR|grep "^-"| wc -l
（3）查看当前目录下的文件夹目录个数（不包含子目录中的目录），同上述理，如果需要查看子目录的，加上R
ls -l|grep "^d"| wc -l
```
## 14 查看内存信息
```
（1）free -m
（2）sudo fdisk -l
（3）df -h
```
## 15 挂载与取消挂载
```
mount [参数] [设备名称] [挂载点]
umount [参数] [挂载点]
```
## 16 opencv安装
```
CUDA version: 9.0.176，CUDNN version: 7.4.2，pyhton==3.6.9环境下，opencv的安装：
pip install opencv-python==3.3.0.10
pip install opencv-contrib-python==3.3.0.10
```
## 17 查看指定进程的信息  
```
ps -aux | grep 进程号  
```
## 18 两台ubuntu电脑之间传输文件  
```
scp -r /home/wangpf/Report2 mingzi@111.111.111.111:/home/wenjianjia
```
## 19 将pip默认下载源设置为清华镜像  
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
## 20 pip安装出现Script file 'D:\Anaconda3\Scripts\pip-script.py' is not present.  
```
easy_install pip
```
## 21 软链接的建立与删除
linux下的软链接类似于windows下的快捷方式。 
（1）软链接的建立  
```
ln -s a b
```
其中，a就是源文件，b是链接文件名，其作用是当进入b目录，实际上是链接进入了a目录。  
（2）软链接的删除  
```
rm -rf b  # 注意不是rm -rf  b/
```
## 22 查看有哪些网络接口
```
ls /sys/class/net
```
## 23 程序停了，仍占显存
方法1：  
```python
import os
pid = list(set(os.popen('fuser -v /dev/nvidia*').read().split()))   # * 是显卡号码
kill_cmd = 'kill -9 ' + ' '.join(pid)
print(kill_cmd)
os.popen(kill_cmd)
```
方法2：  
```python
sudo fuser -v /dev/nvidia2 |awk '{for(i=1;i<=NF;i++)print "kill -9 " $i;}' | sudo sh
```
## 24 终端命令提示符前缀(conda 虚拟环境名)显示或者消失
```
conda config --set changeps1 true   # 显示
conda config --set changeps1 false  # 消失
```
## 100 Ubuntu压缩/解压  
```
.tar
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName
.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName
.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
.bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知
.tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知
.Z
解压：uncompress FileName.Z
压缩：compress FileName
.tar.Z
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName
.7Z
sudo apt-get install p7zip p7zip-full
解压：7z x FileName.7z
压缩：7za -t7z -m0=lzma -mx=9 -mfb=64 -md=32m -ms=on a ./dir.7z dir

.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName
.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName
.lha
解压：lha -e FileName.lha
压缩：lha -a FileName.lha FileName
.rpm
解包：rpm2cpio FileName.rpm | cpio -div
.deb
解包：ar p FileName.deb data.tar.gz | tar zxf -
.tar .tgz .tar.gz .tar.Z .tar.bz .tar.bz2 .zip .cpio .rpm .deb .slp .arj .rar .ace .lha .lzh .lzx .lzs .arc .sda .sfx .lnx .zoo .cab .kar .cpt .pit .sit .sea
解压：sEx x FileName.*
压缩：sEx a FileName.* FileName
注：sEx只是调用相关程序，本身并无压缩、解压功能
```
