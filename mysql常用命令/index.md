# MySql 常用命令


# MySql常用命令

## 基本操作

### 分隔符设置

```sql
DELIMITER $$
DELIMITER ;
```

### 执行sql文件

```bash
mysql -uroot -ppassword -Dmy_database < my_script.sql
source my_script.sql;
```

## DDL

### 数据库

```sql
show databases;
select database();
create database [if not exists] 数据库名 [default charset utf8mb4] [collate 排序规则];
show collation;   查看字符集
show create database 数据库名;
drop database [if exists] 数据库名;
use 数据库名;
```

### 表

```sql
show tables;
create table 表名(
	字段1 数据类型[comment 注释内容][not null][unique][default][check(条件表达式)][primary key][foreign key (外键字段名) references 主表名(列名)] [auto_increment],
	字段2 数据类型[comment 注释内容],
	age INT unsigned  comment '年龄' default 18 check(age>=0 and age<=150),
	constraint 外键名称 foreign key (外键字段名) references 主表名(列名) [on update|delete restrict|cascade|set null],
	foreign key (外键字段名) references 主表名(列名) [on update|delete restrict|cascade|set null],	
	primary key (字段1, 字段2),
	[unique] INDEX 索引名(字段1[(length)] , 字段2[(length)] ),
)[comment 表注释];

show create table 表名; 
desc 表名;
drop table [if exists] 表名；
truncate table 表名;  删除并创建一样的新表
alter table 旧表名 rename to 新表名;
alter table 表名 add 字段名 类型 [comment] [约束];
alter table 表名 add constraint 外键名称 foreign key (外键字段名) references 主表名(列名) [on update|delete restrict|cascade|set null];
ALTER TABLE table_name ADD [unique] INDEX index_name (column1[(length)],column2[(length)]);
ALTER TABLE table_name DROP [unique] INDEX index_name;
CREATE [unique] INDEX index_name ON table(column[(length)])
alter table 表名 modify 字段名 新数据类型 [comment] [约束];
alter table 表名 change 旧字段名 新字段名 新数据类型 [comment] [约束];
alter table 表名 drop 字段名; 
```

### 数据类型

```sql
TINYINT	SMALLINT	MEDIUMINT	INT		BIGINT	FLOAT		DOUBLE	DECIMAL(M,D)	
DATE	TIME	YEAR	DATETIME	TIMESTAMP	
CHAR(n)	VARCHAR(n)
TINYBLOB	TINYTEXT	BLOB	TEXT	MEDIUMBLOB	MEDIUMTEXT	LONGBLOB	LONGTEXT	
```


## DML

```sql
insert into 表名(字段1, 字段2) values (值1, 值2);
insert into 表名 values (值1, 值2);
insert into 表名 values (值1, 值2), (值1, 值2), (值1, 值2);
INSERT IGNORE INTO table_name (column1, column2) VALUES (value1, value2);
REPLACE INTO table_name (column1, column2) VALUES (value1, value2);
INSERT INTO table_name1 (column1, column2) SELECT column3, column4 FROM table_name2;
update 表名 set 字段1=值1, 字段2=值2 [where 条件];
delete from 表名 [where 条件];
```

强制忽略外键约束
```sql
SET FOREIGN_KEY_CHECKS = 0
-- 操作语句
SET FOREIGN_KEY_CHECKS = 1;
```


## DQL

### 查询语句

```sql
select [distinct]
	字段列表
from
	表名列表
where
	分组前条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表 [asc|desc]
limit
	页数码, 页大小;
[for update|share]
select 字段名 as 别名 form 表名;
```

### 比较运算符

```sql
[not] in(值1, 值2)
(值1, 值2) [not] in (select 值1, 值2 from ...)
[not] exists (select 值1, 值2 from ...)
值1>=ANY|ALL (select 值1 from ...)
like '_xxx%' escape '\'
is null
```

### 聚合函数

```sql
count(*)
max(字段名)
min(字段名)
avg(字段名)
sum(字段名)
```

### 连表查询

```sql
SELECT column1, column2 FROM table1
UNION ALL|UNION|INTERSECT|EXCEPT   union去重，union all不去
SELECT column1, column2 FROM table2;

SELECT * FROM table1 JOIN table2 ON table1.id = table2.id; 内连接
SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.id;  左外连接
SELECT * FROM table1 RIGHT JOIN table2 ON table1.id = table2.id; 右外连接
```


## DCL

### 用户管理

```sql
use mysql;
select * from user;
create user '用户名@主机名' identified by '密码'  with grant option;
alter user 'root@%' identified with mysql_native_password by '密码';
drop user '用户名@主机名';
```

备用用户
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

### 权限列表

```txt
(1)授予数据库权限时，<权限类型>可以指定为以下值：
SELECT：表示授予用户可以使用 SELECT 语句访问特定数据库中所有表和视图的权限。
INSERT：表示授予用户可以使用 INSERT 语句向特定数据库中所有表添加数据行的权限。
DELETE：表示授予用户可以使用 DELETE 语句删除特定数据库中所有表的数据行的权限。
UPDATE：表示授予用户可以使用 UPDATE 语句更新特定数据库中所有数据表的值的权限。
REFERENCES：表示授予用户可以创建指向特定的数据库中的表外键的权限。
CREATE：表示授权用户可以使用 CREATE TABLE 语句在特定数据库中创建新表的权限。
ALTER：表示授予用户可以使用 ALTER TABLE 语句修改特定数据库中所有数据表的权限。
SHOW VIEW：表示授予用户可以查看特定数据库中已有视图的视图定义的权限。
CREATE ROUTINE：表示授予用户可以为特定的数据库创建存储过程和存储函数的权限。
ALTER ROUTINE：表示授予用户可以更新和删除数据库中已有的存储过程和存储函数的权限。
INDEX：表示授予用户可以在特定数据库中的所有数据表上定义和删除索引的权限。
DROP：表示授予用户可以删除特定数据库中所有表和视图的权限。
CREATE TEMPORARY TABLES：表示授予用户可以在特定数据库中创建临时表的权限。
CREATE VIEW：表示授予用户可以在特定数据库中创建新的视图的权限。
EXECUTE ROUTINE：表示授予用户可以调用特定数据库的存储过程和存储函数的权限。
LOCK TABLES：表示授予用户可以锁定特定数据库的已有数据表的权限。
ALL 或 ALL PRIVILEGES：表示以上所有权限。

(2) 授予表权限时，<权限类型>可以指定为以下值：
SELECT：授予用户可以使用 SELECT 语句进行访问特定表的权限。
INSERT：授予用户可以使用 INSERT 语句向一个特定表中添加数据行的权限。
DELETE：授予用户可以使用 DELETE 语句从一个特定表中删除数据行的权限。
DROP：授予用户可以删除数据表的权限。
UPDATE：授予用户可以使用 UPDATE 语句更新特定数据表的权限。
ALTER：授予用户可以使用 ALTER TABLE 语句修改数据表的权限。
REFERENCES：授予用户可以创建一个外键来参照特定数据表的权限。
CREATE：授予用户可以使用特定的名字创建一个数据表的权限。
INDEX：授予用户可以在表上定义索引的权限。
ALL 或 ALL PRIVILEGES：所有的权限名。

(3) 授予列权限时，<权限类型>的值只能指定为 SELECT、INSERT 和 UPDATE，同时权限后面需要加上列名列表 column-list。
grant insert(id, name) on test.priv_test to sam@‘localhost’;

(4) 系统·权限。
CREATE USER：表示授予用户可以创建和删除新用户的权限。
SHOW DATABASES：表示授予用户可以使用 SHOW DATABASES 语句查看所有已有的数据库的定义的权限。
FILE：表示授予用户可以使用 LOAD DATA INFILE 和 SELECT … INTO OUTFILE 语句访问服务器上的文件的权限。
PROCESS：表示授予用户可以使用 SHOW PROCESSLIST 语句查看服务器上的所有进程的权限。
RELOAD：表示授予用户可以使用 FLUSH 语句刷新服务器的各种缓存和日志的权限。
REPLICATION CLIENT：表示授予用户可以使用 SHOW MASTER STATUS 和 SHOW SLAVE STATUS 语句查看主从复制的状态信息的权限。
REPLICATION SLAVE：表示授予用户可以作为从服务器连接到主服务器进行复制的权限。
SHUTDOWN：表示授予用户可以使用 SHUTDOWN 语句关闭服务器的权限。
SUPER：表示授予用户可以执行一些高级操作，如设置全局变量，杀死其他用户的进程，启用或禁用日志等的权限。
USAGE：连接（登陆）权限，建立一个用户，就会自动授予其usage权限（默认授权）。

WITH GRANT OPTION：表示授予用户可以将自己的权限授权给其他用户的选项。
WITH ADMIN OPTION：表示授予用户可以将自己的权限授权给其他用户的选项，包括系统权限。
REQUIRE：表示授予用户可以使用安全连接（如SSL或X509）访问服务器的选项。
WITH MAX_QUERIES_PER_HOUR count：表示授予用户每小时最多可以执行的查询数。
WITH MAX_UPDATES_PER_HOUR count：表示授予用户每小时最多可以执行的更新数。
WITH MAX_CONNECTIONS_PER_HOUR count：表示授予用户每小时最多可以建立的连接数。
WITH MAX_USER_CONNECTIONS count：表示授予用户同时可以保持的最大连接数。
```

### 权限管理

```sql
show grants for root@'%'
show grants;
grant 权限列表 on 数据库名.表名/视图名 to '用户名@主机名'|public [with grant option];
grant EXECUTE on procedure/function 数据库名.存储过程名 to '用户名@主机名'|public [with grant option];
grant select(列名1，列名2), update(列名1) on 数据库名.表名 to 'user@localhost'|public [with grant option];
revoke 权限列表 on 数据库名.表名 from '用户名@主机名'|public [CASCADE|RESTRICT];

create role 角色名@主机名;
grant 权限列表 on 数据库名.表名 to 用户名@主机名;
grant 角色名@主机名 to 用户名@主机名;
revoke 角色名@主机名 from 用户名@主机名
```

## 流程控制

### 事务

```sql
START TRANSACTION; 
-- 一系列的SQL语句，可以包括查询、插入、更新和删除等操作
COMMIT|ROLLBACK;
```
设置事务隔离级别
```sql
set global|session transaction isolation level [READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE]
```

### 视图

```sql
CREATE VIEW view_name AS SELECT column1, column2, ... FROM table_name WHERE condition WITH CASCADED|LOCAL CHECK OPTION;
SELECT * FROM view_name;
DROP VIEW view_name;
```

### 变量

```sql
SET @var_name = value; 用户自定义变量的作用域只在当前的MySQL连接中有效
SET @@global.var_name = value; 全局变量的作用域在整个 MySQL 实例中有效
DECLARE var_name data_type DEFAULT value; 局部变量的作用域只在存储过程或函数中有效。
select @var_name; 查看变量
select ... into 变量1,变量2
```

### 临时表

```sql
CREATE TEMPORARY TABLE 表名(...)
CREATE TEMPORARY TABLE 表名 SELECT ...
drop table 表名
```

### 存储过程

```sql
CREATE PROCEDURE procedure_name ([IN|OUT|INOUT] 变量名 变量类型)
BEGIN
	-- 存储过程主体
	[leave procedure_name;]
END;
CALL procedure_name(参数1, 参数2);

IF condition THEN
    statements;
ELSEIF condition THEN
    statements;
ELSE
    statements;
END IF;

CASE
    WHEN condition THEN statement_list
    [WHEN condition THEN statement_list] ...
    [ELSE statement_list]
END CASE;

WHILE 条件语句 DO
	CONTINUE;
	BREAK;
    -- 循环体
END WHILE;

REPEAT
	CONTINUE;
	BREAK;
   -- 循环体
UNTIL 条件语句
END REPEAT;

my_loop: LOOP
    -- 循环体
	iterate my_loop;
	leave my_loop;
END LOOP my_loop;

call 存储过程名称(存储过程的参数列表)
```

### 异常处理

```sql
SIGNAL [SQLSTATE '45000'] SET MESSAGE_TEXT = 'message'; --抛出异常
DECLARE handler_name EXIT|CONTINUE HANDLER FOR SQLEXCEPTION|NOT FOUND|SQLWARNING|SQLSTATE '45000'
BEGIN
	--异常处理语句
	[ROLLBACK;]
END;
```

### 游标

```sql
--游标例子：
DELIMITER //
CREATE PROCEDURE list_employees()
BEGIN
    DECLARE done INT DEFAULT FALSE;
    DECLARE emp_name VARCHAR(255);
    DECLARE emp_salary FLOAT;
    DECLARE cur CURSOR FOR SELECT name, salary FROM employees; --声明游标
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE; --处理异常
    OPEN cur;
    read_loop: LOOP
        FETCH cur INTO emp_name, emp_salary; --fetch游标next
        IF done THEN
            LEAVE read_loop;
        END IF;
        SELECT CONCAT(emp_name, ' earns ', emp_salary);
    END LOOP;
    CLOSE cur;  --关闭游标
END//
DELIMITER ;
```

### 存储函数

```sql
CREATE FUNCTION function_name (参数 参数类型, 参数 参数类型)
	RETURNS 数据类型 [no sql, read sql data]
	BEGIN
    		-- Function body
		RETURN result;
	END;
SELECT 函数名(参数);
```

### 触发器

```sql
CREATE TRIGGER 触发器名
BEFORE|AFTER INSERT|UPDATE|DELETE ON 表名
FOR EACH ROW
BEGIN
	--NEW表表示修改后的表
	--OLD表示修改前的表
	[ROLLBACK;]
END;

select 存储函数名称 (存储函数参数列表);
```

### MYSQL函数

```sql
concat(字符串1, 字符串2, ...)
lower(str)
upper(str)
lpad(str, n, pad) --用字符串pad对str左填充达到n的长度
rpad(str, n, pad)
trim(str) --去除头尾空格
substring(str, start, len) --索引值从1开始不是0

ceil(x) --向上取整
floor(x) 
mod(x,y) --模
rand()  --随机数(0,1)
round(x, y) --对x四舍五入保留y位小数

curdate() --当前date
curtime()
now() --当前datetime
day|month|year(date) --获取年份
day(date)
date_add(date, interval n year|month|day) --当前日期加n年|月|日
datediff(date1, date2) --相差天数d1-d2

if(value, t, f) --value位true则返回t否则返回f
ifnull(t, f)
case when 表达式 then 值 when 表达式 then 值 else 值 end; 
```

### 事件

```sql
SET GLOBAL event_scheduler = ON; --开启事件调度器

CREATE EVENT [IF NOT EXISTS] event_name 
ON SCHEDULE schedule 
  [ON COMPLETION [NOT] PRESERVE]   --是否循环执行
  [ENABLE | DISABLE | DISABLE ON SLAVE]  --事件是否启动
  DO event_body;  
schedule: 
  AT timestamp [+ INTERVAL interval]| EVERY interval 
  [STARTS timestamp [+ INTERVAL interval]] 
  [ENDS timestamp [+ INTERVAL interval]] 
-- INTERVAL中包含的时间单位如下:
  {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE | WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE | DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}

show events;

ALTER EVENT event_name ON SCHEDULE EVERY 2 MINUTE;
ALTER EVENT event_name DO insert into events_list values('event', now());
ALTER EVENT event_name DISABLE|ENABLE;
ALTER EVENT event_name RENAME TO test_event;
```

## 程序举例

### 银行转账存储过程

```sql
CREATE PROCEDURE transfer(
  IN from_account VARCHAR(50),
  IN to_account VARCHAR(50),
  IN amount DECIMAL(10, 2)
)
BEGIN
  DECLARE from_balance DECIMAL(10, 2);
  DECLARE to_balance DECIMAL(10, 2);

  -- 检查参数是否合法
  IF from_account = to_account THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: same account';
    LEAVE transfer;
  END IF;

  IF amount <= 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: invalid amount';
    LEAVE transfer;
  END IF;

  DECLARE EXIT HANDLER FOR SQLEXCEPTION ROLLBACK;

  START TRANSACTION;

  -- 查询转出账户是否存在
  SELECT COUNT(*) INTO @count FROM accounts WHERE account_name = from_account;

  -- 如果不存在，抛出异常
  IF @count = 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: account not found';
    LEAVE transfer;
  END IF;

  -- 查询转出账户的余额
  SELECT balance INTO from_balance FROM accounts WHERE account_name = from_account FOR UPDATE;

  -- 检查余额是否足够转账
  IF from_balance < amount THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: insufficient balance';
    LEAVE transfer;
  END IF;

  -- 更新转出账户的余额
  UPDATE accounts SET balance = balance - amount WHERE account_name = from_account;

  -- 查询转入账户是否存在
  SELECT COUNT(*) INTO @count FROM accounts WHERE account_name = to_account;

  -- 如果不存在，抛出异常
  IF @count = 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: account not found';
    LEAVE transfer;
  END IF;

  -- 查询转入账户的余额
  SELECT balance INTO to_balance FROM accounts WHERE account_name = to_account FOR UPDATE;

  -- 更新转入账户的余额
  UPDATE accounts SET balance = balance + amount WHERE account_name = to_account;

  COMMIT;
  SELECT 'Success' AS result;
END;
```
避免死锁
```sql
CREATE PROCEDURE transfer(
  IN from_account VARCHAR(50),
  IN to_account VARCHAR(50),
  IN amount DECIMAL(10, 2)
)
BEGIN
  DECLARE from_balance DECIMAL(10, 2);
  DECLARE to_balance DECIMAL(10, 2);

  -- 检查参数是否合法
  IF from_account = to_account THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: same account';
    LEAVE transfer;
  END IF;

  IF amount <= 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: invalid amount';
    LEAVE transfer;
  END IF;

  DECLARE EXIT HANDLER FOR SQLEXCEPTION ROLLBACK;

  START TRANSACTION;

  -- 按照字母顺序访问账户
  IF from_account < to_account THEN
    -- 查询和更新转出账户
    SELECT balance INTO from_balance FROM accounts WHERE account_name = from_account FOR UPDATE;
    IF from_balance < amount THEN
      SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: insufficient balance';
      ROLLBACK;
      LEAVE transfer;
    END IF;
    UPDATE accounts SET balance = balance - amount WHERE account_name = from_account;

    -- 查询和更新转入账户
    SELECT balance INTO to_balance FROM accounts WHERE account_name = to_account FOR UPDATE;
    UPDATE accounts SET balance = balance + amount WHERE account_name = to_account;
  ELSE
    -- 查询和更新转入账户
    SELECT balance INTO to_balance FROM accounts WHERE account_name = to_account FOR UPDATE;
    UPDATE accounts SET balance = balance + amount WHERE account_name = to_account;

    -- 查询和更新转出账户
    SELECT balance INTO from_balance FROM accounts WHERE account_name = from_account FOR UPDATE;
    IF from_balance < amount THEN
      SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Error: insufficient balance';
      ROLLBACK;
      LEAVE transfer;
    END IF;
    UPDATE accounts SET balance = balance - amount WHERE account_name = from_account;
  END IF;

  COMMIT;
  SELECT 'Success' AS result;
END;
```

### 加法器存储函数
```sql
CREATE FUNCTION calculate_total(num1 INT, num2 INT)
RETURNS INT
BEGIN
    DECLARE total INT;
    SET total = num1 + num2;
    RETURN total;
END;

SELECT calculate_total(5, 10);
```

### 清理垃圾事件
每天凌晨1点删除orders表中30天前的数据
```sql
CREATE EVENT IF NOT EXISTS clean_old_orders
ON SCHEDULE EVERY 1 DAY
STARTS (TIMESTAMP(CURRENT_DATE) + INTERVAL 1 DAY + INTERVAL 1 HOUR)
DO
  DELETE FROM orders WHERE order_date < DATE_SUB(NOW(), INTERVAL 30 DAY);
```

### 删除保护触发器
如果这是最后一个管理员，那么触发器将回滚事务，防止删除操作。
```sql
CREATE TRIGGER prevent_last_admin_deletion
BEFORE DELETE
ON admin
FOR EACH ROW
BEGIN
   DECLARE admin_count INT;
   SELECT COUNT(*) INTO admin_count FROM admin;
   IF admin_count = 1 THEN
      SIGNAL SQLSTATE '45000'
         SET MESSAGE_TEXT = 'Cannot delete the last admin.';
   END IF;
END;
```


## 锁

### 全局锁 
```sql
FLUSH TABLES WITH WRITE/READ LOCK; --全局锁（锁数据库）：
UNLOCK TABLES;
```

### 表锁
```sql
-- DROP/ALTER/TRUNCATE TABLE过程中自动加表锁
LOCK TABLES table_name WRITE/READ; --表锁
UNLOCK TABLES;
```

### 行级锁
```sql
-- INSERT/UPDATE/DELETE会自动加排他锁（FOR UPDATE）
SELECT ... FOR UPDATE/LOCK IN SHARE MODE
-- InnoDB 在行锁很多时有可能自动升级表锁
```

## 数据库备份

### sql模式

```bash
mysqldump -u username -p dbname [tbname ... ] [--all-databases] [-d 只备份结构]> filename.sql
mysqldump --single-transaction -u username -p  # 保证导出数据一致性
```

### txt模式

修改my.ini或/etc/my.cnf:  secure-file-priv=

```sql
SELECT * FROM runoob_tbl
INTO OUTFILE '/tmp/runoob.txt';

LOAD DATA LOCAL INFILE 'dump.txt'
INTO TABLE mytbl;
```

### csv模式

```sql
SELECT * FROM runoob_tbl
INTO OUTFILE '/tmp/runoob.txt'
FIELDS TERMINATED BY ','
ENCLOSED BY '\"'
LINES TERMINATED BY '\\r\\n';
```

### 日志

```sql
-- 首先查看日志是否开启了记录
-- 查看日志功能设置状态
show variables like 'general_log'; 
-- 打开日志记录功能
set global general_log=on; 
-- 关闭日志记录功能
set global general_log=off; 

-- 查看当前日志输出类型：table / file ，可根据需要具体设置
show variables like 'log_output';
-- 设置日志输出至table
set global log_output='table';
-- 日志输出至table模式，查看日志记录
SELECT * from mysql.general_log ORDER BY event_time DESC limit 0,5;
-- 设置日志输出至file
set global log_output='file'; 
-- 查看日志输出文件的保存路径
show variables like 'general_log_file';
-- 修改日志输出文件的保存路径
set global general_log_file='tmp/general.log'; 
-- 日志输出至table模式，清空日志记录
truncate table mysql.general_log;
-- 日志输出至file模式，查看日志记录
cat /tmp/general.log
```

