## 1 XShell配置   
1.1 主题配色   
生成配色文件XTerm.scs  
```
[XTerm]
text=00ff80
cyan(bold)=00ffff
text(bold)=e9e9e9
magenta=c000c0
green=80ff00
green(bold)=3c5a38
background=042028
cyan=00c0c0
red(bold)=ff0000
yellow=c0c000
magenta(bold)=ff00ff
yellow(bold)=ffff00
red=ff4500
white=c0c0c0
blue(bold)=1e90ff
white(bold)=fdf6e3
black=000000
blue=00bfff
black(bold)=808080
[Names]
name0=XTerm
count=1
```
工具->配色方案->导入->选中XTerm.scs文件->确定   

## 2 VS code 配置  
2.1 远程服务器连接配置  
第一步：在本地安装Open-SSH
在win10本地，设置-->应用-->可选功能-->Open-SSH客户端。  
安装成功的标志：打开cmd或者PowerShell，可以通过ssh user_name@server_ip的方式登录到服务器。  
第二步：在VS code中安装插件python，Remote-SSH，Remote-SSH: Editing Configuration Files，Remote-Container，Remote-WSL，Remote-Development。     
安装成功的标志：侧边栏出现Remote-Explorer的选项卡。     
点击Remote-Explorer选项卡，点击configure，在Select SSH configuration file to edit，选择第一项，编辑里面的内容。  
设置Host、HostName、User和port。  
右击CONNECTIONS中的Host，选择在当前窗口中打开。  
输入登录密码即可登录到服务器。  
选择左侧选项卡中的Explorer（即第一个），选择打开文件夹，在右侧打开指定的工程文件夹即可。  
2.2 主题插件  
Ctrl+Shift+X，在搜索框输入Theme，选择安装自己喜欢的插件。  
本人选择的插件为Cobalt2 Theme Official和vscode-icons。  
注：点击install安装后，还需要点击旁边的reload resquire，自动重启，点击右下方弹出框中的activate，主题才能正式生效。  
2.3 消除vscode编译器中代码的一些奇怪的报错和警告  
在setting.json文件中，添加  
```python
"python.linting.pylintArgs": [
     "--disable=E1001",
     "--disable=W,C",
     "--generated-members"
],
"python.linting.flake8Args": [
     "--max-line-length=1000",
     "--ignore=E402,E117,E226,F401,W293"
],
"python.linting.pydocstyleArgs": [
     "--ignore=D400", 
     "--ignore=D4"
]
```
## 3 jupyter notebook
3.1 jupyter远程连接服务器
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
3.2 常用快捷键  
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


