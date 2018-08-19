1. 查询Student表中的所有记录的Sname、Ssex和Class列。

   SELECT sname,ssex,class From students 

2. 查询教师所有的单位即不重复的Depart列。

   SELECT DISTINCT depart From teachers

3. 查询Student表的所有记录。

   SELECT *  From students

4. 查询Score表中成绩在60到80之间的所有记录。

   SELECT *  From scores WHERE degree BETWEEN 60 And 80 

5. 查询Score表中成绩为85，86或88的记录。

   SELECT *  From scores WHERE degree = 85 or degree = 86 or degree =88 

   SELECT *  From scores WHERE degree IN (85,86,88)

6. 查询Student表中“95031”班或性别为“女”的同学记录。

   SELECT *  From students WHERE class = '95031' OR ssex = '女'

7. 以Class降序查询Student表的所有记录。

   SELECT *  From students ORDER BY class DESC

8. 以Cno升序、Degree降序查询Score表的所有记录。

   SELECT *  From scores ORDER BY cno ASC,degree DESC

9. 查询“95031”班的学生人数。

   SELECT COUNT(*)  From students WHERE class = '95031'

   SELECT COUNT(1)  AS StuNum FROM Students WHERE Class =  '95031'

   - count(1)是聚索引 
   - *count(),自动会优化指定到那一个字段 ，用count(),sql会帮你完成优化的*  
   - count(*)将返回表格中所有存在的行的总数包括值为null的行，然而count(列名)将返回表格中除去null以外的所有行的总数(有默认值的列也会被计入） 

10. 查询Score表中的最高分的学生学号和课程号。

  SELECT sno,cno From scores ORDER BY degree DESC LIMIT 1

  SELECT MAX(degree) From scores //查询最大值

11. 查询‘3-105’号课程的平均分。

    SELECT AVG(degree) From scores WHERE cno = '3-105'

12. 查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。

    SELECT cno,AVG(degree) From scores WHERE cno LIKE '3%' GROUP BY  cno HAVING COUNT(sno) > 5

13. 查询最低分大于70，最高分小于90的Sno列。

    SELECT sno From scores GROUP BY sno HAVING MIN(degree) > 70 And MAX(degree) < 90 

14. 查询所有学生的Sname、Cno和Degree列。

    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno

15. 查询所有学生的Sno、Cname和Degree列。

    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno

    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On (sc.sno = st.sno)

16. 查询所有学生的Sname、Cname和Degree列。

    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On sc.sno = st.sno

    SELECT st.sname,sc.cno,sc.degree From students st JOIN scores sc On (sc.sno = st.sno)

17. 查询“95033”班所选课程的平均分。

    SELECT courses.cname,AVG(degree) FROM scores JOIN students ON students.sno = scores.sno JOIN courses ON scores.cno = courses.cno WHERE students.class = '95033' GROUP BY courses.cno;

18. 假设使用如下命令建立了一个grade表：

    CREATE TABLE grade(low TINYINT,upp TINYINT,rank CHAR(1));
    INSERT INTO grade VALUES(90,100,'A');
    INSERT INTO grade VALUES(80,89,'B');
    INSERT INTO grade VALUES(70,79,'C');
    INSERT INTO grade VALUES(60,69,'D');
    INSERT INTO grade VALUES(0,59,'E');

    现查询所有同学的Sno、Cno和rank列。

    SELECT sc.sno,sc.cno,sc.degree,g.rank FROM scores sc JOIN grade g ON (sc.degree >= g.low AND sc.degree <= g.upp) ORDER BY g.rank ASC,sc.sno ASC;

19. 查询选修“3-105”课程的成绩高于“109”号同学成绩的所有同学的记录。

    SELECT sc.sno,sc.degree FROM students st JOIN scores sc ON sc.sno = st.sno WHERE sc.cno = '3-105' AND sc.degree > (SELECT sc.degree FROM scores sc WHERE sc.sno = '109' AND sc.cno = '3-105');

    SELECT
    	s1.Sno,
    	s1.Degree
    FROM
    	Scores AS s1
    INNER JOIN Scores AS s2 ON (
    	s1.Cno = s2.Cno
    	AND s1.Degree > s2.Degree
    )
    WHERE
    	s1.Cno = '3-105'
    AND s2.Sno = '109'
    ORDER BY
    	s1.Sno;

    

20. 查询score中选学一门以上课程的同学中分数为非最高分成绩的记录。

    SELECT * From scores GROUP BY sno HAVING COUNT(sno) > 1 AND degree != MAX(degree);

21. 查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录。

    SELECT * From scores WHERE cno = '3-105' AND degree > (SELECT degree FROM scores WHERE sno = '109' AND cno = '3-105');

    SELECT
    	s1.Sno,
    	s1.Degree
    FROM
    	Scores AS s1
    INNER JOIN Scores AS s2 ON (
    	s1.Cno = s2.Cno
    	AND s1.Degree > s2.Degree
    )
    WHERE
    	s1.Cno = '3-105'
    AND s2.Sno = '109'
    ORDER BY
    	s1.Sno;

22. 查询和学号为108的同学同年出生的所有学生的Sno、Sname和Sbirthday列。

    SELECT
    	st.sno,
    	st.sname,
    	st.sbirthday
    FROM
    	students st
    WHERE
    	YEAR (st.Sbirthday) = (
    		SELECT
    			YEAR (sbirthday)
    		FROM
    			students
    		WHERE
    			sno = '108'
    	)
    AND st.sno != '108';

    

    SELECT
    	s1.Sno,
    	s1.Sname,
    	s1.Sbirthday
    FROM
    	Students AS s1
    INNER JOIN Students AS s2 ON (
    	YEAR (s1.Sbirthday) = YEAR (s2.Sbirthday)
    )
    WHERE
    	s2.Sno = '108' AND s1.sno != '108';

23. 查询“张旭“教师任课的学生成绩。

24. 查询选修某课程的同学人数多于5人的教师姓名。

25. 查询95033班和95031班全体学生的记录。

26. 查询存在有85分以上成绩的课程Cno.

27. 查询出“计算机系“教师所教课程的成绩表。

28. 查询“计算机系”与“电子工程系“不同职称的教师的Tname和Prof。

29. 查询选修编号为“3-105“课程且成绩至少高于选修编号为“3-245”的同学的Cno、Sno和Degree,并按Degree从高到低次序排序。

30. 查询选修编号为“3-105”且成绩高于选修编号为“3-245”课程的同学的Cno、Sno和Degree.

31. 查询所有教师和同学的name、sex和birthday.

32. 查询所有“女”教师和“女”同学的name、sex和birthday.

33. 查询成绩比该课程平均成绩低的同学的成绩表。

34. 查询所有任课教师的Tname和Depart.

35. 查询所有未讲课的教师的Tname和Depart. 

36. 查询至少有2名男生的班号。

37. `查询Student表中不姓“王”的同学记录。`

38. 查询Student表中每个学生的姓名和年龄。

39. 查询Student表中最大和最小的Sbirthday日期值。

40. 以班号和年龄从大到小的顺序查询Student表中的全部记录。

41. 查询“男”教师及其所上的课程。

42. 查询最高分同学的Sno、Cno和Degree列。

43. 查询和“李军”同性别的所有同学的Sname.

44. 查询和“李军”同性别并同班的同学Sname.

45. 查询所有选修“计算机导论”课程的“男”同学的成绩表。

       



- WHERE语句在GROUP BY语句之前；SQL会在分组之前计算WHERE语句。 
- HAVING语句在GROUP BY语句之后；SQL会在分组之后计算HAVING语句。 

