# 收集的一些Shell命令

# Shell变量
```
// shell变量的三种定义方式：
variable=value
variable='value'
variable="value"
// =号前后不能有空格
// variable 是变量名，value 是赋给变量的值
// 如果 value 不包含任何空白符（例如空格、Tab缩进等），那么可以不使用引号
// 如果 value 包含了空白符，那么就必须使用引号包围起来
// 使用单引号和使用双引号也是有区别的

// 使用变量时加上$符号
// 变量使用时加上{}限定符

// 以单引号' '包围变量的值时，单引号里面是什么就输出什么
// 即使内容中有变量和命令（命令需要反引起来）也会把它们原样输出
// 这种方式比较适合定义显示纯字符串的情况，即不希望解析变量、命令等的场景

// 以双引号" "包围变量的值时，输出时会先解析里面的变量和命令，而不是把双引号中的变量名和命令原样输出
// 这种方式比较适合字符串中附带有变量和命令并且想将其解析后再输出的变量定义

// 将命令的执行结果赋值给变量
variable=`command`
variable=$(command)
// 第一种方式把命令用反引号包围起来，反引号和单引号非常相似，容易产生混淆，所以不推荐使用这种方式
// 第二种方式把命令用$()包围起来，区分更加明显

// 变量类型
// 运行shell时，会同时存在三种变量：
// 局部变量
// 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
// 环境变量
// 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
// shell变量
// shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

// 环境变量：全局环境变量
// 查看系统全局变量，可以使用env命令,
// 查看个别环境变量，可以使用printenv xxx查看.printenv则打印所有的变量
// 

// 自定义局部变量
// 声明变量名时必须加$关键词, 赋值时不要加$关键词,输出时要加$关键词，也可以说使用echo时都要加$。
// 变量名，等号，值三者之间不能有空格。如果变量值有空格时，必须加双引号。
```

```

// 自定义全局变量
// 创建全局变量的方法是先创建一个局部变量,然后导出到全局环境中。
// 通过export来导出，变量名前面不加$。

// 删除变量
// 要删除环境变量可以用unset命令，在unset引用变量名时，不要加$。

```


# Shell特殊变量
```
$? 上个命令的退出状态，或者函数的返回值
$$ 表示当前shell的进程ID echo “pid $$”

$0 当前脚本的文件名
$1 ... $n 传递给脚本或函数的参数 $1是第一个参数 $2是第二个参数
$# 传递给脚本的参数个数
$* 传递给脚本的所有参数
$@ 传递给脚本的所有参数
$* 和 $@ 都表示传递给函数或脚本的所有参数
不被双引号(" ")包含时, 都以"$1" "$2" … "$n" 的形式输出所有参数
当它们被双引号(" ")包含时, "$*" 会将所有的参数作为一个整体，以"$1 $2 … $n"的形式输出所有参数
"$@" 会将各个参数分开，以"$1" "$2" … "$n" 的形式输出所有参数。
```
# 收集的shell常用指令

## 获取当前ip地址
```
ifconfig eth0 | grep "inet " | awk '{print $2}'
```

## 获取ssid 
```
iwlist wlan0 scan | grep ESSID | cut -d"\"" -f 2
```



# linux shell中获取时间

```
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

# 简单的开机检测脚本
```
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
