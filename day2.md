
官网下载安装MySql
MySql是关系型数据库





终端操作MgSql

一、如何登录数据库服务器以及创建数据库，创建表，查询数据库，查询表，查询记录  
   1、登录数据库    
   ```
      mysql -uroot -[密码]
   ```
   2、在数据库服务器中创建数据库“test”  
   ```
      create database test;
   ```  
   3、查询数据库服务器中所有的数据库  
   ```
      show database;
   ```
   4、如何选中某一个数据库（sushe）进行操作    
      use sushe;
   5、查询表（admin是数据库sushe中的一个表）中所有的记录（注：只有选中了数据库才可以查询）
      select * from admin;
      查询admin表中Admin_ID为1的数据
      select * from admin where Admin_ID=1;
   6、查看某个数据库中所有的数据表
      show tables;
   7、创建一个数据表“pet”
      CREATE TABLE pet (
          name VARCHAR(20),
          owner VARCHAR(20),
          species VARCHAR(20),
          sex CHAR(10),
          birth DATE,
          death DATE);
   8、查看具体数据表“pet”的结构
      describe pet;     (或者 desc pet;)
   9、往数据表中添加记录
      INSERT INTO pet
      VALUES('puffball','Diane','hamster','f','1999-03-30',NULL);
   10、退出数据库服务器
      exit;
   11、

二、MySQL常用的数据类型
   MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串（字符）类型。
   1、数值类型
      类型                      大小           范围（有符号）                                       范围（无符号）               用途
      TINYINT                  1字节          （-128，127）                                       （0，255）                   小整数值
      SMALLINT                 2字节          （-32768，32768）                                   （0，65535）                 大整数值
      MEDIUMINT                3字节          （-8388608，8388608）                               （0，16777215）              大整数值
      INT或INTEGER             4字节          （-2147483648，2147483648）                         （0，4294967295）            大整数值
      BIGINT                   8字节          （-9233372036854775808，9233372036854775807）                                   极大整数值
      FLOAT（浮点数值）         4字节          （）                                                （）                        单精度浮点数
      DOUBLE（浮点数值）        8字节          （）                                                （）                        双精度浮点数
   2、日期/时间
      类型               大小         范围                                                    格式                       用途
      DATE              3字节        1000-01/9999-12-31                                      YYYY-MM-DD                 日期值
      TIME              3字节        '-838:59:59'/'838:59:59'                                HH:MM:SS                   时间值或持续时间
      YEAR              1字节        1901/2155                                               YYYY                       年份值
      DATETIME          8字节        1000-01-01 00:00:00/9999-12-31 23:59:59                 YYYY-MM-DD HH:MM:SS        混合日期和时间值
      TIMESTAMP         4字节        1970-01-01 00:00:00/2038，结束时间是第2147483647秒        YYYYMMDD HHMMSS           混合日期和时间值，时间戳
   3、字符串（字符）
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
    数据类型如何选择？   日期选择拿着格式，数值和字符串按照大小！
   
三、如何插入下列数据表？
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
   
   INSERT INTO pet VALUES('Fluffy','Harold','cat','f','1993-02-04',NILL);
   INSERT INTO pet VALUES('Claws','Gwen','cat','m','1994-03-17',NILL);
   INSERT INTO pet VALUES('Buffy','Harold','dog','f','1989-05-13',NILL);
   INSERT INTO pet VALUES('Fang','Benny','dog','m','1990-08-27',NILL);
   INSERT INTO pet VALUES('Bowser','Diane','dog','m','1979-08-31','1995-07-29');
   INSERT INTO pet VALUES('Chirpy','Gwen','bird','f','1998-09-11',NILL);
   INSERT INTO pet VALUES('Whistler','Gwen','bird','NULL,'1997-12-09',NILL);
   INSERT INTO pet VALUES('Slim','Benny','snake','m','1996-04-29',NILL);
   INSERT INTO pet VALUES('Puffball','Diane','hamster','f','1999-03-30',NILL);
   
四、如何查看数据
   1、查看pet表中的所有记录
      select * from pet;
   2、
五、如何删除数据
   1、删除宠物名字是Fluffy的记录
      delete from pet where name='Fluffy';
   2、
六、如何修改数据
   1、把宠物“旺财”的名字修改成“旺旺财”
      update pet set name ='旺旺财' where owner='周星驰';
   2、
七、MySQL建表约束
   1、主键约束
      它们能够唯一确定一张表中的一条记录，也就是我们通过给某个字段添加约束，就可以使得该字段不重复且不为空。
      创建一个id有主键约束的表
      create table user(
          id int primary key,
          name varchar(20)
      );
      insert into user values(1,'张三');
      insert into user values(1,'张三');         # × 这里会报错，因为“1”只能插入一次，因为“id”是主键
      insert into user values(2,'张三');         # √ 改成这样就没有问题了
      insert into user values(NULL,'张三');      # × 这里也会报错，因为主键内容不可为空
      创建一个id和name有联合主键约束的表
      create table user2(
          id int,
          name varchar(20),
          password varchar(20),
          primary key(id,name)
      );
      insert into user2 values(1,'张三','123');
      insert into user2 values(1,'张三','123');        # × 主键不能重复
      insert into user2 values(2,'张三','123');        # √ 联合主键值只要两个加起来不重复就行
      insert into user2 values(1,'李四','123');        # √ 联合主键值只要两个加起来不重复就行
      insert into user2 values(NULL,'李四','123');     # × 联合主键不能为空
   2、自增约束
      自增约束和逐渐约束搭配使用可以自动帮助我们管控主键的值，让它自动增长。
      创建一个user3表，id作为自增约束
      create table user3(
          id int primary key auto-increment,
          name varchar(20)
      );
      insert into user3 (name) values('zhangsan');       # 插入一个“zhangsan”可以自动生成一个id值为1
      insert into user3 (name) values('zhangsan');       # 插入一个“zhangsan”可以自动生成一个id值为2
      如果创建表的时候忘记创建主键约束了，该怎么办？
      create table user4(
          id int,
          name varchar(20)
      );
      alter table user4 add primary key(id);             # 给表user4增加一个主键  （注：alter table 是修改表结构）
      alter table user4 drop primary key;                # 删除主键  （注：drop 是删除）
      alter table user4 modify id int primary key;       # 通过修改字段的方式添加主键  （注：modify 是修改字段）
   3、唯一约束
      约束修饰的字段的值不可以重复（可以为空）
      创建一个user5，再对name添加一个唯一约束
      create table user5(
          id int,
          name varchar(20)
      );
      alter table user5 add unique(name);          # 添加一个唯一约束
      insert into user5 values(1,'zhangsan');      # √
      insert into user5 values(1,'zhangsan');      # × “zhangsan”重复了
      insert into user5 values(1,'lisi');          # √ 
      创建一个user6，在创建的时候直接对name添加唯一约束
      create table user6(
          id int,
          name varchar(20),
          unique(name)              # 添加唯一约束
      );
      创建一个user7，在创建的时候直接对name添加唯一约束
      create table user7(
          id int,
          name varchar(20) unique          # 直接添加唯一约束
      );
      创建一个user8，在创建的时候直接对id和name添加唯一约束
      create table user8(
          id int,
          name varchar(20),
          unique(id,name)              # 对id和name添加唯一约束   （注：两个组合在一不重复就行）
      );
      insert into user8 values(1,'zhangsan');       # √
      insert into user8 values(1,'zhangsan');       # ×
      insert into user8 values(2,'zhangsan');       # √
      insert into user8 values(1,'lisi');           # √
      删除唯一约束
      alter table user7 drop index name;
      通过modify的形式添加唯一约束
      alter table user7 modify name varchar(20) unique;
   4、非空约束
      修饰的字段不能为空NULL
      创建表user9，name值设置非空约束
      create table user9(
          id int,
          name varchar(20) not null      # 直接添加非空约束
      );
      insert into user9 (id) values(1);           # × name有非空约束，且没有设默认值，传值的时候又没有给一个值所以会报错
      insert into user9 values(1,'张三');         # √
      insert into user9 (name) values('lisi');    # √
   5、默认约束
      当我们插入字段值的时候如果没有传值就会使用默认值
      创建表user10，对age使用默认约束
      create table user10(
          id int,
          name varchar(20)，
          age int default 10
      );
      insert into user10 (id,name) values(1,'zhangsan');      # √ age会填入默认值10，如果传了值就不会使用默认值
   6、外键约束
      涉及到两个表：父表，子表（主表，副表）
      ---创建班级表
      create table classes(
          id int primary key,
          name varchar(20)
      );
      ---创建学生表
      create table 
   总结：
   1、添加约束的方式：
      --建表的时候就添加约束
      --可以使用alter 。。。add 。。。
      --alter 。。。modify 。。。
   2、删除约束的方式：
      --alter 。。。drop 。。。
   3、查看表的结构：
      --desc 。。。




   
   
   
   
   
   
   

