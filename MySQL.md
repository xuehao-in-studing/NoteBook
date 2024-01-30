# MySQL

<style>
    table {
    margin-left: auto;
    margin-right: auto;
    }
</style>

`mysql`是一种典型的关系型数据库，什么是关系型数据库？  
就是可以通过A表的外键来关联到B表的主键，从而获取B表中的数据。也即多个表可以通过键来建立联系。  

# 一、关系模型
关系数据库建立在关系模型之上，关系模型本质上是若干的二维表格，可以通过行号和列号唯一确定一个数据。  

表的一行称为`记录`  
表的一列称为`字段`  

## 1.1 主键
`主键`：通过主键这个字段，可以唯一地确定一条记录。  
主键可以只有一个，也可以有多个，有多个时就是`联合主键`。对于联合主键来说，可以有一个键重复，但不能都重复。  

主键的要求：**不可以用具有实际意义的字段作为主键**，因为主键一般是不可以修改的，例如学生证号，身份证号，这些虽然一般意义上不修改，但是都不可以。  
可行：  
- 自增int
- GUID

## 1.2 外键
`外键`：用来关联两个表的键，也就是说两个表同时都有这个键，可以互相关联。  

这种关联关系既可以是一对一，也可以是一对多、多对多的关系，例如一个学生对应一个studentid(一对一);一个学生对应一个班级，但是一个班级可以对应多个学生(一对多);但一个学生可以对应多个老师，一个老师也可以对应多个学生(多对多);。  

## 1.3 索引
索引：用来**提高数据库的查询效率**，不存在索引也可以进行正确的查询。  

添加索引：  
- 根据单个字段添加`ALTER TABLE 表名 ADD INDEX 索引名 (字段名);`
- 根据多个字段添加`ALTER TABLE 表名 ADD INDEX 索引名 (字段1，字段2...);`

删除索引：  
`ALTER TABLE students DROP INDEX index_name;`

唯一索引：  
当面对身份证这类不会重复的字段时，查询时又常常需要，所以可以为这些字段添加唯一索引。  
- `ALTER TABLE 表名 ADD UNIQUE INDEX 索引名 (字段名);`创建唯一索引。
- `ALTER TABLE 表名 ADD CONSTRAINT 索引名 UNIQUE (name);`只创建唯一约束，不创建索引。  

注：也可以通过workbench图形界面进行更改。  


# 二、查询
## 2.1 基本查询
`select * from table`，其中*指代的是查询所有的列，可以选择特定的列名去查询。  
**语句的结束要用分号结束**。  
## 2.2 条件查询
`select 列名 from table where condition1...`  
可以查询符合某些特定条件的数据，也可以用`AND`,`OR`等来进行复合条件查询。  
## 2.3 排序
在查询一些表时，我们希望根据一些键来排序，例如成绩排序。  
`OREDER BY key`就可以根据key来进行排序，降序`DESC`，升序`ASC`。  

## 2.4 分页查询
`LIMIT 3 OFFSET 6`表示每页显示三条数据，跳过6条，也相当于查询第二页的内容。  
可以简写：`LIMIT 6,3`。  

## 2.5 聚合查询
聚合查询是利用sql自带的聚合函数来进行数学统计。  
- COUNT 计算总数，可以根据条件计算
- SUM	计算某一列的合计值，该列必须为数值类型
- AVG	计算某一列的平均值，该列必须为数值类型
- MAX	计算某一列的最大值 
- MIN	计算某一列的最小值  

**分组聚合**
如果想同时查询每个班级的学生数量，每次更改`WHERE`的条件吗，显然不行。  
可以通过`GROUP BY`进行分组查询。可以同时选中多个字段进行`GROUP BY`。  

## 2.6 多表查询
多表查询就是多个表笛卡尔积的结果。但一般两个表笛卡尔积之后就会表的非常大(一个M*K和一个N*K的表返回的就是M*N*K的表)，因此**慎重**使用多表查询。  
语法如下：  
`SELECT * FROM students, classes;`

## 2.7 缩写
可以用`students as s`或者`students s`缩写表示`student`这个表。

## 2.8 连接查询
连接有多个连接:  
- `LEFT JOIN ON A.column=B.column` 左连接，选择左表当中存在的记录，并且符合ON后面的condition
- `RIGHT JOIN ON A.column=B.column` 右表当中存在的且符合condition的记录
- `INNER JOIN ON A.column=B.column` 两张表中同时存在且符合condition的记录

当想对多个表进行查询时，连接往往非常有用。可以A左连接B之后再右连接C，类似于**链式操作**。

# 三、修改
## 3.1 插入
语句：  
`INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);`  
可以同时插入多条语句，也可以

## 3.2 更新
语句：
`UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;`  
可以同时更新多个记录，但要注意，如果没有使用`WHERE`子句进行限制，那么将会将整个表进行更新。  

## 3.3 删除
语句：`DELETE FROM <表名> WHERE ...;`  
删除和更新一样，可以根据条件进行删除，如果没有限制，删除整个表。  


# 四、管理MySQL
## 4.1 SQL命令
|   命令    |   作用    |  
|:-----:|:-----:|  
|mysql -u root -p|登录SQL| 
|SHOW DATABASES|显示所有数据库|  
|CREATE DATABASE test|创建名为test的数据库|  
|DROP DATABASE test|删除名为test的数据库|  
|USE test|切换数据库|  
|SHOW tables|查看所有表|
|DESC tables|详细描述该表|
|DROP TABLE students|删除名为test的表|  
|EXIT|退出MySQL(断开连接)|  
|ALTER TABLE table_name ADD COLUMN column_name VARCHAR(10) NOT NULL;|增添一列|
|ALTER TABLE table_name DROP COLUMN column_name;|删除一列|
|ALTER TABLE table_name CHANGE COLUMN column_name new_name VARCHAR(20) NOT NULL;|更改列|


## 4.2 实用SQL语句

**插入或替换**  
语句：
`REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);`  
希望插入，如果已存在就替换。  
根据是否存在相同的主键选择替换还是插入。  

**插入或更新**  
语句：
`INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;`  
希望插入，如果已存在就更新。  


# 五、事务
在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。例如，一个转账操作，必须在转出方转出后，转入方要收到，这几条语句是不可以分割的。  

这种把多条语句作为一个整体进行操作的功能，被称为数据库事务。  

对于单条的`SELECT`、`INSERT`、`UPDATE`等语句能够执行是因为他们开启了默认提交，也采用了默认的隔离方式。  

**隔离级别**   

对于**两个并发执行的事务**，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。
|Isolation Level|	脏读（Dirty Read）|	不可重复读（Non Repeatable Read）|	幻读（Phantom Read）|
|:-----:|:-----:|:-----:|:-----:|
|Read Uncommitted|Yes|Yes|Yes|
|Read Committed	|-	|Yes|Yes|
|Repeatable Read|-	|-|Yes|
|Serializable|-	|-	|-|

## 5.1 Read Uncommitted
隔离级别最低的事务级别。一个事务可能会读到另一个事务更新但未提交的数据，如果另一个事务回滚，那么就会造成脏读（Dirty Read）。  

例如：  
|时刻	|事务A	|事务B|
|:-----:|:-----:|:-----:|
|1	|SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;	|SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;|
|2	|BEGIN;	|BEGIN;|
|3	|UPDATE students SET name = 'Bob' WHERE id = 1;|	|
|4	|	|SELECT * FROM students WHERE id = 1;|
|5	|ROLLBACK;|	|
|6	|	|SELECT * FROM students WHERE id = 1;|
|7	|	|COMMIT;|


## 5.2 Read Committed
在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

如下表所示：
|时刻|	事务A|	事务B|
|:-----:|:-----:|:-----:|
|1	|SET TRANSACTION ISOLATION LEVEL READ COMMITTED;|	SET TRANSACTION ISOLATION LEVEL READ COMMITTED;|
|2|	BEGIN;|	BEGIN;|
|3|		|SELECT * FROM students WHERE id = 1; -- Alice|
|4|	UPDATE students SET name = 'Bob' WHERE id = 1;|	|
|5|	COMMIT;|  |
|6|		|SELECT * FROM students WHERE id = 1; -- Bob|
|7|		|COMMIT;|

## 5.3 Repeatable Read
在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。  

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。  

例如：  
|时刻|	事务A|	事务B|
|:-----:|:-----:|:-----:|
|1	|SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;|	SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;|
|2	|BEGIN;|	BEGIN;|
|3	|	|SELECT * FROM students WHERE id = 99; -- empty|
|4	|INSERT INTO students (id, name) VALUES (99, 'Bob');|	|
|5	|COMMIT;|	|
|6	|	|SELECT * FROM students WHERE id = 99; -- empty|
|7	|	|UPDATE students SET name = 'Alice' WHERE id = 99; -- 1 row affected|
|8	|	|SELECT * FROM students WHERE id = 99; -- Alice|
|9	|	|COMMIT;|

## 5.4 Serializable
最高的隔离级别，但是事务的进行是串行的，也就是只能在前面的事务执行完毕之后才能够执行后面的事务(类比栈)。  

这样虽然安全性高，不会出现脏读、幻读等问题，但是读取时间效率低，因此一般不采用这种隔离方式
