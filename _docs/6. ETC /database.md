---
title: DataBase CRUD + α
category: 6. ETC
order: 4
---
<br>
CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말이다. 사용자 인터페이스가 갖추어야 할 기능(정보의 참조/검색/갱신)을 가리키는 용어로서도 사용된다.

오늘은 MariaDB를 통해 데이터베이스 생성, 사용자 추가 및 권한부여 , CRUD 를 처리하는 방법을 알아보았다. 

CRUD stands for Create, Read, Update, and Delete, which are the fundamental data processing functions that most computer software has. It is also used as a term referring to the functionalities that a user interface should have, such as referencing, searching, and updating information.

Today, we learned how to create a database, add and grant permissions to users, and process CRUD operations using MariaDB.

**<h1>Create a DataBase and User</h1>**

>**DataBase Creation**
~~~
CREATE DATABASE database_name;
~~~
>**DataBase Selection**
~~~
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
~~~
>**Grant of Authority**
~~~
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost';
~~~
>**Save changes**
~~~
FLUSH PRIVILEGES;
~~~
<br>

**<h1>CRUD</h1>**

>**Table Creation**
~~~
CREATE TABLE table_name (
  column1 datatype,
  column2 datatype,
  column3 datatype,
  ...
);
~~~

>**Data Insertion**
~~~
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
~~~

>**Data Inquiry**
~~~
SELECT column1, column2, ...
FROM table_name
WHERE condition;
~~~
>**Data Update**
~~~
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
~~~
>**Data Deletion**
~~~
DELETE FROM table_name
WHERE condition;
~~~