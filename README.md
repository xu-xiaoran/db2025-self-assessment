# 数据库原理与应用2025-自我评价

---

## 第一周：数据库系统基础概念

 **学习内容**：

  * 数据库与数据库管理系统（DBMS）概念
  * 对数据库、关系模式进行初步了解
  * 下载安装并配置 PostgreSQL
  * 使用 DataGrip 尝试连接

**反思**：

  * 初次安装时就改了默认端口，导致后续连接需要每次手动调整。
  * 后续实验课上修改了.conf配置，恢复了默认端口，问题已解决。

---

## 第二周：关系模型与关系代数

**学习内容**：

  * 关系模型核心概念：表、元组、属性
  * 关系代数基本运算：并、交、差、笛卡尔积、选择、投影、连接
  * 约束：主码、候选码、外码及其完整性作用

**理解**：

  * 关系模式就是表的蓝图或模板。它描述了表应该长什么样子，包含哪些列，每列放什么类型的数据，以及数据需要遵守哪些规则。

  * 关系代数提供了一种形式化的方法来理解和构建复杂的数据库查询。

  * Super Key、Primary Key、Candidate Key、Foreign Key的概念与区分。

    | 关系          | 示例                                                         |
    | ------------- | ------------------------------------------------------------ |
    | 关系模式      | `branch(branch_name, branch_city, assets)`                   |
    | 关系代数      | `π_{branch_name}(σ_{branch_city='成都'}(branch)) `           |
    | Super Key     | `{emp_id}`, `{emp_id, name}`, `{ssn}` in `Employee(emp_id, ssn, name)` |
    | Primary Key   | `account_no` in `Account(account_no, balance, branch_id)`    |
    | Candidate Key | Both `student_id` and `email` in `Student(student_id, email, name)` |
    | Foreign Key   | `branch_id` in` Account(account_no, balance, branch_id)` references `Branch(branch_id）` |

**反思**：

  * 起初对主码、候选码、超码的概念与应用理解不够清晰，后阅读教材，理解更直观。

---

## 第三周：SQL查询语言与数据类型

**学习内容**：

  * 基本数据类型：数值、字符串、null等
  * 创建表的语法（DDL）
  * 查询语句基本结构：SELECT、FROM、WHERE
  * 列别名（AS）、排序（ORDER BY）

**掌握**：

#### 1. 数据类型

- int：整数类型（与机器相关，等价于 integer）
- smallint：小整数整形（int 的子集）
- numeric(p, d)：定点数，（最多）有 p 为数字，小数点右边有 d 位（在 PG 中等价于 decimal）
- float(n)：精度至少为 n (binary) 位的浮点数
- real, double precision：浮点数与双精度浮点数（与机器相关）
- char(n)：固定长度的字符串，长度为 n（等价于 charater）
- varchar(n)：可变长度的字符串，最大长度为 n （等价于character varying）
- text：非 SQL 标准，表示任意长度的字符串
- null: 可能存在但未知（unknown）\可能不存在

#### 2. 表操作

 ```sql
 -- 创建表
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    dept_id INT REFERENCES department(dept_id)
);

CREATE TABLE department (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);
    
-- 删除表
DROP TABLE student;
 ```

#### 3. 数据查询

```sql
SELECT student_id, name 
FROM student 
WHERE dept_id = (
    SELECT dept_id FROM department WHERE dept_name = '统计系'
);
```

#### 4. 排序

```sql
-- 按学号降序查询
SELECT * FROM student ORDER BY student_id DESC;

-- 重命名查询结果列
SELECT student_id AS 学号, name AS 姓名 FROM student;
```

> **核心要点**：用`CREATE`定义表结构，`SELECT`查询数据，主外键维护关联关系，数据类型约束存储格式。


**反思**：

  * 数据类型选择时，字符串长度定义模糊，后来查阅资料避免浪费空间。
  * 复杂查询条件构建时逻辑表达容易出错，通过反复练习帮助理清逻辑。

---

## 第四周：实验课-数据库连接与字符串操作

**学习内容**：

  * 连接数据库，执行SQL脚本文件
  * 字符串相关操作：引号使用、字符串函数、模糊查询
  * 集合操作：交（INTERSECT）、并（UNION）、差（EXCEPT）

**掌握**：

  * 字符串函数如`LENGTH()`, `SUBSTRING()`, `UPPER()`的使用
  * 模糊查询用LIKE和通配符
  * 集合操作要求字段类型和数量匹配

**反思**：

  * 在这周修改了端口配置，解决每次需要手动定义端口的问题。
  * 模糊查询时转义字符使用不熟悉，导致查询结果不准确,对照ppt修改匹配，加深了印象。

---

## 第五周：空值处理与聚合查询

**学习内容**：

  * NULL值的特性及处理方法
  * 聚合函数：COUNT、SUM、AVG、MAX、MIN
  * 分组查询与HAVING子句
  * 嵌套函数与IN、NOT IN、ALL、SOME操作符

**注意点**：

  * NULL不是一个值，任何与NULL的比较结果都是未知（NULL）
  * GROUP BY分组和HAVING条件的区别和使用场景

**示例**：

  ```sql
SELECT department, COUNT(*)
FROM Employees
WHERE salary IS NOT NULL
GROUP BY department
HAVING COUNT(*) > 5;
  ```

**反思**：

  * 初期对NULL值逻辑理解有误，导致查询结果和预期不符。
  * 通过实验反复验证，掌握了聚合和分组的正确用法。

---

## 第六周：数据修改操作

**学习内容**：

  * 删（DELETE）、增（INSERT）、批量导入（COPY）、更新（UPDATE）修改关系（ALTER）、插入（INSERT）、更新（UPDATE）、删除（DELETE）操作
  * 完整性约束：默认值、CHECK约束、UNIQUE约束
  * 使用排名函数（RANK）

**要点**：

  * DML语句语法及其事务控制基础
  * 约束条件保证数据一致性

**示例**：

  ```sql
  INSERT INTO Students (student_id, name, age)   -- 增
  VALUES (1001, 'Tom', 20);

  UPDATE Students SET age = 21 WHERE student_id = 1001;  -- 更新

  DELETE FROM Students WHERE student_id = 1001;   -- 删
  
  CREATE TABLE Students (
  student_id INT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  age INT DEFAULT 18,                         -- 默认值约束：不填时自动为18
  gender VARCHAR(10) CHECK (gender IN ('男', '女')),  -- CHECK约束：只能填“男”或“女”
  email VARCHAR(100) UNIQUE                   -- UNIQUE约束：不允许重复
);

-- 使用 RANK()
SELECT name, score, RANK() OVER (ORDER BY score DESC) AS rank FROM ExamResults;

-- 使用 DENSE_RANK()
SELECT name, score, DENSE_RANK() OVER (ORDER BY score DESC) AS dense_rank FROM ExamResults;

-- 使用ROW_NUMBER()
SELECT student_id, name, score, ROW_NUMBER() OVER (ORDER BY score DESC) AS row_num FROM ExamResults;

  ```

**反思**：

  * 对修改语句的条件把握不严谨，导致误删数据，之后养成先备份的习惯。
  * 对三种排名函数dense_rank()、row_number()、rank()的用法一开始不是很清楚,后来延伸阅读了Sling Academy 教程： [PostgreSQL Ranking Rows with RANK, DENSE_RANK and ROW_NUMBER](https://www.slingacademy.com/article/postgresql-ranking-rows-with-rank-dense-rank-and-row-number)。

---

## 第七周：中级SQL—连接与视图

**学习内容**：

  * 多种连接类型：INNER JOIN、LEFT JOIN、RIGHT JOIN、FULL OUTER JOIN
  * 连接条件的不同写法（NATURAL JOIN、ON、WHERE、USING）
  * 完整性约束复习
  * 视图（VIEW）的创建与使用

**要点**：

  * 理解不同连接的返回结果及适用场景
  * 视图作为虚拟表，简化复杂查询

**示例**：

  ```sql
  SELECT a.name, b.course
  FROM Students a
  INNER JOIN Enrollments b ON a.student_id = b.student_id;

  CREATE VIEW StudentView AS
  SELECT student_id, name FROM Students;
  ```

**反思**：

  * JOIN语法初学时容易混淆连接条件位置，调试中逐步掌握。
  * 视图更新限制不熟悉，阅读了[REDROCK:可更新视图](https://www.rockdata.net/zh-cn/tutorial/ddl-updatable-views/)中的教学，用其中的示例加深了理解。

---

## 第八周：高级数据类型与权限管理

 **学习内容**：

  * 时间和日期类型的使用
  * 数据类型转换函数
  * 数据库权限授权与撤销（GRANT、REVOKE）

**要点**：

  * DATE、TIMESTAMP等类型的存储和格式
  * 学习权限管理保证数据安全

**示例**：

  ```sql
  SELECT CURRENT_DATE, CURRENT_TIMESTAMP;

  GRANT SELECT ON Students TO user_readonly;
  REVOKE INSERT ON Students FROM user_readonly;
  ```


---

## 第九周：函数与触发器、Python访问数据库

**学习内容**：

  * 自定义函数和触发器的编写与应用
  * 通过Python程序访问PostgreSQL数据库

**要点**：

  * 学习函数(FUNCTION)和存储过程(PROCEDURE)的创建与调用
  * 使用触发器(TRIGGER)实现数据变更时的自动化操作
  * 使用Python的`psycopg2`库进行数据库操作
  * 使用ORM框架（如SQLAlchemy）：避免直接拼接SQL，增强安全性

**示例**：

  ```sql
  CREATE FUNCTION update_timestamp()
  RETURNS trigger AS $$
  BEGIN
      NEW.updated_at = NOW();
      RETURN NEW;
  END;
  $$ LANGUAGE plpgsql;

  CREATE TRIGGER update_student_modtime
  BEFORE UPDATE ON Students
  FOR EACH ROW EXECUTE FUNCTION update_timestamp();
  ```

**反思**：

  * 触发器逻辑编写较难，容易犯一些细节错误，借助了大模型进行辅助修改。
  * 一开始不熟悉用python访问数据库，感觉似乎用DataGrip就足够方便简洁了。但查询发现，用python操作数据库可以完成自动化、系统集成或复杂处理，因此还是非常有必要熟练掌握的。

---

## 第十周：实体-联系（ER）模型设计

**学习内容**：

  * ER模型基本概念：实体、属性、联系
  * 将ER图转化为关系模式
  * UML图作为ER模型替代方案

**要点**：

  * 实体集合和联系集合的区分
  * 属性的类别：简单/复合属性、单值/多值属性、派生属性
  * 一个实体转化为一个关系模式。实体属性就是关系的属性，实体的码就是关系的码。
  * 一对一、一对多、多对多关系建模
  * 使用`draw.io`绘制美观的ER图，好用！

**反思**：

  * 对复杂关系容易理不清逻辑，会遗漏约束条件
  * 对ER图符号不熟悉，容易混淆属性、箭头，后查阅资料教程，辅助我完成了作业中的绘图。

---

## 第十一、十二周：关系范式与模式分解

**学习内容**：

  * 关系范式：第一范式(1NF)、第二范式(2NF)、第三范式(3NF)、BCNF
  * 函数依赖理论
  * 模式分解及其保持数据一致性

**要点**：

  * 核心目标：消除冗余，确保数据一致性
  * 优先BCNF：若允许依赖丢失
  * 退而3NF：需保持依赖时
  * 工具辅助：使用依赖闭包计算工具（如DBMS内置函数）

**反思**：

  * 忽略无损分解验证，直接分解导致数据丢失，后面巩固理解，通过交集属性闭包验证。
  * BCNF可能丢失依赖（如 dept_advisor(s_ID, i_ID, dept_name) 分解后需额外连接查询）

---

## 第十三、十四周：存储结构、索引、查询优化及事务

**学习内容**：

  * 数据存储结构概述
  * 索引类型：有序与哈希索引
  * 查询优化原理
  * 事务的ACID属性
  * 并发控制机制

**理解**：

##### 1. 数据库存储 

- 存储层级：CPU寄存器 → CPU缓存 → DRAM → SSD → HDD（访问延迟逐级增加）
- 数据组织：Page、堆文件
- 行存储、列存储、大对象存储

##### 2. 索引

- 索引类型：有序索引、哈希索引
- 索引选择：聚集 vs.非聚集、B+树索引、稠密 vs. 稀疏、Hash索引
- 代价：索引占用存储空间，写入时需维护索引（可能降低写入性能）  
- 使用场景：  
  - 高频查询字段（如主键、外键）适合建索引  
  - 低区分度字段（如`gender`）建索引可能无效  

##### 3. 事务（ACID）

- 原子性（A）、一致性（C）、隔离性（I）、持久性（D）

  | 隔离级别         | 脏读 | 不可重复读 | 幻读 |
  | ---------------- | ---- | ---------- | ---- |
  | Read Uncommitted | ✓    | ✓          | ✓    |
  | Read Committed   | ✗    | ✓          | ✓    |
  | Repeatable Read  | ✗    | ✗          | ✓    |
  | Serializable     | ✗    | ✗          | ✗    |


**总结**：

- 存储：优化I/O，减少随机访问，合理选择行/列存储  
- 索引：权衡查询加速与写入开销，优先覆盖高频查询  
- 事务：平衡一致性与并发性能，避免过度隔离  

**示例**：  

- **索引**：  

  ```sql
  CREATE INDEX idx_salary ON Employee(salary);    -- 默认B+tree
  -- 范围查询
  SELECT * FROM Employee WHERE salary BETWEEN 50000 AND 100000;
  -- 排序查询（可复用索引）
  SELECT * FROM Employee ORDER BY salary DESC;
  
  CREATE INDEX idx_email_hash ON Users USING HASH(email);   -- HASH索引
  SELECT * FROM Users WHERE email = 'a@x.com';  -- 可用索引（等值查询）
  SELECT * FROM Users WHERE email LIKE 'a%';    -- 无法使用 Hash 索引
  ```

- **事务隔离**：  

  ```sql
  -- 基本事务操作（默认 Read Committed）
  BEGIN;
  UPDATE Students SET age = 22 WHERE student_id = 1001;
  COMMIT;
  ```

---

# 总结

通过一学期的系统学习，我对数据库系统的理论基础与实践操作有了更加扎实的掌握。学习过程中，我主要在复杂 SQL 语法、多表连接以及范式理论等内容上遇到了一些困难，但通过查阅资料、反复练习、向同学请教以及借助大模型辅助，我逐步克服了这些问题。综合本学期的学习收获、作业完成情况以及个人在学习中投入的时间与精力，我为自己本学期的表现评分为 47 分。
