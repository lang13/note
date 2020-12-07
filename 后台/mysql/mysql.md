### mysql

#### SQL语句

##### 时间格式化工具`date_format`

```sql
DATE_FORMAT(date,format) 
```

format参数的格式有

| %a   | 缩写星期名                                     |
| ---- | ---------------------------------------------- |
| %b   | 缩写月名                                       |
| %c   | 月，数值                                       |
| %D   | 带有英文前缀的月中的天                         |
| %d   | 月的天，数值(00-31)                            |
| %e   | 月的天，数值(0-31)                             |
| %f   | 微秒                                           |
| %H   | 小时 (00-23)                                   |
| %h   | 小时 (01-12)                                   |
| %I   | 小时 (01-12)                                   |
| %i   | 分钟，数值(00-59)                              |
| %j   | 年的天 (001-366)                               |
| %k   | 小时 (0-23)                                    |
| %l   | 小时 (1-12)                                    |
| %M   | 月名                                           |
| %m   | 月，数值(00-12)                                |
| %p   | AM 或 PM                                       |
| %r   | 时间，12-小时（hh:mm:ss AM 或 PM）             |
| %S   | 秒(00-59)                                      |
| %s   | 秒(00-59)                                      |
| %T   | 时间, 24-小时 (hh:mm:ss)                       |
| %U   | 周 (00-53) 星期日是一周的第一天                |
| %u   | 周 (00-53) 星期一是一周的第一天                |
| %V   | 周 (01-53) 星期日是一周的第一天，与 %X 使用    |
| %v   | 周 (01-53) 星期一是一周的第一天，与 %x 使用    |
| %W   | 星期名                                         |
| %w   | 周的天 （0=星期日, 6=星期六）                  |
| %X   | 年，其中的星期日是周的第一天，4 位，与 %V 使用 |
| %x   | 年，其中的星期一是周的第一天，4 位，与 %v 使用 |
| %Y   | 年，4 位                                       |
| %y   | 年，2 位                                       |

> 例子，查询blog表数据所有的年份，且按年份降序排列

```sql
SELECT DATE_FORMAT(b.create_time, '%Y') AS `year` FROM blog b GROUP BY `year` ORDER BY `year` DESC
```

##### 交集查询

```sql
SELECT Sno, Sname, Sage, Sdept FROM student
INNER JOIN (SELECT * FROM student WHERE sdept = 'CS') t0 USING(Sno, Sname, Sage, Sdept)
INNER JOIN (SELECT * FROM student WHERE sage <= 20) t1 USING(Sno, Sname, Sage, Sdept)
```

##### 差集查询

```sql
SELECT * FROM student 
LEFT JOIN(SELECT * FROM student WHERE sage <= 20)
WHERE sdept ='CS'
```

##### join

```sql
SELECT *FROM Student S1
LEFT JOIN Student S2 ON(S2.Sage<=19)
WHERE S1.Sdept='CS'
```

#####　修改自增ｉｄ的初始值

```sql
ALTER TABLE course AUTO_INCREMENT 200;
```

#### check (检查insert的值)

```sql
#可以在新建表的时候，在字段后面添加
creste table test(
	tzc_Sage INT CHECK(tzc_Sage < 35 and tzc_Sage > 15) DEFAULT NULL,
)
#也可自在新建表后添加
ALTER TABLE `tzc_student2` ADD CONSTRAINT ageRange CHECK(tzc_Sage < 35 AND tzc_Sage > 15)
```

#### INTERVAL(分段)

```sql
# 例子1
SELECT
  ELT (
    INTERVAL (`totalResult`, 0, 60, 70, 80, 90, 100),
    '不及格',
    '60~70',
    '70~80',
    '80~90',
    '>=90'
  ) report,
  COUNT(studentId) `sum`
FROM
  sc
WHERE courseId = 204
GROUP BY report
ORDER BY report
```

#### 行列转换

https://www.cnblogs.com/xiaoxi/p/7151433.html