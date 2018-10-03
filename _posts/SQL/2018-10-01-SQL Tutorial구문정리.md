---
layout: post
title: "SQL Tutorial 구문 정리"
date: 2018-10-01
tag:
- TIL
image: "../image/TIL.png"
categories: TIL
comments: true
---

## SQL 구문 정리

SQL은 데이터베이스를 관리하기 위한 프로그램이다.

데이터베이스는 대게 테이블을 가지고 관리하며 각 테이블은 이름으로 식별되어 레코드(행)이 쌓여 관리된다.



테이블 의 예시

| CustomerID | CustomerName                       | ContactName        | Address                       | City        | PostalCode | Country |
| ---------- | ---------------------------------- | ------------------ | ----------------------------- | ----------- | ---------- | ------- |
| 1          | Alfreds Futterkiste                | Maria Anders       | Obere Str. 57                 | Berlin      | 12209      | Germany |
| 2          | Ana Trujillo Emparedados y helados | Ana Trujillo       | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
| 3          | Antonio Moreno Taquería            | Antonio Moreno     | Mataderos 2312                | México D.F. | 05023      | Mexico  |
| 4          | Around the Horn                    | Thomas Hardy       | 120 Hanover Sq.               | London      | WA1 1DP    | UK      |
| 5          | Berglunds snabbköp                 | Christina Berglund | Berguvsvägen 8                | Luleå       | S-958 22   | Sweden  |



##### SQL 문의 기초

SQL문의 끝은 세미콜론으로 끝나야된다.

 SQL문은 대소문자를 구분하지 않는다.



###### SELECT 구문

```sql
SELECT column1, column2, ...
FROM table_name;
```

(table_name)에서 column을 불러온다.



만약 모든 행을 불러오고싶으면 아래와같이 하면된다.

> SELECT * FROM (DB이름) 



###### SELECT DISTINCT 구문

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

중복된 이름을 제외한 행을 불러온다.



###### WHERE 구문

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

condition을  충족하는 레코드만 가져온다.

###### AND, OR, NOT

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition AND condition2 AND condition3 ...;
```

AND 전부 만족하는 레코드를 가져온다

OR 하나만 만족해도 불러온다.

NOT해당 condition을 제외한 레코드를 불러온다.



###### 드ORDER BY 구문

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

column1, column2 순으로

ASC(ascending) 오름차순

DESC(descending) 내림차순 하여

정렬한다.



INSERT INTO 구문

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

해당 테이블의 지정된 열에

VALUES값을 집어 넣는다.



###### IS NULL, IS NOT NULL (NULL VALUE)구문

```sql
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
# column_name이 빈칸인 것만 가져온다.

SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL
# column_name이 빈칸이 아닌것만 가져온다.
```



###### UPDATE 구문

```SQL
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

 table_name 테이블의 where을 column을 value로 

> **참고 :**  WHERE절이 없으면 모든 행에 모든 값을 적용해버리니 주의



###### DELETE 구문

```SQL
DELETE FROM table_name
WHERE condition;
```

table_name테이블의 condition 자료를 삭제

#WHERE 구문이 없으면 테이블 자체를 삭제 한다.



###### SELECT TOP 구문

```SQL
SELECT TOP number | percent column_name(s)
FROM table_name
WHERE Condition;
```

table_name에서 (condition 조건을 만족하는) TOP number 갯수의 레코드를 불러온다.

> 퍼센트로 표현하면 아래와 같다.

```sql
SELECT TOP 50 PERCENT * FROM Customers;
```



###### MIN, MAX 구문

```SQL
SELECT MIN(column_name)
FROM table_name
WHERE condition;

SELECT MAX(column_name)
FROM table_name
WHERE condition;
```

table_name에서 (condition을 만족하는) 최소(최대) 값을 가져온다.

ex)

```sql
SELECT MIN(Price) AS SmallestPrice
FROM Products;
#AS는 제목을 붙여준다.
```

Products 테이블에서 최소값을 SmallestPrice로 가져온다.



##### COUNT, AVG, SUM 구문

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;

SELECT AVG(column_name)
FROM table_name
WHERE condition;

SELECT SUM(column_name)
FROM table_name
WHERE condition;
```

table_name에서 (condition 조건을 만족하는) 갯수를(평균을, 합산을) 구한다.



###### LIKE 구문

```sql
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;
```

column 값을 table에서 가져오는데,

columnN이 pattern인 것들을 가져온다.

> 패턴 목록
>
> LIKE 'a%' : a로 시작하는
>
> LIKE '%a' : a로 끝나는
>
> LIKE '%or%' : 둘다 만족하는
>
> LIKE '_r%'  : 두번째 글짜가 r인
>
> LIKE 'a\_%\_%' : a로 시작하면서 최소 3글자
>
> LIKE 'a%o' : a로시작하면서 o로 끝나는 글자
>
> LIKE '[bsp]%' : a,s,p로 시작하는 모든 글자
>
> LIKE '[a-d]%' : a부터 d까지의 글자로시작하는 모든 글자

*참고) _와 %는 와일드카드로 _는 단수 %는 복수를 지칭한다*



###### IN 연산자

```sql
SELECT column_name(s) FROM table_name
WHERE Column_name IN ('condition1','condition2'...)
```

 테이블에서 column_name의 값이 condition들을 만족하는 모든 행



###### BETWEEN 구문

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
(ORDER BY column_name; 해당 열을 기준으로 정렬)
```

테이블에서 column_name의 값이 value들 사이에 있는 모든 값

column_name이 단어라면 알파벳 순으로 따진다.



###### Aliases

일종의 별칭을 말한다. 해당 쿼리에서만 이름이 변경되어 나타난다.

```sql
SELECT column_name AS alias_name
FROM table_name;
```

column_name을 alias_name으로 임시로 바꾼다.



###### JOIN 구문

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
(INNER JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID; 이와같이 테이블 3개를 합칠수도 있다.)
```

Orders 테이블과 Customers테이블에서 CustomerID를 기준으로(ON) 완전히 일치하는(INNER) 부분을 가져온다. 

그리고

Orders테이블에서 OrderID, OrderDate열을

Customers테이블에서 CustomerName열을 선택해서 정렬한다.

> JOIN의 종류

* (inner) JOIN 값이 일치하는 부분만 합친다.
* LEFT (outer) JOIN 왼쪽 테이블을 전부가져오고 오른쪽 테이블은 중복되는 부분만 가져온다.
* RIGHT (outer) JOIN 오른쪽 테이블을 전부가져오고 왼쪽 테이블은 중복되는 부분만 가져온다.
* FULL (outer) JOIN 모든 테이블의 내용을 가져온다.

> JOIN은 자기자신만 가지고도 합칠 수 있다. (Self-Join)

```sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City 
ORDER BY A.City;
```

Customers를 A그룹 B그룹으로 나눈후

A와  B의 이름이 다르면서  City가 같은 리스트를 만들어

Customer1,2,city에 각각 넣어 City이름을 기준으로 정렬해 테이블을 만들어 보여준다.



###### UNION 구문

```sql
SELECT column_name(s) FROM table1
UNION (ALL)
SELECT column_name(s) FROM table2;
```

테이블1의 column과 테이블2 column 리스트를 고유값만 쭉 나열한다.

UNION ALL은 중복값까지 모두 나열한다.



###### GROUP BY 구문

```SQL
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

```SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```

Customers 테이블에서 ID갯수의 합과 Country을 가져오는데,

Country를 기준으로 갯수를 합하여 갯수의 내림차순으로 보여준다.



###### HAVING 구문

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

위와 같은 조건으로 가져오는데,

HAVING의 조건을 만족하는 레코드만 가져온다.



###### EXISTS 구문

EXISTS 뒤의 조건을 만족하는 레코드가 있는지 확인하는 구문이다.

조건을 만족하는 레코드가 하나라도 있으면 True를 반환한다.

```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```

```sql
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE SupplierId = Suppliers.supplierId AND Price < 20);
```

 TRUE를 반환하고 제품 가격이 20 미만인 공급 업체를 나열한다.