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
