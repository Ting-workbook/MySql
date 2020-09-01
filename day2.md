
# 终端操作MgSql
## 一、如何登录数据库服务器以及创建数据库，创建表，查询数据库，查询表，查询记录
 官网下载安装MySql  
 MySql是关系型数据库  
  1. 登录数据库    
   ```sql
      mysql -uroot -[密码]
   ```
  2. 在数据库服务器中创建数据库“test”  
   ```sql
      create database test;
   ```  
  3. 查询数据库服务器中所有的数据库 
   ```sql
      show database;
   ```
  4. 如何选中某一个数据库（sushe）进行操作  
   ```sql
      use sushe;
   ```
  5. 查询表（admin是数据库sushe中的一个表）中所有的记录（注：只有选中了数据库才可以查询）
   ```sql
      select * from admin;
   ```
   查询admin表中Admin_ID为1的数据  
   ```sql
      select * from admin where Admin_ID=1;
   ```
 6. 查看某个数据库中所有的数据表 
   ```sql
      show tables;
   ```
 7. 创建一个数据表“pet”
   ```sql
      CREATE TABLE pet (
          name VARCHAR(20),
          owner VARCHAR(20),
          species VARCHAR(20),
          sex CHAR(10),
          birth DATE,
          death DATE);
   ```
8. 查看具体数据表“pet”的结构
   ```sql
      describe pet;     (或者 desc pet;)
   ```
9. 往数据表中添加记录
   ```sql
      INSERT INTO pet
      VALUES('puffball','Diane','hamster','f','1999-03-30',NULL);
   ```
10. 退出数据库服务器
   ```sql
      exit;
   ```

## 二、MySQL常用的数据类型
   MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串（字符）类型。  
   **1、数值类型**
   ```
      类型                      大小           范围（有符号）                                       范围（无符号）               用途
      TINYINT                  1字节          （-128，127）                                       （0，255）                   小整数值
      SMALLINT                 2字节          （-32768，32768）                                   （0，65535）                 大整数值
      MEDIUMINT                3字节          （-8388608，8388608）                               （0，16777215）              大整数值
      INT或INTEGER             4字节          （-2147483648，2147483648）                         （0，4294967295）            大整数值
      BIGINT                   8字节          （-9233372036854775808，9233372036854775807）                                   极大整数值
      FLOAT（浮点数值）         4字节          （）                                                （）                        单精度浮点数
      DOUBLE（浮点数值）        8字节          （）                                                （）                        双精度浮点数
   ```
   **2、日期/时间**
   ```
      类型               大小         范围                                                    格式                       用途
      DATE              3字节        1000-01/9999-12-31                                      YYYY-MM-DD                 日期值
      TIME              3字节        '-838:59:59'/'838:59:59'                                HH:MM:SS                   时间值或持续时间
      YEAR              1字节        1901/2155                                               YYYY                       年份值
      DATETIME          8字节        1000-01-01 00:00:00/9999-12-31 23:59:59                 YYYY-MM-DD HH:MM:SS        混合日期和时间值
      TIMESTAMP         4字节        1970-01-01 00:00:00/2038，结束时间是第2147483647秒        YYYYMMDD HHMMSS           混合日期和时间值，时间戳
   ```
   **3、字符串（字符）**  
   ```
      类型               大小                    用途
      CHAR              0-255字节               定长字符串
      VARCHAR           0-65535字节             变长字符串
      TINYBLOB          0-255字节               不超过255个字符的二进制字符串
      TINYTEXT          0-255字节               短文本字符串
      BLOB              0-65535字节             二进制形式的长文本数据
      TEXT              0-65535字节             长文本数据
      MEDIUMBLOB        0-16777215字节          二进制形式的中等长度文本数据
      MEDIUMTEXT        0-16777215字节          中等长度文本数据
      LONGBLOB          0-4294967295字节        二进制形式的极大文本数据
      LONGTEXT          0-4294967295字节        极大文本数据
   ```
    数据类型如何选择？   日期选择拿着格式，数值和字符串按照大小！  
   
## 三、如何插入下列数据表？
```
   +---------------+-------------+-------------+---------+-----------------+------------------+  
   |  name         |  owner      |  species    |  sex    |  birth          |  death           |  
   +---------------+-------------+-------------+---------+-----------------+------------------+  
   |  Fluffy       |  Harold     |  cat        |  f      |  1993-02-04     |  NULL            |  
   |  Claws        |  Gwen       |  cat        |  m      |  1994-03-17     |  NULL            |  
   |  Buffy        |  Harold     |  dog        |  f      |  1989-05-13     |  NULL            |  
   |  Fang         |  Benny      |  dog        |  m      |  1990-08-27     |  NULL            |  
   |  Bowser       |  Diane      |  dog        |  m      |  1979-08-31     |  1995-07-29      |  
   |  Chirpy       |  Gwen       |  bird       |  f      |  1998-09-11     |  NULL            |  
   |  Whistler     |  Gwen       |  bird       |  NULL   |  1997-12-09     |  NULL            |  
   |  Slim         |  Benny      |  snake      |  m      |  1996-04-29     |  NULL            |  
   |  Puffball     |  Diane      |  hamster    |  f      |  1999-03-30     |  NULL            |  
   +---------------+-------------+-------------+---------+-----------------+------------------+  
 ```
     
   ```sql
   INSERT INTO pet VALUES('Fluffy','Harold','cat','f','1993-02-04',NILL);
   INSERT INTO pet VALUES('Claws','Gwen','cat','m','1994-03-17',NILL);
   INSERT INTO pet VALUES('Buffy','Harold','dog','f','1989-05-13',NILL);
   INSERT INTO pet VALUES('Fang','Benny','dog','m','1990-08-27',NILL);
   INSERT INTO pet VALUES('Bowser','Diane','dog','m','1979-08-31','1995-07-29');
   INSERT INTO pet VALUES('Chirpy','Gwen','bird','f','1998-09-11',NILL);
   INSERT INTO pet VALUES('Whistler','Gwen','bird','NULL,'1997-12-09',NILL);
   INSERT INTO pet VALUES('Slim','Benny','snake','m','1996-04-29',NILL);
   INSERT INTO pet VALUES('Puffball','Diane','hamster','f','1999-03-30',NILL);
   ```
   
## 四、如何查看数据
   **1、查看pet表中的所有记录**
   ```sql
      select * from pet;
   ```
   **2、**
## 五、如何删除数据  
   **1、删除宠物名字是Fluffy的记录**  
   ```sql
      delete from pet where name='Fluffy';
   ```
   2、
## 六、如何修改数据
   **1、把宠物“旺财”的名字修改成“旺旺财”**  
   ```sql
      update pet set name ='旺旺财' where owner='周星驰';
   ```   
   **2、**
## 七、MySQL建表约束  
### 1、主键约束   
它们能够唯一确定一张表中的一条记录，也就是我们通过给某个字段添加约束，就可以使得该字段不重复且不为空。  
1. 创建一个id有主键约束的表    
   ``` 
      create table user(  
          id int primary key,  
          name varchar(20)  
      );  
      insert into user values(1,'张三');  
      insert into user values(1,'张三');         -- × 这里会报错，因为“1”只能插入一次，因为“id”是主键  
      insert into user values(2,'张三');         -- √ 改成这样就没有问题了  
      insert into user values(NULL,'张三');      -- × 这里也会报错，因为主键内容不可为空  
   ``` 
2. 创建一个id和name有联合主键约束的表    
  ```sql  
      create table user2(
          id int,
          name varchar(20),
          password varchar(20),
          primary key(id,name)
      );
      insert into user2 values(1,'张三','123');
      insert into user2 values(1,'张三','123');        -- × 主键不能重复
      insert into user2 values(2,'张三','123');        -- √ 联合主键值只要两个加起来不重复就行
      insert into user2 values(1,'李四','123');        -- √ 联合主键值只要两个加起来不重复就行
      insert into user2 values(NULL,'李四','123');     -- × 联合主键不能为空
   ```  
### 2、自增约束   
自增约束和逐渐约束搭配使用可以自动帮助我们管控主键的值，让它自动增长。   
1. 创建一个user3表，id作为自增约束  
   ```sql  
      create table user3(
          id int primary key auto-increment,
          name varchar(20)
      );
      insert into user3 (name) values('zhangsan');       -- 插入一个“zhangsan”可以自动生成一个id值为1
      insert into user3 (name) values('zhangsan');       -- 插入一个“zhangsan”可以自动生成一个id值为2
   ```    
2. 如果创建表的时候忘记创建主键约束了，该怎么办？  
   ```sql   
      create table user4(
          id int,
          name varchar(20)
      );
      alter table user4 add primary key(id);             -- 给表user4增加一个主键  （注：alter table 是修改表结构）
      alter table user4 drop primary key;                -- 删除主键  （注：drop 是删除）
      alter table user4 modify id int primary key;       -- 通过修改字段的方式添加主键  （注：modify 是修改字段）
   ```   
### 3、唯一约束  
约束修饰的字段的值不可以重复（可以为空）  
1. 创建一个user5，再对name添加一个唯一约束  
   ```sql
      create table user5(
          id int,
          name varchar(20)
      );
      alter table user5 add unique(name);          -- 添加一个唯一约束
      insert into user5 values(1,'zhangsan');      -- √
      insert into user5 values(1,'zhangsan');      -- × “zhangsan”重复了
      insert into user5 values(1,'lisi');          -- √ 
   ```  
2. 创建一个user6，在创建的时候直接对name添加唯一约束   
   
    ```sql  
      create table user6(
          id int,
          name varchar(20),
          unique(name)              -- 添加唯一约束
      );
    ``` 
    
3. 创建一个user7，在创建的时候直接对name添加唯一约束   
      ```sql
      create table user7(  
          id int,   
          name varchar(20) unique          -- 直接添加唯一约束    
      );  
      ```
4. 创建一个user8，在创建的时候直接对id和name添加唯一约束  
   ```sql
      create table user8(  
          id int,  
          name varchar(20),  
          unique(id,name)              -- 对id和name添加唯一约束   （注：两个组合在一不重复就行）  
      );  
      insert into user8 values(1,'zhangsan');       -- √    
      insert into user8 values(1,'zhangsan');       -- ×  
      insert into user8 values(2,'zhangsan');       -- √  
      insert into user8 values(1,'lisi');           -- √  
   ```
5. 删除唯一约束  
   ```sql
      alter table user7 drop index name;  
   ```
6. 通过modify的形式添加唯一约束  
   ```sql
      alter table user7 modify name varchar(20) unique;  
   ```
### 4、非空约束 
修饰的字段不能为空NULL  
1. 创建表user9，name值设置非空约束  
   ```sql   
      create table user9(
          id int,
          name varchar(20) not null      -- 直接添加非空约束
      );
      insert into user9 (id) values(1);           -- × name有非空约束，且没有设默认值，传值的时候又没有给一个值所以会报错
      insert into user9 values(1,'张三');         -- √
      insert into user9 (name) values('lisi');    -- √
   ```    
### 5、默认约束 
当我们插入字段值的时候如果没有传值就会使用默认值  
1. 创建表user10，对age使用默认约束  
   ```sql   
      create table user10(
          id int,
          name varchar(20)，
          age int default 10
      );
      insert into user10 (id,name) values(1,'zhangsan');      -- √ age会填入默认值10，如果传了值就不会使用默认值
   ```  
### 6、外键约束  
涉及到两个表：父表，子表（主表，副表）  
1. ---创建班级表  
   ```sql  
      create table classes(
          id int primary key,
          name varchar(20)
      );
   ```   
2. ---创建学生表  
   ```sql   
      create table students(  
          id int primary key,  
          name varchar(20),  
          class_id int,  
          foreign key (class_id) references classes(id)    -- students表里的class_id的值必须来自于classes表里的id字段   
      );  
   ```     
3. --向students表中插入数据  
   ``` sql   
       insert into students values(1001,'张三',1);  
       insert into students values(1002,'张三',2);  
       insert into students values(1003,'张三',3);  
       insert into students values(1004,'张三',4);  
       insert into students values(1005,'李四',5);    -- × 外键约束失败，主表 class 中没有的数据值，在副表 students 中是不可以使用的。
   ```   
4. --向classes表中插入数据  
   ```sql    
      insert into classes values(1,'一班');  
      insert into classes values(2,'二班');  
      insert into classes values(3,'三班');  
      insert into classes values(4,'四班'); 
   ```   
   ```sql
   delete from classes where id=4;    -- × 不能删除被 students 表已经引用的数据
   ```
**总结:**  
 * 主表（父表） class 中没有的数据值，在副表（子表） students 中是不可以使用的。  
 * 主表（父表）中的记录被副表（子表）引用，是不可以被删除的。  
        
**附加：**  
* 添加约束的方式：  
   --建表的时候就添加约束  
   --可以使用alter 。。。add 。。。  
   --alter 。。。modify 。。。  
* 删除约束的方式：  
   --alter 。。。drop 。。。  
* 查看表的结构：  
   --desc 。。。
### 数据库的三大设计范式
1. 第一范式（1NF）  
数据表中的所有字段都是不可分割的原子值  
--创建一个student2表  
```sql
  create table student2(
      id int primary key,
      name varchar(20),
      address varchar(30)
  );
```  
--向student2中插入数据  
```sql
  insert into student2 values(1,'张三','中国四川省成都市武侯区武侯大道100号');
  insert into student2 values(2,'李四','中国四川省成都市武侯区京城大道200号');
  insert into student2 values(3,'王五','中国四川省成都市高新区天府大道90号');
```
像这种字段值还可以继续拆分的，就不满足第一范式  
  
--创建一个student3表
```sql
  create table student3(
      id int primary key,
      name varchar(20),
      cuntry varchar(30),
      privence varchar(30),
      city varchar(30),
      details varchar(30)
  );
```  
--向student3表中插入数据
```sql
  insert into student3 values(1,'张三','中国','四川省','成都市','武侯区武侯大道100号');
  insert into student3 values(2,'李四','中国','四川省','成都市','武侯区京城大道200号');
  insert into student3 values(3,'王五','中国','四川省','成都市','高新区天府大道90号');
```
注：范式设计得越详细，对于某些实际操作可能更好，但是不一定都是好处。  
   
2. 第二范式  
必须是满足第一范式的前提下，第二范式要求，除主键外的每一列都必须完全依赖于主键。  
如果要出现不完全依赖，只可能发生在联合主键的情况下。  
———— 创建一个订单表  
```sql
  create table myorder(
      product_id int,
      customer_id int,
      product_name varchar(20),
      customer_name varchar(20),
      primary key(product_id,customer_id)
  );
```
这里出现的问题：  
————除主键以外的其他列，只依赖于主键的部分字段。（比如这里的product_name只依赖于product_id）  
   
解决方案：  
————拆表  
```sql
  create table myorder(
      order_id int primary key,
      product_id int,
      customer_id int
  );
  
  create table product(
      id int primary key,
      name varchar(20)
  );
  
  create table customer(
      id int primary key,
      name varchar(20)
  );
```  
————分成三个表之后，就满足了第二范式的设计！  
   
3. 第三范式（3NF）  
必须先满足第二范式，除开主键列的其他列之间不能有传递依赖关系。  
```sql
  create table myorder(
      order_id int primary key,
      product_id int,
      customer_id int,
      customer_phone varchar(15)
  );
```
解决方案：  
————拆表  
```sql
  create table myorder(
      order_id int primary key,
      product_id int,
      customer_id int
  );
  create table customer(
      id int primary key,
      name varchar(20),
      phone varchar(15)
  );
```


   
   
   
   
   
   
   

