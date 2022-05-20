---
title: Shell初级使用
date: 2022-05
tags: [Shell]
---

# Shell初级使用

## Shell变量
shell变量的三种定义方式：
1. variable=value
2. variable='value'
3. variable="value"

        =号前后不能有空格

        variable 是变量名，value 是赋给变量的值

        如果 value 不包含任何空白符（例如空格、Tab缩进等），那么可以不使用引号

        如果 value 包含了空白符，那么就必须使用引号包围起来

        使用单引号和使用双引号也是有区别的

        使用变量时加上$符号
        变量使用时加上{}限定符

        以单引号' '包围变量的值时，单引号里面是什么就输出什么
        即使内容中有变量和命令（命令需要反引起来）也会把它们原样输出
        这种方式比较适合定义显示纯字符串的情况，即不希望解析变量、命令等的场景

        以双引号" "包围变量的值时，输出时会先解析里面的变量和命令，而不是把双引号中的变量名和命令原样输出
        这种方式比较适合字符串中附带有变量和命令并且想将其解析后再输出的变量定义

### 将命令的执行结果赋值给变量
```bash
variable=`command`
variable=$(command)

第一种方式把命令用反引号包围起来，反引号和单引号非常相似，容易产生混淆，所以不推荐使用这种方式
第二种方式把命令用$()包围起来，区分更加明显
```

### 变量类型
运行shell时，会同时存在三种变量：
1. 局部变量

        局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。

2. 环境变量
        
        所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。

3. shell变量
        
        shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

4. 环境变量：全局环境变量

        查看系统全局变量，可以使用env命令,
        查看个别环境变量，可以使用printenv xxx查看.printenv则打印所有的变量

5. 自定义局部变量
        
        声明变量名时必须加$关键词, 赋值时不要加$关键词,输出时要加$关键词，也可以说使用echo时都要加$。
        变量名，等号，值三者之间不能有空格。如果变量值有空格时，必须加双引号。


6. 自定义全局变量
        
        创建全局变量的方法是先创建一个局部变量,然后导出到全局环境中。
        通过export来导出，变量名前面不加$。

7. 删除变量
        
        要删除环境变量可以用unset命令，在unset引用变量名时，不要加$。


### Shell特殊变量
```bash
$? 上个命令的退出状态，或者函数的返回值
$$ 表示当前shell的进程ID echo “pid $$”

$0 当前脚本的文件名
$1 ... 
$n 传递给脚本或函数的参数 
$1 是第一个参数 
$2 是第二个参数

$# 传递给脚本的参数个数
$* 传递给脚本的所有参数
$@ 传递给脚本的所有参数
$* 和 $@ 都表示传递给函数或脚本的所有参数

不被双引号(" ")包含时, 都以"$1" "$2" … "$n" 的形式输出所有参数
当它们被双引号(" ")包含时, "$*" 会将所有的参数作为一个整体，以"$1 $2 … $n"的形式输出所有参数
"$@" 会将各个参数分开，以"$1" "$2" … "$n" 的形式输出所有参数。
```

## Shell IF 判断逻辑
### 基本语法:
        if [ command ]; then
        符合该条件执行的语句
        fi

### 扩展语法：
        if [ command ];then
        符合该条件执行的语句
        elif [ command ];then
        符合该条件执行的语句
        else
        符合该条件执行的语句
        fi

### 文件/目录判断
```bash
[ -a FILE ] 如果 FILE 存在则为真。 
[ -b FILE ] 如果 FILE 存在且是一个块文件则返回为真。
[ -c FILE ] 如果 FILE 存在且是一个字符文件则返回为真。
[ -d FILE ] 如果 FILE 存在且是一个目录则返回为真。 
[ -e FILE ] 如果 指定的文件或目录存在时返回为真。
[ -f FILE ] 如果 FILE 存在且是一个普通文件则返回为真。
[ -g FILE ] 如果 FILE 存在且设置了SGID则返回为真。
[ -h FILE ] 如果 FILE 存在且是一个符号符号链接文件则返回为真。（该选项在一些老系统上无效）
[ -k FILE ] 如果 FILE 存在且已经设置了冒险位则返回为真。
[ -p FILE ] 如果 FILE 存并且是命令管道时返回为真。
[ -r FILE ] 如果 FILE 存在且是可读的则返回为真。
[ -s FILE ] 如果 FILE 存在且大小非0时为真则返回为真。
[ -u FILE ] 如果 FILE 存在且设置了SUID位时返回为真。
[ -w FILE ] 如果 FILE 存在且是可写的则返回为真。（一个目录为了它的内容被访问必然是可执行的）
[ -x FILE ] 如果 FILE 存在且是可执行的则返回为真。
[ -O FILE ] 如果 FILE 存在且属有效用户ID则返回为真。
[ -G FILE ] 如果 FILE 存在且默认组为当前组则返回为真。（只检查系统默认组）
[ -L FILE ] 如果 FILE 存在且是一个符号连接则返回为真。 
[ -N FILE ] 如果 FILE 存在 and has been mod如果ied since it was last read则返回为真。 
[ -S FILE ] 如果 FILE 存在且是一个套接字则返回为真。
[ FILE1 -nt FILE2 ] 如果 FILE1 比 FILE2 新, 或者 FILE1 存在但是 FILE2 不存在则返回为真。 
[ FILE1 -ot FILE2 ] 如果 FILE1 比 FILE2 老, 或者 FILE2 存在但是 FILE1 不存在则返回为真。
[ FILE1 -ef FILE2 ] 如果 FILE1 和 FILE2 指向相同的设备和节点号则返回为真。
```

### 字符串判断
```bash
[ -z STRING ]    如果STRING的长度为零则返回为真，即空是真
[ -n STRING ]    如果STRING的长度非零则返回为真，即非空是真
[ STRING1 ]　   如果字符串不为空则返回为真,与-n类似
[ STRING1 == STRING2 ]   如果两个字符串相同则返回为真
[ STRING1 != STRING2 ]    如果字符串不相同则返回为真
[ STRING1 < STRING2 ]     如果 “STRING1”字典排序在“STRING2”前面则返回为真。 
[ STRING1 > STRING2 ]     如果 “STRING1”字典排序在“STRING2”后面则返回为真。
```

### 数值判断
```bash
[ INT1 -eq INT2 ]          INT1和INT2两数相等返回为真 ,=
[ INT1 -ne INT2 ]          INT1和INT2两数不等返回为真 ,<>
[ INT1 -gt INT2 ]           INT1大于INT2返回为真 ,>
[ INT1 -ge INT2 ]          INT1大于等于INT2返回为真,>=
[ INT1 -lt INT2 ]            INT1小于INT2返回为真 ,<
[ INT1 -le INT2 ]           INT1小于等于INT2返回为真,<=
```

### 逻辑判断
```bash
[ ! EXPR ]       逻辑非，如果 EXPR 是false则返回为真。
[ EXPR1 -a EXPR2 ]      逻辑与，如果 EXPR1 and EXPR2 全真则返回为真。
[ EXPR1 -o EXPR2 ]      逻辑或，如果 EXPR1 或者 EXPR2 为真则返回为真。
[  ] || [  ]           用OR来合并两个条件
[  ] && [  ]        用AND来合并两个条件
```

### 其他判断
```bash
[ -t FD ]  如果文件描述符 FD （默认值为1）打开且指向一个终端则返回为真
[ -o optionname ]  如果shell选项optionname开启则返回为真
```

### IF高级特性
1. 双圆括号((  ))：表示数学表达式
    
    在判断命令中只允许在比较中进行简单的算术操作，而双圆括号提供更多的数学符号，而且在双圆括号里面的'>','<'号不需要转意。

2. 双方括号[[  ]]：表示高级字符串处理函数
    
    双方括号中判断命令使用标准的字符串比较，还可以使用匹配模式，从而定义与字符串相匹配的正则表达式。

3. 双括号的作用：

        在shell中，[ $a != 1 || $b = 2 ]是不允许，要用[ $a != 1 ] || [ $b = 2 ]，而双括号就可以解决这个问题的，[[ $a != 1 || $b = 2 ]]。
        
        又比如这个[ "$a" -lt "$b" ]，也可以改成双括号的形式(("$a" < "$b"))

### sample
#### 判断目录$doiido是否存在，若不存在，则新建一个   
        if [ ! -d "$doiido"]; then
        　　mkdir "$doiido"
        fi

#### 判断普通文件$doiido是否存，若不存在，则新建一个
        if [ ! -f "$doiido" ]; then
        　　touch "$doiido"
        fi

#### 判断$doiido是否存在并且是否具有可执行权限
        if [ ! -x "$doiido"]; then
        　　mkdir "$doiido"
        chmod +x "$doiido"
        fi

#### 是判断变量$doiido是否有值
        if [ ! -n "$doiido" ]; then
        　　echo "$doiido is empty"
        　　exit 0
        fi

#### 两个变量判断是否相等
        if [ "$var1" = "$var2" ]; then
        　　echo '$var1 eq $var2'
        else
        　　echo '$var1 not eq $var2'
        fi

#### 测试退出状态：
        if [ $? -eq 0 ];then
        echo 'That is ok'
        fi

#### 数值的比较：
        if [ "$num" -gt "150" ]
        echo "$num is biger than 150"
        fi

#### a>b 且 a<c
        (( a > b )) && (( a < c ))
        [[ $a > $b ]] && [[ $a < $c ]]
        [ $a -gt $b -a $a -lt $c ]

9：a>b或a<c
(( a > b )) || (( a < c ))
[[ $a > $b ]] || [[ $a < $c ]]
[ $a -gt $b -o $a -lt $c ]

10：检测执行脚本的用户
if [ "$(whoami)" != 'root' ]; then
    echo "You have no permission to run $0 as non-root user."
    exit 1;
fi
上面的语句也可以使用以下的精简语句
[ "$(whoami)" != 'root' ] && ( echo "You have no permission to run $0 as non-root user."; exit 1 )

11：正则表达式
doiido="hero"
if [[ "$doiido" == h* ]];then 
    echo "hello，hero"
fi


# shell常用组合命令

## 获取当前ip地址
        ifconfig eth0 | grep "inet " | awk '{print $2}'


## 获取ssid 
        iwlist wlan0 scan | grep ESSID | cut -d"\"" -f 2

## 获取时间
```bash
#!/bin/bash

#明天凌晨 对应的毫秒时间戳
tomorrow=`date -d next-day +%Y-%m-%d`
timeStamp=`date -d "$tomorrow 00:00:00" +%s`
currentTimeStamp=$(($timeStamp*1000+`date "+%N"`/1000000))
#将current转换为时间戳，精确到毫秒
echo $currentTimeStamp
#当前时间对应的毫秒时间戳<
current=`date "+%Y-%m-%d %H:%M:%S"`
timeStamp=`date -d "$current" +%s`
currentTimeStamp=$((timeStamp*1000+`date "+%N"`/1000000))
#将current转换为时间戳，精确到毫秒
echo $curre
```

## 简单的开机检测脚本
```bash
#!/bin/bash

# var
process_name="mqtt_logger_main"
process_path=/home/cv/mqtt_logger/mqtt_logger_main
process_log=/home/cv/mqtt_logger/log.txt

# check function
fun(){
        while [ true ]
        do
                ps -ef | grep ${process_name} | grep -v grep
                if [ $? -ne 0 ]
                then
                        echo "not exist, create process"
                        ($process_path) &
                        pid=$(ps -ef | grep ${process_name} | grep -v grep | awk '{print $2}')
                        date=$(date "+%Y-%m-%d %H:%M:%S")
                        echo "${date} ${process_name} PID=${pid} start." >> ${process_log}
                else
                        echo "exist, ignore"
                fi
                sleep 1m #sleep 1 minute
        done
}

# only run
fun

# error quit
echo "error quit!" >> ${process_log}
```

## 查看当前操作系统类型
```bash
#!/bin/sh
SYSTEM=`uname -s`
if [ $SYSTEM = "Linux" ] ; then
    echo "Linux"
elif [ $SYSTEM = "FreeBSD" ] ; then
    echo "FreeBSD"
elif [ $SYSTEM = "Solaris" ] ; then
    echo "Solaris"
else
    echo "What?"
fi
```

### if利用read传参判断
```bash
#!/bin/bash
read -p "please input a score:" score
echo -e "your score [$score] is judging by sys now"

if [ "$score" -ge "0" ]&&[ "$score" -lt "60" ];then
    echo "sorry,you are lost!"
elif [ "$score" -ge "60" ]&&[ "$score" -lt "85" ];then
    echo "just soso!"
elif [ "$score" -le "100" ]&&[ "$score" -ge "85" ];then
    echo "good job!"
else
    echo "input score is wrong , the range is [0-100]!"
fi
```

## 判断文件是否存在
```bash
#!/bin/sh
today=`date -d yesterday +%y%m%d`
file="apache_$today.tar.gz"
cd /home/chenshuo/shell
if [ -f "$file" ];then
    echo "OK"
else
    echo "error $file" >error.log
    mail -s "fail backup from test" loveyasxn924@126.com <error.log
fi
```

## 这个脚本在每个星期天由cron来执行。如果星期的数是偶数，他就提醒你把垃圾箱清理：
```bash
#!/bin/bash
WEEKOFFSET=$[ $(date +"%V") % 2 ]

if [ $WEEKOFFSET -eq "0" ]; then
    echo "Sunday evening, put out the garbage cans." | mail -s "Garbage cans out" your@your_domain.org
fi
```

## 挂载硬盘脚本(windows下的ntfs格式硬盘)
```bash
#! /bin/sh
dir_d=/media/disk_d
dir_e=/media/disk_e
dir_f=/media/disk_f

a=`ls $dir_d | wc -l`
b=`ls $dir_e | wc -l`
c=`ls $dir_f | wc -l`

echo "checking disk_d..."
if [ $a -eq 0 ]; then
  echo "disk_d is not exsit,now creating..."
  sudo mount -t ntfs /dev/disk/by-label/software /media/disk_d
else
  echo "disk_d exits"
fi

echo "checking disk_e..."
if [ $b -eq 0 ]; then
  echo "disk_e is not exsit,now creating..."
  sudo mount -t ntfs /dev/disk/by-label/elitor /media/disk_e
else
  echo "disk_e exits"
fi

echo "checking disk_f..."
if [ $c -eq 0 ]; then
  echo "disk_f is not exsit,now creating..."
  sudo mount -t ntfs /dev/disk/by-label/work /media/disk_f
else
  echo "disk_f exits"
fi
```
## 判断字符串是否为空
```bash
#!/bin/bash
str1="hello world"
if [[ -n "$str1" ]]; then
    echo "str1 is not empty"
fi
str2=""
if [[ ! -n "$str2" ]]; then
    echo "str2 is empty"
fi
```

