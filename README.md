# firewall_database
数据库防火墙策略库（分析sql安全库）

1：支持不同数据库不同固定策略的拦截正则匹配。

2：支持Sql注入的判定和分析。

3：支持sql的词法分析获取更多详细的信息，以便实现其他更高的安全策略。

4：sql语句预警功能，支持sql频次的记录

支持c接口版本和c++版本。

部分测试案例输出：
SQL：select * from student
True expression detected (SQL tautology)
sql safe

SQL：SELECT * FROM products WHERE id=1 AND 1>(SELECT count(*) FROM information_schema.columns A, information_schema.columns B, information_schema.columns C)
True expression detected (SQL tautology)

SQL：SELECT * FROM products WHERE categoryid=1; UPDATE members SET password='pwd' WHERE username='admin'
Multiple queries found

SQL：SELECT * FROM products WHERE id=1 AND 1=2 OR sleep(0.1)
Query has 'or' token
True expression detected (SQL tautology)

SQL：SELECT * FROM products WHERE id=1 AND 1=1
True expression detected (SQL tautology)

INSERT INTO members (username, isadmin, password) VALUES ('attacker', 1, /*', 0, '*/'pwd')
Query uses sensitive tables

SQL：SELECT id, username, first_name, last_name, email FROM members WHERE username='invalid-username' UNION SELECT 1, username, passwords FROM members WHERE 'x'='x'
Query uses sensitive tables
SQL：Query has 'union' token
True expression detected (SQL tautology)

SELECT name, description, price FROM products WHERE category=1 UNION SELECT 'A', 'B', 3 FROM all_tables
Query has 'union' token

SQL：SELECT name, email FROM members WHERE id=1; IF SYSTEM_USER='sa' SELECT 1/0 ELSE SELECT 5
Query uses sensitive tables
Multiple queries found

SQL：SELECT name from student where id=100 and age>20
sql safe

SELECT name from student where depto>20 or pto>30
Query has 'or' token

SQL：SELECT * from admin
Query uses sensitive tables

SQL：Delete From myTable where '1'='1'
True expression detected (SQL tautology)

SQL：select * from student;sleep(10)
Multiple queries found

 SQL：SELECT ELT(1, 'ej', 'Heja', 'hej', 'foo')
Query has SQL fuction that can be used to bruteforce db contents

性能：单核100000次注入判定，100000次固定正则判定，平均耗时1.5s以内

如果感兴趣可以联系873988212@qq.com
