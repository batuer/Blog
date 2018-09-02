##### 基础练习

1. 查询Student表中的所有记录的Sname、Ssex和Class列。

   ```sql
   SELECT sname,ssex,class From students 
   ```

2. 查询教师所有的单位即不重复的Depart列。

   ```sql
   SELECT DISTINCT depart From teachers
   ```

3. 查询Student表的所有记录。

   ```sql
   SELECT *  From students
   ```

4. 查询Score表中成绩在60到80之间的所有记录。

   ```sql
   SELECT *  From scores WHERE degree BETWEEN 60 And 80 
   ```

5. 查询Score表中成绩为85，86或88的记录。

   ```sql
   SELECT *  From scores WHERE degree = 85 or degree = 86 or degree =88 
   
   SELECT *  From scores WHERE degree IN (85,86,88)
   ```

6. 查询Student表中“95031”班或性别为“女”的同学记录。

   ```sql
   SELECT *  From students WHERE class = '95031' OR ssex = '女'
   ```

7. 以Class降序查询Student表的所有记录。

   ```sql
   SELECT *  From students ORDER BY class DESC
   ```

8. 以Cno升序、Degree降序查询Score表的所有记录。

   ```sql
   SELECT *  From scores ORDER BY cno ASC,degree DESC
   ```

9. 查询“95031”班的学生人数。

   ```sql
   SELECT COUNT(*)  From students WHERE class = '95031'
   
   SELECT COUNT(1)  AS StuNum FROM Students WHERE Class =  '95031'
   ```

10. 查询Score表中的最高分的学生学号和课程号。

  ```sql
  SELECT sno,cno From scores ORDER BY degree DESC LIMIT 1
  
  SELECT MAX(degree) From scores //查询最大值
  ```

11. 查询‘3-105’号课程的平均分。

    ```sql
    SELECT AVG(degree) From scores WHERE cno = '3-105'
    ```

12. 查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。

    ```sql
    SELECT cno,AVG(degree) From scores WHERE cno LIKE '3%' GROUP BY  cno HAVING COUNT(sno) > 5
    ```

13. 查询最低分大于70，最高分小于90的Sno列。

    ```sql
    SELECT sno From scores GROUP BY sno HAVING MIN(degree) > 70 And MAX(degree) < 90 
    ```

14. 查询所有学生的Sname、Cno和Degree列。

    ```sql
    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno
    ```

15. 查询所有学生的Sno、Cname和Degree列。

    ```sql
    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno
    
    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On (sc.sno = st.sno)
    ```

16. 查询所有学生的Sname、Cname和Degree列。

    ```sql
    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno
    
    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On (sc.sno = st.sno)
    ```

17. 查询“95033”班所选课程的平均分。

    ```sql
    SELECT courses.cname,AVG(degree) FROM scores JOIN students ON students.sno = scores.sno JOIN courses ON scores.cno = courses.cno WHERE students.class = '95033' GROUP BY courses.cno;
    ```

18. 假设使用如下命令建立了一个grade表：

    ```sql
    CREATE TABLE grade(low TINYINT,upp TINYINT,rank CHAR(1));
    
    INSERT INTO grade VALUES(90,100,'A');
    
    INSERT INTO grade VALUES(80,89,'B');
    
    INSERT INTO grade VALUES(70,79,'C');
    
    INSERT INTO grade VALUES(60,69,'D');
    
    INSERT INTO grade VALUES(0,59,'E');
    ```

    现查询所有同学的Sno、Cno和rank列。

    ```sql
    SELECT sc.sno,sc.cno,sc.degree,g.rank FROM scores sc JOIN grade g ON (sc.degree >= g.low AND sc.degree <= g.upp) ORDER BY g.rank ASC,sc.sno ASC;
    ```

19. 查询选修“3-105”课程的成绩高于“109”号同学成绩的所有同学的记录。

    ```sql
    SELECT sc.sno,sc.degree FROM students st JOIN scores sc ON sc.sno = st.sno WHERE sc.cno = '3-105' AND sc.degree > (SELECT sc.degree FROM scores sc WHERE sc.sno = '109' AND sc.cno = '3-105');
    
    SELECT s1.Sno,s1.Degree FROM Scores AS s1 INNER JOIN Scores AS s2 ON (s1.Cno = s2.Cno AND s1.Degree > s2.Degree) WHERE s1.Cno = '3-105' AND s2.Sno = '109' ORDER BY s1.Sno;
    ```

20. 查询score中选学一门以上课程的同学中分数为非最高分成绩的记录。

    ```sql
    SELECT * From scores GROUP BY sno HAVING COUNT(sno) > 1 AND degree != MAX(degree);
    ```

21. 查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录。

    ```sql
    SELECT * From scores WHERE cno = '3-105' AND degree > (SELECT degree FROM scores WHERE sno = '109' AND cno = '3-105');
    
    SELECT s1.Sno,s1.Degree FROM Scores AS s1 INNER JOIN Scores AS s2 ON (s1.Cno = s2.Cno AND s1.Degree > s2.Degree) WHERE s1.Cno = '3-105' AND s2.Sno = '109' ORDER BY s1.Sno;
    ```

22. 查询和学号为108的同学同年出生的所有学生的Sno、Sname和Sbirthday列。

    ```sql
    SELECT st.sno,st.sname,st.sbirthday FROM students st WHERE YEAR (st.Sbirthday) = (SELECT YEAR(sbirthday) FROM students WHERE sno = '108') AND st.sno != '108';
    
    SELECT s1.Sno,s1.Sname,s1.Sbirthday FROM Students AS s1 INNER JOIN Students AS s2 ON (YEAR (s1.Sbirthday) = YEAR (s2.Sbirthday)) WHERE s2.Sno = '108' AND s1.sno != '108';
    ```

23. 查询“张旭“教师任课的学生成绩。

    ```sql
    SELECT sc.degree FROM scores sc JOIN courses c On sc.cno = c.cno JOIN teachers t On t.tno = c.tno WHERE t.tname = '张旭';
    ```

24. 查询选修某课程的同学人数多于5人的教师姓名。

    ```sql
    SELECT cno From scores GROUP BY cno HAVING COUNT(sno) > 5;
    
    SELECT t.tname FROM scores sc JOIN courses c ON c.cno = sc.cno JOIN teachers t ON t.tno = c.tno GROUP BY c.tno HAVING(count(sno) > 5);
    	
    SELECT DISTINCT t.tname FROM scores sc JOIN courses c ON sc.cno = c.cno JOIN teachers t On t.tno = c.tno WHERE c.cno in (SELECT cno From scores GROUP BY cno HAVING COUNT(sno) > 5);
    ```

25. 查询95033班和95031班全体学生的记录。

    ```sql
    SELECT * FROM Students WHERE Class IN ('95033','95031') ORDER BY Class;
    ```

26. 查询存在有85分以上成绩的课程Cno.

    ```sql
    SELECT DISTINCT Cno FROM Scores WHERE Degree > 85;
    ```

27. 查询出“计算机系“教师所教课程的成绩表。

    ```sql
    SELECT sc.degree,t.depart FROM scores sc Join courses c On c.cno = sc.cno JOIN teachers t On t.tno = c.tno WHERE t.depart = '计算机系';
    ```

28. 查询“计算机系”与“电子工程系“不同职称的教师的Tname和Prof。

    ```sql
    SELECT Tname,Prof FROM Teachers WHERE Depart='计算机系' AND Prof NOT IN( SELECT DISTINCT Prof FROM Teachers WHERE Depart='电子工程系');
    			
    SELECT tname,prof FROM teachers WHERE depart ='电子工程系' And prof NOT IN(SELECT DISTINCT prof FROM teachers WHERE depart = '计算机系');
    ```

29. 查询选修编号为“3-105“课程且成绩至少高于选修编号为“3-245”的同学的Cno、Sno和Degree,并按Degree从高到低次序排序。

    ```sql
    SELECT sc.cno,sc.sno,sc.degree FROM scores sc WHERE sc.cno = '3-105' AND sc.degree > ANY (SELECT degree FROM scores WHERE cno = '3-245') ORDER BY sc.degree DESC;	
    ```

30. 查询选修编号为“3-105”且成绩高于选修编号为“3-245”课程的同学的Cno、Sno和Degree.

    ```sql
    SELECT sc.cno,sc.sno,sc.degree FROM scores sc WHERE sc.cno = '3-105' AND sc.degree >ALL(SELECT  degree FROM scores WHERE cno = '3-245') ORDER BY sc.degree DESC;
    ```

31. 查询所有教师和同学的name、sex和birthday.

    ```sql
    SELECT tname,tsex,tbirthday From teachers UNION Select sname,ssex,sbirthday FROM students;
    ```

32. 查询所有“女”教师和“女”同学的name、sex和birthday.

    ```sql
    SELECT tname,tsex,tbirthday From teachers WHERE tsex = '女' UNION Select sname,ssex,sbirthday FROM students WHERE ssex = '女';
    ```

33. 查询成绩比该课程平均成绩低的同学的成绩表。

    ```sql
    SELECT sc.* From scores sc WHERE sc.degree < (SELECT AVG(degree) FROM scores WHERE cno = sc.cno) ORDER BY sc.sno;
    
    SELECT s1.* FROM Scores AS s1 JOIN (SELECT Cno,AVG(Degree) AS aDegree FROM Scores GROUP BY Cno) s2 ON(s1.Cno=s2.Cno AND s1.Degree<s2.adegree) ORDER BY s1.sno;
    ```

34. 查询所有任课教师的Tname和Depart.

    ```sql
    SELECT te.tname,te.depart FROM teachers te Where te.tno In (SELECT tno From courses);
    
    SELECT te.tname,te.depart FROM teachers te Where te.tno In (SELECT tno From courses WHERE cno IN (SELECT sc.cno From scores sc ));
    
    SELECT Tname,Depart FROM Teachers WHERE Tno IN(SELECT Tno FROM Courses);
    ```

35. 查询所有未讲课的教师的Tname和Depart. 

    ```sql
    SELECT te.tname,te.depart FROM teachers te Where te.tno not In (SELECT tno From courses);
    SELECT te.tname,te.depart FROM teachers te Where NOT EXISTS (SELECT tno From courses WHERE tno = te.tno);
    
    SELECT Tname,Depart FROM Teachers WHERE Tno not IN(SELECT Tno FROM Courses);
    SELECT Tname,Depart FROM Teachers WHERE NOT EXISTS (SELECT tno FROM courses WHERE tno = teachers.tno);
    ```

36. 查询至少有2名男生的班号。

    ```sql
    SELECT st.class,COUNT(sno) boycount FROM students st WHERE st.ssex ='男' GROUP BY st.class HAVING boycount >= 2;
    
    SELECT Class,COUNT(1) AS boyCount FROM Students WHERE Ssex='男' GROUP BY Class HAVING boyCount>=2;
    ```

37. `查询Student表中不姓“王”的同学记录。`

    ```sql
    SELECT * FROM students WHERE sname not LIKE '王%';
    ```

38. 查询Student表中每个学生的姓名和年龄。

    ```sql
    SELECT sname,sbirthday FROM students ;
    
    SELECT Sname,YEAR(NOW())-YEAR(Sbirthday) AS Sage FROM Students;
    ```

39. 查询Student表中最大和最小的Sbirthday日期值。

    ```sql
    SELECT MAX(DAY(sbirthday)) maxDay,MIN(DAY(sbirthday)) minDay FROM students ;  
    
    SELECT MIN(Sbirthday),MAX(Sbirthday) FROM Students;
    ```

40. 以班号和年龄从大到小的顺序查询Student表中的全部记录。

    ```sql
    SELECT * FROM students GROUP BY class DESC, sbirthday ASc;
    ```

41. 查询“男”教师及其所上的课程。

    ```sql
    SELECT te.*,c.* FROM teachers te JOIN courses c On c.tno = te.tno WHERE te.tsex ='男';
    ```

42. 查询最高分同学的Sno、Cno和Degree列。

    ```sql
    SELECT sno,cno,degree FROM scores WHERE degree = (SELECT MAX(degree) FROM scores);
    
    SELECT * FROM Scores GROUP BY Cno HAVING Degree=Max(Degree);
    ```

43. 查询和“李军”同性别的所有同学的Sname.

    ```sql
    SELECT sname FROM students WHERE ssex = (SELECT s1.ssex FROM students s1 WHERE s1.sname = '李军') AND sname != '李军'; 
    
    SELECT s1.sname From students s1 JOIN students s2 On (s1.ssex = s2.ssex) WHERE s2.sname = '李军' And s1.sname != '李军';
    ```

44. 查询和“李军”同性别并同班的同学Sname.

    ```sql
    SELECT s1.sname From students s1 JOIN students s2 On (s1.ssex = s2.ssex And s1.class = s2.class) WHERE s2.sname = '李军' And s1.sname != '李军';
    ```

45. 查询所有选修“计算机导论”课程的“男”同学的成绩表。

    ```sql
    SELECT sc.* FROM scores sc JOIN students st On st.sno = sc.sno JOIN courses c On c.cno = sc.cno WHERE c.cname = '计算机导论' And st.ssex = '男';
    
    SELECT * FROM Scores WHERE Sno IN (SELECT Sno FROM Students WHERE Ssex='男') AND Cno IN (SELECT Cno FROM Courses WHERE Cname='计算机导论');     
    ```

##### 经典练习

1. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数。

2. 查询同时存在" 01 "课程和" 02 "课程的情况。

3. 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )。

4. 查询不存在" 01 "课程但存在" 02 "课程的情况。

5. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩。

6. 查询在 SC 表存在成绩的学生信息。

7. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )。

8. 查有成绩的学生信息。

9. 查询「李」姓老师的数量。

10. 查询学过「张三」老师授课的同学的信息。

11. 查询没有学全所有课程的同学的信息。

12. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息。

13. 查询和" 01 "号的同学学习的课程   完全相同的其他同学的信息。

14. 查询没学过"张三"老师讲授的任一门课程的学生姓名。

15. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩。

16. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息。

17. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩。

18. 查询各科成绩最高分、最低分和平均分：以如下形式显示：

    以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率

    及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90

    要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

19. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺。

20.  按各科成绩进行排序，并显示排名， Score 重复时合并名次。

21. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺。

22. 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺。

23. 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比。

24. 查询各科成绩前三名的记录。

25. 查询每门课程被选修的学生数。

26. 查询出只选修两门课程的学生学号和姓名。

27. 查询男生、女生人数。

28. 查询名字中含有「风」字的学生信息。

29. 查询同名同性学生名单，并统计同名人数。

30. 查询 1990 年出生的学生名单。

31. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列。

32. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩。

33. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数。

34. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）。

35. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数。

36. 查询不及格的课程查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名。

37. 求每门课程的学生人数。

38. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩。

39. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩。

40. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩。

41. 查询每门功成绩最好的前两名。

42. 统计每门课程的学生选修人数（超过 5 人的课程才统计）。

43. 检索至少选修两门课程的学生学号。

44. 查询选修了全部课程的学生信息。

45. 查询各学生的年龄，只按年份来算。

46. 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一。

47. 查询本周过生日的学生。

48. 查询下周过生日的学生。

49. 查询本月过生日的学生。

50. 查询下月过生日的学生。

 

 

 

 

 



###### Count

- count(1)是聚索引 
- *count(),自动会优化指定到那一个字段 ，用count(),sql会帮你完成优化的*  
- count(*)将返回表格中所有存在的行的总数**包括值为null的行**
- count(列名)将返回表格中除去n**ull以外**的所有行的总数(有默认值的列也会被计入） 

###### Where、Having

- WHERE语句在GROUP BY语句之前；SQL会在分组之前计算WHERE语句。 
- HAVING语句在GROUP BY语句之后；SQL会在分组之后计算HAVING语句。 

###### All、Any、Some(<> 等同于!=)

- .. >ALL 父查询中的结果集大于子查询中每一个结果集中的值,则为真
- .. >ANY,SOME 父查询中的结果集大于子查询中任意一个结果集中的值,则为真
- .. =ANY 与子查询IN相同
- .. <>ANY OR作用，父查询中的结果集不等于子查询中的a或者b或者c,则为真
- .. NOT IN AND作用，父查询中的结果集不等于子查询中任意一个结果集中的值,则为真

###### In、Not In、Exists、Not Exists

- In的时候，会把子句中的查询作为结果缓存下来，然后对主查询中的每个记录进行查询。

- Exists的时候，不在对子查询的结果进行缓存，子查询的返回的结果并不重要。使用exists的时候，我们使先对主查询进行查询，然后根据子查询的结果是否为真来决定是否返回。 

- 子查询表大的用Exists，子查询表小的用In。 

  ```sql
  SELECT  * From rel WHERE rel.id NOT IN (SELECT rel_id FROM item );
  SELECT  * From rel WHERE NOT EXISTS (SELECT item.id From item WHERE item.rel_id = rel.id);
  ```

###### UNION、UNION ALL

- UNION 操作符用于合并两个或多个 SELECT 语句的结果集。 
- UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。 
- **默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL**。 
- UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。 

###### Like

- 'A_Z': 所有以 'A' 起头，另一个任何值的字原，且以 'Z' 为结尾的字串。 'ABZ' 和 'A2Z' 都符合这一个模式，而 'AKKZ' 并不符合 (因为在 A 和 Z 之间有两个字原，而不是一个字原)。
- 'ABC%': 所有以 'ABC' 起头的字串。举例来说，'ABCD' 和 'ABCABC' 都符合这个套式。
- '%XYZ': 所有以 'XYZ' 结尾的字串。举例来说，'WXYZ' 和 'ZZXYZ' 都符合这个套式。
- '%AN%': 所有含有 'AN' 这个套式的字串。举例来说， 'LOS ANGELES' 和 'SAN FRANCISCO' 都符合这个套式。

