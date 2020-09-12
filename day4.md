# SQL 的四种链接查询
## 内连接
1. inner join 或者 join
## 外连接
1. 左连接 left join 或者 left outer jion
2. 右连接 right join 或者 right outer join
3. 完全外连接 full join 或者 full outer join
## 创建两个表
```sql
  create database testJion;
```
* person 表
```
id
name
cardId
```
```sql
-- 创建表
  create table person(
      id int,
      name varchar(20),
      cardId int
  );
```
```sql
-- 添加数据
  insert into person values(1,'张三',1);
  insert into person values(2,'李四',3);
  insert into person values(3,'王五',6);
```
* card 表
```
id
name
```
```sql
-- 创建表
  create table card(
      id int,
      name varchar(20)
  );
```
```sql
-- 添加数据
  insert into card values(1,'饭卡');
  insert into card values(2,'建行卡');
  insert into card values(3,'农行卡');
  insert into card values(4,'工商卡');
  insert into card values(5,'邮政卡');
```
###  注：这里并没有创建外键 ###
##








