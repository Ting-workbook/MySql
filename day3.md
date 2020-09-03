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
  insert into student values('110','张飞','男','1974-06-03','95038');
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
  insert into score values('101','3-105','90');
  insert into score values('102','3-105','91');
  insert into score values('104','3-105','89');
  insert into score values('103','3-105','92');
  insert into score values('105','3-105','88');
  insert into score values('109','3-105','76');
  insert into score values('103','6-166','85');
  insert into score values('105','6-166','79');
  insert into score values('109','6-166','81');
  insert into score values('103','3-245','86');
  insert into score values('105','3-245','75');
  insert into score values('109','3-245','68');
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
* 查询“95031”班的学生学号
```sql
  select sno from student where calss='95031';
```
* 从成绩表中查询学号为“95031”班的学生的成绩信息
```sql
  select * from score 
       where sno in(select sno from student where calss='95031');
```
* 查询“95031”班学生每门课的平均分
```sql
  select cno,avg(degree) from score 
       where sno in(select sno from student where calss='95031')
       group by cno;
```
18. 查询选修“3-105”课程的成绩高于“109”号同学“3-105”成绩的所有的学生记录    （子查询）
* 查询“109”号同学选修“3-105”课程的成绩
```sql
  select degree from score where sno='109' and cno='3-105';
```
* 查询成绩表中选修“3-105”课程的成绩高于“109”号同学“3-105”成绩的所有记录
```sql
  select * from score 
       where cno='3-105'
       and degree>(select degree from score where sno='109' and cno='3-105');
```
19. 查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录   （子查询）
```sql
  select * from score 
       where degree>(select degree from score where sno='109' and cno='3-105');
```
20. 查询和学号为108、101的同学同年出生的所有学生的 sno、sname 和 sbirthday 列       （year 函数与带 in 关键字的子查询）
* 查询学号为108、101同学的出生年份
```sql
  select year(sbirthday) from student where sno in(108,101);        -- year 函数提取年份
```
* 查询和学号为108、101的同学同年出生的所有学生
```sql
  select * from student
       where year(sbirthday) in (select year(sbirthday) from student where sno in(108,101));       -- 年份相同
```
* 查询和学号为108、101的同学同年出生的所有学生的 sno、sname 和 sbirthday 列 
```sql
  select sno,sname,sbirthday from student
       where year(sbirthday) in (select year(sbirthday) from student where sno in(108,101));       -- 年份相同
```
21. 查询“张旭”教师任课的学生成绩    （多层嵌套子查询）
* 查询“张旭”教师的教师号
```sql
  select tno from teacher where tname='张旭';
```
* 查询“张旭”教师任职的那门课的课程号
```sql
  select cno from course where tno in (select tno from teacher where tname='张旭');
```
* 从成绩表中查询“张旭”教师任课的学生成绩
```sql
  select * from score where cno in (select cno from course where tno=(select tno from teacher where tname='张旭'));
```
22. 查询选修某课程的同学人数多于5人的教师姓名   （多表查询）
* 先通过课程号分组查看成绩表中的课程编号
```sql
  select cno from score group  by cno;
```
* 查询选修人数超过5人的课程号
```sql
  select cno from score group by cno having count(*)>5;        -- having 在 group by 后面用于过滤条件
```
* 在课程表里查询选修人数超过5人的教师号
```sql
  select tno from score where cno=(select cno from score group by cno having count(*)>5);
```
* 在教师表里查询选修人数超过5人的教师姓名
```sql
  select tname from teacher where tno=(select tno from score where cno=(select cno from score group by cno having count(*)>5));
```
** 这里 = 换成 in 更具代表性 **
23. 查询95033班和95.31班全体学生的记录
```sql
  select * from student where class in (95031,95031);     -- in 表示或者关系
```
24. 查询存在有85分以上成绩的课程 cno、degree
```sql
  select cno,degree from score where degree>85;
```
25. 查询出“计算机系”教师所教课程的成绩表
* 在教师表里查询计算机系的教师的教师号
```sql
  select tno from teacher where depart='计算机系';
```
* 在课程表里查询计算机系的教师的课程号
```sql
  select cno from course where tno in (select tno from teacher where depart='计算机系');
```
* 在成绩表里查询“计算机系”教师所教课程的成绩
```sql
  select * from score where cno in (select cno from course where tno in (select tno from teacher where depart='计算机系'));
```
26. 查询“计算机系”与“电子工程系”不同职称的教师的 tname 和 prof     （union 和 not in 的使用）
* 查询电子工程系教师的职称
```sql
  select prof from teacher where depart='电子工程系';
```
* 查询计算机系与电子工程与电子工程系不同职称的教师的 tname 和 prof
```sql
   select prof from teacher where depart='计算机系' and prof not in (select prof from teacher where depart='电子工程系')
   union        -- 求并集 
   select prof from teacher where depart='电子工程系' and prof not in (select prof from teacher where depart='计算机系')
```
27. 查询选修编号为“3-105”课程，且成绩至少高于选修编号为“3-245”的同学的 cno、sno 和 degree，并且 degree 从高到低次序排列
* 分别查询选修“3-245”和选修“3-105”的成绩
```sql
  select * from score where cno='3-245';
  select * from score where cno='3-105';
```
* 至少？  大于“3-245”其中至少一个，降序
```sql
 select cno,sno,degree from score 
      where cno='3-105' 
      and degree>any(select * from score where cno='3-245')      -- any 表示任意一个
      order by degree desc;        -- 降序
```
28. 查询编号为“3-105”且成绩高于选修“3-245”课程的同学的 cno、sno 和 degree
* 且？  高于任何一个（每一个）
```sql
 select cno,sno,degree from score 
      where cno='3-105' 
      and degree>all(select * from score where cno='3-245');        -- all 表示所有
```
29. 查询所有教师和同学的 name、sex 和 birthday，然后并起来，并取别名
* 分别查询所有教师和同学的 name、sex 和 birthday，然后并起来，并取别名
```sql
  select tname as name,tsex as sex,tbirthday as birthday from teacher
  union
  select sname,ssex,sbirthday from student;
```
30. 查询所有“女”教师和“女”同学的 name、sex 和 birthday
```sql
  select tname as name,tsex as sex,tbirthday as birthday from teacher where tsex='女'
  union
  select sname,ssex,sbirthday from student where ssex='女';
```











