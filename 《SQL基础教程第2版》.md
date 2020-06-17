# 第0~1章
创建数据库
CREATE DATABASE shop;

登录输入以下指令：
psql.exe –U postgres
输入数据库密码，显示出“ postgres=# ”，意味着连接成功了

输入  \q  按下回车键，结束psql

登录同时连接指定数据库：选项“ -d shop ”是指定“数据库 shop ”的意思
psql.exe –U postgres –d shop

数据库（DB）
管理数据库的计算机系统称为数据库管理系统（DBMS）
关系数据库管理系统（RDBMS）

SQL根据操作目的可以分为DDL、DML和DCL
DDL（Data Definition Language，数据定义语言）：
    CREATE  ： 创建数据库和表等对象
    DROP    ： 删除数据库和表等对象
    ALTER   ： 修改数据库和表等对象的结构
DML（Data Manipulation Language，数据操纵语言）：
    SELECT  ： 查询表中的数据
    INSERT  ： 向表中插入新数据
    UPDATE  ： 更新表中的数据
    DELETE  ： 删除表中的数据
DCL（Data Control Language，数据控制语言）：
    COMMIT  ： 确认对数据库中的数据进行的变更
    ROLLBACK： 取消对数据库中的数据进行的变更
    GRANT   ： 赋予用户操作权限
    REVOKE  ： 取消用户的操作权限

SQL语句要以分号（ ; ）结尾，输入指令不用加分号 ;

创建表
CREATE TABLE Product
(product_id CHAR(4)  NOT NULL,
product_name VARCHAR(100) NOT NULL,
product_type VARCHAR(32)  NOT NULL,
sale_price INTEGER,
purchase_price INTEGER,
regist_date DATE,
PRIMARY KEY (product_id));

表、字段等的命名规则：只能使用半角英文字母、数字、下划线（_）作为数据库、表和
列的名称，名称必须以半角英文字母开头，不能重复

数据库类型：  INTEGER  CHAR   VARCHAR   DATE

删除表 DROP TABLE Product;

添加列 ALTER TABLE Product ADD COLUMN product_name_pinyin VARCHAR(100);

开启事务 begin transaction;
提交     commit;

插入数据
INSERT INTO Product VALUES ('0001', 'T 恤衫 ', ' 衣服 ', 1000, 500, '2009-09-20');


# 第2章 查询基础
所有包含 NULL 的计算，结果都是 NULL
不能对 NULL 进行比较运算 判断是否为 NULL 使用： IS NULL ，  IS NOT NULL
使用  DISTINCT 删除重复数据


# 第3章 聚合与排序
常用的聚合函数（所谓聚合，就是将多行汇总为一行）
COUNT ： 计算表中的记录数（行数）（只有count(*)会包含null的行，非*时不包含null行）
SUM ： 计算表中数值列中数据的合计值
AVG ： 计算表中数值列中数据的平均值（需要包含null的行可以使用 COALESCE 函数）
MAX ： 求出表中任意列中数据的最大值
MIN ： 求出表中任意列中数据的最小值

不只在 COUNT 函数的参数中可以使用 DISTINCT ，所有的聚合函数都可以使用 DISTINCT

MAX / MIN 函数几乎适用于所有数据类型的列。 SUM / AVG 函数只适用于数值类型的列

使用 GROUP BY 子句可以像切蛋糕那样将表分割

使用聚合函数和 GROUP BY 子句时需要注意以下4点。
① 只能写在 SELECT 子句之中
② GROUP BY 子句中不能使用 SELECT 子句中列的别名
③ GROUP BY 子句的聚合结果是无序的
④ WHERE 子句中不能使用聚合函数

GROUP BY 分组列中包含 NULL 时，在结果中会以“不确定”行（空行）的形式表现出来，
也就是所有null值的行会作为一组特定数据进行分组，即划分为单独的一组

子句的书写顺序
1.  SELECT 子句 → 2.  FROM 子句 → 3.  WHERE 子句 → 4.  GROUP BY 子句 →
5.  HAVING 子句 → 6.  ORDER BY 子句

DBMS内部运行 SELECT 语句的执行顺序：
1.FROM → 2.WHERE → 3.GROUP BY → 4.HAVING → 5.SELECT → 6.ORDER BY

只有 SELECT 子句和 HAVING 子句以及 ORDER BY 子句中能够使用 COUNT 等聚合函数

WHERE 子句 = 指定行所对应的条件
HAVING 子句 = 指定组所对应的条件
HAVING 子句用来指定过滤分组的条件

HAVING 子句和包含 GROUP BY 子句时的 SELECT 子句一样，能够使用的要素有相同的限制，只能使用如下 3 种要素：
常数   聚合函数   GROUP BY 子句中指定的列名（即聚合键）

使用 ORDER BY 子句对查询结果进行排序，排序健中包含 NULL 时，会在开头或末尾进行汇总
ORDER BY 子句中可以使用 SELECT 子句中未出现的列或者聚合函数

ORDER BY 排序键中包含 NULL 时，会在开头或末尾进行汇总，具体是开头还是末尾根据DBMS不同


# 第4章 数据更新
INSERT INTO < 表名 > ( 列 1 ,  列 2 ,  列 3 ,  …… )  VALUES ( 值 1 ,  值 2 ,  值 3 ,  …… );
将列名和值用逗号隔开，分别括在 （） 内，这种形式称为清单
对表进行全列 INSERT 时，可以省略表名后的列清单
INSERT 语句中想给某一列赋予 NULL 值时，可以直接在 VALUES 子句的值清单中写入 NULL

oracle 不支持多行 insert 插入语法

插入默认值的方式：
在 VALUES 子句中指定 DEFAULT 关键字
在列清单和 VALUES 中省略设定了默认值的列
如果省略了没有设定默认值的列，该列的值就会被设定为 NULL 
如果省略的是设置了 NOT NULL 约束的列， INSERT 语句就会出错

将整个表全部删除，可以使用 DROP TABLE 语句
如果只想删除表中全部数据，需使用 DELETE 语句，语法 DELETE FROM < 表名 > [WHERE < 条件 >];
如果想删除部分数据行，只需在 WHERE 子句中书写对象数据的条件即可
DELETE 语句的删除对象并不是表或者列，而是记录（行）。
非标准的 TRUNCATE 只能删除表中的全部数据，不能使用 WHERE 删除部分数据，语法 TRUNCATE < 表名 >;

更新数据 UPDATE < 表名 > SET < 列名 > = < 表达式 > [WHERE < 条件 >];
更新多列时可以使用逗号分隔形式或清单形式（形式2非标准，某些DBMS不支持）：
形式1 UPDATE Product SET name = '新名称', price = 10 WHERE type = '服装';
形式2 UPDATE Product SET (name, price) = ('新名称', 10) WHERE type = '服装';


事务是需要在同一个处理单元中执行的一系列更新处理的集合
事务处理的终止指令包括 COMMIT （提交处理）和 ROLLBACK （取消处理）
开启事务 BEGIN TRANSACTION; START TRANSACTION; 或 默认自动开启

DBMS 的事务具有原子性（Atomicity）、一致性或完整性（Consistency）、隔离性
（Isolation）和持久性（Durability）四种特性。
通常将这四种特性的首字母结合起来，统称为 ACID 特性


# 第5章 复杂查询
视图和表的区别：表 保存的是实际的数据，而视图 保存的是 SELECT 语句（视图本身并不存储数据）
使用 CREATE VIEW 语句创建视图 语句中 AS 不能省略
CREATE VIEW  视图名称 (< 视图列名 1>, < 视图列名 2>,  …… ) AS <SELECT 语句 >
使用 DROP VIEW 语句删除视图

通过 SELECT 语句查询数据：
实际上就是从存储设备（硬盘）中读取数据，进行各种计算之后，再将结果返回给用户这样一个过程

从视图中读取数据（视图归根到底就是 SELECT 语句）：
视图会在内部执行该视图对应的 SELECT 语句并创建出一张临时表返回给用户

视图的优点有两点：
1、是由于视图无需保存数据，因此可以节省存储设备的容量
2、是可以将频繁使用的 SELECT 语句保存成视图，提高复用率

视图的限制：
① 定义视图时不能使用 ORDER BY 子句（仅个别DBMS支持）
② 不能对视图进行更新（insert、update）等操作（在满足特定条件时也可以，但还是不推荐）

子查询就是一次性视图（ SELECT 语句）， FROM 中的子查询必须有一个别名 AS可以省略

标量就是单一的意思，标量子查询就是返回单一值的子查询，必须而且只能返回 1 行 1 列的结果
能够使用常数或者列名的地方，无论是 SELECT 子句、 GROUP BY 子句、 HAVING 子句，还是ORDER BY 子句，几乎所有的地方都可以使用 标量子查询

SQL 是按照先内层子查询后外层查询的顺序来执行的

关联名称就是像 P1 、 P2 这样作为表别名的名称
关联名称的作用域存在一个有效范围的限制

子查询内部设定的关联名称，只能在该子查询内部使用，也就是“内部可以看到外部，而外部看不到内部”


# 第6章 函数、谓词、CASE
函数大致可以分为以下几种：
● 算术函数（用来进行数值计算的函数） ABS MOD ROUND
● 字符串函数（用来进行字符串操作的函数） || LENGTH LOWER REPLACE SUBSTRING UPPER
● 日期函数（用来进行日期操作的函数） CURRENT_DATE CURRENT_TIME CURRENT_TIMESTAMP EXTRACT
● 转换函数（用来转换数据类型和值的函数） CAST  COALESCE
● 聚合函数（用来进行数据聚合的函数） COUNT SUM AVG MAX MIN

CAST ——类型转换，语法： CAST (转换前的值 AS  想要转换的数据类型)
COALESCE ——将 NULL 转换为其他值

谓词就是返回值为真值的函数： = < > <> LIKE  BETWEEN  IN  NOT IN  EXISTS  NOT EXISTS

EXISTS 的作用就是“判断是否存在满足某种条件的记录” EXIST （存在）谓词的主语是“记录”
EXISTS 通常都会使用关联子查询作为参数

CASE 表达式的语法，结尾 END 不能省略，语法如下：
CASE WHEN < 求值表达式 > THEN < 表达式 >
WHEN < 求值表达式 > THEN < 表达式 >
WHEN < 求值表达式 > THEN < 表达式 >
...
ELSE < 表达式 >
END


# 第7章 集合运算
集合运算就是对满足同一规则的记录进行的加减等四则运算，集合运算符会除去重复的记录
集合运算的特征就是以行方向为单位进行操作，即这些集合运算会导致记录行数的增减

表的加法—— UNION（并集）
选取表中公共部分—— INTERSECT（交集）
记录的减法—— EXCEPT（差集）
交叉联结 CROSS JOIN （笛卡儿积）

集合运算的注意事项：
① 作为运算对象的记录的列数必须相同
② 作为运算对象的记录中列的类型必须一致
③ 可以使用任何 SELECT 语句，但 ORDER BY 子句只能在最后使用一次

联结（ JOIN ）运算，就是将其他表中的列添加过来，进行“添加列”的运算
需要掌握两种联结：内联结和外联结

所谓联结运算就是“以 A 中的列作为桥梁，将 B 中满足同样条件的列汇集到同一结果之中”

进行内联结时必须使用 ON 子句，并且要书写在 FROM 和 WHERE 之间
使用联结时 SELECT 子句中多表同时存在的列必须按照“ < 表的别名 > . < 列名 > ”的格式进行书写

外联结中使用 LEFT 、 RIGHT 来指定主表，最终的结果中会包含主表内所有的数据

交叉联结的集合运算符是 CROSS JOIN （笛卡儿积）
交叉联结是对两张表中的全部记录进行交叉组合，结果记录数通常是两张表中行数的乘积


# 第8章 SQL高级处理
窗口函数也称为 OLAP 函数（MySQL5.7尚未支持）
OLAP（OnLine Analytical Processing）意思是对数据库数据进行实时分析处理

窗口函数语法：
< 窗口函数 >  OVER ([ PARTITION BY < 列清单 >]
ORDER BY < 排序用列清单 >)

作为窗口函数的聚合函数： SUM 、 AVG 、 COUNT 、 MAX 、 MIN 
专用窗口函数： RANK 、 DENSE_RANK 、 ROW_NUMBER

PARTITION BY 在横向上对表进行分组，而 ORDER BY 决定了纵向排序的规则

通过 PARTITION BY 分组后的记录集合称为“窗口”（非“窗户”的意思，而是代表范围）
窗口函数兼具分组和排序两种功能

不指定 PARTITION BY 表示将整个表作为一个大的窗口也即单独就一个分组

原则上窗口函数只能在 SELECT 子句中使用

使用 ROWS （“行”）和 PRECEDING （“之前”）两个关键字，将框架指定为“截止到之前 ~ 行”
示例：“ ROWS 2 PRECEDING ”就是将框架指定为“截止到之前 2 行”

使用关键字 FOLLOWING （“之后”）替换 PRECEDING ，可以指定“截止到之后 ~ 行”

同时指定前1行及后1行：
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING

将聚合函数作为窗口函数使用时，会以当前记录为基准来决定汇总对象的记录

GROUPING 运算符包含 3 种： ROLLUP   CUBE   GROUPING SETS

ROLLUP（汇总）同时得出合计和小计（一次计算出不同聚合键组合的结果）

CUBE（立方体）是将 GROUP BY 子句中聚合键的“所有可能的组合”的汇总结果集中到一个结果中

GROUPING SETS 只会返回小计结果，使用场景比较少


# 第9章 通过应用程序连接数据库
系统（软件或程序）其实就是由应用和数据库组合而成的
应用程序世界和数据库世界需要通过“驱动”（driver）这座桥梁来连接

