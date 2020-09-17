# MySQL 事务
## 事务用来做什么
MySQL 中事务其实是一个最小的不可分割的工作单元，事务能够保证一个业务的完整性。
* 比如我们的银行转账
```sql
-- a-> -100 (a账户支出100元)
  update user set money=money-100 where name='n';
-- b-> +100 (b账户收入100元)
  update user set money=money+100 where name='n';
-- 实际的程序中，只有一条语句执行成功了，而另外一条没有执行成功。
-- 出现数前后不一致。
```
##### 多条 sql 语句，可能会有同时成功的要求，要么就同时失败。 #####

## 如何控制事务commit、rollback
### 1、默认是开启事务的（自动提交）。
```sql
  select @@autocommit;
```
* 默认事务开启的作用是：当我们去执行一个 SQL 语句的时候，效果会立马显现出来，且不能回滚。
```sql
  create datebase bank;
  create table user(
      id int primary key,
      name varchar(20),
      money int
  );
  insert into user values(1,'a',1000);
-- 事务回滚：撤销 SQL 语句执行效果
  rollback;
-- 结果还是未能撤销，插入的那条语句还是存在
```
### 2、设置 MySQL 自动提交为 false （手动提交）
* 设置自动提交为 false 之后，再插入就相当于进入暂存，此时可以回滚。需要手动提交：commit; 之后才不能回滚。
```sql
  set autocommit=0;
-- 上面的操作关闭了 MySQL 的自动提交 （commit）
  insert into user values(2,'b',1000);
-- 此时查询结果时可以看到 user 表中有 a、b 两条记录
  rollback;
-- 此时回滚就起了作用，再查看结果时 user 表中就只有一条 a 记录
```
```sql
  set autocommit=0;
-- 上面的操作关闭了 MySQL 的自动提交 （commit）
  insert into user values(2,'b',1000);
-- 此时查询结果时可以看到 user 表中有 a、b 两条记录
  commit;
-- 手动提交数据，这时再撤销的话是不可以的 （持久性）
  rollback;
-- 这时候再回滚就没有效果了，查询 user 表中还是 a、b 两条记录
```
### 3、如果说这个时候a向b转账
```sql
  update user set money=money-100 where name='a';
  update user set money=money+100 where name='b';
-- 此时 a 账户上显示900，b 账户上显示1100。
  rollback;
-- 此时撤回成功，a 账户1000，b账户1000。
```
##### 事务给我们提供一个反悔的机会 #####
##### 如果觉得没有问题不需要修改可以提交了的话，可以手动commit ######

## 手动开启事务 begin、start transaction
##### “ begin; ”或者“ start transaction; ”都可以帮我们手动开启一个事务 #####
* 还原 autocommit 的默认值
```sql
  set autocommit=1;
```
```sql
  select * from user;
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 1000  |
| 2  | b    | 1000  |
```
* 转账
```sql
  update user set money=money-100 where name='a';
  update user set money=money+100 where name='b';
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 900   |
| 2  | b    | 1100  |
```
* 事务回滚
```sql
  rollback;    -- 发现现在回滚无效了，并没有被撤销
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 900   |
| 2  | b    | 1100  |
```
### 1、使用 begin
```sql
  begin;
```
* 转账
```sql
  update user set money=money-100 where name='a';
  update user set money=money+100 where name='b';
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 800   |
| 2  | b    | 1200  |
```
* 事务回滚
```sql
  rollback;     -- 现在可以被回滚（撤销）
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 900   |
| 2  | b    | 1100  |
```
##### 手动开启事务是可以被回滚撤回的 #####
### 2、使用 start transaction
```sql
  start transaction;
```
* 转账
```sql
  update user set money=money-100 where name='a';
  update user set money=money+100 where name='b';
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 800   |
| 2  | b    | 1200  |
```
* 事务回滚
```sql
  rollback;    -- 可以被回滚（撤回）
```
* 结果如下：
```
| id | name | money |
|----|------|-------|
| 1  | a    | 900   |
| 2  | b    | 1100  |
```
##### 同上手动开启事务时可以被回滚撤销的 #####
##### 如果开启一个事务需要它真实生效（不可撤销）可以再转账语句之后提交“ commit; ” #####

## 事务 - ACID 四大特征于使用
1. A 原子性：事务时最小的单位，不可再分割。
2. C 一致性：事务要求，同一事务中的 SQL 语句，必须保证同时成功或者同时失败。
3. I 隔离性：事务1 和 事务2 之间是具有隔离性的。
4. D 持久性：事务一旦结束（commit），就不可以返回（rollback）。
* 事务开启：
```sql
  set autocommit=0;     -- 修改默认提交
  begin;     -- 事务开启
  start transaction;      -- 事务开启
```
* 事务手动提交
```sql
  commit;
```
* 事务手动回滚
```sql
  rollback;
```
### 事务的隔离性 - 脏读
```sql
-- bank 数据库     user 表
  insert into user values(3,'小明',1000);
  insert into user values(4,'淘宝店',1000);
```
* 结果如下：
```
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 1000  |
| 4  | 淘宝店 | 1000  |
```
* 查看数据的隔离级别
```sql
-- 如果是查看 MySQL 8.0 的隔离级别的话
  select @@global.transaction_isolation;    -- 系统级别的
  select @@transaction_isolation;    -- 会话级别的
  
--如果是查看 MySQL 5.x 的隔离级别的话
  select @@global.tx_isolation;     -- 系统级别的
  select @@tx.isolation;      -- 会话级别的
  
```

#### 1、read uncommitted; 读未提交的
##### 如果有 事务a 和 事务b，事务a 对数据进行操作，在操作过程中，事务没有被提交，但是 b 可以看见 a 操作的结果。 #####
* 修改隔离级别为 read uncommitted
```sql
  set global transaction isolation level read uncommitted;
```
* 转账：小明在淘宝店买鞋子：800块钱（小明->成都ATM  淘宝店->广州ATM）
```sql
  start transaction;
  update set money=money-800 where name='小明';
  uodate set money=money+800 where name='淘宝店';
```
* 结果如下：
```
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 200   |
| 4  | 淘宝店 | 1800  |
```
* 给淘宝店打电话，让淘宝店查一下是不是到账了，步骤如下：
```sql
  use bank;
  select * from user;
```
* 结果如下：
```
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 200   |
| 4  | 淘宝店 | 1800  |
```
* 发货
* 淘宝店主晚上请女友吃饭，正好花了1800
* 小明在成都 rollback;
* 现在结果如下：
```
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 1000  |
| 4  | 淘宝店 | 1000  |
```
* 结账发现钱不够
##### 如果两个不同的地方，都在进行操作，如果 事务a 开启之后，他的数据可以被其他事务读取到。这样就会出现脏读。 #####
##### 脏读：一个事务读到了另一个事务没有提交的数据，这就叫做脏读。实际开发中是不允许出现的。 #####

#### 2、read committed; 读已经提交的
* 修改隔离级别
```sql
  set global transaction isolation level read committed;           -- 修改隔离级别
  select @@global.transaction_isolation;            -- 查看隔离级别
```
* 事例背景
```sql
-- bank 数据库       user 表
-- 小张：银行会计
  start transaction;        -- 小张开启事务做报表
  select * from user;        -- 小张查看数据
/**
结果如下：
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 1000  |
| 4  | 淘宝店 | 1000  |
**/

-- 小张去厕所抽烟
-- 小王：去开了一个户
  use bank;
  start transaction;
  insert into user values(5,'c',100);
  commit;
  select * from user;
/**
结果如下：
| id |  name  | money |
|----|--------|-------|
| 1  |  a     | 800   |
| 2  |  b     | 1200  |
| 3  | 小明   | 1000  |
| 4  | 淘宝店 | 1000  |
| 5  |  c     | 100   |
**/

-- 小张抽完烟回来了，算了一下他们的平均数
  select avg(monney) from user;
/**
结果如下：
| avg(money) |
|------------|
| 820.0000   |
**/

-- money 的平均值不是 1000，变少了？
```
##### 虽然我只能读到另外一个事务提交的数据，但是还是会出现问题，就是读取同一个数据发现钱会不一致。 #####
##### 上面的现象叫做  不可重复现象：这种现象在 read committed; 会出现。 #####

#### 3、

#### 4、














