# SQL 笔记

参考：mysql菜鸟教程

## 基础命令：

```sql
mysql -u root -p   // log in
CREATE DATABASE 数据库名;
use 数据库名;       // select a database
drop database <数据库名>;
SHOW DATABASES;
SHOW TABLES;
show columns from user;
```

## 数据类型：

包含：数值类型、日期时间类型、字符(串)类型

## 进阶命令(1):

```sql
create table table_name(column_name column_type);
drop table table_name;
insert into table_name (field1, field2,...fieldN) values (value1, value2,...valueN);
now()   //返回当前日期时间的函数

select语句：
select * from table_name
select field1, field2,...fieldN from table_name1, table_name2... table_nameN where condition1 [and[or]] condition2 ...
```

**where条件：**

下表中实例假定 A 为 10, B 为 20

| 操作符 | 描述                                                         | 实例                 |
| ------ | ------------------------------------------------------------ | -------------------- |
| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

**注意：判断等于只需要一个等号**

MySQL 的 WHERE 子句的字符串比较是不区分大小写的。 你可以使**用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写**的。 

```
SELECT * from runoob_tbl WHERE BINARY runoob_author='runoob.com'; 
SELECT * from runoob_tbl WHERE BINARY runoob_author='RUNOOB.COM'; 
//第一句指定了小写，第二句指定了大写
```

**update语句：**

```sql
UPDATE table_name SET field1=new-value1, field2=new-value2 [WHERE Clause]
eg: 
update runoob_tbl set runoob_title = '学习c++' where runoob_id=3; 
注释：将runoob_id等于3的记录，设置runoob_title字段为'学习c++'


```

