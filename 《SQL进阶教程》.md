日本的省级行政单位有都、道、府、县，统称 都道府县
包含一都（东京都）、一道（北海道）、二府（京都府和大阪府）和诸多的县

1-1 CASE 表达式
-------------------------
-- 搜索 CASE 表达式 示例
CASE WHEN sex =  ' 1 ' THEN  ' 男 '
WHEN sex =  ' 2 ' THEN  ' 女 '
ELSE  ' 其他 ' END

注意事项
1：各分支返回的数据类型必须一致
2：不要忘了写 END
3：养成写 ELSE 子句的习惯

新手用 WHERE 子句进行条件分支，高手用 SELECT 子句进行条件分支
新手用 HAVING 子句进行条件分支，高手用 SELECT 子句进行条件分支


1. 在 GROUP BY 子句里使用 CASE 表达式，可以灵活地选择作为聚合的单位的编号或等级
2. 在聚合函数中使用 CASE 表达式，可以轻松地将行结构的数据转换成列结构的数据
3. 相反，聚合函数也可以嵌套进 CASE 表达式里使用
4. 相比依赖于具体数据库的函数， CASE 表达式有更强大的表达能力和更好的可移植性
5. 正因为 CASE 表达式是一种表达式而不是语句，才有了这诸多优点

1-2 自连接
-------------------------
排列 P 2 3 ＝ 6
组合 C 2 3 ＝ 3

1-3 三值逻辑和NULL 表达式
-------------------------
AND 的情况：? false ＞? unknown ＞? true
OR  的情况：? true  ＞? unknown ＞? false


1-4 HAVING 子句
-------------------------
简单地求平均值有一个缺点，那就是很容易受到离群值（outlier）的影响
能更准确反映出群体趋势的指标——众数（mode）、中位数（median）

1. 表不是文件，记录也没有顺序，所以 SQL 不进行排序。
2. SQL 不是面向过程语言，没有循环、条件分支、赋值操作。
3. SQL 通过不断生成子集来求得目标集合。
SQL 不像面向过程语言那样通过画流程图来思考问题，而是通过画集合的关系图来思考。
4.  GROUP BY 子句可以用来生成子集。
5.  WHERE 子句用来调查集合元素的性质，而 HAVING 子句用来调查集合本身的性质。


1-5 外连接的用法
-------------------------
左外连接（LEFT?OUTER?JOIN）  右外连接（RIGHT?OUTER?JOIN）
全外连接（FULL?OUTER?JOIN）

求两个集合的异或集方法：
1 (A UNION B) EXCEPT (A INTERSECT B) 
2 (A EXCEPT B) UNION (B EXCEPT A)
3 使用全外连接


1-6 用关联子查询比较行与行
-------------------------
自连接关联子查询

1. 作为面向集合语言，SQL 在比较多行数据时，不进行排序和循环。
2. SQL 的做法是添加比较对象数据的集合，通过关联子查询（或者自
连接）一行一行地偏移处理。如果选用的数据库支持窗口函数，也可以考
虑使用窗口函数。
3. 求累计值和移动平均值的基本思路是使用冯·诺依曼型递归集合。
4. 关联子查询的缺点是性能及代码可读性不好。



1-7 用 SQL 进行集合运算
-------------------------
INTERSECT 比 UNION 和 EXCEPT 优先级更高
1. 在集合运算方面，SQL 的标准化已包含交(INTERSECT)、并(UNION)、差(EXCEPT)集。
2. 如果集合运算符不指定 ALL 可选项，重复行会被排除掉，但会发生排序，所以性能方面不够好。
3. UNION 和 INTERSECT 都具有幂等性这一重要性质，而 EXCEPT 不具有幂等性。
4. 标准 SQL 没有关系除法的运算符，需要自己实现。
5. 判断两个集合是否相等时，可以通过幂等性或一一映射两种方法。
6. 使用 EXCEPT 可以很简单地求得补集。


1-8 EXISTS 谓词的用法
-------------------------
1. SQL 中的谓词指的是返回真值的函数。
2. EXISTS 与其他谓词不同，接受的参数是集合。
3. 因此 EXISTS 可以看成是一种高阶函数。
4. SQL 中没有与全称量词相当的谓词，可以使用 NOT EXISTS 代替。

1-9 用 SQL 处理数列
-------------------------
1. SQL 处理数据的方法有两种。
2. 第一种是把数据看成忽略了顺序的集合。
3. 第二种是把数据看成有序的集合，此时的基本方法如下。
　a. 首先用自连接生成起点和终点的组合
　b. 其次在子查询中描述内部的各个元素之间必须满足的关系
4. 要在 SQL 中表达全称量化时，需要将全称量化命题转换成存在量
化命题的否定形式，并使用 NOT EXISTS 谓词。这是因为 SQL 只实现了
谓词逻辑中的存在量词。

1-10 HAVING 子句又回来了
-------------------------
通过 GROUP BY 生成的子集有一个对应的名字，叫作划分（partition）


1-11 让 SQL 飞起来
-------------------------
如下命令在操作运算时会进行排序：
● ? GROUP BY 子句
● ? ORDER BY 子句
● ?聚合函数（ SUM 、 COUNT 、 AVG 、 MAX 、 MIN ）
● ? DISTINCT
● ?集合运算符（ UNION 、 INTERSECT 、 EXCEPT ）
● ?窗口函数（ RANK 、 ROW_NUMBER 等）

去重时使用 EXISTS 代替 DISTINCT，以 避免排序
在极值函数中使用索引（MAX/MIN）
能写在 WHERE 子句里的条件不要写在 HAVING 子句里
在 GROUP BY 子句和 ORDER BY 子句中使用索引
先进行连接再进行聚合


1-12 SQL 编程方法
-------------------------




关系模型的创始人 E.F. Codd
《大型数据库中关系存储的可推导性、冗余与一致性》
《大型共享数据库的关系模型》

R? ? （D1×D2×D3 · · · ×Dn）
关系 R 是定义域 D1, D2, …, Dn 的笛卡儿积的子集

在关系模型中，关系对关系运算符也是封闭的，即 SQL 中 SELECT 子句的输入输出都是表

关系模型是为摆脱地址指针而生的

声明式语言 SQL 和函数式语言 Lisp 

GROUP BY 和 PARTITION BY 具有分组功能，都可以根据指定的列为表分组
区别在于 GROUP BY 在分组之后会把每个分组聚合成一行数据，而 PARTITION BY 不改变行数量

SQL 的思维方式为面向集合的思维模式，而普通编程语言的思维方式为面向过程思维模式

找到让编程语言互相连通的关键点，加深对多种编程语言的理解是非常重要的

消除 NULL 的具体方法：
(1) 首先分析能不能设置默认值
(2) 仅在无论如何都无法设置默认值时允许使用 NULL

在 SQL 中，使用 GROUP BY 聚合之后，就不能再引用原表中除聚合键之外的列


