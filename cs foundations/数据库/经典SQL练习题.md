# 经典SQL练习题

参考CSDN博客: https://blog.csdn.net/qaz13177_58_/article/details/5575711/

在sql fiddle上执行:http://sqlfiddle.com/#!9/30090/1

SQL语句好像不区分关键字的大小写.

先建立表

```sql
CREATE TABLE STUDENT
(SNO VARCHAR(3) NOT NULL, 
SNAME VARCHAR(4) NOT NULL,
SSEX VARCHAR(2) NOT NULL, 
SBIRTHDAY DATETIME,
CLASS VARCHAR(5));
  
CREATE TABLE COURSE
(CNO VARCHAR(5) NOT NULL, 
CNAME VARCHAR(10) NOT NULL, 
TNO VARCHAR(10) NOT NULL);

CREATE TABLE SCORE 
(SNO VARCHAR(3) NOT NULL, 
CNO VARCHAR(5) NOT NULL, 
DEGREE NUMERIC(10, 1) NOT NULL);

CREATE TABLE TEACHER 
(TNO VARCHAR(3) NOT NULL, 
TNAME VARCHAR(4) NOT NULL, TSEX VARCHAR(2) NOT NULL, 
TBIRTHDAY DATETIME NOT NULL, PROF VARCHAR(6), 
DEPART VARCHAR(10) NOT NULL);

INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (108 ,'曾华' 
,'男' ,'1977-09-01',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (105 ,'匡明' 
,'男' ,'1975-10-02',95031);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (107 ,'王丽' 
,'女' ,'1976-01-23',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (101 ,'李军' 
,'男' ,'1976-02-20',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (109 ,'王芳' 
,'女' ,'1975-02-10',95031);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (103 ,'陆君' 
,'男' ,'1974-06-03',95031);

INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('3-105' ,'计算机导论',825);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('3-245' ,'操作系统' ,804);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('6-166' ,'数据电路' ,856);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('9-888' ,'高等数学' ,100);

INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (103,'3-245',86);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (105,'3-245',75);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (109,'3-245',68);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (103,'3-105',92);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (105,'3-105',88);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (109,'3-105',76);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (101,'3-105',64);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (107,'3-105',91);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (108,'3-105',78);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (101,'6-166',85);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (107,'6-106',79);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (108,'6-166',81);

INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (804,'李诚','男','1958-12-02','副教授','计算机系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (856,'张旭','男','1969-03-12','讲师','电子工程系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)
VALUES (825,'王萍','女','1972-05-05','助教','计算机系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (831,'刘冰','女','1977-08-14','助教','电子工程系');
```

题目

```sql
select Sname, Ssex, Class from student;
select distinct depart from teacher;  //distinct
select *  from student;
select *  from Score where degree between 60 and 80;  // between and
select *  from Score where degree in (85,86,88);  //in
select *  from student where class = '95031' or Ssex='女';    //or
select *  from student order by class desc;    //order by ... desc
select *  from Score order by Cno asc, Degree desc;  //asc 升序, 条件直接逗号隔开
select count(*) from student where class=95031;   //count()函数统计记录条数

10.
select Sno, Cno from SCORE where degree = (select max(degree) from score);  // max()函数
或
select sno, cno from score order by degree desc limit 1;  // limit 1

11.
select avg(degree) from score where cno='3-105';  //avg

12.查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。 //'较难'
like, group by ... having ...

13.查询最高分小于90, 最低分大于70的所有sno  ['较难']

14.查询所有学生的Sname, Cno和Degree列

15.


insert into grade values(90,100,'A');
insert into grade values(80,89,'B');
insert into grade values(70,79,'C');
insert into grade values(60,69,'D');
insert into grade values(0,59,'E');
commit;

http://sqlfiddle.com/#!9/d1202/3

```

