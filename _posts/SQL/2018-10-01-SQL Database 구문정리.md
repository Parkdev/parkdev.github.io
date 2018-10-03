---
layout: post
title: "SQL Database 구문 정리"
date: 2018-10-02
tag:
- TIL
image: "../image/TIL.png"
categories: TIL
comments: true
---

## SQL Database구문 정리

SQL 튜토리얼 구문에 이어 데이터 베이스 구문을 정리해보자



##### CREATE DATABASE 구문

```sql
CREATE DATABASE databasename;
```

DB를 생성한다



##### DROP DATABASE 구문

```SQL
DROP DATABASE databasename;
```

DB를 삭제한다.



##### CREATE TABLE 구문

```sql
CREATE TABLE table_name (
	col1 datatype,
	col2 datatype,
	...
);

#다른 테이블을 복사해서 새로운 테이블을 만들 수도 있다.
CREATE TABLE new_table_name AS
	SELECT col1, col2, ...
	FROM existing_table_name
	WHERE condition;
```

DB를 생성이후 정리할 table를 만드는 구문



##### DROP TABLE  구문

```sql
DROP TABLE table_name;
```

테이블을 삭제한다.



##### ALTER TABLE 구문

```SQL
ALTER TABLE table_name
ADD colunm_name datatype;

#ADD 대신 DROP를 사용하면 삭제도 가능하다.
ALTER TABLE table_name
DROP COLUMN column_name datatype;

#ADD대신 ALTER을 사용하면 데이터 유형의 변경도 가능하다. (SQL버전마다 상이하다.)
ALTER TABLE table_name
ALTER COLUMN column_name datatype;
```

테이블에 열을 추가/삭제/변경할때 사용하는 구문



##### Create Constraints

```sql
CREATE TABLE table_name (
	col1 datatype constraint,
	col2 datatype constraint);
```

데이타 타입에 제약조건을 달때 사용한다.

>  제약조건의 종류

* NOT NULL : 빈칸을 허용하지 않는다.

* UNIQUE : 고유한 값을가져야한다.

* PRIMARY KEY : NOT NULL + UNIQUE이다. 빈칸을 허용하지 않으면서 고유해야한다.

* FOREIGN KEY  :  외래키 설정;  해당키가 연관된 테이블의 키(들)와 일치하지 않으면 작성을 허용하지 않는다.

  * ex) PersonID int FOREIGN KEY REFERENCES Persons (PersonID) :

    ​	PersonID는 정수여야하며 Persons테이블의 PersonID값들중 하나를 가지고 있어야 한다.

* CHECK : 모든값이 특정 조건을 만족해야 작성이 가능하다.  

  * ex) Age int CHECK (Age>= 18) : Age 행은 int값을 가져야하며 18이상이어야한다.

* DEFAULT : 해당 칸이 작성되지 않았을때 기본값을 보여준다.

  * ex) City varchar(255) DEFAULT 'Sandnes' : City 열이 비었을때 'Sandnes'가 입력된다.
  * ex) OrderDate date DEFAULT GETDATE() : OrderDate 열이 비었을때 오늘날자가 입력된다.

* INDEX: : 데이터를 빠르게 검색하기 위해 설정한다.



> 모든 CONSTRAINT는 이미 있는셀에 설정/혹은 제거가 가능하다. 

```SQL
ALTER TABLE table_name
ADD/DROP #constraint (ID);

#여러 열에 한번에 설정도 가능하다.
ALTER TABLE table_name
ADD/DROP CONSTRAINT PK_Person  #constraint (ID,LastName); 

#SQL 버전마다 조금씩 상이하니 주의가 필요하다.
```



> INDEX 는 다른 제약 조건과는 작성하는 방식이 다르다.

```SQL
#인덱스 키값은 중복이 가능하다
CREATE INDEX index_name
ON table_name (column1, column2, ...);

#index 키값을 고유하게 만들 수도 있다. (질문필요 이해가 잘안된다)
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
```



##### AUTO INCREMENT Field

```sql
CREATE TABLE Persons (
    ID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);
```

리스트를 추가할때마다 자동으로 숫자를 올려주며 작성된다.



> 숫자를 특정숫자부터 시작할려면 아래와같이하면된다.
>
> ```sql
> ALTER TABLE Persons AUTO_INCREMENT=100;
> ```
>
> 이와 같이 작성시 숫자가 100부터 시작된다.



##### SQL DATE

SQL에서 날짜를 작성할때 해당행의 날짜형식과 입력하려는 날짜형식을 꼭 일치시켜야한다.

날짜 형식은 총 4가지가 있다.

* DATE - YYYY-MM-DD

* DATETIME -  YYYY-MM-DD HH : MI : SS

* TIMESTAMP - YYYY-MM-DD HH : MI : SS

* YEAR - YYYY 또는 YY 

만약 DATETIME 형식으로 작성된 리스트를 DATE형식으로 검색하고자하면 검색실패가 뜬다.

이를 관리하기 가장 쉬운 방법은 모든 날짜를 하나로 통일 시키는게 좋다. (주로 DATE형식이 추천된다.)

