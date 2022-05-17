---
title: Linux tar 指令
date: 2022-05
tags: [linux, tar, 指令]
---

# linux tar 解压
### .tar
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName

### .gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName

### .tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName

### .bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName

### .tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName

### .bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知

### .tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知

### .Z
解压：uncompress FileName.Z
压缩：compress FileName

### .tar.Z
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName

### .zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName

### .rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName

### .lha
解压：lha -e FileName.lha
压缩：lha -a FileName.lha FileName

### .rpm
解包：rpm2cpio FileName.rpm | cpio -div

### .deb
解包：ar p FileName.deb data.tar.gz | tar zxf -

## gzip
语法：gzip [选项] 压缩（解压缩）的文件名该命令的各选项含义如下：

-c 将输出写到标准输出上，并保留原有文件。
-d 将压缩文件解压。
-l 对每个压缩文件，显示下列字段：     
        压缩文件的大小；
        未压缩文件的大小；
        压缩比；
        未压缩文件的名字
-r 递归式地查找指定目录并压缩其中的所有文件或者是解压缩。
-t 测试，检查压缩文件是否完整。
-v 对每一个压缩和解压的文件，显示文件名和压缩比。
-num 用指定的数字 num 调整压缩的速度，-1 或 --fast 表示最快压缩方法（低压缩比），-9 或--best表示最慢压缩方法（高压缩比）。系统缺省值为 6。

指令实例：
gzip *% 把当前目录下的每个文件压缩成 .gz 文件。
    gzip -dv *% 把当前目录下每个压缩的文件解压，并列出详细的信息。
    gzip -l *% 详细显示例1中每个压缩的文件的信息，并不解压。
    gzip usr.tar% 压缩 tar 备份文件 usr.tar，此时压缩文件的扩展名为.tar.gz。


总结

*.tar 用 tar –xvf 解压
*.gz 用 gzip -d或者gunzip 解压
*.tar.gz和.tgz 用 tar –xzf 解压
*.bz2 用 bzip2 -d或者用bunzip2 解压
*.tar.bz2用tar –xjf 解压
*.Z 用 uncompress 解压
*.tar.Z 用tar –xZf 解压
*.rar 用 unrar e解压
*.zip 用 unzip 解压

## 压缩
tar –cvf jpg.tar *.jpg  将目录里所有jpg文件打包成tar.jpg
tar –czf jpg.tar.gz *.jpg   将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
tar –cjf jpg.tar.bz2 *.jpg 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
tar –cZf jpg.tar.Z *.jpg   将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
rar a jpg.rar *.jpg rar格式的压缩，需要先下载rar for linux
zip jpg.zip *.jpg   zip格式的压缩，需要先下载zip for linux

## 解压
tar –xvf file.tar  解压 tar包
tar -xzvf file.tar.gz 解压tar.gz
tar -xjvf file.tar.bz2   解压 tar.bz2
tar –xZvf file.tar.Z   解压tar.Z
unrar e file.rar 解压rar
unzip file.zip 解压zip

### .tar.xz
需要安装apt install xz-utils
tar -xf backup.tar.xz 
xz -d -v filename.tar.xz