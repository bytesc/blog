# Sqlite3 常用命令


# Sqlite3 常用命令

## Sqlite 命令

```sql
.databases	-- 列出数据库的名称及其所依附的文件。
.exit	-- 退出 SQLite 提示符。
.help	
.output FILENAME	-- 发送输出到 FILENAME 文件。
.output stdout	-- 发送输出到屏幕。
.read FILENAME	-- 执行 FILENAME 文件中的 SQL。
.schema [TABLENAME]	-- 显示 CREATE 语句。
.show	-- 显示各种设置的当前值。
.stats ON|OFF	-- 开启或关闭统计。
.tables [PATTERN]	-- 列出匹配 PATTERN 的表的名称。
.timeout MS	-- 尝试打开锁定的表 MS 毫秒。
select * from sqlite_master -- sqlite_master表存储所有表的相关信息
```

## 数据类型
```txt

INT TINYINT SMALLINT MEDIUMINT BIGINT

CHARACTER(20) VARCHAR(255) NCHAR(55) NVARCHAR(100) TEXT CLOB

BLOB REAL DOUBLE FLOAT REAL

```

## 创建数据库
```bash
$ sqlite3 DatabaseName.db
```

## 打开数据库
```sql
.open tabaseName.db
```

## 备份

```sql
.output backup.sql 
.dump [TABLENAME]	-- 以 SQL 格式导出数据库。
.read backup.sql --恢复
```
