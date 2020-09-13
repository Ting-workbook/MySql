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
##### 注：这里并没有创建外键 #####
## 1、inner join 查询 （内连接）
```sql
  select * from person inner join card on person.cardId=card.id;    
  -- 内连查询，其实就是两张表中的数据，通过某个字段相等，查询出相关记录数据。（ 把inner join 改成 join 得到的结果也是一样的 ）
  -- 此时 person 表里的“王五”就没有被查询出来
```
## 2、left join 查询 （左外连接）
```sql
  select * from person left join card on person.cardId=card.id;
  --  左外连接，会把左边表格里的所有数据取出来，而右边表格的数据，如果有相等的就显示出来，如果没有就会补NULL
```
```sql
  select * from person left outer join card on person.cardId=card.id;
  -- 和上面的结果是一样的
```
## 3、right jion 查询 （右外连接）
```sql
  select * from person right join card on person.cardId=card.id;
  --  右外连接，会把右边表格里的所有数据取出来，而左边表格的数据，如果有相等的就显示出来，如果没有就会补NULL
```
```sql
  select * from person right outer join card on person.cardId=card.id;
  -- 和上面的结果是一样的
```
## full join 查询 （全外连接）
##### 注：MySQL 是不支持全外连接的 #####
```sql
  select * from person left join card on person.cardId=card.id
  union
  select * from person right join card on person.cardId=card.id;
  -- mysql 中的全外连接只能将左外连接和右外连接合在一起
```









