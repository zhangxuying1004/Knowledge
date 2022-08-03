# The config process of my macbook pro

## 1 Homebrew
### 1.0 准备工作
检查raw.Githubusercontent.co和github.com是否可以ping通。  
如果能，直接跳过这一步；  
如果不能，在[IpAddress](https://www.ipaddress.com/)中分别搜索出这两个网站的ip，并安装如下方式添加到```/etc/hosts```文件的末尾：
```
# Homebrew install
185.199.108.133    raw.Githubusercontent.com
# Github
140.82.114.4    github.com
```
## 1.2 官方安装方式 (网络情况较好)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
## 1.3 国内镜像安装 （推荐）
```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```
## 1.4 检查
执行如下命令：
```
brew -v
```
如果显示homebrew-core和homebrew-cask的信息，则表明homebrew安装成功；  
如果报错，则依次执行如下命令（使用brew install时出现Error: Command failed with exit 128: git也是一样）：
```
git config --global --add safe.directory /opt/homebrew/Library/Taps/homebrew/homebrew-core
git config --global --add safe.directory /opt/homebrew/Library/Taps/homebrew/homebrew-cask
```
## 2 Git
执行如下命令：
```
brew install git
```
