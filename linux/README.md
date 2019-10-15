## 1 Ubuntu压缩/解压
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
