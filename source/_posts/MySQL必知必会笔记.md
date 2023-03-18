---
title: MySQL必知必会笔记
abbrlink: f5143bd9
date: 2023-03-10 15:22:40
tags:
categories:
---

SQL -- Structured Qurey Language - 结构化查询语言
- 是一种专门用来与数据库通信的语言。

<!-- more-->

### SQL 基础概念
---

**数据库**：保存有组织的数据的容器（通常是一个文件或一组文件）。
**表**：某种特定类型数据的结构化清单。
**模式**：关于数据库和表的布局及特性的信息。
**列**：表中的一个字段。所有表都是由一个或多个列组成的。
**数据类型**：所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据。
**行**：表中的一个记录。
**主键**：一列（或一组列），其值能够唯一区分表中每个行。

### MySQL 简介
---

MySQL是一种DBMS（数据库管理系统），即它是一种**数据库软件**。
MySQL工具
- mysql命令行应用程序
- MySQL Administrator
- MySQL Query Browser
  
MySQL Workbench 已经成为最新的图形交互式客户端软件。

### 使用MySQL
---
<a href="https://www.w3cschool.cn/mysql/mysql-install.html"><mark><b>mysql安装教程</b></mark>

连接到mysql，需要满足一下几点：
- 主机名，本地服务器为localhost
- 端口，默认值3306
- 一个合法的用户名
- 用户口令

**关键字**：作为mysql语言的组成部分的一个保留字。绝不要用关键字命令一个表或列。

#### **选择数据库**

USE crashcourse;

#### **了解数据库和表**

SHOW DATABASES;
SHOW TABLES;
SHOW COLLUMNS FROM customers;

### 检索数据
---

SELECT 语句

```sql
-- 1.检索单个列
SELECT prod_name FROM products;

-- 2.检索多个列
SELECT prod_id, prod_name, prod_price
FROM products;

-- 3.检索所有列
SELECT *
FROM products;

-- 4.检索不同的行
SELECT DISTINCT vend_id
FROM products;

-- 5.限制结果
SELECT prod_name
FROM products
LIMIT 5 OFFSET 5; --指示从行5开始的5行；从行0开始。

-- 6.使用完全限定的表名
SELECT products.prod_name
FROM products;
```

### 排序检索数据
---

**子句**：SQL语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。

ORDER BY子句

```sql
-- 1.排序数据
SELECT prod_name
FROM products
ORDER BY prod_name;

-- 2.按多个列排序
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price, prod_name;

-- 3.指定排序方向；DESC - 降序，ASC（ASCENDING） - 升序（默认）
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC, prod_name;

-- 4.ORDER BY & LIMIT 组合
SELECT prod_id, prod_price
FROM products
ORDER BY prod_price DESC
LIMIT 1;
```

{% note info %}
#### ORDER BY 子句位置
位于FROM子句之后，LIMIT子句之前。
{% endnote %}

### 过滤数据
---

WHERE 子句

{% note info %}
#### WHERE 子句位置
WHERE子句应位于ORDER BY子句之前，否则产生错误。
{% endnote %}

**WHERE 子句操作符**
<center>

| **操作符** | **说明**    |
|:-------:|:---------:|
| =       | 等于        |
| <>      | 不等于       |
| \!=     | 不等于       |
| <       | 小于        |
| <=      | 小于等于      |
| >       | 大于        |
| >=      | 大于等于      |
| BETWEEN | 在指定的两个值之间 |

</center>

```sql
-- 1.检查单个值
SELECT prod_name, prod_price
FROM products
WHERE prod_name = 'fuses';

SELECT prod_name, prod_price
FROM products
WHERE prod_price < 10;

SELECT prod_name, prod_price
FROM products
WHERE prod_price <= 10;

-- 2.不匹配检查
SELECT vend_id, prod_name
FROM products
WHERE vend_id <> 1003;

SELECT vend_id, prod_name
FROM products
WHERE vend_id != 1003;

-- 3.范围值检查
SELECT prod_name, prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;

-- 4.空值检查
SELECT cust_id
FROM customers
WHERE cust_email IS NULL;
```

### 数据过滤
---

**操作符**：用来联结或改变WHERE子句中的子句的关键字。也称为逻辑操作符。

**AND操作符**：用在WHERE子句中的关键字，用来指示检索满足所有给定条件的行。
```sql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;
```

**OR操作符**：WHERE子句中使用的关键字，用来表示检索匹配任一给定条件的行。
```sql
SELECT prod_price, prod_name
FROM products
WHERE vend_id = 1002 OR vend_id = 1003;
```

**计算次序**：WHERE可包含任意数目的AND和OR操作符，允许两者结合以进行复杂和高级的过滤。
```sql
SELECT prod_price, prod_name
FROM products
WHERE vend_id = 1002 OR vend_id = 1003 AND prod_price >= 10;
-- 由于AND操作符优先级高于OR操作符，所以这里会先计算 vend_id = 1003 AND prod_price >= 10

-- 正确表示可用圆括号括起来，既可以避免出错，又能清晰表达意图。
SELECT prod_price, prod_name
FROM products
WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
```

**IN操作符**：WHERE子句中用来指定要匹配值的清单的关键字，功能与OR相当。
```sql
SELECT prod_price, prod_name
FROM products
WHERE vend_id IN (1002, 1003);
```

**NOT操作符**：WHERE子句中用来否定后跟条件的关键字。
```sql
SELECT prod_price, prod_name
FROM products
WHERE vend_id NOT IN (1002, 1003)
ORDER BY prod_name;
```

### 用通配符进行过滤
---

**通配符**：用来匹配值的一部分的特殊字符。
**搜索模式**：由字面值、通配符或两者组合构成的搜索条件。

**百分号（%）通配符**
- %表示任何字符出现任意次数

```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE 'jet%';

-- 搜索模式‘%anvil%’表示匹配任何位置包含文本anvil的值，而不论它之前或之后出现什么字符。
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE '%anvil%';
```

**下划线（ _ ）通配符**
- _表示只匹配单个任意字符

```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE '_ ton anvil';
```

### 用正则表达式进行搜索
---

```sql
-- 1.基本字符匹配
SELECT prod_name
FROM products
WHERE prod_name REGEXP '.000'
ORDER BY prod_name;
-- .是正则表达式语言中一个特殊字符，它表示匹配任意一个字符
```

{% note primary %}
#### LIKE 与 REGEXP

```sql
SELECT prod_name
FROM products
WHERE prod_name LIKE '1000'
ORDER BY prod_name;

SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name;
```
#### 区别：
LIKE匹配整个列值，REGEXP匹配部分（或完整）列值；

默认匹配不区分大小写，如果需要区分，可使用BINARY关键字；
`WHERE prod_name REGEXP BINARY 'JetPack .000'`
{% endnote %}

```sql
-- 2.进行OR匹配
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000'
ORDER BY prod_name;

-- 3.匹配几个字符之一
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[123] Ton'
ORDER BY prod_name;

-- 4.匹配范围
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[1-5] Ton'
ORDER BY prod_name;

-- 5.匹配特殊字符
SELECT vend_name
FROM vendors
WHERE vend_name REGEXP '\\.'
ORDER BY vend_name;
-- mysql使用双斜杠（\\）进行转义
```

**匹配字符类**
| **Character Class Name** | **Meaning**                              |
|:------------------------:|-----------------------------------------|
| alnum                    | Alphanumeric characters                   |
| alpha                    | Alphabetic characters                     |
| blank                    | Whitespace characters                     |
| cntrl                    | Control characters                        |
| digit                    | Digit characters                          |
| graph                    | Graphic characters                        |
| lower                    | Lowercase alphabetic characters           |
| print                    | Graphic or space characters               |
| punct                    | Punctuation characters                    |
| space                    | Space, tab, newline, and carriage return  |
| upper                    | Uppercase alphabetic characters           |
| xdigit                   | Hexadecimal digit characters              |

**匹配多个实例**
<center>重复元字符</center>

| **元字符** | **说明** |
|:-----------:|--------------------------------|
| *       | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。                                                                |
| +       | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。                                                  |
| ?       | 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 或 "does" 。? 等价于 {0,1}。                                                     |
| {n}     | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。                                             |
| {n,}    | n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。         |
| {n,m}   | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。  |

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
ORDER BY prod_name;

SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'
ORDER BY prod_name;

SELECT prod_name
FROM products
WHERE prod_name REGEXP '[0-9][0-9][0-9][0-9]'
ORDER BY prod_name;
```

**定位符**
| **元字符** | **说明** |
|:-------:|:------:|
| ^       | 文本的开始  |
| $       | 文本的结尾  |
| [[:<:]] | 词的开始   |
| [[:>:]] | 词的结尾   |

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '^[0-9\\.]'
ORDER BY prod_name;
```

{% note default %}
#### 简单的正则表达式测试
```sql
SELECT 'hello' REGEXP '[0-9]';
-- 返回0表示没有匹配，反之，匹配；
```
{% endnote %}

### 创建计算字段
---

**字段**：基本上与列的意思相同，经常互换使用，不过数据库列一般称为列，而术语字段通常用在计算字段的连接上。
**拼接**：将值联结到一起构成单个值。
```sql
SELECT Concat(vend_name, '(', vend_country, ')')
FROM vendors
ORDER BY vend_name;
-- Concat()拼接串，至少包含一个指定的串，每个串之间用逗号分隔
```

**别名**：是一个字段或值的替换名。
```sql
SELECT Concat(RTrim(vend_name), '(', RTrim(vend_country), ')') AS vend_title
FROM vendors
ORDER BY vend_name;
-- RTrim删除数据右侧多余空格来整理数据
```

**执行算术计算**
```sql
SELECT prod_id, quantity, item_price, quantity*item_price AS expended_price
FROM orderitems
WHERE order_num = 20005;
```

### 使用数据处理函数
---

{% note warning %}
**函数没有SQL的可移植性强**
如果决定使用函数，应保证做好代码注释，以便以后你或者他人能确切地知道所编写的SQL代码含义。
{% endnote %}

```sql
-- 1.文本处理函数
SELECT vend_name, Upper(vend_name) AS vend_name_upcase
FROM vendors
ORDER BY vend_name;
-- Upper() 将文本转换为大写

-- 2.日期和时间处理函数
SELECT cust_id, order_num
FROM orders
WHERE Year(order_date) = 2005 AND Month(order_date) = 9;
-- Year() 从日期中返回年份，Month() 从日期中返回月份

-- 3.数值处理函数
-- Abs()/Cos()/Exp()/Mod()/Pi()/Rand()/Sin()/Sqrt()/Tan()...
```

### 汇总数据
---

**聚集函数**：运行在行组上，计算和返回单个值的函数。
```sql
-- 1.AVG() 函数
SELECT AVG(prod_price) AS avg_price
FROM products;
-- AVG() 返回均值。忽略列值为NULL的行。

-- 2.COUNT() 函数
SELECT COUNT(*) AS num_cust
FROM customers;
-- COUNT(*) 计算所有行，包括NULL值行；COUNT(column)计算特定列中非NULL值的行。

SELECT COUNT(cust_email) AS num_cust
FROM customers;
-- 邮件地址非NULL值的只有三行，总共是五行。

-- 3.MAX() 函数
-- 返回指定列中的最大值。忽略列值为NULL的行。
SELECT MAX(prod_price) AS max_price
FROM products;

-- 4.MIN() 函数
-- 返回指定列中的最小值。忽略列值为NULL的行。
SELECT MIN(prod_price) AS min_price
FROM products;

-- 5.SUM() 函数
-- 返回指定列值的和。
SELECT SUM(quantity) AS item_ordered
FROM orderitems
WHERE order_num = 20005;

-- 6.聚集不同值
-- 默认ALL,包含所有行；指定DISTINCT,只包含不同值
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM products
WHERE vend_id = 1003;

-- 7.组合聚集函数
SELECT COUNT(*) AS num_items,
    MIN(prod_price) AS price_min,
    MAX(prod_price) AS price_max,
    AVG(prod_price) AS price_avg
FROM products;
```

### 分组数据
---

```sql
-- 1.数据分组
SELECT COUNT(*) AS num_prods
FROM products
WHERE vend_id = 1003;

-- 2.创建分组
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id;

-- 3.过滤分组
SELECT cust_id, COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```

{% note info%}
#### HAVING & WHERE 差别
WHERE在数据分组前过滤，HAVING在数据分组后进行过滤。
{% endnote %}

```sql
-- 组合使用HAVING & WHERE 子句
-- 列出具有2个以上、价格为10以上的产品的供应商
SELECT vend_id, COUNT(*) AS num_prods
FROM products
WHERE prod_price >= 10
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```

### 使用子查询
---

**查询**：任何SQL语句都是查询。但此术语一般指SELECT语句。
**子查询**：嵌套在其他查询中的查询。
```sql
-- 1.利用子查询进行过滤
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));

-- 2.作为计算字段使用子查询
SELECT cust_name, cust_state, (SELECT COUNT(*)
                               FROM orders
                               WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;
```

**相关子查询**：涉及外部查询的子查询。

### 联结表
---

**外键**：外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。
**可伸缩性**：能够适应不断增加的工作量而不失败。设计良好的数据库或应用程序称之为*可伸缩性好*。
**联结**：联结是一种机制，用一条SELECT语句关联多表，称之为联结。
```sql
-- 1.创建联结
SELECT vend_name, prod_name, prod_price
FROM vendors, products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name, prod_name;
```

**笛卡儿积**：由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行数将是第一个表中的行数乘以第二个表中的行数。
```sql
-- 2.WHERE子句的重要性
SELECT vend_name, prod_name, prod_price
FROM vendors, products
ORDER BY vend_name, prod_name;
```

**内部联结**：又称等值联结，它基于两个表之间的相等测试。
```sql
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
ON vendors.vend_id = products.vend_id;
```
```sql
-- 3.联结多个表
SELECT cust_name, cust_contact
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
    AND orderitems.order_num = orders.order_num
    AND prod_id = 'TNT2'; 
-- 与之前使用子查询得到的结果一样
```

### 创建高级联结
---

```sql
-- 1.使用表别名
SELECT cust_name, cust_contact
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
    AND oi.order_num = o.order_num
    AND prod_id = 'TNT2'; 

-- 2.自联结
SELECT p1.prod_id, p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
    AND p2.prod_id = 'DTNTR';

-- 3.自然联结
-- 排除多次出现，使每个列只返回一次
SELECT c.*, o.order_num, o.order_date,
    oi.prod_id, oi.quantity, oi.item_price
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
    AND oi.order_num = o.order_num
    AND prod_id = 'FB'; 

-- 4.外部联结
-- 联结包含了那些在相关表中没有关联行的行
-- 左外联结 LEFT OUTER JOIN, 即包含左边表所有行
SELECT customers.cust_id, orders.order_num
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id;

-- 右外联结 RIGHT OUTER JOIN, 即包含右边表所有行
SELECT customers.cust_id, orders.order_num
FROM customers RIGHT OUTER JOIN orders
ON orders.cust_id = customers.cust_id;
```

### 组合查询
---

**并**：复合查询，mysql允许执行多个查询，并将结果作为单个查询结果集返回。
```sql
-- 1.使用UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002);

-- 2.使用UNION ALL包含重复行
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION ALL
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002);

-- 3.对组合查询结果排序
-- ORDER BY子句位于最后一条SELECT语句之后，只能对整个组合查询进行排序，不可对单一SELECT语句进行排序，即不允许出现多条ORDER BY子句
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002)
ORDER BY vend_id, prod_price;
```

### 全文本搜索
---

{% note warning %}
#### 并非所有引擎都支持全文本搜索
MyISAM -- 支持全文本搜索
InnoDB -- 不支持
{% endnote %}

```sql
-- 1.启用全文本搜索支持
CREATE TABLE productnotes
(
    note_id int NOT NULL AUTO_INCREMENT,
    prod_id char(10) NOT NULL,
    note_date datetime NOT NULL,
    note_text text NULL,
    PRIMARY KEY(note_id),
    FULLTEXT(note_text)
) ENGINE=MyISAM;

-- 2.进行全文本搜索
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit');

-- 3.使用查询扩展
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit' WITH QUERY EXPANSION);

-- 4.布尔文本搜索
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('heavy' IN BOOLEAN MODE);
```

### 插入数据
---

INSERT语句是用来插入（或添加）行到数据库表的。
```sql
-- 1.插入完整的行
INSERT INTO customers(
    cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country,
    cust_contact,
    cust_email
)
VALUES(
    'Pep E. LaPew',
    '100 Main Street',
    'Los Angeles',
    'CA',
    '90046',
    'USA',
    NULL,
    NULL
);

-- 2.插入多行
INSERT INTO customers(
    cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country
)
VALUES(
    'Pep E. LaPew',
    '100 Main Street',
    'Los Angeles',
    'CA',
    '90046',
    'USA'
), (
    'M. Martian',
    '42 Galaxy Way',
    'New York',
    'NY',
    '11213',
    'USA'
);

-- 3.插入检索出的数据
INSERT INTO customers(
    cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country
)
SELECT cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country
FROM custnew;
```

### 更新和删除数据
---

{% note danger %}
#### 不要省略WHERE子句
在使用UPDATE & DELETE语句时，一不小心，可能更新表的所有行或者删除所有行，mysql无撤销按钮。
{% endnote %}

```sql
-- 1.更新数据
UPDATE customers
SET cust_name = 'The Fudds',
    cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;

UPDATE customers
SET cust_email = NULL
WHERE cust_id = 10005;
-- NULL 用来去除cust_email列中的值。

-- 2.删除数据
DELETE FROM customers
WHERE cust_id = 10006;
```

### 创建 & 操纵表
---

```sql
-- 1.表创建，CREATE TABLE 之后给定表列名和定义，逗号分隔；
CREATE TABLE customers
(
    cust_id         int         NOT NULL AUTO_INCREMENT,
    cust_name       char(50)    NOT NULL,
    cust_address    char(50)    NULL,
    cust_city       char(50)    NULL,
    cust_state      char(5)     NULL,
    cust_zip        char(10)    NULL,
    cust_country    char(50)    NULL,
    cust_contact    char(50)    NULL,
    cust_email      char(255)   NULL,
    PRIMARY KEY (cust_id)
)   ENGINE=InnoDB;

-- 1) 使用NULL值允许更新或插入时该列为空
-- 2）主键可以包含多列，用逗号隔开；比如
    PRIMARY KEY (order_num, order_item)
-- 3) 主键可以使用AUTO_INCREMENT 自动增量，由mysql对该列进行维护；查询列值可使用命令：
    SELECT last_insert_id();
-- 4) 可以对特定列指定默认值，比如默认数量设置为1；
    quantity int NOT NULL DEFAULT 1
-- 5) InnoDB是一个可靠的事务处理引擎，不支持全文本搜索

-- 2.更新表
-- 添加列
ALTER TABLE vendors
ADD vend_phone CHAR(20);

-- 删除列
ALTER TABLE vendors
DROP COLUMN vend_phone;

-- 定义外键
ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY (order_num) REFERENCES orders (order_num);

-- 3.删除表
DROP TABLE customers2;

-- 4.重命名表
RENAME TABLE customers2 TO customers;
-- 重命名多个表可用逗号隔开
```

### 使用视图
---

**视图**：虚拟表
使用视图的**优点**：
- 重用SQL语句
- 简化复杂的SQL操作
- 使用表的组成部分而不是整个表
- 保护数据，授予表的特定部分访问权而不是整个表
- 更行数据格式和表示，可返回与底层表不同的格式和表示

```sql
-- 1.利用视图简化复杂的联结
-- 1) 创建视图
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
    AND orderitems.order_num = orders.order_num;

-- 2) 检索产品TNT2的客户
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';

-- 2.用视图重新格式化检索出的数据
-- 1）创建视图
CREATE VIEW vendorlocations AS
SELECT Concat(RTrim(vend_name), '(', RTrim(vend_country), ')') AS vend_title
FROM vendors
ORDER BY vend_name;

-- 2) 检索所有行
SELECT *
FROM vendorlocations;

-- 3.用视图过滤不想要的数据
-- 1）创建视图
CREATE VIEW customeremaillist AS
SELECT cust_id, cust_name, cust_email
FROM customers
WHERE cust_email IS NOT NULL;

-- 2) 检索所有行
SELECT *
FROM customeremaillist;

-- 4.使用视图与计算字段
-- 1）创建视图
CREATE VIEW orderitemsexpanded AS
SELECT order_num, prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM orderitems;

-- 2) 检索订单20005的详细内容
SELECT *
FROM orderitemsexpanded
WHERE order_num = 20005;
```

{% note warning %}
#### 更新视图
可以更新，但存在限制；视图主要用于数据检索。
不能使用情况：
- 分组
- 联结
- 子查询
- 并
- 聚集函数
- DISTINCT
- 导出（计算）列

{% endnote %}

### 使用存储过程
---

**存储过程**：可以理解为编程语言中的函数，将一条或多条SQL语句集合在一起供以后调用，也可将其视为Windows批处理文件。
**优点**：简单/安全/高性能
```sql
-- 1.使用存储过程
-- 1）执行存储过程
CALL productpricing(@pricelow, @pricehigh, @priceaverage);

-- 2）创建存储过程
CREATE PROCEDURE productpricing()
BEGIN
    SELECT Avg(prod_price) AS priceaverage
    FROM products;
END;

-- 执行
CALL productpricing();
```

{% note warning %}
#### mysql命令行执行程序使用时需注意分隔符

```sql
DELIMITER //    -- 临时修改分隔符为 //

CREATE PROCEDURE productpricing()
BEGIN
    SELECT Avg(prod_price) AS priceaverage
    FROM products;
END //

DELIMITER ;    -- 恢复分隔符为 ；
```

{% endnote %}

```sql
-- 3) 删除存储过程
DROP PROCEDURE productpricing;
-- 或者
DROP PROCEDURE IF EXISTS productpricing;
```

**变量**：内存中一个特定的位置，用来临时存储数据。
```sql
-- 4) 使用参数
-- 示例1
CREATE PROCEDURE productpricing(
    OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
    SELECT Min(prod_price) INTO pl
    FROM products;
    SELECT Max(prod_price) INTO ph
    FROM products;
    SELECT Avg(prod_price) INTO pa
    FROM products;
END;

-- 执行
CALL productpricing(@pricelow, @pricehigh, @priceaverage);

-- 打印
SELECT @pricelow, @pricehigh, @priceaverage;

-- 示例2：使用IN & OUT 参数
CREATE PROCEDURE ordertotal(
    IN onumber INT,
    OUT ototal DECIMAL(8,2)
)
BEGIN
    SELECT Sum(item_price*quantity)
    FROM orderitems
    WHERE order_num = onumber
    INTO ototal;
END;

-- 执行
CALL ordertotal(20005, @total);

-- 打印
SELECT @total;

-- 5）建立智能存储过程
-- Name: ordertotal
-- Parameters: 
--  onumber = order number
--  taxable = 0 if not taxable, 1 if taxable
--  ototal  = order total variable
CREATE PROCEDURE ordertotal(
    IN onumber INT,
    IN taxable BOOLEAN,
    OUT ototal DECIMAL(8,2)
)  COMMENT 'Obtain order total, optionally adding tax'
BEGIN
    -- Declare variable for total
    DECLARE total DECIMAL(8,2);
    -- Declare tax percentage
    DECLARE taxrate INT DEFAULT 6;

    -- Get the order total
    SELECT Sum(item_price*quantity)
    FROM orderitems
    WHERE order_num = onumber
    INTO total;

    -- Is this taxable?
    IF taxable THEN
        -- Yes, so add taxrate to the total
        SELECT total+(total/100*taxrate) TNTO total;
    END IF;

    -- And finally, save to out variable
    SELECT total INTO ototal;
END;

-- 执行 不含税
CALL ordertotal(20005, 0, @total);
SELECT @total;

-- 执行 含税
CALL ordertotal(20005, 1, @total);
SELECT @total;

-- 6）检查存储过程
SHOW CREATE PROCEDURE ordertotal;
```

{% note info %}
1. 获取存储过程列表创建时间和创建人详细信息
   ```sql
    SHOW PROCEDURE STATUS;
   ```
2. 限制过程状态结果
   ```sql
   -- LIKE 指定过滤模式
    SHOW PROCEDURE STATUS LIKE 'ordertotal';
   ```

{% endnote %}

### 使用游标
---

**游标**：是一个存储在mysql服务器上的数据库查询

{% note danger %}
**mysql游标只能用于存储过程**
{% endnote %}

```sql
-- 1.创建游标
CREATE PROCEDURE processorders()
BEGIN
    DECLARE ordernumbers CURSOR
    FOR
    SELECT order_num FROM orders;
END;

-- 2.打开/关闭游标
OPEN ordernumbers;
CLOSE ordernumbers;

-- 3.使用游标检索数据
CREATE PROCEDURE processorders()
BEGIN
    -- Declare local variables
    DECLARE done BOOLEAN DEFAULT 0;
    DECLARE o INT;

    -- Declare the cursor
    DECLARE ordernumbers CURSOR
    FOR
    SELECT order_num FROM orders;

    -- Declare continue handler
    DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

    -- Open the cursor
    OPEN ordernumbers;

    -- Loop through all rows
    REPEAT
        -- Get order number
        FETCH ordernumbers INTO o;

    -- End of loop
    UNTIL done END REPEAT;

    -- Close the cursor
    CLOSE ordernumbers;
END;

-- 4.检索数据并进行处理然后保存结果到表中
CREATE PROCEDURE processorders()
BEGIN
    -- Declare local variables
    DECLARE done BOOLEAN DEFAULT 0;
    DECLARE o INT;
    DECLARE t DECIMAL(8,2);

    -- Declare the cursor
    DECLARE ordernumbers CURSOR
    FOR
    SELECT order_num FROM orders;

    -- Declare continue handler
    DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

    -- Create a table to store the results
    CREATE TABLE IF NOT EXISTS ordertotals
    (
        order_num   INT,
        total       DECIMAL(8,2)
    );

    -- Open the cursor
    OPEN ordernumbers;

    -- Loop through all rows
    REPEAT
        -- Get order number
        FETCH ordernumbers INTO o;

        -- Get the total for this order
        CALL ordertotal(o, 1, t);

        -- Insert order and total into ordertotals
        INSERT INTO ordertotals(ordernum, total)
        VALUES(o, t);

    -- End of loop
    UNTIL done END REPEAT;

    -- Close the cursor
    CLOSE ordernumbers;
END;

-- 检索存储结果表的所有行
CALL processorders();
SELECT *
FROM ordertotals;
```

### 使用触发器
---

**触发器**：mysql响应增、删、改而自动执行的一条mysql语句，即事件触发。
```sql
-- 1.创建触发器
CREATE TRIGGER newproduct AFTER INSERT ON products
FOR EACH ROW SELECT 'Product added';

-- 2.删除触发器
DROP TRIGGER newproduct;

-- 3.使用触发器
-- 1）INSERT触发器
--  INSERT触发器代码内可引用一个名为NEW的虚拟表，BEFOR INSERT触发器中，NEW的值可被更新（允许更改被插入的值）；
--  对AUTO_INCREMENT列，NEW在INSERT执行前包含0，执行后包含新的自动生成值。
CREATE TRIGGER neworder AFTER INSERT ON orders
FOR EACH ROW SELECT NEW.order_num;

-- 测试触发器，执行插入操作会打印自动生成值
INSERT INTO orders(order_date, cust_id)
VALUES(Now(), 10001);

-- 2）DELETE触发器
--  在DELETE触发器代码内可以引用一个名为OLD的虚拟表，访问被删除的行；
--  OLD中的值是只读的，不能更新。
CREATE TRIGGER deleteorder BEFORE DELETE ON orders
FOR EACH ROW
BEGIN
    INSERT INTO archive_orders(order_num, order_date, cust_id)
    VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
END;

-- 3) UPDATE触发器
-- UPDATE触发器代码内可引用两个虚拟表，OLD访问旧值，NEW访问新值；
-- BEFOR UPDATE触发器内，NEW中的值可以被更新；
-- OLD中的值是只读的，不能更新。
CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
```

### 管理事务处理
---

{% note info %}
#### 并非所有引擎都支持事务处理
MyISAM --不支持
InnoDB --支持事务处理管理
{% endnote %}

**事务处理**：是一种机制，用来管理必须成批执行的mysql操作，以保证数据库不包含不完整的操作结果。
**事务**：一组sql语句
**回退**：撤销指定sql语句的过程
**提交**：将未存储的sql语句结果写入数据库表
**保留点**：事务处理中设置的临时占位符，可以对它发布回退，但与回退整个事务处理不同

```sql
-- 1.标识事务的开始
START TRANSACTION;

-- 2.使用ROLLBACK
SELECT * FROM ordertotals;
START TRANSACTION;
DELETE FROM ordertotals;
SELECT * FROM ordertotals;
ROLLBACK;
SELECT * FROM ordertotals;

-- 3.使用COMMIT
START TRANSACTION;
DELETE FROM orderitems WHERE order_num = 20010;
DELETE FROM orders WHERE order_num = 20010;
COMMIT;
```

{% note info %}
#### 隐含事务关闭
COMMIT或ROLLBACK语句执行后，事务会自动关闭，将来的更改会隐含提交。
{% endnote %}

```sql
-- 4.使用保留点
SAVEPOINT delete1;

-- 回退到保留点
ROLLBACK TO delete1;
```

{% note info %}
#### 释放保留点
保留点在COMMIT或ROLLBACK语句执行后自动释放，也可以使用
```sql
RELEASE SAVEPOINT;
```
明确释放保留点。
{% endnote %}

```sql
-- 5.更改默认提交行为
SET autocommit=0;
-- 关闭自动提交
-- 针对每次连接专用，而不是服务器
```

### 全球化 & 本地化
---

**字符集**：字母和符号的集合
**编码**：某个字符集成员的内部表示
**校对**：规定字符如何比较的指令
```sql
-- 1.显示字符集 & 校对
SHOW CHARACTER SET;
SHOW COLLATION;
SHOW VARIABLES LIKE 'character%';
SHOW VARIABLES LIKE 'collation%';

-- 2.指定字符集 & 校对
CREATE TABLE mytable
(
    column1     INT,
    column2     VARCHAR(10),
    column3     VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci
)   DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
```

### 安全管理
---

**访问控制**：宗旨是设置适当的访问权限，不多也不少。

```sql
-- 1.检索所有用户表信息
USE mysql；
SELECT user FROM user;

-- 2.创建用户账号
CREATE USER ben IDENTIFIED BY 'p@$$w0rd';

-- 3.重命名用户账号
RENAME USER ben TO bforta;

-- 4.删除用户账号
DROP USER bforta;

-- 5.设置访问权限
-- 查询用户被赋予的访问权限
SHOW GRANTS FOR bforta;

-- 授予访问权限
GRANT SELECT ON crashcourse.* TO bforta;
-- 简化多次授权
GRANT SELECT, INSERT ON crashcourse.* TO bforta;

-- 撤销访问权限
REVOKE SELECT ON crashcourse.* FROM bforta;

-- 6.更改口令
-- 更新用户bforta口令
SET PASSWORD FOR bforta = Password('n3w p@$$w0rd');

-- 更新当前登录用户口令
SET PASSWORD = Password('n3w p@$$w0rd');
```

### 数据库维护
---

1.备份数据
解决方案：
- 命令行程序mysqldump转储所有数据库内容到某个外部文件
- 命令行程序mysqlhtocopy
- mysql语句BACKUP TABLE 或 SELECT INTO OUTFILE转储

2.进行数据库维护
```sql
-- ANALYZE TABLE 检查表键是否正确
ANALYZE TABLE orders;

CHECK TABLE orders, orderitems;
```

- ANALYZE TABLE
- CHECK TABLE
- CHANGED
- EXTENDED
- FAST
- MEDIUM
- QUICK

3.诊断启动问题
需要手动启动服务器进行检查，可在命令行上执行mysqld启动。
重要参数：
+ --help
+ --safe-mode
+ --verbose
+ --version

4.查看日志文件
+ 错误日志，通常名为hostname.err，位于data目录中，可使用--log-error命令行选项更改
+ 查询日志，通常名为hostname.log，位于data目录中，可使用--log命令行选项更改
+ 二进制日志，通常名为hostname-bin，位于data目录中，可使用--log-bin命令行选项更改
+ 缓慢查询日志，通常名为hostname-slow.log，位于data目录中，可使用--log-slow-queries命令行选项更改

```sql
-- 刷新和重新开始所有日志文件
FLUSH LOGS;
```

### 改善性能
---

在诊断应用滞缓现象和性能问题是，性能不良的数据库或者数据库查询通常是最常见的祸因，以下总结的内容并不能完全决定mysql的性能，结合之前各章重点，提供进行性能优化探讨和分析的一个出发点。
+ 生产服务器应该遵循mysql特定的硬件建议
+ mysql预先设置了一系列默认配置，可使用SHOW VARIABLES;和SHOW STATUS;查询
+ mysql是一个多用户多线程DBMS，即同时执行多个任务，可使用SHOW PROCESSLIST;显示所有活动进程，也可使用KILL命令将执行缓慢进程杀死
+ 总有不止一种方法编写同一条SELECT语句，应试验联结/并/子查询等，找出最佳方法
+ EXPLAIN语句让MYSQL解释它将如何执行一条SELECT语句
+ 使用存储过程比分散执行各条mysql语句更快
+ 使用正确的数据类型
+ 决不要检索比需求还多的数据
+ 某些操作支持可选的DELAYED关键字，将控制权立即返回给调用程序
+ 导入数据时关闭自动提交；删除索引（包括FULLTEXT索引）并在导入完成后再重建它们
+ 必须确定索引数据库表以改善数据检索的性能
+ SELECT语句有一系列复杂OR条件，修改成多条SELECT语句和连接它们的UNION语句可极大改进性能
+ 索引改善数据检索的性能，但损害数据插入/删除/更新的性能
+ LIKE很慢，最好使用FULLTEXT
+ 数据库是不断变化的实体，理想的优化和配置也会需要改变
+ 最重要的规则既是每条规则在某些条件下都会被打破

<a href="http://dev.mysql.com/doc/"><mark>浏览文档</mark></a>
- MYSQL文档有许多提示和技巧，一定要查看这些非常有价值的资料
