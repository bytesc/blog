# Linux 环境变量


# Linux环境变量

## Linux环境变量分类

### 按照生命周期来分
1. 永久的：需要用户修改相关的配置文件，变量永久生效。
2. 临时的：用户利用export命令，在当前终端下声明环境变量，关闭Shell终端失效。

### 按照作用域来分
1. 系统环境变量：系统环境变量对该系统中所有用户都有效。
2. 用户环境变量：顾名思义，这种类型的环境变量只对特定的用户有效。


## Linux设置环境变量的方法

### 对所有用户永久生效

在/etc/profile文件末尾添加变量 
```bash
vim /etc/profile 
```
```txt
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```
```bash
source /etc/profile
# 修改文件后要运行生效，不然只能在下次重进此用户时生效。
```

### 对单一用户永久生效
在用户目录下的.bash_profile文件末尾增加变量
```bash
vim ~/.bash.profile
```
```txt
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```
```bash
source /etc/profile
# 修改文件后要运行生效，不然只能在下次重进此用户时生效。
```

### 只对当前shell临时有效
直接运行export命令定义变量
```bash
export 变量名=变量值
```

## Linux中常见的环境变量

```txt
PATH：指定命令的搜索路径
HOME：指定用户的主工作目录（即用户登陆到Linux系统中时，默认的目录）。
HISTSIZE：指保存历史命令记录的条数。
LOGNAME：指当前用户的登录名。
HOSTNAME：指主机的名称，许多应用程序如果要用到主机名的话，通常是从这个环境变量中来取得的
SHELL：指当前用户用的是哪种Shell。
LANG/LANGUGE：和语言相关的环境变量，使用多种语言的用户可以修改此环境变量。
MAIL：指当前用户的邮件存放目录。
注意：上述变量的名字并不固定，如HOSTNAME在某些Linux系统中可能设置成HOST
```

## 修改和查看环境变量

echo 显示某个环境变量值
```bash
echo $PATH
# 查看环境变量
export HELLO="hello"
# 设置一个新的环境变量
export PATH=$PAHT:<PATH 1>:<PATH 2>:<PATH 3>:--------:< PATH n >
# 环境变量追加

env 
# 显示所有环境变量
set 
# 显示本地定义的shell变量
unset HELLO
# 清除环境变量 
readonly HELLO
# 设置只读环境变量 
```

