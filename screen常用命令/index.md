# screen 常用命令


# screen 常用命令

## linux 安装
```bash
# CentOS
yum install screen
# Debian/Ubuntu
apt-get install screen
```
## 常用命令

### 新建一个screen会话
```bash
screen -S <名字>
```
### 退出当前screen会话
```bash
# ctrl+a , 然后ctrl+d
```
### 查看所有screen会话
```bash
screen -ls
```
### 恢复screen会话
```bash
screen -r <会话序列号>
# 会话序列号可通过screen -ls获得
```
### 关闭screen会话
```bash
screen -X -S <序列号> quit
# 或会话内输入exit
```
