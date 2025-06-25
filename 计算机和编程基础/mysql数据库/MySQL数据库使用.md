# MySQL数据库使用

## 一 ,MySQL数据库操作

#### （1），mysql的启用和停止

```
net start mysql92 #启用
```

```
net stop mysql92 #停用
```

#### （2），mysql的登录和退出

```
mysql -u root -p # 登录
```

```
quit; # 退出
```



## 二 ，数据库和sql

#### （1），数据库的创建

```
create database shop;
```

#### （2），表单的创建

```
CREATE TABLE product (
    product_id CHAR(4) NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    product_type VARCHAR(32) NOT NULL,
    sale_price INT,
    purchase_price INT,
    regist_data DATE,
    PRIMARY KEY (product_id)
);

```

#### （3），向表中插入数据

```
START TRANSACTION;

INSERT INTO product VALUES ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
INSERT INTO product VALUES ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
INSERT INTO product VALUES ('0003', '运动T恤', '衣服', 4000, 2800, NULL);
INSERT INTO product VALUES ('0004', '菜刀', '厨房用具', 3000, 2800, NULL);
INSERT INTO product VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-09-20');
INSERT INTO product VALUES ('0006', '叉子', '厨房用具', 500, NULL, '2009-01-15');
INSERT INTO product VALUES ('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
INSERT INTO product VALUES ('0008', '圆珠笔', '办公用品', 100, NULL, '2009-11-11');

COMMIT;
```

#### （4），表的删除

```
drop table product;
```

#### （5），表的更新

```
# 添加列
alter table product add column product_name_pinyin varchar(100);
# 删除列
alter table product drop column product_name_pinyin;
```

#### （6），表名的更新

```
rename table roduct to product;
```



## 三 , 查询基础

### 1，select语句基础

#### （1）列的查询

```
# 查询指定的列
select product_id, product_name, purchase_price
from product;
```

```
# 查询表中所有的列
select *
from product；
```

#### （2）为列设置别名

```
# 别名为中文需要使用"
SELECT 
product_id AS "商品编号",
product_name AS "商品名称",
purchase_price AS "进货单价"
FROM product;
```

#### （3）书写常数

```
# 字符串常数，数字常数，日期常数
SELECT '商品' AS string, 38 AS number, '2009-02-24' AS date,
product_id, product_name
FROM product;
```

#### （4）筛选掉结果中重复的行

```
SELECT DISTINCT product_type, regist_data
FROM product;
```

#### （5）指定查询的条件

```
SELECT product_name
FROM product
WHERE product_type = '衣服';
```

#### （6）书写注释

```
# 单行注释
SELECT DISTINCT product_id, purchase_price
-- 本select语句会从结果中删除重复行
FROM product;

# 双行注释
SELECT DISTINCT product_id, purchase_price 
/* 本select语句，
  会从结果中删除重复行。*/
FROM product;
```



### 2，算术运算符和比较运算符

#### （1）算术运算符

```
# + _ * / 可以使用括号
#包含null的运算，结果肯定是null
SELECT product_name, sale_price,
sale_price * 2 AS "sale_price_*2"
FROM product；
```

#### （2）比较运算符

```
# <> 不等于
SELECT product_name, product_type
FROM product
WHERE sale_price <> 500;

SELECT product_name, product_type, regist_data
FROM product
WHERE regist_data < '2009-09-27';
```

#### （3）对字符串使用不等号

```
CREATE TABLE chars(
chr CHAR(3) NOT NULL,
PRIMARY KEY (chr));

START TRANSACTION;
INSERT INTO chars VALUES ('1');
INSERT INTO chars VALUES ('2');
INSERT INTO chars VALUES ('3');
INSERT INTO chars VALUES ('10');
INSERT INTO chars VALUES ('11');
INSERT INTO chars VALUES ('222');;
COMMIT;

SELECT chr 
FROM chars 
WHERE chr > '2';

# 输出为 222 和 3 按字符串在字典中出现的顺序进行排列
```

#### （4）不能对null使用比较运算符

```
SELECT product_name, purchase_price
FROM product 
WHERE purchase_price IS NULL;

SELECT product_name, purchase_price
FROM product
WHERE purchase_price IS NOT NULL;
```



### 3，逻辑运算符

#### （1）not运算符

```
SELECT product_name, product_type, sale_price
FROM product 
WHERE NOT sale_price >=1000;
```

#### （2）and运算符和or运算符

```
SELECT product_name, purchase_price
FROM product
WHERE product_type = '厨房用具' AND sale_price >= 3000;

SELECT product_name, purchase_price
FROM product
WHERE product_type = '厨房用具' OR sale_price >= 3000;
```

#### （3）通过括号强化处理

```
SELECT product_name, product_type, regist_data
FROM product 
WHERE product_type = '办公用品'
AND (regist_data = '2009-09-11' OR regist_data = '2009-09-20');
```



## 四，聚合与排序

### 1，对表进行聚合查询

#### （1）计算表中数据的行数

```
# count: 计算表中的行数
# sum ： 计算数据的合计值
# avg ：计算平均值
# max ：计算最大值
#min ： 计算最小值

# 计算全部数据
SELECT COUNT(*)
FROM product;

SELECT COUNT(purchase_price)
FROM product;
```

#### （2）计算合计值

```
SELECT SUM(sale_price)
FROM product;

SELECT SUM(sale_price), SUM(purchase_price)
FROM product;
```

#### （3）计算平均值

```
SELECT AVG(sale_price)
FROM product;

SELECT AVG(sale_price), AVG(purchase_price)
FROM product;
```

#### （4）计算最大值和最小值

```
SELECT MAX(sale_price), MIN(purchase_price)
FROM product;

SELECT MAX(regist_data), MIN(regist_data)
FROM product;
```

#### （5）使用聚合函数统计唯一数据

```
SELECT COUNT(DISTINCT product_type)
FROM product;
```



### 2，对表进行分组

#### （1）group by 子句

```
SELECT product_type, COUNT(*)
FROM product 
GROUP BY product_type;
```

#### （2）聚合键中含有null的情况

```
# null也会被作为一组特殊数据

SELECT purchase_price, COUNT(*)
FROM product
GROUP BY purchase_price;
```

#### （3）使用where语句时group by的执行结果

```
# 先根据where子句指定条件进行过滤，然后再汇总处理

SELECT purchase_price, COUNT(*)
FROM product
WHERE product_type = '衣服'
GROUP BY purchase_price;
```

注意：在使用聚合函数和group by 子句中，select子句只能存在 ：常数，聚合函数，聚合键（group by 中指定的列名）



### 3，为聚合结果指定条件

#### （1）having子句

```
SELECT product_type, COUNT(*)
FROM product 
GROUP BY product_type
having COUNT(*) = 2;

SELECT product_type, AVG(sale_price)
FROM product
GROUP BY product_type
HAVING AVG(sale_price) >= 2500;

SELECT product_type, COUNT(*)
FROM product
GROUP BY product_type
HAVING product_type = '衣服';
```

注意：having子句能够使用的三种要素和上面一样

#### （2）having子句和where子句

```
where子句指定行所对应的条件
having子句指定组所对应的条件
```



### 4，对查询结果进行排序

#### （1）order by子句

```
SELECT product_id, product_name, sale_price, purchase_price
FROM product 
ORDER BY sale_price;

# 使用降序
SELECT product_id, product_name, sale_price, purchase_price
FROM product
ORDER BY sale_price DESC;
```

#### （2）指定多个排序键

```
SELECT product_id, product_name, sale_price, purchase_price
FROM product
ORDER BY sale_price, product_id;
```

#### （3）null的顺序

```
# 使用含有null的列作为排序键时，null会在开头或末尾汇总显示

SELECT product_id, product_name, sale_price,purchase_price
FROM product
ORDER BY purchase_price;
```

#### （4）在排序键中使用显示的别名

```
SELECT product_id AS id, product_name, sale_price AS sp, purchase_price
FROM product
ORDER BY sp, id;
```

#### （5）order  by子句中可以使用的列

```
# 可以使用存在于表中，但不包含在select子句中的列
SELECT product_name,sale_price, purchase_price
FROM product
ORDER BY product_id;

# 也可以使用聚合函数
SELECT product_type, COUNT(*)
FROM product
GROUP BY product_type
ORDER BY COUNT(*)
```



## 五，数据更新

### 1，数据的插入

#### （1）insert的基本用法

```
INSERT INTO product(
product_id, product_name, product_type, sale_price, purchase_price, regist_data)
VALUES(
'0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
# 字符型和日期型需要用单引号括起来
```

#### （2）多行插入

```
INSERT INTO product VALUES
('0002', '打孔器', '办公用品', 500, 320, '2009-09-11')
('0003', '运动T恤', '衣服', 4000, 2800, NULL);
						
```

#### （3）列清单的省略

```
INSERY INTO product VALUES(
'0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20')
```

#### （4）插入NULL

```
#直接在values子句中写入null
#不能向设置了not null约束的列中插入null
```

 #### （5）设置和插入默认值

```
# 设置默认值
CREATE TABLE product(
sale_price INT DEFAULT 0，
略);
```

```
# 显示插入
INSERT TABLE product VALUES(
'0007', '擦菜板', '厨房用具', DEFAULT, 790, '2009-04-28');
```

```
# 隐式插入
# 直接省略设置了默认值的列
# 如果省略了设置默认值的列，该列的值就会被设定为NULL
INSERT TABLE product VALUES(
'0007', '擦菜板', '厨房用具', 790, '2009-04-28');，
```

#### （6）从其他表中复制数据

```
# 创建复制表
CREATE TABLE productcopy
(product_id CHAR(4) NOT NULL,
product_name VARCHAR(100) NOT NULL,
product_type VARCHAR(32) NOT NULL,
sale_price INT,
purchase_price INT,
regist_data DATE,
PRIMARY KEY (product_id));
```

```
# 复制
INSERT INTO productcopy
SELECT *
FROM product;
```

```
# 插入其它表的数据合计值

# 创建表
CREATE TABLE productcopy1(
product_type VARCHAR(32) NOT NULL,
sum_sale_price INT,
sum_purchase_price INT,
PRIMARY KEY (product_type));

# 插入
INSERT INTO productcopy1(
product_type, sum_sale_price, sum_purchase_price)
SELECT product_type, SUM(sale_price), SUM(purchase_price)
FROM product
GROUP BY product_type;
```



### 2，数据的删除

#### （1）DELETE语句的基本语法

```
# DELETE语句会留下表，而删除表中的全部内容
# DROP TABLE 语句可以将表完全删除

DELETE FROM productcopy;
```

```
# 指定删除对象的delete语句

DELETE FROM productcopy
WHERE sale_price >= 4000;												
```

#### （2）DROP TABLE语句

```
DROP TABLE productcopy;
```



### 3,数据的更新

#### （1）UPDATE语句的基本语法

```
UPDATE productcopy1
SET sum_purchase_price = 5000;
```

```
# 指定条件的update语句

UPDATE productcopy1
SET sum_sale_price = sum_sale_price / 10
WHERE product_type = '厨房用具';
```

```
# 使用null进行更新
#只有未设置not null约束和主键约束的列才可以清空为null

UPDATE productcopy1
SET sum_purchase_price = NULL
WHERE product_type = '厨房用具';
```

#### （2）多列更新

```
UPDATE productcopy1
SET sum_sale_price = sum_sale_price * 10,
sum_purchase_price = sum_purchase_price / 2
WHERE product_type = '衣服';
```



### 4,事务

#### （1）什么是事务

事务就是需要在同一个处理单元中执行的一系列更新处理的集合。

#### （2）创建事务

```
START TRANSACTION;
 UPDATE product
 SET sale_price = sale_price - 1000
 WHERE product_name = '运动T恤';
 
 UPDATE product
 SET sale_price = sale_price + 1000
 WHERE product_name = 'T恤衫';
 
COMMIT;
```

commit 是提交处理的结束命令，一旦提交，就无法恢复到之前的状态了。

rollback 取消事务处理的结束命令， 相当于放弃回滚。

#### （3）ACID特性

（A）原子性：事务结束时，其中包含的全部更新处理要么全部执行要么全部不执行。

（C）一致性：事务中包含的处理要满足数据库提前设置的约束，如主键约束或者not null约束。

（I）隔离性：保证不同事务之间互不干扰的特性。

（D）持久性：指在事务（无论提交还是回滚）结束后，DBMS能够保证该时间点的数据状态会被保存的特性。

dbms：数据库管理系统



## 六，复杂查询

### 1，视图

视图相当于一张零时表，表中存储的是实际数据，而视图中保存的是从表中取出数据所使用的select语句

（不储存数据，指储存查询逻辑）

使用视图时可以通过select语句频繁调用，视图中的数据会随着原表的变化自动更新

#### （1）创建和使用视图

```
# 创建视图

CREATE VIEW productsum (product_type, cnt_product)
AS
SELECT product_type, COUNT(*)
  FROM product
  GROUP BY product_type; 
```

```
# 使用视图

SELECT product_type, cnt_product
  FROM productsum;
```

```
# 创建多重视图

CREATE VIEW productsumjim (product_type, cnt_product)
AS
SELECT product_type, cnt_product
  FROM productsum
  WHERE product_type = '办公用品';
  
# 查询

SELECT product_type, cnt_product
  FROM productsumjim;
```

#### （2）视图的限制

定义视图时不能对视图使用 ORDER BY子句，但查询时可以使用

```
SELECT product_type, cnt_product
  FROM productsum
 ORDER BY cnt_product;
```

只可以对简单视图进行更新

- 基于单个表。
- 不包含聚合函数（如 SUM、AVG）。
- 不包含 GROUP BY、HAVING、DISTINCT。
- 不包含子查询。

#### （3）删除视图

```
DROP VIEW productsumjim;
```



### 2，子查询

  #### （1）子查询和视图

简单来说子查询就是一张一次性视图

```
SELECT product_type, cnt_product
  FROM (SELECT product_type, COUNT(*) AS cnt_product
    FROM product
    GROUP BY product_type)
  AS productsum;
  
 # 子查询会先执行内层from子句的select语句，然后才会执行外层的select语句
 
 # 理论上子查询的层数没有限制，因此子查询的from子句中还可以继续使用子查询
```

#### （2）标量子查询

标量就是单一的意思，标量子查询的一个特殊限制就是必须而且只能返回1行1列的结果

```
SELECT AVG(sale_price)
  FROM product;
```

```
# 在where语句中不能使用聚合函数否则就会报错，但是可以使用子查询

#使用子查询的语句会先从子查询开始执行

SELECT product_id, product_name, sale_price
  FROM product
  WHERE sale_price > (SELECT AVG(sale_price)
                      FROM product);
```

#### （3）标量子查询的书写位置

标量子查询的书写位置通常在任何可以使用单一值的位置都能使用

```
SELECT product_id, product_name, sale_price,
  (SELECT AVG(sale_price)
  FROM product) AS avg_price
FROM product;
  
SELECT product_type, AVG(sale_price)
  FROM product
  GROUP BY product_type
  HAVING AVG(sale_price) > (SELECT AVG(sale_price)
                            FROM product);  
```

#### （4）标量子查询的注意事项

标量子查询绝对不能返回多行结果比如和group by一起使用，

如果标量子查询产生了多行结果那它就是普通的子查询了，

因此不能被用在=  <> 等需要单一输入值的地方，也不能用在select等子句中

```
SELECT product_id, product_name, sale_price,
  (SELECT AVG(sale_price)
  FROM product
  GROUP BY product_type) AS avg_price
FROM product;
  
 # 返回后报错，因为标量子查询返回了多行数据
```



### 3，关联子查询

#### （1）关联子查询的特点

1. **子查询不能独立执行**，它必须使用主查询的数据。
2. **子查询会被主查询的每一行触发**，每一行都会执行一次子查询。
3. **子查询的结果随主查询的不同行而变化**，它不像普通子查询那样只执行一次。

#### （2）关联子查询的作用

**可以根据每一行的具体情况动态计算数据**，适用于**需要按某些条件细分计算的场景**，如：

- **查找某个类别中高于平均价格的商品**
- **查找每个部门工资高于部门均值的员工**
- **查找某个用户购买次数超过该城市平均购买次数的情况**

```
# 查找每个组内商品价格高于该组平均价格的商品

SELECT product_type, product_name,sale_price
  FROM product AS p1 
  WHERE sale_price > (SELECT AVG(sale_price)
                        FROM product AS p2
                      WHERE p1.product_type = p2.product_type -- 让子查询只计算当前p1商品类别的数据
                        GROUP BY product_type);
  
```



## 七，函数，谓词和CASE表达式

### 1，各种各样的函数

所谓函数就是输入某一值得到相应输出结果的功能，输入值称为参数，输出值称为返回值

函数大致可以分为以下几种：

- 算术函数
- 字符串函数
- 日期函数
- 转换函数
- 聚合函数

#### （1）算术函数

```
# 创建一个samplemath表

CREATE TABLE samplemath(
m NUMERIC (10,3), -- 表示一个数值类型，其中总共可以有 10 位数字，其中 3 位是小数位
n INT,
p INT);

# 插入数据

START TRANSACTION;

INSERT INTO samplemath VALUES (500, 0, NULL);
INSERT INTO samplemath VALUES (-180, 0, NULL);
INSERT INTO samplemath VALUES (NULL, NULL, NULL);
INSERT INTO samplemath VALUES (NULL, 7, 3);
INSERT INTO samplemath VALUES (NULL, 5, 2);
INSERT INTO samplemath VALUES (NULL, 4, NULL);
INSERT INTO samplemath VALUES (8, NULL, 3);

COMMIT;
```

ABS函数：求绝对值

```
SELECT m,
  ABS(m) AS abs_col
FROM samplemath;
```

MOD函数：求余

```
# 求 n/p 的余数

SELECT n, p, MOD(n,p) AS mod_col
FROM samplemath;
```

ROUND函数：四舍五入

```
# 对m列数值进行n列位数的四舍五入处理

SELECT m, n, ROUND(m, n) AS round_col
FROM samplemath;
```

#### （2）字符串函数

```
# 创建一个samplestr表

CREATE TABLE samplestr(
str1 VARCHAR(40),
str2 VARCHAR(40),
str3 VARCHAR(40));

# 添加数据

START TRANSACTION;

INSERT INTO samplestr VALUES ('opx', 'rt', NULL);
INSERT INTO samplestr VALUES ('abc', 'def', NULL);
INSERT INTO samplestr VALUES ('aaa', 'bbb', 'ccc');
INSERT INTO samplestr VALUES ('asesf', NULL, 'sfhs');

COMMIT;
```

CONCAT：拼接函数

```
SELECT str1, str2, CONCAT(str1, str2) AS str_concat
FROM samplestr;
```

LENGTH：查询字符串长度函数

```
SELECT str1, LENGTH(str1) AS len_str
  FROM samplestr;
```

LOWER：小写转换函数

```
SELECT str1, LOWER(str1) AS low_str
  FROM samplestr
  WHERE str1 IN ('aaa', 'abc'); -- IN 运算符用于检查某个值是否在指定的集合中，可以指定多个值,特定检索
```

UPPER：大写转换函数

```
select str1, UPPER(str1) AS up_str
  FROM samplestr
  WHERE str1 IN ('ABC', 'AAA');
```

REPLACE：字符串替换函数

```
SELECT str1, REPLACE(str1,'a','QWQ') AS rep_str
  FROM samplestr;
```

SUBSTRING：字符串截取函数

```
SELECT str1, SUBSTRING(str1, 2, 3) AS sub_str -- 从第二个字符开始截取三个字符
  FROM samplestr;
```

#### （3）日期函数

CURRENT_DATE：当前日期

```
SELECT CURRENT_DATE;
```

CURRENT_TIME：当前时间

```
SELECT CURRENT_TIME;
```

CURRENT_TIMESTAMP：当前日期和时间

```
SELECT CURRENT_TIMESTAMP;
```

EXTRACT：截取日期函数

```
SELECT CURRENT_TIMESTAMP,
  EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
  EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
  EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
  EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
  EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS minute,
  EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second;
```

#### （4）转换函数

CAST：类型转换

```
# 将字符串转换为数值类型

SELECT CAST('0001' AS SIGNED INT) AS int_col;
```

```
# 将字符串转换为日期类型

SELECT CAST('2009-12-14' AS DATE) AS date_col;
```

COALESCE：将null转换为其它值

该函数会返回可变参数中左侧开始第一个不是null的值

```
SELECT COALESCE(NULL, 1) AS col_1,
        COALESCE(NULL, 'test', NULL) AS col_2,
        COALESCE(NULL, NULL, '2009-11-01') AS col_3;
```



### 2，谓词

谓词可以看作是 **“判断某条数据是否满足某个条件的规则”**，它的结果要么是 **真（TRUE）**，要么是 **假（FALSE）**，有时候也可能是 **未知（UNKNOWN）**（比如涉及 `NULL` 时）

```
CREATE TABLE samplelike(
strcool VARCHAR(6) NOT NULL,
PRIMARY KEY (strcool));
```

```
START TRANSACTION;

INSERT INTO samplelike VALUES ('abcddd');
INSERT INTO samplelike VALUES ('dddabc');
INSERT INTO samplelike VALUES ('abdddc');
INSERT INTO samplelike VALUES ('abcdd');
INSERT INTO samplelike VALUES ('ddabc');
INSERT INTO samplelike VALUES ('abddc');

COMMIT;
```



#### （1）LIKE谓词：字符串的部分一致查询

%：代表“0个字符以上的任意字符串”

_：一个下划线代表“任意一个字符”

```
# 前方一致
SELECT *
  FROM samplelike
WHERE strcool like 'ddd%';
```

```
# 中间一致
SELECT *
  FROM samplelike
WHERE strcool like '%ddd%';
```

```
# 后方一致
SELECT *
  FROM samplelike
WHERE strcool like '%ddd';
```

```
SELECT *
  FROM samplelike
WHERE strcool like 'abc__';
```

#### （2）BETWEEN谓词：范围查询

```
# 选取销售单价为100到1000的商品

SELECT product_name, sale_price
  FROM product
WHERE sale_price BETWEEN 100 AND 1000;
```

结果会包含100和1000两个零界值

```
# 如果不想包含两个临界值

SELECT product_name, sale_price
  FROM product
WHERE sale_price > 100 AND sale_price < 1000;
```

#### （3）is null ，is not null：判断是否为null

略

#### （4）IN谓词：OR的简便用法

```
# 使用or

SELECT product_name, sale_price
  FROM product
WHERE sale_price > 100 AND sale_price < 1000;
```

```
# 使用in

SELECT product_name, purchase_price
  FROM product
WHERE purchase_price IN (320, 500, 5000);
```

```
# 使用not in

SELECT product_name, purchase_price
  FROM product
WHERE purchase_price NOT IN (320, 500, 5000);
```

注意：使用 in 和 not in 是无法选取出null 数据的

#### （5）使用子查询作为IN谓词的参数

IN谓词具有其它谓词所没有的用法，那就是可以使用子查询作为其参数

```
SELECT product_name, sale_price
  FROM product
WHERE product_type IN (SELECT product_type
                            FROM product
                          WHERE product_type = '厨房用具');
```

```
SELECT product_name, sale_price
  FROM product
WHERE product_type NOT IN (SELECT product_type
                            FROM product
                          WHERE product_type = '厨房用具');
```

优点：如果在select语句中使用了子查询，那么即使数据发生了变更，还可以继续使用同样的select语句

#### （6）EXISTS谓词：通常和关联子查询一起使用



### 3，CASE表达式

`CASE` 语句类似于 **“如果...那么...否则...”** 的逻辑：

- 如果 `product_type = '衣服'`，就返回 `'a:衣服'`
- 如果 `product_type = '办公用品'`，就返回 `'b:办公用品'`
- 如果 `product_type = '厨房用具'`，就返回 `'c:厨房用具'`
- 如果 `product_type` 不是这三种情况，就返回 `NULL`

```
SELECT product_name,
  CASE 
    WHEN product_type = '衣服' THEN CONCAT('a:', product_type)
    WHEN product_type = '办公用品' THEN CONCAT('b:', product_type)
    WHEN product_type = '厨房用具' THEN CONCAT('c:', product_type)
    ELSE NULL
  END AS abc_product_type
FROM product;
```

CONCAT()：字符串拼接函数



CASE表达式的便利之处在于它是一个表达式，而表达式可以书写在任何位置

按类别计算总销售额:

```
SELECT SUM(CASE WHEN product_type = '衣服'
              THEN sale_price ELSE 0 END) AS sum_price_clothes,
       SUM(CASE WHEN product_type = '厨房用具'
               THEN sale_price ELSE 0 END) AS sum_price_kitchen,
       SUM(CASE WHEN product_type = '办公用品'
               THEN sale_price ELSE 0 END) AS sum_price_office
FROM product;
```



## 八，集合运算

### 1，表的加减法

#### （1）UNION：表的加法

```
# 默认剔除重复项

SELECT product_id, product_name
  FROM product
UNION
SELECT product_id, product_name
  FROM product2;
```

#### （2）EXCEPT：表的减法（mysql中不支持）

```
# product表减去product2表中记录之后的剩余部分、

SELECT product_id, product_name
  FROM product
EXCEPT
SELECT product_id, product_name
  FROM product2;
```

#### （3）集合运算的注意事项

- 作为运算对象的记录的列数必须相同
- 作为运算对象的记录中列的类型必须一致
- 可以使用任意select语句，但order by 子句只能在最后使用一次

```
SELECT product_id, product_name
  FROM product
WHERE product_type = '厨房用具'
UNION
SELECT product_id, product_name
  FROM product2
WHERE product_type = '厨房用具'
ORDER BY product_id;
```

#### （4）ALL选项：包含重复行的集合运算

```
SELECT product_id, product_name
  FROM product
UNION ALL
SELECT product_id, product_name
  FROM product2;
```

#### （5）INRERSECT：选取表中的公共部分（mysql中不支持）

```
SELECT product_id, product_name
  FROM product
INTERSECT
SELECT product_id, product_name
  FROM product2;
```



### 2，联结

集合运算以行方向为单位操作，联结是以列为单位进行操作

#### （1）INNER JOIN：内联结

```
CREATE TABLE shopproduct (
    shop_id VARCHAR(10) NOT NULL,           
    shop_name VARCHAR(50) NOT NULL,         
    product_id VARCHAR(10) NOT NULL,        
    product_name VARCHAR(50) NOT NULL,      
    sale_price INT NOT NULL,                     
    PRIMARY KEY (shop_id, product_id)       -- 联合主键 -> PRIMARY KEY (shop_id, product_id)
);
```

```
START TRANSACTION;
INSERT INTO shopproduct VALUES ('000A', '东京', '0001', 'T恤衫', 1000);
INSERT INTO shopproduct VALUES ('000A', '东京', '0002', '打孔器', 500);
INSERT INTO shopproduct VALUES ('000A', '东京', '0003', '运动T恤', 4000);
INSERT INTO shopproduct VALUES ('000B', '名古屋', '0002', '打孔器', 500);
INSERT INTO shopproduct VALUES ('000B', '名古屋', '0003', '运动T恤', 4000);
INSERT INTO shopproduct VALUES ('000C', '大阪', '0006', '叉子', 500);
INSERT INTO shopproduct VALUES ('000C', '大阪', '0007', '擦菜板', 880);
INSERT INTO shopproduct VALUES ('000D', '福冈', '0001', 'T恤衫', 1000);
COMMIT;
```

```
SELECT sp.shop_id, sp.shop_name, sp.product_id, p.product_name, p.sale_price
  FROM shopproduct as sp INNER JOIN product as p
    ON sp.product_id = p.product_id;
```

- 使用关键字INNER JOIN 就可以将两张表联结在一起的，sp和p分别是这两张表的别名
- ON之后指定两张表联结所使用的列（联结键），必不可少，必需写在from和where之间
- 在select子句中，像sp.shop_id这样使用<表的别名>.<列名>的形式来制定列

```
# 结合where子句使用

SELECT sp.shop_id, sp.shop_name, sp.product_id, p.product_name, p.sale_price
  FROM shopproduct AS sp INNER JOIN product AS p
  ON sp.product_id = p.product_id
WHERE sp.shop_id = '000A';

```

#### （2）OUTER JOIN：外联结

内联结只能选取出同时存在于两张表中的数据，

外联结，需要通过left或right设置主表，最终的结果会包含主表中的全部数据

```
SELECT sp.shop_id, sp.shop_name, sp.product_id, p.product_name, p.sale_price
  FROM shopproduct AS sp RIGHT OUTER JOIN product AS p
  ON sp.product_id = p.product_id;
```



## 九，SQL高级处理

### 1，窗口函数

#### （1）PARTITION BY语句

```
SELECT product_name, product_type, sale_price,
  RANK() OVER (PARTITION BY product_type
                             ORDER BY sale_price) AS ranking
FROM product;
```

- PARTITION BY：在横向上对表进行分组，但不合并
- ORDER BY：决定了纵向排序
- 通过 PARTITION BY分组后的记录集合称为窗口，窗口代表范围

#### （2）不使用PARTITION BY语句

不使用PARTITION BY语句，就变成了全部商品的排序

```
SELECT product_name, product_type, sale_price,
  RANK() OVER (ORDER BY sale_price) AS ranking
FROM product;
```

#### （3）专用窗口函数的种类

```
SELECT product_name, product_type, sale_price,
    RANK() OVER (ORDER BY sale_price) AS ranking,
    DENSE_RANK() OVER (ORDER BY sale_price) AS dense_ranking,
    ROW_NUMBER() OVER (ORDER BY sale_price) AS row_num
  FROM product;
```

简单来说就是：有三条记录排在1位时

- rank()：1,1,1,4
- dense_rank：1,1,1,2
- row_rank：1,2,3,4

#### （4）在SELECT子句之外，使用窗口函数是没有意义的

#### （5）作为窗口函数使用的聚合函数

```
SELECT product_id, product_name, sale_price,
    SUM(sale_price) OVER (ORDER BY product_id) AS current_sum
  FROM product;
```

```
SELECT product_id, product_name, sale_price,
    AVG(sale_price) OVER (ORDER BY product_id) AS current_avg
  FROM product;
```

#### （6）计算移动平均

```
SELECT product_id, product_name, sale_price,
    AVG(sale_price) OVER (ORDER BY product_id
                                ROWS 2 PRECEDING) AS moving_avg
  FROM product;
```

ROWS 2 PRECEDING:当前行和之前的两行一起计算平均值

可以使用关键词 FOLLOWING （之后）替换PRECEDING

```
# 将当前记录的前后行作为汇总对象

SELECT product_id, product_name, sale_price,
    AVG(sale_price) OVER (ORDER BY product_id
                                ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg
  FROM product;
```



### 2，GROUPING运算符

#### （1）ROLLUP：同时得出合计和小计

```
SELECT product_type, SUM(sale_price) AS sum_price
    FROM product
  GROUP BY product_type WITH ROLLUP;
```

#### （2）GROUPING函数：在使用rollup分组中产生null时返回1

```
SELECT GROUPING(product_type) AS product_type,
          GROUPING(regist_data) AS regist_data, SUM(sale_price) AS sum_price
  FROM product
GROUP BY product_type, regist_data WITH ROLLUP;
```

#### （3）CUBE：用数据搭积木

略

#### （4）GROUPING SETS：取得期望的积木

略



## 十，通过应用程序连接数据库

