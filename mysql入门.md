## 1、数据库操作语句

### 1.1、登陆数据库

mysql  {-h**......**}  {-P**......**(大写)}  -u**......**  -p**......**(小写)   

> -h：目标数据库IP
>
> -P：目标端口，默认3306
>
> -u：目标数据库用户名
>
> -p：目标数据库密码
>
> 登陆本地数据库时，前面两个参数可省略



### 1.2、创建数据库

create database 

{if not exsts}| 判断是否存在该数据库，不存在则创建

`数据库名称`

{character set '字符集'}|设置字符集，默认utf8;



### 1.3、删除数据库

drop database 

{if exists}|判断是否存在，存在则删除

`数据库名称`;



### 1.4、切换数据库

use `数据库名称`;



### 1.5、修改数据库

alter database `数据库名`  



### 1.6、查看当前使用的数据库

select database();



## 2、表的操作语句

### 2.1、创建表

```mysql
CREATE TABLE student(
	`id` INT NOT NULL AUTO_INCREMENT,
	`username` VARCHAR(20) NOT NULL,
	`password` VARCHAR(20) NOT NULL,
	PRIMARY KEY (`id`)
);

```



### 2.2、修改表

```mysql
--修改表名
ALTER TABLE 旧表名  RENAME AS 新表名

--增加表的字段
ALTER TABLE 表名 ADD age INT(11)

--修改表的字段的数据类型
ALTER TABLE 表名 MODIFY age VARCHAR(11)  --修改约束

ALTER ATBLE 表名 CHANGE age age1 INT(2)  --字段重命名

--删除表的字段
ALTER TABLE 表名 DROP 字段名
```





### 2.3、删除表

```mysql
DROP TABLE IF EXISTS `newStudent`;
```



### 2.4、蠕虫复制

```mysql
-- 创建类似表
CREATE TABLE `newStudent` LIKE `student`;

-- 插入查询的结果到表中
INSERT INTO `newStudent` (SELECT * FROM `student`);
```





## 3、数据操作语句

### 3.1、插入数据

```mysql
-- 插入全部
INSERT	INTO student VALUES(1,'zhangsan','123456');
-- 插入某些列
INSERT	INTO student (`username`,`password`) VALUES('lisi','212324');
-- 插入多组数据
INSERT	INTO student (`username`,`password`) VALUES('zhaoliu','334455'),('yanqi','445566'),('wangba','556677'),('lijiu','667788');
```



### 3.2、更新数据

```mysql
UPDATE `student` SET `password`=233233 WHERE `id`=2;
```



### 3.3、删除数据

```mysql
delete from `student` where `id`=7;
```



### 3.4、查找数据

| NAME    | price | sales_volume | produced_date |
| ------- | ----- | ------------ | ------------- |
| 华为P40 | 5999  | 1000         | 2020-08-20    |



#### 3.4.1、普通条件查询 ->where

| 符号             | 含义               |
| ---------------- | ------------------ |
| >                | 大于               |
| <                | 小于               |
| >=               | 大于等于           |
| <=               | 小于等于           |
| <> 或 !=         | 不等于             |
| AND 或 &&        | 与                 |
| OR 或 \|\|       | 或                 |
| NOT 或 ！        | 非                 |
| BETWEEM...AND... | 在。。。之间。。。 |
| IN(...)          | 多选一             |
| IS NULL          | 空                 |
| IS NOT NULL      | 非空               |



```mysql
-- 普通条件
SELECT `NAME`,`price`  FROM `goods` WHERE `price`>2000 && `price`<5000;

-- 区间
select * from `goods` where `produced_date` >= '2020-01-01' and `produced_date` <= '2020-12-30';

-- BETWEEN...AND... 》》》 区间
select * from `goods` where `produced_date` between '2020-01-01' and '2020-12-30';

-- 或
SELECT * FROM `goods` WHERE `NAME` = '华为P40' OR `NAME` = '小米11';

-- IN(...) 》》》 多选一
SELECT * FROM `goods` WHERE `NAME` in ('华为P40' , '小米11');
```



#### 3.4.2、起别名 -> AS

```mysql
SELECT `NAME` AS 品牌,`price` AS 价格  FROM `goods` WHERE `price`>2000 && `price`<5000;

```



#### 3.4.3、四则运算 ->{+-*/}

```mysql
SELECT `NAME`,(`price` {+-*/} 1)  FROM `goods` WHERE `price`>2000 && `price`<5000;

```



#### 3.4.4、去重查询 ->DISTINCT

```mysql
select distinct `NAME` from `goods` ;

```



#### 3.4.5、模糊查询 -> LIKE

| 运算符      | 语法               | 描述                               |
| ----------- | ------------------ | ---------------------------------- |
| IS NULL     | A IS NULL          | 如果操作符为NULL，结果为真         |
| IS NOT NULL | A IS NOT NULL      | 如果操作符不为NULL，结果为真       |
| BETWEEN     | A BETWEEN B AND C  | 若a在b和c之间，则结果为真          |
| LIKE        | a like b           | SQL比配，如果a匹配b，则结果为真    |
| **IN**      | a in (a1,a2,a3...) | 假设a在a1,a2,a3...之间，则结果为真 |

> '_' ：表示匹配**任意一个**字符
>
> ‘%’：表示匹配**任意数量**字符

```mysql
-- 查询`NAME`字段带米字的数据
SELECT `NAME` AS `品牌`,`price` AS `价格` FROM `goods` WHERE `NAME` LIKE '%米%';
-- 查询`NAME`字段第二个字是米字的数据
SELECT `NAME` AS `品牌`,`price` AS `价格` FROM `goods` WHERE `NAME` LIKE '_米%';

-- 查询`NAME`字段倒数第二个字是米字的数据
SELECT * FROM `goods` WHERE `NAME` LIKE '%米_'

-- 查询`NAME`字段为三个字符的数据
SELECT * FROM `goods` WHERE `NAME` LIKE '___'


```



#### 3.4.6、排序查询 ->ORDER BY {ASC/DESC}

- DESC：降序排序
- ASC ：升序排序（默认升序）

```mysql
-- 查询`sales_volume` > 1000的商品信息并按照`price`排序
SELECT * FROM `goods` WHERE `sales_volume` > 1000 ORDER BY `price` ASC;

-- 多条件排序
SELECT * FROM `goods` WHERE `sales_volume` > 1000 ORDER BY `price` DESC,`sales_volume` ASC;

```







## 关于数据库引擎

 关于数据库引擎

- INNODB 默认使用

- MYISAM 早些年使用的

  |            | MYISAM | INNODB        |
  | ---------- | ------ | ------------- |
  | 事务支持   | 不支持 | 支持          |
  | 数据行锁定 | 不支持 | 支持          |
  | 外键约束   | 不支持 | 支持          |
  | 全文索引   | 支持   | 不支持        |
  | 表空间大小 | 较小   | 较大，约为2倍 |

  常规使用操作：

  - MYSAAM 节约空间，速度较快
  - INNODB 安全性高，事务的处理，多表多用户操作

<!---
wenyepeng/wenyepeng is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
