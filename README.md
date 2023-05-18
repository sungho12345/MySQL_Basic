# MySQL_Basic

## MySQL(Database2)
```
# At Mysql Shell
\sql
\c root@localhost

<데이타베이스 사용>
# 데이타베이스 만들기
CREATE DATABASE db_name;
# 데이타베이스 삭제
DROP DATABASE db_name;
# 데이타베이스 보기
SHOW DATABASES;
# 데이타베이스를 사용하겠다
USE opentutorials;

<표 만들기>
CREATE TABLE table_name(
  column1 datatype(length),
  column2 datatype(length),
  ...
  PRIMARY KEY(column1)
)

# 첫번째 column
# id INT(11) NOT NULL AUTO_INCREMENT,
id INT(11) - 정수형인 id행을 만든다.
NOT NULL - 값이 없는 것을 허용하지 않겠다.
AUTO_INCREMENT - id 행을 자동으로 1씩 늘어나게 해준다

# 두번째 column
# title VARCHAR(100) NOT NULL,

# 세번째 column
# description TEXT NULL,
TEXT 약 65000개의 문자를 허용한다.
NULL 값이 없는 것을 허용한다.

# 네번째 column
# created DATETIME NOT NULL,
DATETIME 날짜와 시간을 둘다 포함한다.

# 다섯번째 column
# author VARCHAR(30) NULL,

# 여섯번째 column
# profile VARCHAR(100) NULL

# PRIMARY KEY(id)
# 우리가 생성하는 topic table에 id(column)이 PRIMARY(메인) KEY야
```
#### set password
```
SET PASSWORD = PASSWORD('');
```

### CRUD(create read update delete)
#### INSERT
```
INSERT INTO table_name (column1, ...)
VALUES (value1, ....)
# INSERT INTO topic (title,description,created,author,profile) VALUES('MySQL','MySQL is ...',NOW(),'egoing','developer');
DESC topic; # 토픽의 구조가 나옴
```
#### SELECT
```
# 엄청 많이 쓰임.
SELECT* FROM topic;
+----+------------+------------------+---------------------+--------+---------------------------+
| id | title      | description      | created             | author | profile                   |
+----+------------+------------------+---------------------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...     | 2023-04-10 19:50:01 | egoing | developer                 |
|  2 | ORACLE     | ORACLE is...     | 2023-04-10 19:51:50 | egoing | developer                 |
|  3 | SQL Server | SQL Server is... | 2023-04-10 19:53:23 | duru   | data administrator        |
|  4 | PostgreSQL | PostgreSQL is... | 2023-04-10 19:54:53 | taeho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is...    | 2023-04-10 19:55:32 | egoing | developer                 |
+----+------------+------------------+---------------------+--------+---------------------------+

# topic을 선택해서 데이터를 보겠다.

# 보고싶은 데이터만 추출 가능
SELECT id,title,created,author FROM topic;

+----+------------+---------------------+--------+
| id | title      | created             | author |
+----+------------+---------------------+--------+
|  1 | MySQL      | 2023-04-10 19:50:01 | egoing |
|  2 | ORACLE     | 2023-04-10 19:51:50 | egoing |
|  3 | SQL Server | 2023-04-10 19:53:23 | duru   |
|  4 | PostgreSQL | 2023-04-10 19:54:53 | taeho  |
|  5 | MongoDB    | 2023-04-10 19:55:32 | egoing |
+----+------------+---------------------+--------+
```
```
# WHERE을 통해서 author가 egoing인 것만 볼 수 있다.
SELECT id, title, created, author FROM topic WHERE author='egoing';

+----+---------+---------------------+--------+
| id | title   | created             | author |
+----+---------+---------------------+--------+
|  1 | MySQL   | 2023-05-17 22:56:53 | egoing |
|  2 | ORACLE  | 2023-05-17 22:58:40 | egoing |
|  5 | MongoDB | 2023-05-17 23:01:10 | egoing |
+----+---------+---------------------+--------+
```

```
# ORDER BY {col_name} DESC | ASC 를 이용해서 sort기능을 사용할 수 있다.
SELECT id, title, created, author FROM topic WHERE author='egoing' ORDER BY id DESC;
# DESC 내림차순, ASC 오름차순
+----+---------+---------------------+--------+
| id | title   | created             | author |
+----+---------+---------------------+--------+
|  5 | MongoDB | 2023-05-17 23:01:10 | egoing |
|  2 | ORACLE  | 2023-05-17 22:58:40 | egoing |
|  1 | MySQL   | 2023-05-17 22:56:53 | egoing |
+----+---------+---------------------+--------+
```

```
LIMIT {숫자}를 통해서 가져올 데이터를 제한할 수 있다.
SELECT id, title, created, author FROM topic WHERE author='egoing' ORDER BY id DESC LIMIT 2;

+----+---------+---------------------+--------+
| id | title   | created             | author |
+----+---------+---------------------+--------+
|  5 | MongoDB | 2023-05-17 23:01:10 | egoing |
|  2 | ORACLE  | 2023-05-17 22:58:40 | egoing |
+----+---------+---------------------+--------+
```
#### UPDATE
```
UPDATE topic SET description='Oracle is ...', title='ORACLE' WHERE id=2;

문법 : 
UPDATE table_reference SET assignment_list(col_name = value) WHERE where_condition;

전
|  2 | ORACLE     | ORACLE is         |
후
|  2 | ORACLE     | Oracle is ...     |

WHERE을 절대 빠트리지 말자
```
#### DELETE
```
DELETE FROM topic WHERE id = 5;
id 값이 5인 데이터가 삭제됨.
WHERE을 절대 빠트리지 말자(없으면 topic 전부 삭제됨)
```
## 관계형 데이터베이스

### author
|id|name|profile|
|---|---|-------|
|1|egoing|developer|
|2|duru|database administrator|
|3|taeho|data scientist, developer|

### topic
|id|title|description|created|author|
|---|---|-------|-------|-------|
|1|MySQL|My SQL is ...|2018-01-10|egoing|
|2|ORAACLE|Oracle is ...|2018-01-15|egoing|
|3|SQL Server|SQL Server is ...|2018-01-18|duru|
|4|PosrgreSQL|PostgreSQL|2018-01-20|taeho|
|5|MongoDB|MongoDB is ...|2018-01-30|egoing|

### 바꾸고 싶은 방향
|id|title|description|created|author_id|
|---|---|-------|-------|-------|
|1|MySQL|My SQL is ...|2018-01-10|1|
|2|ORAACLE|Oracle is ...|2018-01-15|1|
|3|SQL Server|SQL Server is ...|2018-01-18|2|
|4|PosrgreSQL|PostgreSQL|2018-01-20|3|
|5|MongoDB|MongoDB is ...|2018-01-30|1|

#### 저장은 분산해서, 보는건 합쳐서

### 테이블 분리하기
```
# author 테이블 만들기
CREATE TABLE author(
    id INT(11) NOT NULL AUTO_INCREMENT,   name VARCHAR(20) NOT NULL,   profile VARCHAR(200) NULL,   PRIMARY KEY(id) 
    );


DESC author;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int          | NO   | PRI | NULL    | auto_increment |
| name    | varchar(20)  | NO   |     | NULL    |                |
| profile | varchar(200) | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+

# 테이블에 요소 추가
INSERT INTO author (id, name, profile) VALUES(1, 'egoing', 'developer');

# author_id로 변환
INSERT INTO topic(id, title, description, created, author_id) VALUES(1, 'MySQL', 'MySQL is ...', '2018-1-1 12:10:11', 1);
+----+-------+--------------+---------------------+-----------+
| id | title | description  | created             | author_id |
+----+-------+--------------+---------------------+-----------+
|  1 | MySQL | MySQL is ... | 2018-01-01 12:10:11 |         1 |
+----+-------+--------------+---------------------+-----------+


INSERT INTO topic(id, title, description, created, author_id) VALUES(2, 'Oracle', 'Oracle is ...', '2018-01-03 13:01:10', 1);


INSERT INTO author (id, name, profile) VALUES(2, 'duru', 'data administrator');

INSERT INTO topic(id, title, description, created, author_id) VALUES(3, 'SQL Server', 'SQL Server is ...', '2018-01-20 11:01:10', 2);


INSERT INTO author (id, name, profile) VALUES(3, 'ateho', 'data scientist, developer');

INSERT INTO topic(id, title, description, created, author_id) VALUES(4, 'PostgreSQL', 'Postgre is ...', '2018-01-23 1:3:3', 3);

INSERT INTO topic(id, title, description, created, author_id) VALUES(5, 'MongoDB', 'MongoDB is ...', '2018-01-30 12:31:3', 1);

+----+------------+-------------------+---------------------+-----------+
| id | title      | description       | created             | author_id |
+----+------------+-------------------+---------------------+-----------+
|  1 | MySQL      | MySQL is ...      | 2018-01-01 12:10:11 |         1 |
|  2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 |         1 |
|  3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 |         2 |
|  4 | PostgreSQL | Postgre is ...    | 2018-01-23 01:03:03 |         3 |
|  5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 |         1 |
+----+------------+-------------------+---------------------+-----------+
```

### Join
#### 테이블과 테이블을 조인하자
```
# SELECT * FROM topic;
+----+------------+-------------------+---------------------+-----------+
| id | title      | description       | created             | author_id |
+----+------------+-------------------+---------------------+-----------+
|  1 | MySQL      | MySQL is ...      | 2018-01-01 12:10:11 |         1 |
|  2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 |         1 |
|  3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 |         2 |
|  4 | PostgreSQL | Postgre is ...    | 2018-01-23 01:03:03 |         3 |
|  5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 |         1 |
+----+------------+-------------------+---------------------+-----------+

# SELECT * FROM author;
+----+--------+---------------------------+
| id | name   | profile                   |
+----+--------+---------------------------+
|  1 | egoing | developer                 |
|  2 | duru   | data administrator        |
|  3 | ateho  | data scientist, developer |
+----+--------+---------------------------+
```
#### author_id의 값과 같은 값을 가지고 있는 author테이블에 있는 행을 가져와서 topic테이블에 붙여라
```
SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----+------------+-------------------+---------------------+-----------+----+--------+---------------------------+
| id | title      | description       | created             | author_id | id | name   | profile                   |
+----+------------+-------------------+---------------------+-----------+----+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2018-01-01 12:10:11 |         1 |  1 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 |         1 |  1 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 |         2 |  2 | duru   | data administrator        |
|  4 | PostgreSQL | Postgre is ...    | 2018-01-23 01:03:03 |         3 |  3 | ateho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 |         1 |  1 | egoing | developer                 |
+----+------------+-------------------+---------------------+-----------+----+--------+---------------------------+

SELECT topic.id,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----+------------+-------------------+---------------------+--------+---------------------------+
| id | title      | description       | created             | name   | profile                   |
+----+------------+-------------------+---------------------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2018-01-01 12:10:11 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 | duru   | data administrator        |
|  4 | PostgreSQL | Postgre is ...    | 2018-01-23 01:03:03 | ateho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 | egoing | developer                 |
+----+------------+-------------------+---------------------+--------+---------------------------+

SELECT topic.id AS topic_id,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----------+------------+-------------------+---------------------+--------+---------------------------+
| topic_id | title      | description       | created             | name   | profile                   |
+----------+------------+-------------------+---------------------+--------+---------------------------+
|        1 | MySQL      | MySQL is ...      | 2018-01-01 12:10:11 | egoing | developer                 |
|        2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 | egoing | developer                 |
|        3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 | duru   | data administrator        |
|        4 | PostgreSQL | Postgre is ...    | 2018-01-23 01:03:03 | ateho  | data scientist, developer |
|        5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 | egoing | developer                 |
+----------+------------+-------------------+---------------------+--------+---------------------------+
```
<a href='https://www.youtube.com/watch?v=h_XDmyz--0w&list=PLuHgQVnccGMCgrP_9HL3dAcvdt8qOZxjW&index=2'>참고 강의</a>
