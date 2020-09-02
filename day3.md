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
```sql
  select sname, ssex, class from student;
```
3. 查询教师所有的单位即不重复的depart列
```sql
  select distinct depart from teacher;     -- detinct是排除重复的
```
4. 查询score表中成绩在60到80之间的所有记录  
```sql
  select * from score where degree between 60 and 80;     -- between。。。and。。。是查询区间
```
```sql
  select * from score where degree > 60 and degree < 80;       -- 直接使用运算符比较
```
5. 查询score表中成绩为85，86或88的记录
```sql
  select * from score where degree in(85,86,88);       -- in表示或者关系
```
6. 查询student表中“95031”班或者性别为“女”的同学记录  
```sql
  select * from student where class='95031' or ssex='女';      -- or表示不同字段的或者关系
```
7. 以class降序查询student表的所有记录  
```sql
  select * student order by class desc;       -- order by 。。。desc 表示降序(asc表示升序，默认也是升序)
```
8. 以cno升序、degree降序查询score表的所有记录  
```sql
  select * from student order by cno asc,degree desc;      -- 可以用逗号并列起来
```
9. 查询“95031”班的学生人数  
```sql
  select count(*) from student where class='95031';       -- count表示统计
```
10. 查询score表中的最高分的学生学号和课程号。（子查询或者排序）
```sql  
  select sno,cno from score where degree=(select max(degree) from score);     -- 嵌套
```
（1）找到最高分  
```sql
  select max(degree) from score;
```
（2）找到最高分的 sno 和 cno  
```sql
  select sno,cno from score where degree=(select max(degree) from score);
```
（3）另一种方法：排序的做法   
```sql
  select sno,cno from score order by degree desc limit 0,1;       -- limit 0,1 表示从0开始，查找1条
```  
11. 查询每门课的成绩  
* 得知道有多少门课
```sql
  select * from course;
```  
* 计算3-105课程的平均成绩  
```sql
  select avg(degree) from score where cno='3-105';
```  
* 计算每门课程的平均成绩  
```sql
  select cno,avg(degree) from score group by cno;      -- group by cno 表示按照课程号分组
```
12. 查询 score 表中至少有2名学生选修，并以3开头的课程的平均分数
* 哪些课程至少有两名学生选修，并且以3开头
```sql
  select cno from score group by cno
       having count(cno)>=2 and like '3%';
```   
* 查询至少两名学生选修，并且以3开头的课程的平均成绩  
```sql
  select cno,avg(degree) from score
       group by cno
       having count(cno)>=2
       and cno like '3%';
```
* 还可以再添加一列查询这门课程有几个人  
```sql
  select cno,avg(degree),count(*) from score
       group by cno        -- group by 分组
       having count(cno)>=2        -- having 开条件
       and cno like '3%';       -- like 模糊查询
```
13. 查询分数大于70，小于90的 sno 列
* 方法一
```sql
  select sno,degree from score
       where degree>70 and degree<90;
```
* 方法二
```sql
  select sno,degree from score
       where degree between 70 and 90;
```
14. 查询所有学生的 sname、cno 和 degree 列
* 分析：这里涉及 student 表以及 score 表
```sql
  select sno,sanme from student;
  select sno,cno,degree from score;
```
* 多表查询（总）
```sql
  select sname,cno,degree from student,score
       where student.sno=score.sno;
```
15. 查询所有学生的 sno、cname 和 degree 列
* 分析：这里涉及到 course、score 表
```sql
  select cno,cname from course;
  select cno,sno,degree from score;
```
* 多表查询（总）
```sql
  select sno,cname,degree from course,score
       where course.cno = score.cno;
```
16. 查询所有学生的 sname、cname 和 degree 列
* 分析：sanme -> student, cname -> course, degree -> score  多表查询
```sql
  select sname,cname,degree,student.sno,course.cno from student,course,score
       where student.sno = score.sno
       and course.cno = score.cno;
```
* 为了方便查看，还可以给 sno 和 cno 取个别名
```sql
  select sname,cname,degree,student.sno as stu_sno,course.cno as cou_cno from student,course,score
       where student.sno = score.sno
       and course.cno = score.cno;
```
17. 查询“95031”班学生每门课的平均分
* 查询“95031”班的学生信息
```sql
  select * from student where calss='95031';
```







