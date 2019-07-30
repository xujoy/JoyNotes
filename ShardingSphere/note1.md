#### 1. DDL (数据定义语言 Data Definition Language)

建议、修改、删除数据库对象
Create 、Alter、Drop、Truncate

#### 2. DML(数据操作语言 D Manipulation L)

修改数据表数据，和事务相关.执行完后-->事务控制-->改变数据库
Insert、Update、Delete

#### 3. TCL(事务控制语言 Transaction Control Language)

维护数据一致性
Commit、Rollbach、SavePoint

#### 4. DQL(数据查询语言 D Query L)

查询数据
Select

#### 5. DCL(数据控制语言 D Control L)

执行权限的授予和收回操作

Grant、Revoke、Create User

------

### 1.解析引擎

1.4.x ----- Before--->Druid

1.5.x -------After --->自研SQL解析引擎

3.0.x -------After---> ANTLR
	DDL -> TCL -> DAL –> DCL -> DML –>DQL

### 2.路由引擎

	t_order和t_order_item 关联表通过 order_id 的奇偶进行数据分片

单表SQL

```sql
SELECT * FROM t_order WHERE order_id IN (1, 2);
```

路由SQL:

```sql
SELECT * FROM t_order_0 WHERE order_id IN (1, 2);
SELECT * FROM t_order_1 WHERE order_id IN (1, 2);
```

单表关联SQL:

```sql
SELECT * FROM t_order o JOIN t_order_item i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
```

路由关联SQL:

```sql
SELECT * FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
SELECT * FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
```

笛卡尔路由SQL(慎用):

```sql

SELECT * FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
SELECT * FROM t_order_0 o JOIN t_order_item_1 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
SELECT * FROM t_order_1 o JOIN t_order_item_0 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);
SELECT * FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id  WHERE order_id IN (1, 2);

```

笛卡尔积：
	集合 A {0,1} ,	集合 B {0,1} 	
	笛卡尔积：A*B= { (0,0) , (0,1 ) , (1,0) , (1,1)}


		
		











```

```