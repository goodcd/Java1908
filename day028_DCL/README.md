# `day028`事务和权限管理

> 作者: 张大鹏



## 001.准备`sql`

- 1.创建部门表

```sql
-- 创建部门表
CREATE TABLE dept(
	-- 部门编号
	id INT PRIMARY KEY AUTO_INCREMENT,
	-- 部门名字
	NAME VARCHAR(20)
);
```

- 2.创建员工表

```sql
-- 创建员工表
CREATE TABLE emp(
	-- id
	id INT PRIMARY KEY AUTO_INCREMENT,
	-- 姓名
	NAME VARCHAR(20),
	-- 性别
	gender CHAR(1),
	-- 工资
	salary DOUBLE,
	-- 入职日期
	join_date DATE,
	-- 部门编号
	dept_id INT,
	-- 外键,关联部门表dept.id
	FOREIGN KEY(dept_id) REFERENCES dept(id)
);
```

- 3.插入部门数据

```sql
-- 插入部门数据
INSERT INTO dept(NAME) VALUES
("公关部"),
("财务部"),
("人事部");
```

- 4.插入员工数据

```sql
-- 插入员工数据
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
```

- 5.查询是否插入成功

```sql
-- 查询是否插入成功
SELECT *FROM dept;
SELECT *FROM emp;
```



## 002.隐式内连接查询

> 使用where条件消除无用数据

- 1.查询所有员工信息和对应的部门信息

```sql
-- 查询所有员工信息和对应的部门信息
SELECT
t1.*,t2.`name`
FROM 
emp t1,dept t2
WHERE
t1.`dept_id`=t2.`id`;
```

- 2.查询员工的姓名，性别,部门

```sql
-- 查询员工的姓名，性别,部门
SELECT
t1.`name`,t1.`gender`
FROM
emp t1,dept t2
WHERE
t1.`dept_id`=t2.`id`;
```



## 003.显示内连接查询

> 语法： select 字段列表 from 表名1 [inner] join 表名2 on 条件

- 1.查询所有员工信息和对应的部门信息

```sql
-- 显式内连接查询
-- 1.查询所有员工信息和对应的部门信息
SELECT
* FROM emp
JOIN
dept
ON
emp.`dept_id`=dept.`id`;
```



## 004.左外连接查询

> 语法：select 字段列表 from 表1 left [outer] join 表2 on 条件；
>
> 查询的是左表所有数据以及其交集部分。

- 1.查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称

```sql
-- 左外连接查询
-- 查询所有员工信息，如果员工有部门，则查询部门名称，
-- 没有部门，则不显示部门名称
SELECT
* FROM emp
LEFT JOIN
dept
ON
emp.`dept_id`=dept.`id`;
```



## 005.右外连接

> 语法：select 字段列表 from 表1 right [outer] join 表2 on 条件；
>
> 查询的是右表所有数据以及其交集部分。



## 006.子查询

> 概念：查询中嵌套查询，称嵌套查询为子查询。

- 1.查询工资最高的员工信息
  - 1.查询最高的工资是多少
  - 2.查询员工信息，并且工资等于最高工资

```sql
-- 子查询
-- 查询工资最高的员工信息
SELECT
*
FROM
emp
WHERE
salary=(SELECT	MAX(salary) FROM emp);
```



## 007.子查询分类

- 1.子查询的结果是单行单列的
  - 子查询可以作为条件，使用运算符去判断。 运算符： > >= < <= =
- 2.子查询的结果是多行单列的
  - 子查询可以作为条件，使用运算符in来判断
- 3.子查询的结果是多行多列的
  - 子查询可以作为一张虚拟表参与查询



## 008.练习1

1.创建部门表

```sql
-- 创建部门表
CREATE TABLE practice01_dept(
  -- id
  id INT PRIMARY KEY AUTO_INCREMENT,
  -- 部门名称
  NAME VARCHAR(50),
  -- 部门所在地
  address VARCHAR(50)
);
```

2.添加四个部门

```sql
-- 添加四个部门
INSERT INTO practice01_dept(id,NAME,address) 
VALUES
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 查看是否添加成功
SELECT
*
FROM
practice01_dept;
```

3.创建职务表并添加4个职务

```sql
-- 创建职务表并添加四个职务
CREATE TABLE practice01_job(
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(20),-- 职务名称
  description VARCHAR(50)-- 职务描述
);

-- 添加职务
INSERT INTO practice01_job(id,NAME,description)
VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');
```

4.创建员工表并添加员工

```sql
-- 员工表
CREATE TABLE practice01_emp(
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(50), -- 姓名
  job_id INT,-- 职务id
  mgr INT,-- 上级领导id
  join_date DATE,-- 入职日期
  salary DECIMAL(7,2),  -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT,-- 所在部门编号
  CONSTRAINT emp_job_fk FOREIGN KEY (job_id) REFERENCES practice01_job(id),
  CONSTRAINT emp_dept_fk FOREIGN KEY (dept_id) REFERENCES practice01_dept(id)
);

-- 添加员工
INSERT INTO practice01_emp
(id,NAME,job_id,mgr,join_date,salary,bonus,dept_id)
VALUES
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);

-- 查询是否添加成功
SELECT * FROM practice01_emp;
```

5.创建工资等级表并添加五个工资等级

```sql
-- 工资等级表
CREATE TABLE practice01_salary_grade(
 id INT PRIMARY KEY AUTO_INCREMENT,
 -- 最低工资
 low INT,
 -- 最高工资
 high INT
);

-- 添加五个工资等级
INSERT INTO practice01_salary_grade
(id,low,high)
VALUES
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);

-- 查看是否添加成功
SELECT * FROM practice01_salary_grade;
```



> 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述

```sql
-- 查询员工编号，员工姓名，工资，职务名称，职务描述
/*
分析:
  1.员工编号,姓名,工资 在practice01_emp表
  2.职务名称和职务描述 在practice01_job表
  3.where条件 emp.job_id=job.id
*/
SELECT
t1.`id`,t1.`name`,t1.`salary`,
t2.`name`,t2.`description`
FROM
practice01_emp t1,practice01_job t2
WHERE
t1.`job_id`=t2.`id`;
```

> 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置

```sql
-- 2.查询员工编号，员工姓名，工资，职务名称，
-- 职务描述，部门名称，部门位置
/*
分析:
  1.员工编号,姓名,工资 在emp
  2.职务名称,描述 在job
  3.部门名称,位置 在dept
  4.条件是 emp.job_id=job.id and emp.dept_id=dept.id
*/
SELECT
t1.`id`,t1.`name`,t1.`salary`,
t2.`name`,t2.`description`,
t3.`name`,t3.`address`
FROM
practice01_emp t1,
practice01_job t2,
practice01_dept t3
WHERE
t1.`job_id`=t2.`id`
AND
t1.`dept_id`=t3.`id`;
```

> 3.查询员工姓名，工资，工资等级

```sql
-- 3.查询员工姓名，工资，工资等级
/*
分析:
  1.姓名 工资  emp
  2.工资等级 grade
  3.where条件  between
*/
SELECT
t1.`name` 员工姓名,
t1.`salary` 员工工资,
t2.`id` 工资等级
FROM
practice01_emp t1,
practice01_salary_grade t2
WHERE
t1.`salary` BETWEEN t2.`low` AND t2.`high`;
```

> 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

```sql
-- 4.查询员工姓名，工资，职务名称，职务描述，
-- 部门名称，部门位置，工资等级
/*
分析:
  1.员工姓名,工资 emp
  2.职务名称,描述 job
  3.部门名称,位置 dept
  4.工资等级 grade
  5.条件 = and  between
*/
SELECT
t1.`name` 姓名,
t1.`salary` 工资,
t2.`name` 职位,
t2.`description` 工作内容,
t3.`name` 部门,
t3.`address` 工作地点,
t4.`id` 工资等级
FROM
practice01_emp t1,
practice01_job t2,
practice01_dept t3,
practice01_salary_grade t4
WHERE
t1.`job_id`=t2.`id`
AND
t1.`dept_id`=t3.`id`
AND
(t1.`salary` BETWEEN t4.`low` AND t4.`high`);
```

> 5.查询出部门编号、部门名称、部门位置、部门人数

```sql
-- 5.查询出部门编号、部门名称、部门位置、部门人数
/*
分析:
  1.部门编号,名称,位置 dept
  2.部门人数 count(id)
  3.条件
      1.emp.dept_id=dept.id
      2.group by(dept_id)
*/
SELECT
t2.`id` 部门编号,
t2.`name` 部门名称,
t2.`address` 部门位置,
COUNT(t1.`id`) 部门人数
FROM
practice01_emp t1,
practice01_dept t2
WHERE
t1.`dept_id`=t2.`id`
GROUP BY
t1.`dept_id`;
-- 检查
SELECT * FROM practice01_dept;
-- 发现没有财务部的人
-- 再次检查
SELECT * FROM practice01_emp;
SELECT * FROM practice01_emp t1
WHERE
t1.`dept_id`=(SELECT id FROM practice01_dept
WHERE NAME LIKE "财务部"
);
-- 发现的确没有
```



## 009.事务

> 概念: 如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。

操作

- 1. 开启事务： start transaction;
- 2. 回滚：rollback;
- 3. 提交：commit;



## 010.事务案例

```sql
CREATE TABLE account (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    balance DOUBLE
);
-- 添加数据
INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);
SELECT * FROM account;
UPDATE account SET balance = 1000;
-- 张三给李四转账 500 元
-- 0. 开启事务
START TRANSACTION;
-- 1. 张三账户 -500
UPDATE account SET balance = balance - 500 WHERE NAME = 'zhangsan';
-- 2. 李四账户 +500
-- 出错了...
UPDATE account SET balance = balance + 500 WHERE NAME = 'lisi';
-- 发现执行没有问题，提交事务
COMMIT;
-- 发现出问题了，回滚事务
ROLLBACK;
```



## 011,事务提交方式

自动提交

- `mysql`就是自动提交的
- 一条`DML`(增删改)语句会自动提交一次事务。

手动提交

- `Oracle` 数据库默认是手动提交事务
- 需要先开启事务，再提交

修改事务默认的提交方式

```sql
查看事务的默认提交方式：
SELECT @@autocommit; -- 1 代表自动提交  0 代表手动提交
修改默认提交方式： 
set @@autocommit = 0;
```



## 012.事务的四大特征

1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
3. 隔离性：多个事务之间。相互独立。
4. 一致性：事务操作前后，数据总量不变



## 013.事务的隔离级别

> 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

存在问题

1. 脏读：一个事务，读取到另一个事务中没有提交的数据
2. 不可重复读(虚读)：在同一个事务中，两次读取到的数据不一样。
3. 幻读：一个事务操作(`DML`)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。



隔离级别

1. `read uncommitted`：读未提交
			* 产生的问题：脏读、不可重复读、幻读
2. `read committed`：读已提交 （`Oracle`）
  * 产生的问题：不可重复读、幻读
3. `repeatable read`：可重复读 （`MySQL`默认）
  * 产生的问题：幻读
4. `serializable`：串行化
  * 可以解决所有的问题

```
 注意：隔离级别从小到大安全性越来越高，但是效率越来越低
		* 数据库查询隔离级别：
			* select @@tx_isolation;
		* 数据库设置隔离级别：
			* set global transaction isolation level  级别字符串;
```



## 014.`DCL`

SQL分类：
 	1. `DDL`：操作数据库和表
	2. `DML`：增删改表中数据
	3. ` DQL`：查询表中数据
	4. `DCL`：管理用户，授权



> `DBA`：数据库管理员



管理用户

- 1.添加用户
  - `语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`
- 2.删除用户
  - `语法：DROP USER '用户名'@'主机名';`
- 3.修改用户密码
  - `UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';`
  - `UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';`
  - `SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');`
  - `SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');`



## 015.`Mysql`忘记密码处理方法

1. `cmd `-- >` net stop mysql` 停止`mysql`服务
2. 使用无验证方式启动`mysql`服务： `mysqld --skip-grant-tables`
3. 打开新的`cmd`窗口,直接输入`mysql`命令，敲回车。就可以登录成功
4. `use mysql`;
5. `update user set password = password('你的新密码') where user = 'root';`
6. 关闭两个窗口
7. 打开任务管理器，手动结束`mysqld.exe` 的进程
8. 启动`mysql`服务
9. 使用新密码登录。



## 016.权限管理

1.查询权限

```sql
-- 查询权限
SHOW GRANTS FOR '用户名'@'主机名';
SHOW GRANTS FOR 'lisi'@'%';
```

2.授予权限

```sql
-- 授予权限
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
-- 给张三用户授予所有权限，在任意数据库任意表上
GRANT ALL ON *.* TO 'zhangsan'@'localhost';
```

3.撤销权限

```sql
-- 撤销权限：
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
```

