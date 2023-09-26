# MySql 快速安装


# MySql快速安装

## Linux安装

### 安装
```bash
sudo su
sudo apt update
sudo apt install mysql-server
mysql --version
```

### 查看服务
```bash
netstat -tap | grep mysql
netstat -ano
netstat -anp
```

### 登录

```bash
mysql -u root -p
```

### 改密码

```sql
ALTER user 'root'@'localhost' IDENTIFIED with mysql_native_password BY '123456';
quit
```

### 跳过密码验证
```bash
sudo vim /etc/my.cnf
```
末尾添加
```txt
[mysqld]
skip-grant-tables
```

### 远程访问授权

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# bind-address = 127.0.0.1
```
```sql
create user root@'%' identified by '123456';
grant all privileges on *.* to root@'%' with grant option;

use mysql;
update user set host = '%' where user = 'root';
flush privileges;
```

### 开关服务

```bash
# ubuntu
systemctl restart mysql;
systemctl status mysql;
systemctl start
systemctl stop
systemctl reload

service mysql stop
service mysql start
service mysql restart

/etc/init.d/mysql start
/etc/init.d/mysql stop
/etc/init.d/mysql restart

# redhat 均为 mysqld
/etc/init.d/mysqld restart
```

### 查看

端口（默认3306）
```sql
SHOW VARIABLES LIKE 'port';
```
安装位置
```bash
whereis mysql
```
配置文件位置
```bash
/etc/my.cnf
```

### 备用用户

```bash
sudo cat /etc/mysql/debian.cnf
# debian-sys-maint
# ztv*************

mysql -u debian-sys-maint -p
```
```sql
use mysql;
select user,plugin from user;

update user set plugin='mysql_native_password' where user='root'; # 修改其密码格式
select user,plugin from user; # 查询其用户

flush privileges;

alter user 'root'@'localhost' identified with mysql_native_password by '123456';

flush privileges;
```

## Windows源码安装

### 配置环境变量

```txt
......\mysql-8.0.31-winx64\bin
```

### 编辑配置文件

mysql-8.0.31-winx64目录下写`my.ini`

```txt
[mysqld]
port=3306
basedir=D:\\dev\\mysql-8.0.31-winx64
datadir=D:\\dev\\mysql-8.0.31-winx64\\data
secure-file-priv=
```

如果需要跳过登录密码（忘记密码）：
my.ini添加：
```txt
skip-grant-tables
```
或者：
```bash
mysqld --console --skip-grant-tables --shared-memory 
```

### 初始化
```bash
mysqld --initialize-insecure
```

### 创建服务
```bash
mysqld --install mysql8031

mysqld -remove
mysqld -install
mysqld --initialize
```

### 开关服务
```bash
net start mysql8031
net stop mysql8031
```

### 登录
```bash
mysql -u root -p 123456
```

### 设密码
```sql
flush privileges;
ALTER user 'root'@'localhost' IDENTIFIED BY '123456';
```


