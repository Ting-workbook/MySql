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













