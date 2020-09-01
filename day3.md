## 八、查询练习  
** 学生表 **  
```
Student  
学号   
姓名   
性别    
出生年月日  
所在班级  
```
```sql
  create database selectTest;
  use selectTest;
  create table student(
      sno varchar(20) primary key,
      sname varchar(20) not null,
      ssex varchar(10) not null,
      sbirthday datetime,
      class varchar(20)
  );
```
   
** 教师表 **   
```
Teacher
教师编号
教师名字
教师性别
出生年月日
职称
所在部门
```
```sql
  create table teacher(
      tno varchar(20) primary key,
      tname varchar(20) not null,
      tsex varchar(10) not null,
      tbirthday datetime,
      prof varchar(20)
  );
``` 
   
** 课程表 **  
```
Course  
课程号  
课程名称  
教师编号  
```  
```sql
  create table course(
      cno varchar(20) primary key,
      cname varchar(20) not null,
      tno varchar(20) not null,
      foreign key(tno) references teacher(tno)
  );
```

** 成绩表 **
```
Score  
学号  
课程号  
成绩  
```
```sql
  create table score(
      sno varchar(20) not null,
      cno varchar(20) not null,
      degree decimal,
      foreign key(sno) references student(sno),
      foreign key(cno) references course(cno),
      primary key(sno,cno)
  );
```
   
** 往数据表中添加数据 **  
———— 添加学生信息
```sql
  insert into student values('101','曾华','男','1977-09-01','95033');
  insert into student values('102','匡明','男','1975-10-02','95031');
  insert into student values('103','王丽','女','1976-01-23','95033');
  insert into student values('104','李军','男','1976-02-20','95033');
  insert into student values('105','王芳','女','1975-02-10','95031');
  insert into student values('106','陆君','男','1974-06-03','95031');
  insert into student values('107','王尼玛','男','1976-02-20','95033');
  insert into student values('108','张全蛋','男','1975-02-18','95031');
  insert into student values('109','赵铁柱','男','1974-06-03','95031');
```
———— 添加教师信息
```sql
  insert into teacher values('804','李诚','男','1958-12-02,'副教授','计算机系');
  insert into teacher values('856','张旭','男','1969-03-12,'讲师','电子工程系');
  insert into teacher values('825','王萍','女','1972-05-05,'助教','计算机系');
```
———— 添加课程表
```sql
  insert into course values('3-105','计算机导论','825');
  insert into course values('3-245','操作系统','804');
  insert into course values('6-166','数字电路','856');
  insert into course values('9-888','高等数学','831');
```
———— 添加成绩表
```sql
  insert into score values('103','3-245','86');
  insert into score values('105','3-245','75');
  insert into score values('109','3-245','68');
  insert into score values('103','3-105','92');
  insert into score values('105','3-105','88');
  insert into score values('109','3-105','76');
  insert into score values('103','6-166','85');
  insert into score values('105','6-166','79');
  insert into score values('109','6-166','81');
```
1. 查询学生表里的所有记录
```sql
  select * from student;
```
2. 查询student表中的所有记录的sname、ssex和class列

