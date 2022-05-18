
---
### ls命令

ls命令是列出目录内容(List Directory Contents)的意思。运行它就是列出文件夹里的内容，可能是文件也可能是文件夹。

    “ls -l”命令以详情模式(long listing fashion)列出文件夹的内容。

    "ls -a"命令会列出文件夹里的所有内容，包括以"."开头的隐藏文件。

    注意：在Linux中，文件以“.”开头的就是隐藏文件，并且每个文件，文件夹，设备或者命令都是以文件对待。
    ls -l 命令输出：
        1.d (代表了是目录).
        2.rwxr-xr-x 是文件或者目录对所属用户，同一组用户和其它用户的权限。
        3.上面例子中第一个ravisaive 代表了文件文件属于用户ravisaive
        4.上面例子中的第二个ravisaive代表了文件文件属于用户组ravisaive
        5.4096 代表了文件大小为4096字节.
        6.May 8 01:06 代表了文件最后一次修改的日期和时间.
        7.最后面的就是文件/文件夹的名字


 
---
### lsblk命令

"lsblk"就是列出块设备。除了RAM外，以标准的树状输出格式，整齐地显示块设备。

    “lsblk -l”命令以列表格式显示块设备(而不是树状格式)。

    注意：lsblk是最有用和最简单的方式来了解新插入的USB设备的名字，特别是当你在终端上处理磁盘/块设备时。

 
---
### md5sum命令

“md5sum”就是计算和检验MD5信息签名。md5 checksum(通常叫做哈希)使用匹配或者验证文件的文件的完整性，因为文件可能因为传输错误，磁盘错误或者无恶意的干扰等原因而发生改变。

 
---
### dd命令

“dd”命令代表了转换和复制文件。可以用来转换和复制文件，大多数时间是用来复制iso文件(或任何其它文件)到一个usb设备(或任何其它地方)中去，所以可以用来制作USB启动器。

    root@raspberry:~# dd if=/home/user/Downloads/debian.iso of=/dev/sdb1 bs=512M; sync

    注意：在上面的例子中，usb设备就是sdb1（你应该使用lsblk命令验证它，否则你会重写你的磁盘或者系统），请慎重使用磁盘的名，切忌。

    dd 命令在执行中会根据文件的大小和类型 以及 usb设备的读写速度，消耗几秒到几分钟不等。

---
### uname命令

"uname"命令就是Unix Name的简写。显示机器名，操作系统和内核的详细信息。

    root@raspberrypi:/# uname -a
    Linux raspberrypi 3.6.11+ #528 PREEMPT Tue Aug 20 00:25:53 BST 2013 armv6l GNU/Linux

    注意： uname显示内核类别， uname -a显示详细信息。
    上面的输出详细说明了uname -a
        1.“Linux“: 机器的内核名
        2.“tecmint“: 机器的节点名
        3.“3.8.0-19-generic“: 内核发布版本
        4.“#30-Ubuntu SMP“: 内核版本
        5.“i686“: 处理器架构
        6.“GNU/Linux“: 操作系统名

---
### history命令

“history”命令就是历史记录。它显示了在终端中所执行过的所有命令的历史。

    注意：按住“CTRL + R”就可以搜索已经执行过的命令，它可以在你写命令时自动补全。

---
### sudo命令

“sudo”(super user do)命令允许授权用户执行超级用户或者其它用户的命令。通过在sudoers列表的安全策略来指定。

    注意：sudo 允许用户借用超级用户的权限，然而"su"命令实际上是允许用户以超级用户登录。所以sudo比su更安全。
    
    并不建议使用sudo或者su来处理日常用途，因为它可能导致严重的错误如果你意外的做错了事，这就是为什么在linux社区流行一句话：

---
### mkdir命令

“mkdir”(Make directory)命令在命名路径下创建新的目录。然而如果目录已经存在了，那么它就会返回一个错误信息"不能创建文件夹，文件夹已经存在了"("cannot create folder, folder already exists")

---
### touch 命令

“touch”命令代表了将文件的访问和修改时间更新为当前时间。touch命令只会在文件不存在的时候才会创建它。如果文件已经存在了，它会更新时间戳，但是并不会改变文件的内容。

---
### chmod 命令

“chmod”命令就是改变文件的模式位。chmod会根据要求的模式来改变每个所给的文件，文件夹，脚本等等的文件模式（权限）。

在文件(文件夹或者其它，为了简单起见，我们就使用文件)中存在3中类型的权限

Read (r)=4
Write(w)=2
Execute(x)=1

所以如果你想给文件只读权限，就设置为'4';只写权限，设置权限为'2';只执行权限，设置为1; 读写权限，就是4+2 = 6, 以此类推。

现在需要设置3种用户和用户组权限。第一个是拥有者，然后是用户所在的组，最后是其它用户。

这里root的权限是 rwx（读写和执行权限），
所属用户组权限是 r-x (只有读和执行权限, 没有写权限)，
对于其它用户权限是 -x(只有只执行权限)

为了改变它的权限，为拥有者，用户所在组和其它用户提供读，写，执行权限。

 
---
### chown命令

“chown”命令就是改变文件拥有者和所在用户组。每个文件都属于一个用户组和一个用户。在你的目录下，使用"ls -l",你就会看到像这样的东西。


复制代码

root@raspberrypi:/opt# ls -l
total 24
drwxr-xr-x 2 root root 4096 Aug 26 02:51 labpark
drwxr-xr-x 3 root root 4096 Aug 22 12:20 node
drwxr-xr-x 2 root root 4096 Aug 21 06:57 python
drwxr-xr-x 7 root root 4096 Aug 19 16:41 vc
-rw-r--r-- 1 root root 13 Aug 22 10:04 vsftpd.txt
d-wx-wx-wx 2 root root 4096 Aug 22 11:26 wwwroot

复制代码

在这里，目录labpark属于用户"root",和用户组"root",

“chown”命令用来改变文件的所有权，所以仅仅用来管理和提供文件的用户和用户组授权。

测试试用的是一个Raspberry，所以系统里有一个默认用户pi，属于用户组pi的，试用下面的命令，

root@raspberrypi:/opt# chown pi:pi labpark/

可以看到labpark的所属组已经更改为pi pi


复制代码
root@raspberrypi:/opt# ls -l
total 24
drwxr-xr-x 2 pi   pi   4096 Aug 26 02:51 labpark
drwxr-xr-x 3 root root 4096 Aug 22 12:20 node
drwxr-xr-x 2 root root 4096 Aug 21 06:57 python
drwxr-xr-x 7 root root 4096 Aug 19 16:41 vc
-rw-r--r-- 1 root root   13 Aug 22 10:04 vsftpd.txt
d-wx-wx-wx 2 root root 4096 Aug 22 11:26 wwwroot

复制代码

注意：“chown”所给的文件改变用户和组的所有权到新的拥有者或者已经存在的用户或者用户组。

 

 

1.  apt命令

Debian系列以“apt”命令为基础，“apt”代表了Advanced Package Tool。APT是一个为Debian系列系统（Ubuntu，Kubuntu等等）开发的高级包管理器，在Gnu/Linux系统上，它会为包自动地，智能地搜索，安装，升级以及解决依赖。








root@raspberrypi:/opt# apt-get install nginx

Reading package lists... Done

Building dependency tree

Reading state information... Done

nginx is already the newest version.

0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded. 


　　


复制代码
root@raspberrypi:/opt# apt-get update
Get:1 http://mirrordirector.raspbian.org wheezy Release.gpg [490 B]
Hit http://raspberrypi.collabora.com wheezy Release.gpg
Hit http://archive.raspberrypi.org wheezy Release.gpg
Get:2 http://mirrordirector.raspbian.org wheezy Release [14.4 kB]
Hit http://raspberrypi.collabora.com wheezy Release
Hit http://archive.raspberrypi.org wheezy Release
Hit http://raspberrypi.collabora.com wheezy/rpi armhf Packages
Hit http://archive.raspberrypi.org wheezy/main armhf Packages
Get:3 http://mirrordirector.raspbian.org wheezy/main armhf Packages [7,415 kB]
1% [3 Packages 22.9 kB/7,415 kB 0%] [Waiting for headers] [Waiting for headers]^C

复制代码

 

注意：上面的命令会导致系统整体的改变，所以需要root密码（查看提示符为"#"，而不是“$”）.和yum命令相比，Apt更高级和智能。

见名知义，apt-cache用来搜索包中是否包含子包mplayer, apt-get用来安装，升级所有的已安装的包到最新版。

关于apt-get 和 apt-cache命令更多信息，请查看 25 APT-GET和APT-CACHE命令

 

 

13. tar命令

“tar”命令是磁带归档(Tape Archive)，对创建一些文件的的归档和它们的解压很有用。

 

root@raspberry:~# tar -zxvf abc.tar.gz (记住'z'代表了.tar.gz)

root@raspberry:~# tar -jxvf abc.tar.bz2 (记住'j'代表了.tar.bz2)

root@raspberry:~# tar -cvf archieve.tar.gz(.bz2) /path/to/folder/abc

注意： "tar.gz"代表了使用gzip归档，“bar.bz2”使用bzip压缩的，它压缩的更好但是也更慢。

了解更多"tar 命令"的例子，请查看 18 Tar命名例子

 

14. cal 命令

“cal”（Calender），它用来显示当前月份或者未来或者过去任何年份中的月份。


复制代码
root@raspberrypi:/opt# cal
    August 2013
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31

复制代码

显示指定的年份的月份，可以是过去的也可以是未来的。


复制代码
root@raspberrypi:/opt# cal 10 1986
    October 1986
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31

复制代码

注意： 你不需要往回调整日历50年，既不用复杂的数据计算你出生那天，也不用计算你的生日在哪天到来，[因为它的最小单位是月，而不是日]。

 

15. date命令

“date”命令使用标准的输出打印当前的日期和时间，也可以深入设置。

root@raspberrypi:/opt# date
Mon Aug 26 03:21:36 UTC 2013

设置系统时间：

root@raspberrypi:/opt# date --set='26 aug 2013 11:42'
Mon Aug 26 11:42:00 UTC 2013

注意：这个命令在脚本中十分有用，以及基于时间和日期的脚本更完美。而且在终端中改变日期和时间，让你更专业！！！（当然你需要root权限才能操作这个，因为它是系统整体改变）

 

16. cat命令

“cat”代表了连结（Concatenation），连接两个或者更多文本文件或者以标准输出形式打印文件的内容。

在book.txt中只有一个book单词，在facebook.txt中只有facebook一个单词，然后连接之后输出如下：

root@raspberrypi:/opt/labpark# cat book.txt facebook.txt
book
facebook

 

 

! 叫做非，带'！'的反向字符串为真

更多请阅读Linux cat 命令的实例 13 Linux中cat命令实例

 

17. cp 命令

“copy”就是复制。它会从一个地方复制一个文件到另外一个地方。

root@raspberrypi:/opt/labpark# cp book.txt book_backup.txt

 

注意： cp，在shell脚本中是最常用的一个命令，而且它可以使用通配符（在前面一块中有所描述），来定制所需的文件的复制。

 

 

 

18. mv 命令

“mv”命令将一个地方的文件移动到另外一个地方去。

root@raspberrypi:/opt/labpark# mv nihao.txt /opt/python/

注意：mv 命令可以使用通配符。mv需谨慎使用，因为移动系统的或者未授权的文件不但会导致安全性问题，而且可能系统崩溃。

 

 

19. pwd 命令

“pwd”（print working directory），在终端中显示当前工作目录的全路径。

root@raspberrypi:/opt/labpark# pwd
/opt/labpark

注意： 这个命令并不会在脚本中经常使用，但是对于新手，当从连接到nux很久后在终端中迷失了路径，这绝对是救命稻草。

 

 

20. cd 命令

最后，经常使用的“cd”命令代表了改变目录。它在终端中改变工作目录来执行，复制，移动，读，写等等操作。

 

注意： 在终端中切换目录时，cd就大显身手了。“cd 〜”会改变工作目录为用户的家目录，而且当用户发现自己在终端中迷失了路径时，非常有用。“cd ..”从当前工作目录切换到(当前工作目录的)父目录。

这些命令肯定会让你在Linux上很舒服。但是这并不是结束。例如，如果你熟练使用这些命令，欢呼吧，少年，你会发现你已从小白级别提升为了中级用户了。在下篇文章，我会介绍像“kill”,"ps","grep"等等命令，期待吧，我不会让你失望的。

 

 

 

 

对中级 Linux 用户非常有用的 20 个命令

21. 命令: Find

搜索指定目录下的文件，从开始于父目录，然后搜索子目录。

root@raspberrypi:/opt/labpark# find book*
book_backup.txt
book.txt

 

root@raspberrypi:/opt/labpark# find -name *.c
./hello.c

 

root@raspberrypi:/opt/labpark# find -iname FACE*
./facebook.txt

 

注意： `-name‘选项是搜索大小写敏感。可以使用`-iname‘选项，这样在搜索中可以忽略大小写。（*是通配符，可以搜索所有的文件；‘.sh‘你可以使用文件名或者文件名的一部分来制定输出结果）

 

注意：以上命令查找根目录下和所有文件夹以及加载的设备的子目录下的所有包含‘tar.gz'的文件。

’find'命令的更详细信息请参考35 Find Command Examples in Linux

 

 

22. 命令: grep

 

‘grep‘命令搜索指定文件中包含给定字符串或者单词的行。举例搜索‘/etc/passwd‘文件中的‘pi'

root@raspberrypi:/opt/labpark# grep pi /etc/passwd
pi:x:1000:1000:,,,:/opt/wwwroot:/bin/bash

使用’-i'选项将忽略大小写。








root@raspberrypi:/opt/labpark# grep -i  'PI' /etc/passwd

pi:x:1000:1000:,,,:/opt/wwwroot:/bin/bash 


使用’-r'选项递归搜索所有自目录下包含字符串 “localhost“.的行。

root@raspberrypi:/opt/labpark# grep -r "localhost" /etc/php5/
/etc/php5/cgi/php.ini:SMTP = localhost
/etc/php5/cli/php.ini:SMTP = localhost
/etc/php5/fpm/php.ini:SMTP = localhost

注意：您还可以使用以下选项：
1.-w 搜索单词 (egrep -w ‘word1|word2‘ /path/to/file).
2.-c 用于统计满足要求的行 (i.e., total number of times the pattern matched) (grep -c ‘word‘ /path/to/file).
3.�Ccolor 彩色输出 (grep �Ccolor server /etc/passwd).

 

23. 命令: man

‘man‘是系统帮助页。Man提供命令所有选项及用法的在线文档。几乎所有的命令都有它们的帮助页，例如：


复制代码
root@raspberrypi:/opt/labpark# man
What manual page do you want?
root@raspberrypi:/opt/labpark# man man
MAN(1)                        Manual pager utils                        MAN(1)

NAME
       man - an interface to the on-line reference manuals

SYNOPSIS
       man  [-C  file]  [-d]  [-D]  [--warnings[=warnings]]  [-R encoding] [-L
       locale] [-m system[,...]] [-M path] [-S list]  [-e  extension]  [-i|-I]
       [--regex|--wildcard]   [--names-only]  [-a]  [-u]  [--no-subpages]  [-P
       pager] [-r prompt] [-7] [-E encoding] [--no-hyphenation] [--no-justifiaü
       cation]  [-p  string]  [-t]  [-T[device]]  [-H[browser]] [-X[dpi]] [-Z]
       [[section] page ...] ...
       man -k [apropos options] regexp ...
       man -K [-w|-W] [-S list] [-i|-I] [--regex] [section] term ...
       man -f [whatis options] page ...
       man -l [-C file] [-d] [-D] [--warnings[=warnings]]  [-R  encoding]  [-L
       locale]  [-P  pager]  [-r  prompt]  [-7] [-E encoding] [-p string] [-t]
       [-T[device]] [-H[browser]] [-X[dpi]] [-Z] file ...
       man -w|-W [-C file] [-d] [-D] page ...
       man -c [-C file] [-d] [-D] page ...
       man [-hV]

DESCRIPTION

复制代码

上面是man命令的系统帮助页，类似的有cat和ls的帮助页。

注意：系统帮助页是为了命令的使用和学习而设计的。

 

 

24. 命令: ps

ps命令给出正在运行的某个进程的状态，每个进程有特定的id成为PID。


复制代码
root@raspberrypi:/opt/labpark# ps
  PID TTY          TIME CMD
 2801 pts/1    00:00:01 bash
 2868 pts/1    00:00:00 su
 2885 pts/1    00:00:00 su
 2892 pts/1    00:00:00 bash
 2981 pts/1    00:00:00 ps

复制代码

使用‘-A‘选项可以列出所有的进程及其PID。


复制代码
root@raspberrypi:/opt/labpark# ps -A
  PID TTY          TIME CMD
    1 ?        00:00:01 init
    2 ?        00:00:00 kthreadd
    3 ?        00:00:00 ksoftirqd/0
    5 ?        00:00:00 kworker/0:0H
    6 ?        00:00:00 kworker/u:0
    7 ?        00:00:00 kworker/u:0H
    8 ?        00:00:00 khelper
    9 ?        00:00:00 kdevtmpfs
   10 ?        00:00:00 netns
   12 ?        00:00:00 bdi-default
   13 ?        00:00:00 kblockd
   14 ?        00:00:00 khubd
   15 ?        00:00:00 rpciod
   16 ?        00:00:00 khungtaskd
   17 ?        00:00:00 kswapd0
   18 ?        00:00:00 fsnotify_mark
   19 ?        00:00:00 nfsiod
   20 ?        00:00:00 crypto
   27 ?        00:00:00 kthrotld
   28 ?        00:00:00 VCHIQ-0
   29 ?        00:00:00 VCHIQr-0
   30 ?        00:00:00 VCHIQs-0
   31 ?        00:00:00 iscsi_eh
   32 ?        00:00:00 dwc_otg
   33 ?        00:00:00 DWC Notificatio
   35 ?        00:00:00 deferwq
   36 ?        00:00:00 kworker/u:2
   37 ?        00:00:15 mmcqd/0
   38 ?        00:00:00 jbd2/mmcblk0p2-
   39 ?        00:00:00 ext4-dio-unwrit
  154 ?        00:00:00 udevd
  636 ?        00:00:00 udevd
  650 ?        00:00:00 udevd
 1523 ?        00:00:01 ifplugd
 1546 ?        00:00:00 ifplugd
 1686 ?        00:00:00 kworker/0:2
 1784 ?        00:00:00 dhclient
 1839 ?        00:00:00 rc
 1848 ?        00:00:02 startpar
 1914 ?        00:00:00 rsyslogd
 1940 ?        00:00:00 php5-fpm
 1941 ?        00:00:00 php5-fpm
 1942 ?        00:00:00 php5-fpm
 1967 ?        00:00:00 cron
 2005 ?        00:00:00 dbus-daemon
 2048 ?        00:00:00 nginx
 2050 ?        00:00:00 nginx
 2069 ?        00:00:00 mysqld_safe
 2443 ?        00:00:14 mysqld
 2444 ?        00:00:00 logger
 2501 ?        00:00:00 sshd
 2529 ?        00:00:00 thd
 2537 ?        00:00:00 vsftpd
 2692 ?        00:00:00 ntpd
 2715 ?        00:00:00 rc.local
 2718 ?        00:00:00 rc.local
 2720 ?        00:00:25 python
 2723 ?        00:00:02 sshd
 2728 ?        00:00:00 console-kit-dae
 2795 ?        00:00:00 polkitd
 2801 pts/1    00:00:01 bash
 2868 pts/1    00:00:00 su
 2875 pts/1    00:00:00 bash
 2885 pts/1    00:00:00 su
 2892 pts/1    00:00:00 bash
 2934 ?        00:00:00 kworker/0:1
 2971 ?        00:00:00 flush-179:0
 2982 pts/1    00:00:00 ps

复制代码

 

注意：当你要知道有哪些进程在运行或者需要知道想杀死的进程PID时ps命令很管用。你可以把它与‘grep‘合用来查询指定的输出结果，例如：

root@raspberrypi:/opt/labpark# ps -A | grep -i nginx
 2048 ?        00:00:00 nginx
 2050 ?        00:00:00 nginx

 

ps命令与grep命令用管道线分割可以得到我们想要的结果。

 

25. 命令: kill

也许你从命令的名字已经猜出是做什么的了,kill是用来杀死已经无关紧要或者没有响应的进程.它是一个非常有用的命令,而不是非常非常有用.你可能很熟悉Windows下要杀死进程可能需要频繁重启机器因为一个在运行的进程大部分情况下不能够杀死,即使杀死了进程也需要重新启动操作系统才能生效.但在linux环境下,事情不是这样的.你可以杀死一个进程并且重启它而不是重启整个操作系统.

杀死一个进程需要知道进程的PID.

假设你想杀死已经没有响应的‘nginx'进程,运行如下命令:

root@raspberrypi:/opt/labpark# kill 2048

注意:kill需要PID作为参数,pkill可以选择应用的方式,比如指定进程的所有者等.

 

 

26. 命令: whereis

whereis的作用是用来定位命令的二进制文件\资源\或者帮助页.举例来说,获得ls和kill命令的二进制文件/资源以及帮助页:

root@raspberrypi:/# whereis ls
ls: /bin/ls /usr/share/man/man1/ls.1.gz

root@raspberrypi:/# whereis kill
kill: /bin/kill /usr/share/man/man2/kill.2.gz /usr/share/man/man1/kill.1.gz

 

注意:当需要知道二进制文件保存位置时有用.

 

 

 

27. 命令: service

‘service‘命令控制服务的启动、停止和重启，它让你能够不重启整个系统就可以让配置生效以开启、停止或者重启某个服务。

查看当前服务状态


复制代码
root@raspberrypi:/# service --status-all
 [ ? ]  alsa-utils
 [ - ]  bootlogs
 [ ? ]  bootmisc.sh
 [ ? ]  checkfs.sh
 [ ? ]  checkroot-bootclean.sh
 [ - ]  checkroot.sh
 [ - ]  console-setup
 [ + ]  cron
 [ + ]  dbus
 [ ? ]  dphys-swapfile
 [ ? ]  fake-hwclock
 [ - ]  hostname.sh
 [ ? ]  hwclock.sh
 [ + ]  ifplugd
 [ - ]  kbd
 [ - ]  keyboard-setup
 [ ? ]  killprocs
 [ ? ]  kmod
 [ - ]  lightdm
 [ - ]  motd
 [ ? ]  mountall-bootclean.sh
 [ ? ]  mountall.sh
 [ ? ]  mountdevsubfs.sh
 [ ? ]  mountkernfs.sh
 [ ? ]  mountnfs-bootclean.sh
 [ ? ]  mountnfs.sh
 [ ? ]  mtab.sh
 [ ? ]  mysql
 [ ? ]  networking
 [ - ]  nfs-common
 [ + ]  nginx
 [ - ]  ntp
 [ + ]  php5-fpm
 [ ? ]  plymouth
 [ ? ]  plymouth-log
 [ - ]  procps
 [ ? ]  rc.local
 [ - ]  rmnologin
 [ - ]  rpcbind
 [ - ]  rsync
 [ + ]  rsyslog
 [ ? ]  sendsigs
 [ + ]  ssh
 [ - ]  sudo
 [ + ]  triggerhappy
 [ + ]  udev
 [ ? ]  udev-mtab
 [ ? ]  umountfs
 [ ? ]  umountnfs.sh
 [ ? ]  umountroot
 [ - ]  urandom
 [ + ]  vsftpd
 [ - ]  x11-common

复制代码

 

查看nginx服务的状态：service nginx status

root@raspberrypi:/# service nginx status
[ ok ] nginx is running.

 

停止nginx服务：

root@raspberrypi:/# service nginx stop
Stopping nginx: nginx.
root@raspberrypi:/# service nginx status
[FAIL] nginx is not running ... failed!

启动nginx服务：

root@raspberrypi:/# service nginx start
Starting nginx: nginx.
root@raspberrypi:/# service nginx status
[ ok ] nginx is running.

 

注意：要想使用service命令，进程的脚本必须放在‘/etc/init.d‘，并且路径必须在指定的位置。

如果要运行“service apache2 start”实际上实在执行“service /etc/init.d/apache2 start”.

 

 

 
---
### alias

alias是一个系统自建的shell命令，允许你为名字比较长的或者经常使用的命令指定别名。


我经常用‘ls -l‘命令，它有五个字符（包括空格）。于是我为它创建了一个别名‘l'。

root@raspberrypi:/# alias l='ls -l'
root@raspberrypi:/# alias
alias l='ls -l'

试试它是否能用：


复制代码
root@raspberrypi:/# l
total 92
drwxr-xr-x   2 root root  4096 Jul 26 12:19 bin
drwxr-xr-x   2 root root 16384 Jan  1  1970 boot
drwxr-xr-x   2 root root  4096 Jan  1  1970 boot.bak
drwxr-xr-x  12 root root  3060 Aug 26 10:43 dev
drwxr-xr-x 100 root root  4096 Aug 26 10:43 etc
drwxr-xr-x   3 root root  4096 Jul 26 11:47 home
drwxr-xr-x  13 root root  4096 Aug 19 16:36 lib
drwx------   2 root root 16384 Jul 26 11:22 lost+found
drwxr-xr-x   2 root root  4096 Jul 26 11:24 media
drwxr-xr-x   2 root root  4096 Jun 15 15:44 mnt
drwxr-xr-x   7 root root  4096 Aug 26 03:11 opt
dr-xr-xr-x  79 root root     0 Jan  1  1970 proc
drwx------   7 root root  4096 Aug 23 01:47 root
drwxr-xr-x  14 root root   620 Aug 26 11:45 run
drwxr-xr-x   2 root root  4096 Jul 26 12:19 sbin
drwxr-xr-x   2 root root  4096 Jun 20  2012 selinux
drwxr-xr-x   3 root root  4096 Aug 22 10:17 srv
dr-xr-xr-x  12 root root     0 Jan  1  1970 sys
drwxrwxrwt   4 root root  4096 Aug 26 11:39 tmp
drwxr-xr-x  10 root root  4096 Jul 26 11:24 usr
drwxr-xr-x  11 root root  4096 Jul 26 13:43 var

复制代码

去掉’l'别名，要使用unalias命令：

root@raspberrypi:/# unalias l
root@raspberrypi:/# alias
root@raspberrypi:/# ^C
root@raspberrypi:/# l
bash: l: command not found

去掉‘l’别名之后，再输入‘l’，就会报 commadn not found！

 

---
### df

报告系统的磁盘使用情况。在跟踪磁盘使用情况方面对于普通用户和系统管理员都很有用。 ‘df‘ 通过检查目录大小工作，但这一数值仅当文件关闭时才得到更新。

    -h 参数，按M为单位输出

---
### du

估计文件的空间占用。 逐层统计文件（例如以递归方式）并输出摘要。

---
### rm

'rm' 标准移除命令。 rm 可以用来删除文件和目录。

    'rm' 不能直接删除目录,需要加上相应的'-rf'参数才可以。

    警告: "rm -rf" 命令是一个破坏性的命令,假如你不小心删除一个错误的目录。一旦你使用'rm -rf' 删除一个目录,在目录中所有的文件包括目录本身会被永久的删除,所以使用这个命令要非常小心。

---
### passwd

这是一个很重要的命令，在终端中用来改变自己密码很有用。显然的，因为安全的原因，你需要知道当前的密码。

    root@raspberrypi:/opt/wwwroot# passwd
    Enter new UNIX password:
    Retype new UNIX password:
    passwd: password updated successfully

---
### lpr

这个命令用来在命令行上将指定的文件在指定的打印机上打印。

    root@raspberry:~# lpr -P deskjet-4620-series 1-final.pdf

---
### cmp

比较两个任意类型的文件并将结果输出至标准输出。如果两个文件相同， ‘cmp‘默认返回0；如果不同，将显示不同的字节数和第一处不同的位置。

    比较一下这两个文件，看看命令的输出。

    root@raspberrypi:/opt/labpark# cmp book.txt test.txt
    book.txt test.txt differ: byte 4, line 1

---
### wget

Wget是用于非交互式（例如后台）下载文件的免费工具.支持HTTP, HTTPS, FTP协议和 HTTP 代理。

---
### mount

mount 是一个很重要的命令，用来挂载不能自动挂载的文件系统。你需要root权限挂载设备。

在插入你的文件系统后，首先运行"lsblk"命令，识别出你的设备，然后把分配的设备名记下来。

---
### ifconfig

ifconfig用来配置常驻内核的网络接口信息。在系统启动必要时用来设置网络适配器的信息。之后，它通常是只需要在调试时或当系统需要调整时使用。

    “-a”参数用来显示所有网络适配器(网卡)的详细信息,包括那些停用的适配器。

---
### netstat

netstat命令显示各种网络相关的信息，如网络连接,路由表,接口统计,伪装连接,组播成员身份等....

    root@raspberrypi:~# netstat -a

    显示所有tcp相关端口：
    root@raspberrypi:~# netstat -at

    显示所有连接的统计信息：
    root@raspberrypi:~# netstat -s

    不解析netstat 输出的主机、端口和用户名称的话 
    root@raspberrypi:~# netstat -an

    netstat 持续输出的动态信息，通过传递中断输出指令 (ctrl + c)来停止。
    root@raspberrypi:~# netstat -c

---
### nslookup

网络实用程序，用于获得互联网服务器的信息。顾名思义，该实用程序将发现通过查询 DNS 域的名称服务器信息。

    root@raspberrypi:~# nslookup

---
### uptime

你连接到你的 Linux 服务器时发现一些不寻常或恶意的东西，你会做什么？猜测......不，绝不！你可以运行uptime来验证当服务器无人值守式到底发生了什么事情。

    root@raspberrypi:~# uptime
    04:09:59 up 7 min,  1 user,  load average: 0.01, 0.31, 0.24

---
### w

是否觉得命令'w'很滑稽？但是事实上不是的。它是一个命令，尽管只有一个字符长！命令"w"是uptime和who命令，以前后的顺序组合在一起。

    root@raspberrypi:~# w
    04:10:30 up 7 min,  1 user,  load average: 0.01, 0.27, 0.23
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    root     pts/0    10.2.58.132      04:04    0.00s  0.65s  0.03s w

---
### top

显示CPU进程信息。这个命令自动刷新，默认是持续显示CPU进程信息，除非使用了中断指令（Ctrl+c）。

---
### mkfs.ext4

这个命令在指定的设备上创建一个新的ext4文件系统，如果这个命令后面跟的是个错误的设备，那么整个设备就会被擦除和格式化，所以建议不要运行这个命令，除非你清楚自己正在干什么。

    Mkfs.ext4 /dev/sda1 (sda1 block will be formatted)
    mkfs.ext4 /dev/sdb1 (sdb1 block will be formatted)

---
### vi/emac/nano 命令

vi (visual), emac, nano 是 linux 中最常用的一些编辑器。

---
### rsync

Rsync复制文件，参数-P开启进度条。如果你已经安装了rsync，你可以使用一个简单的别名。

    root@raspberrypi:~# alias cp='rsync -aP'

    现在尝试在终端复制一个大文件，这样将会看到显示剩余部分的输出，与进度条类似。

    而且，保持和维护备份是系统管理员不得不做的最重要、最无聊的工作之一。Rsync是一个用于新建和维护备份的非常好用的终端工具（也存在许多其它工具）。

    [root@raspberry~]$ rsync -zvr IMG_5267\ copy\=33\ copy\=ok.jpg ~/Desktop/ 

    sending incremental file list 
    IMG_5267 copy=33 copy=ok.jpg 

    sent 2883830 bytes  received 31 bytes  5767722.00 bytes/sec 
    total size is 2882771  speedup is 1.00
 
    注意: -z表示压缩， -v表示详细信息，-r表示递归。

---
### free

跟踪内存的使用和资源一样重要，就像管理员执行的任何其它任务，可以使用 'free' 命令来在这里救援.

    root@raspberrypi:/opt/labpark# free
                total       used       free     shared    buffers     cached
    Mem:        448760     134304     314456          0      10664      66648
    -/+ buffers/cache:      56992     391768
    Swap:       102396          0     102396


    以可读的格式显示，检查当前内存使用

    root@raspberrypi:/opt/labpark# free -h
                total       used       free     shared    buffers     cached
    Mem:          438M       131M       307M         0B        10M        65M
    -/+ buffers/cache:        55M       382M
    Swap:          99M         0B        99M

 

    设定 时间间隔 后 ，持续检查 使用状态

    root@raspberrypi:/opt/labpark# free -s 3
                total       used       free     shared    buffers     cached
    Mem:        448760     134552     314208          0      10680      66652
    -/+ buffers/cache:      57220     391540
    Swap:       102396          0     102396

                total       used       free     shared    buffers     cached
    Mem:        448760     134552     314208          0      10680      66652
    -/+ buffers/cache:      57220     391540
    Swap:       102396          0     102396

---
### mysqldump 命令

好了，现在你从名字上就能明白这个命令所代表的作用。mysqldump 命令会转储(备份)数据库的全部或特定一部分数据到一个给定的文件中。例如：

[root@raspberry~]$ mysqldump -u root -p --all-databases > /home/server/Desktop/backupfile.sql

 

注意： mysqldump 需要 mysql 在运行中并且有正确的授权密码。我们在 用mysqldump命令备份数据库中讨论了一些有用的 “mysqldump” 命令用法。

---
### mkpasswd 命令

根据指定的长度，产生一个难猜的随机密码。

    [root@raspberry ~]$ mkpasswd -l 10
    zI4+Ybqfx9
    [root@raspberry ~]$ mkpasswd -l 20 
    w0Pr7aqKk&hmbmqdrlmk

    注意： -l 10 产生一个10个字符的随机密码，而-l 20 产生 20个字符的密码,它可以设置为任意长度来取得所希望的结果。这个命令很有用，经常在脚本语言里使用来产生随机的密码。你可能需要 yum 或 apt ‘expect’ 包来使用这个命令。

    apt-get install expect
    //或者
    yum  install expect
 
---
### paste

合并两个或多个文本文件，按行来进行合并。

    合并a1.txt, a2.txt 两个或多个文本文件，按行来进行合并.输出到 a3.txt 文件之后查看a3.txt

    root@raspberrypi:/opt/labpark# paste a1.txt a2.txt > a3.txt

---
### lsof

lsof 是"list open files("列表中打开的文件") 的缩写，显示您的系统当前已打开的所有文件。这是非常有用的对于想找出哪些进程使用某一特定文件，或显示为单个进程打开所有文件。一些有用的 10 个lsof 命令示例，你可能会感兴趣阅读。
