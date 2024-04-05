

|              | SQL 数据库                                                   | NoSQL 数据库                                                 |
| :----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 数据存储模型 | 结构化存储，具有固定行和列的表格                             | 非结构化存储。文档：JSON 文档，键值：键值对，宽列：包含行和动态列的表，图：节点和边 |
| 发展历程     | 开发于 1970 年代，重点是减少数据重复                         | 开发于 2000 年代后期，重点是提升可扩展性，减少大规模数据的存储成本 |
| 例子         | Oracle、MySQL、Microsoft SQL Server、PostgreSQL              | 文档：MongoDB、CouchDB，键值：Redis、DynamoDB，宽列：Cassandra、 HBase，图表：Neo4j、 Amazon Neptune、Giraph |
| ACID 属性    | 提供原子性、一致性、隔离性和持久性 (ACID) 属性               | 通常不支持 ACID 事务，为了可扩展、高性能进行了权衡，少部分支持比如 MongoDB 。不过，MongoDB 对 ACID 事务 的支持和 MySQL 还是有所区别的。 |
| 性能         | 性能通常取决于磁盘子系统。要获得最佳性能，通常需要优化查询、索引和表结构。 | 性能通常由底层硬件集群大小、网络延迟以及调用应用程序来决定。 |
| 扩展         | 垂直（使用性能更强大的服务器进行扩展）、读写分离、分库分表   | 横向（增加服务器的方式横向扩展，通常是基于分片机制）         |
| 用途         | 普通企业级的项目的数据存储                                   | 用途广泛比如图数据库支持分析和遍历连接数据之间的关系、键值数据库可以处理大量数据扩展和极高的状态变化 |
| 查询语法     | 结构化查询语言 (SQL)                                         | 数据访问语法可能因数据库而异                                 |



## 基本类型



![](https://oss.javaguide.cn/github/javaguide/mysql/summary-of-mysql-field-types.png)

## 语句

### DDL

对于 数据库 ( **DATABASE** ) 数据表 ( **TABLE** ) 视图 ( **VIEW** ) 索引 ( **INDEX** ) 的语句

`CREATE`、`ALTER`、`DROP`

```sql
# 普通创建
CREATE TABLE user (
  id int(10) unsigned NOT NULL COMMENT 'Id',
  username varchar(64) NOT NULL DEFAULT 'default' COMMENT '用户名',
  password varchar(64) NOT NULL DEFAULT 'default' COMMENT '密码',
  email varchar(64) NOT NULL DEFAULT 'default' COMMENT '邮箱'
) COMMENT='用户表';

# 根据已有的表创建新表
CREATE TABLE vip_user AS SELECT * FROM user;

/* -------------------------------------------- */

# 删除数据表，包括数据和结构
DROP TABLE user;

/* -------------------------------------------- */

# 修改数据表
ALTER TABLE user
ADD age int(3);

ALTER TABLE user
DROP COLUMN age;

ALTER TABLE `user`
MODIFY COLUMN age tinyint;

ALTER TABLE user
ADD PRIMARY KEY (id);

ALTER TABLE user
DROP PRIMARY KEY;
```



### DML

`insert` `update` `delete` `select`

```sql
# 插入一行
INSERT INTO user
VALUES (10, 'root', 'root', 'xxxx@163.com');
# 插入多行
INSERT INTO user
VALUES (10, 'root', 'root', 'xxxx@163.com'), (12, 'user1', 'user1', 'xxxx@163.com'), (18, 'user2', 'user2', 'xxxx@163.com');

/* -------------------------------------------- */

UPDATE user
SET username='robot', password='robot'
WHERE username = 'root';

/* -------------------------------------------- */

# 删除表中的指定数据
DELETE FROM user WHERE username = 'robot';

# 清空表中的所有数据
TRUNCATE TABLE user;

/* -------------------------------------------- */

select		5
		...
	from		1
		...
	where		2 /* 运算符 = <> > < >= <= Between Like In And Or Not */
		...
	group by	3 /* 分组汇总 */
		...
	having		4 /* 对汇总的结果进行过滤 */
		...
	order by	6 /* 对一个列或者多个列排序 默认 ASC 升序, DESC 降序 */
		...
	limit		7 
		...;
```



### TCL

事务处理语句，事务具有ACID特性

1. **原子性**（`Atomicity`）：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
2. **一致性**（`Consistency`）：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；
3. **隔离性**（`Isolation`）：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
4. **持久性**（`Durability`）：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

🌈 这里要额外补充一点：**只有保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。也就是说 A、I、D 是手段，C 是目的！**

---

不能回退 `SELECT` 语句，回退 `SELECT` 语句也没意义；也不能回退 `CREATE` 和 `DROP` 语句。

**MySQL 默认是隐式提交**，每执行一条语句就把这条语句当成一个事务然后进行提交。当出现 `START TRANSACTION` 语句时，会关闭隐式提交；当 `COMMIT` 或 `ROLLBACK` 语句执行后，事务会自动关闭，重新恢复隐式提交。

- `START TRANSACTION` - 指令用于标记事务的起始点。

- `SAVEPOINT` - 指令用于创建保留点。

- `ROLLBACK TO` - 指令用于回滚到指定的保留点；如果没有设置保留点，则回退到 `START TRANSACTION` 语句处。

- `COMMIT` - 提交事务。

```sql
-- 开始事务
START TRANSACTION;

-- 插入操作 A
INSERT INTO `user`
VALUES (1, 'root1', 'root1', 'xxxx@163.com');

-- 创建保留点 updateA
SAVEPOINT updateA;

-- 插入操作 B
INSERT INTO `user`
VALUES (2, 'root2', 'root2', 'xxxx@163.com');

-- 回滚到保留点 updateA
ROLLBACK TO updateA;

-- 提交事务，只有操作 A 生效
COMMIT;

```

---

SQL 标准定义了四个隔离级别：

- **READ-UNCOMMITTED(读取未提交)** ：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
- **READ-COMMITTED(读取已提交)** ：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。
- **REPEATABLE-READ(可重复读)** ：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- **SERIALIZABLE(可串行化)** ：最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。

|                隔离级别                | 脏读 | 不可重复读 | 幻读 |
| :------------------------------------: | :--: | :--------: | :--: |
|            READ-UNCOMMITTED            |  √   |     √      |  √   |
|             READ-COMMITTED             |  ×   |     √      |  √   |
| REPEATABLE-READ（InnoDB 默认隔离级别） |  ×   |     ×      |  √   |
|              SERIALIZABLE              |  ×   |     ×      |  ×   |

### DCL

   权限控制

`GRANT` `REVOKE`



## 连接

JOIN 是“连接”的意思，顾名思义，SQL JOIN 子句用于将两个或者多个表联合起来进行查询。

连接表时需要在每个表中选择一个字段，并对这些字段的值进行比较，值相同的两条记录将合并为一条。**连接表的本质就是将不同表的记录合并起来，形成一张新表。当然，这张新表只是临时的，它仅存在于本次查询期间**。

使用 `JOIN` 连接两个表的基本语法如下：

```sql
select table1.column1, table2.column2...
from table1
join table2
on table1.common_column1 = table2.common_column2;
```

`table1.common_column1 = table2.common_column2` 是连接条件，只有满足此条件的记录才会合并为一行。您可以使用多个运算符来连接表，例如 =、>、<、<>、<=、>=、!=、`between`、`like` 或者 `not`，但是最常见的是使用 =。

---

**`ON` 和 `WHERE` 的区别**：

- 连接表时，SQL 会根据连接条件生成一张新的临时表。`ON` 就是连接条件，它决定临时表的生成。
- `WHERE` 是在临时表生成以后，再对临时表中的数据进行过滤，生成最终的结果集，这个时候已经没有 JOIN-ON 了。

所以总结来说就是：**SQL 先根据 ON 生成一张临时表，然后再根据 WHERE 对临时表进行筛选**。

------

SQL 允许在 `JOIN` 左边加上一些修饰性的关键词，从而形成不同类型的连接，如下表所示：

| 连接类型                                 | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| INNER JOIN 内连接                        | （默认连接方式）只有当两个表都存在满足条件的记录时才会返回行。 |
| LEFT JOIN / LEFT OUTER JOIN 左(外)连接   | 返回左表中的所有行，即使右表中没有满足条件的行也是如此。     |
| RIGHT JOIN / RIGHT OUTER JOIN 右(外)连接 | 返回右表中的所有行，即使左表中没有满足条件的行也是如此。     |
| FULL JOIN / FULL OUTER JOIN 全(外)连接   | 只要其中有一个表存在满足条件的记录，就返回行。               |
| SELF JOIN                                | 将一个表连接到自身，就像该表是两个表一样。为了区分两个表，在 SQL 语句中需要至少重命名一个表。 |
| CROSS JOIN                               | 交叉连接，从两个或者多个连接表中返回记录集的笛卡尔积。       |

<img src="https://oss.javaguide.cn/p3-juejin/701670942f0f45d3a3a2187cd04a12ad~tplv-k3u1fbpfcp-zoom-1.png" style="zoom:80%;" />

------

## 锁

### Lock_Type —— 锁类型

锁类型基本分为表级锁(Table)和行级锁(Record)

MyISAM 仅仅支持表级锁(table-level locking)，一锁就锁整张表，这在并发写的情况下性非常差。InnoDB 不光支持表级锁(table-level locking)，还支持行级锁(row-level locking)，默认为行级锁。

行级锁的粒度更小，仅对相关的记录上锁即可（对一行或者多行记录加锁），所以对于并发写入操作来说， InnoDB 的性能更高。

**表级锁和行级锁对比**：

- **表级锁：** MySQL 中锁定粒度最大的一种锁（全局锁除外），是 **针对非索引字段加的锁** ，对当前操作的整张表加锁，实现简单，资源消耗也比较少，加锁快，不会出现死锁。不过，触发锁冲突的概率最高，高并发下效率极低。表级锁和存储引擎无关，MyISAM 和 InnoDB 引擎都支持表级锁。
- **行级锁：** MySQL 中锁定粒度最小的一种锁，是 **针对索引字段加的锁** ，只针对当前操作的行记录进行加锁。 行级锁能大大减少数据库操作的冲突。其加锁粒度最小，并发度高，但加锁的开销也最大，加锁慢，会出现死锁。行级锁和存储引擎有关，是在存储引擎层面实现的。
  - **行锁（Record Lock）**：也被称为记录锁，属于单个行记录上的锁。
  - **间隙锁（Gap Lock）**：锁定一个范围，不包括记录本身。
  - **临键锁（Next-Key Lock）**：Record Lock+Gap Lock，锁定一个范围，包含记录本身，主要目的是为了解决幻读问题（MySQL 事务部分提到过）。记录锁只能锁住已经存在的记录，为了避免插入新记录，需要依赖间隙锁。

### Lock_Mode —— 锁模式

S, X, IS, IX, and gap locks

不论是表级锁还是行级锁，都存在共享锁（Share Lock，S 锁）和排他锁（Exclusive Lock，X 锁）这两类：

- **共享锁（S 锁）**：又称读锁，事务在读取记录的时候获取共享锁，允许多个事务同时获取（锁兼容）。
- **排他锁（X 锁）**：又称写锁/独占锁，事务在修改记录的时候获取排他锁，不允许多个事务同时获取。如果一个记录已经被加了排他锁，那其他事务不能再对这条事务加任何类型的锁（锁不兼容）。

排他锁与任何的锁都不兼容，共享锁仅和共享锁兼容。

|      | S 锁   | X 锁 |
| :--- | :----- | :--- |
| S 锁 | 不冲突 | 冲突 |
| X 锁 | 冲突   | 冲突 |



意向锁是表级锁，共有两种：

- **意向共享锁（Intention Shared Lock，IS 锁）**：事务有意向对表中的某些记录加共享锁（S 锁），加共享锁前必须先取得该表的 IS 锁。
- **意向排他锁（Intention Exclusive Lock，IX 锁）**：事务有意向对表中的某些记录加排他锁（X 锁），加排他锁之前必须先取得该表的 IX 锁。

意向锁之间是互相兼容的。

|       | IS 锁 | IX 锁 |
| ----- | ----- | ----- |
| IS 锁 | 兼容  | 兼容  |
| IX 锁 | 兼容  | 兼容  |

意向锁和共享锁和排它锁互斥（这里指的是表级别的共享锁和排他锁，意向锁不会与行级的共享锁和排他锁互斥）。

|      | IS 锁 | IX 锁 |
| ---- | ----- | ----- |
| S 锁 | 兼容  | 互斥  |
| X 锁 | 互斥  | 互斥  |



------

## 索引

