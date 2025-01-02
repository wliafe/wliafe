# 常用数据库命令

```sql
-- 展示所有数据库
show databases;
-- 使用数据库
use 数据库名称;
-- 展示当前数据库所有表
shwo tables;
-- 展示表结构
desc 表名称;
```

# MySQL数据库导入导出

## MySQL数据库导出数据和表结构

```bash
mysqldump -u 用户名 -p 密码 数据库名 > 数据库名.sql
```

## MySQL数据库导出表结构

```bash
mysqldump -u 用户名 -p 密码 -d 数据库名 > 数据库名.sql
```

## MySQL数据库导入.sql文件

```bash
mysql -u 用户名 -p 密码 数据库名 < 数据库名.sql
```

**数据库中命令方式导入**

```sql
-- 选择数据库
use 数据库名称;
-- 导入数据（注意sql文件的路径）
source sql文件路径;
```