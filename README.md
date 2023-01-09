# MySQL

Youtube Link : https://youtu.be/vgIc4ctNFbc


- DML(Data Manipulation Language)
    - 데이터 조작 언어
    - 데이터를 조작(선택,삽입,수정,삭제)하는 데 사용되는 언어
    - DML구문이 사용되는 대상은 테이블의 행
    - DML 사용하기 위해서는 꼭 그 이전에 테이블이 정의되어 있어야 함
    - SQL문 SELECT, INSERT, UPDATE, DELETE가 이 구문에 해당
    - 트랜잭션(Transaction)이 발생하는 SQL도 이 DML에 속함
        - 테이블의 데이터를 변경(입력/수정/삭제)할 때 실제 테이블에 완전히 적용하지 않고, 임시로 적용시키는 것
        - 취소가능

---

- DDL(Data Definition Language)
    - 데이터 정의 언어
    - 데이터베이스, 테이블, 뷰, 인덱스 등의 데이터 베이스 개체를 생성 / 삭제 / 변경하는 역할
    - CREATE, DROP, ALTER 구문
    - DDL은 트랜잭션 발생시키지 않음
    - ROLLBACK이나 COMMIT 사용 불가
    - DDL문은 실행 즉시 MYSQL에 적용

---

- DCL(Data Control Language)
    - 데이터 제어 언어
    - 사용자에게 어떤 권한을 부여하거나 빼앗을 때 주로 사용하는 구문
    - GRANT / REVOKE / D….

---

## SHOW DATABASES

- 현재 서버에 어떤 DB가 있는지 보기


---

## USE

- 사용할 데이터베이스 지정
- 지정해 놓은 후 특별히 다시 USE문 사용하거나 다른 DB를 사용하겠다고 명시하지 않는 이상 모든 SQL문은 지정 DB에서 수행


- Workbench에서 직접 선택해 사용 가능
    - [Navigator]→[SCHEMAS]→데이터베이스 선택


---

## SHOW TABLE

- 데이터베이스 world의 테이블 이름 보기


---

## SHOW TABLE STATUS

- 데이터베이스 wrold의 테이블 정보 조회


---

## Describe

- city 테이블에 무슨 열이 있는지 확인
    - DESCRIBE city;
    
    
    - DESC city


---

### SELECT

- <SELECT…FROM>
- 요구하는 데이터를 가져오는 구만
- 일반적으로 가장 많이 사용되는 구문
- 데이터베이스 내 테이블에서 원하는 정보를 추출
- SELET의 구문 형식

```sql
SELECT * FROM city; //전체 보기

SELECT NAME from city // city안의 name만 보기

SELECT name, population from city; //name과 population만 보기

```

- SELECT 열 이름
    - 테이블에서 필요로 하는 열만 가져오기 가능
    - 여러 개으 열을 가져고오 싶을 때는 콤마로 구분
    - 열 이름의 순서는 출력하고 싶은 순서대로 배열 가능

### SELECT FROM WHERE

- 기본적인 WHERE 절
    - 조회하는 결과에 특정한 조건으로 원하는 데이터만 보고 싶을 때 사용
    - SELECT 필드 이름 
    FROM 테이블 이름 WHERE 조건식;
    - 조건이 없을 경우 테이블의 크기가 클수록 찾는 시간과 노력이 증가

```sql
SELECT * // 전체선택
FROM city //city 선택
WHERE population >= 8000000; // 800만이 넘는 도시들만 보여줘
```

- MySQL 함수 및 연산자 :

https://dev.mysql.com/doc/refman/8.0/en/functions.html

- 관계 연산자의 사용
    - OR 연산자
    - AND 연산자
    - 조건 연산자(=, <, >, ≤, ≥, <>, ≠등)
    - 관계 연산자(NOT, AND, OR 등)
    - 연산자의 조합으로 데이터를 효율적으로 추출

```sql
SELECT *
FROM city
WHERE Population < 8000000
AND population > 7000000; //700만~800만 사이
```

### 한국에 있는 도시들 보기

```sql
select *
from city
where countrycode = 'kor'
```

### 미국에 있는 도시들 보기

```sql
select *
from city
where countrycode = 'usa'
```

### 한국에 있는 도시 중 인구수가 1,000,000이 넘는 도시

```sql
select *
from city
where countrycode = 'KOR'
and population > 1000000
```

### BETWEEN

- 데이터가 숫자로 구성되어 있어 연속적인 값은 BETWEEN…AND 사용 가능

```sql
SELECT *
FROM city
where population between 7000000 and 8000000
```

### IN

- 이산적인(Discrete) 값의 조건에서는 IN() 사용 가능

```sql
select *
from city
where name in('seoul', 'new york', 'tokyo')
```

### 한국, 미국, 일본의 도시들 보기

```sql
select *
from city
where countrycode in('KOR', 'USA', JPN')
and population > 7000000
```

### LIKE

- 문자열의 내용 검색하기 위해 LIKE 연산
- 문자 뒤에 %- 무엇이든(%) 허용
- 한 글자와 매치하기 위해서는 ‘_’ 사용

```sql
select *
from city
where countrycode LIKE 'KO_'
```

```sql
select *
from city
where Name LIKE 'tel %'
```

### Sub Query

- 서브 쿼리
- 쿼리문 안에 또 쿼리문이 들어 있는 것
- 서브 쿼리의 결과가 둘 이상이 되면 에러 발생

```sql
select *
from city
where countrycode = ( SELECT countrycode from city where name = 'seoul');
// country코드는 모르는데 seoul이 있는 나라의 도시들을 출력하고 싶을때 사용
```

### ANY

- 서브쿼리의 여러 개의 결과 중 한 가지만 만족해도 가능
- SOME은 ANY와 동일한 의미로 사용
- =ANY 구문은 IN과 동일한 의미

```sql
select *
from city
where population > Any (	SELECT population 
							from city 
                            where District = 'new york');
// ANY의 결과 중 만족하는 것들을 모두 출력해낸다.
```

```sql
select *
from city
where population > SOME (	SELECT population 
							from city 
                            where District = 'new york');
// ANY의 결과 중 만족하는 것들을 모두 출력해낸다.
```

### ALL

- 서브쿼리의 여러개의 결과를 모두 만족 시켜야 함

```sql
select *
from city
where population > ALL (	SELECT population 
							from city 
                            where District = 'new york');
// ANY의 결과 중 만족하는 것들을 모두 출력해낸다.
```

### ORDER BY

- 결과가 출력되는 순서를 조절하는 구문
- 기본적으로 오름차순(ASCENDING) 정렬 [줄임 ASC]
- 내림차순(DESCENDING)으로 정렬
    - 열 이름 뒤에 DESC 적어줄 것
- DESC(내림차순)은 default 이므로 생략 가능

```sql
select *
from city
order by population DESC
```

```sql
select *
from city
order by population ASC
```

- ORDER BY 구문을 혼합해 사용하는 구문도 가능

```sql
select *
from city
order by countrycode asc, population desc

//countrycode는 오름차순, pupulation은 내림차순으로 혼합 사
```

### 인구수로 내림차순하여 한국에 있는 도시 보기

```sql
select *
from city
where countrycode = 'kor'
order  by population desc;
```

### 국가 면적 크기로 내림차순하여 나라 보기(country table)

```sql
select *
from country
order by surfacearea desc;
```

### DISTINCT(분명한)

- 중복된 것은 1개씩만 보여주면서 출력
- 테이블의 크기가 클수록 효율적

```sql
select distinct countrycode
from city;
```

### LIMIT

- 출력 개수를 제한
- 상위은 N개만 출력하는 ‘LIMIT N’구문
- 서버의 처리량을 많이 사용해 서버의 전반적인 성능을 나쁘게 하는 악성 쿼리문을 개선할 때 사용

```sql
select *
from city
order by population desc
LIMIT 10
```

### Group By

- 그룹으로 묶어주는 역할
- 집계 함수(Aggregate Function)를 함께 사용
    - AVG() : 평균
    - MIN() : 최소값
    - MAX() : 최대값
    - COUNT : 행의 개수
    - COUNT(DISTINCT) : 중복 제외된 행의 개수
    - STDEV() : 표준 편차
    - VARIANCE() : 분산
- 효율적인 데이터 그룹화(Grouping)
- 읽기 좋게 하기 위해 별칭(Alias) 사용

```sql
select countrycode, max(population)
from city
group by CountryCode;
//country코드를 묶고 population(인구 수)가 가장 많은 사람들
//위주로 묶어 달라
```

```sql
select countrycode, min(population)
from city
group by CountryCode;
```

```sql
select countrycode, avg(population)
from city
group by CountryCode;
```

- as ‘______’를 쓰면 colum명을 바꿀 수 있음
    - as ‘maximum’
    - as ‘minumum’
    - as ‘averge’

### 도시는 몇개인가?

```sql
select count(*)
from city;
```

### 도시들의 평균 인구수는?

```sql
select avg(population)
from city;
```

### HAVING

- WHERE과 비슷한 개념으로 조건 제한
- 집계 함수에 대해서 조건 제한하는 편리한 개념
- HAVING절은 반드시 GROUP BY절 다음 에 나와야 함

```sql
select countrycode, max(population)
from city
group by countrycode
having max(Population) > 8000000;
```

### ROLLUP

- 총합 또는 중간합계가 필요할 경우 사용
- GROUP BY 절과 함께 WITH ROLLUP문 사용

```sql
select countrycode, name, sum(population) as 'sum'
from city
group by countrycode, name with rollup;

//countrycode, name, sum(population)을 보여주고
// city에서 값을 불러온다.
// countrycode로 그룹을 묶어주고, countrycode의 중간 합계 후
// 합을 보여준다.
```

### JOIN

[JOIN](https://www.notion.so/JOIN-375e3a5e697145e19b58ae116f8a83cd)

- JOIN은 데이터베이스 내의 여러 테이블에서 가져온 레코드를 조합하여 하나의 테이블이나 결과 집합으로 표현

```sql
select *
from city
join country on city.CountryCode = country.Code;
```

### city, country, countrylanguage 테이블 3개를 JOIN 하기

```sql
select *
FROM city
JOIN country on city.CountryCode = country.Code
JOIN countrylanguage on city.CountryCode = countrylanguage.CountryCode;
```

```sql
select *
from city;

select *
from country;

select *
from countrylanguage;

//세개의 테이블을 각각 실행해보면서 겹치는 나라의 코드를 이용해서 테이블 합치기
```

## MySQL 내장함수

- 사용자가 편의를 위해 다양한 기능의 내장 함수를 미리 정의하여 제공
- 대표적인 내장 함수의 종류
    - 문자열 함수
    - 수학 함수
    - 날짜와 시간 함수
    

### LENGTH()

- 전달받은 문자열의 길이를 반환

```sql
select length('asdasdwq')
```

### CONCAT()

- 전달받은 문자열을 모두 결합하여 하나의 문자열로 반환
- 전달받은 문자열 중 하나라도 NULL이 존재하면 NULL을 반환

```sql
select concat('my', 'sql op', 'en source')
=> mysql open source
```

```sql
concat('mysql','null','open source')
=> null  
```

### LOCATE()

- 문자열 내에서 찾는 문자열이 처음으로 나타나는 위치를 찾아서 해당 위치를 반환
- 찾는 문자열이 문자열 내에 존재하지 않으면 0을 반환
- MySQL에서는 문자열의 시작 인덱스를 1부터 계산

```sql
select locate('abc', 'ababababaabacabc'),
locate('abc', 'abababaabafacacabacabacabcbabc');

-> mysql은 인덱스번호를 1부터 매긴다.
```

### LEFT(), RIGHT()

- LEFT() : 문자열의 왼쪽부터 지정한 개수만큼의 문자를 반환
- RIGHT() : 문자열의 오른쪽부터 지정한 개수만큼의 문자를 반환

```sql
select
LEFT('MySQL is an open source relational database management system', 5)
RIGHT('MySQL is an open source relational database mangement system', 6)

left => MySQL
RIGHT => system
```

### LOWER(), UPPER()

- LOWER() : 문자열의 문자를 모두 소문자로 변경
- UPPER() : 문자열의 문자를 모두 대문자로 변경

```sql
select 
LOWER('MySQL is an open source relational database management system'),
UPPER('MySQL is an open source relational database mangement system);
```

### REPLACE()

- 문자열에서 특정 문자열을 대체 문자열로 교체

```sql
select replace('MSSQL', 'MS', 'MY');

=> MYSQL
```

### TRIM()

- 문자열의 앞이나 뒤, 또는 양쪽 모두에 있는 특정 문자를 제거
- TRIM() 함수에서 사용할 수 있는 지정자
    - BOTH : 전달받은 문자열의 양 끝에 존재하는 특정 문자를 제거 (기본 설정)
    - LEADING : 전달받은 문자열 앞에 존재하는 특정 문자를 제거
    - TRAILING : 전달받은 문자열 뒤에 존재하는 특정 문자를 제거
- 만약 지정자를 명시하지 않으면, 자동으로 BOTH로 설정
- 제거할 문자를 명시하지 않으면, 자동으로 공백을 제거

```sql
select TRIM('	##MYSQL##	'), //공백을 없애준다.
TRIM(LEADING '#' FROM '##MYSQL##'), //문자열 앞의 #을 없애준다.
TRIM(TRAILING '#' FROM '##MYSQL##'); //문자열 뒤의 #을 없애준다.

TRIM both => ##MYSQL##
TRIM leading => MYSQL##
TRIM trailing => ##MYSQL
```

### FLOOR(), CELI(), ROUNT()

- FLOOR() : 내림
- CEIL() : 올림
- ROUND() : 반올림

```sql
SELECT FLOOR(10.95),
FLOOR(11.01),
FLOOR(-10.95),
FLOOR(-11.01),
CEIL(10.95),
CEIL(11.01),
CEIL(11),
CEIL(-10.95),
CEIL(-11.01),
ROUND(10.49),
ROUND(10.5),
ROUND(-10.5),
ROUND(-10.49);
```

### FROMAT()

- 숫자 타입의 데이터를 세 자리마다 쉼표(,)를 사용하는 ‘#,###,###.##’형식으로 변환
- 반환되는 데이터의 형식은 문자열 타입
- 두 번째 인수는 반올림할 소수 부분의 자릿수

```sql
select from('123123123123.123123,5);
=> 123,123,123,123.12312

select from('12345123123123.12312,4);
=> 12,345,123,123,123.1231
```


### SQRT(), POW(), EXP(), LOG()

- SQRT() : 양의 제곱근
- pow() : 첫 번째 인수로는 밑수를 전달하고, 두 번째 인수로는 지수를 전달하여 거듭제곱 계산
- EXP() : 인수로 지수를 전달받아, e의 거듭제곱을 계산
- LOG() : 자연로그 값을 계산

```sql
select sqrt(4), pow(2,3), exp(3), log(3);
```

### SIN(), COS(), TAN()

- SIN() : 사인값 반환
- COS() : 코사인값 반환
- TAN() : 탄젠트값 반환

```sql
select sin(PI()/2),
cos(PI()),
TAN(PI()/4);
```

### ABS(), RAND()

- ABS(X) : 절대값을 반환
- RAND() : 0.0보다 크거나 같고 1.0보다 적은 하나의 실수를 무작위로 생성

```sql
select abs(-3), rand(), round(rand()*100,0);
```

### NOW(), CURDATE(), CURTIMA()

- NOW() : 현재 날짜와 시간을 반환, 반환되는 값을 ‘YYYY-MM-DD HH:MM:SS’또는 YYYYMMDDHHMMSS형태로 반환
- CYRDATE() :현재 날짜를 반환, 이때 반환 되는 값을 ’YYYY-MM-DD’또는 YYYYMMDD형태로 반환
- CURTIME() :환현재 시작을 반환, 이때 반환되는 값을 ‘HH:MM:SS’또는 HHMMSS형태로 반환

```sql
select now(),
curdate(),
curtime();
```

### DATE(), MONTH(), DAY(), HOUR(), MINUTE(), SECOND()

- DATE() : 전달받은 값에 해당하는 날짜 정보를 반환
- MONTH() : 월에 대당하는 값을 반환하며, 0부터 12사이의 값을 가짐
- DAY() : 일에 해당하는 값을 반환하며, 0부터 31사이의 값을 가짐
- HOUR() : 시간에 해당하는 값을 반환하며, 0부터 23사이의 값을 가짐
- MINUTE() : 분에 해당하는 값을 반환하며, 0부터 59사이의 값을 가짐
- SECOND() : 초에 해당하는 값을 반환하며, 0부터 59사이의 값을 가짐

```sql
select now(), date(now()), month(now()), day(now()), hour(now()), 
minute(now()), second(now());
```

### MONTHNAME(), DAYNAME()

- MONTHNAME() :  월에 해당하는 이름을 반환
- DAYNAME() : 요일에 해당하는 이름을 반환

```sql
select
now(),
monthname(now()),
dayname(now());
```

### DAYOFWEEK(), DAYOFMONTH(), DAYOFYEAR()

- DAYOFWEEK() : 일자가 해당 주에서 몇 번째 날인지를 반환, 1부터 7사이의 값을 반환(일요일 = 1, 토요일 = 7)
- DAYOFMONTH() : 일자가 해당 월에서 몇 번째 날인지를 반환, 9부터 31사이의 값을 반환
- DAYOFYEAR() : 일자가 해당 연도에서 몇 번째 날인지를 반환, 1부터 366사이의 값을 반환

```sql
select
dayofmonth(now()),
dayofweek(now()),
dayofyear(now());
```

### DAY_FORMAT

- 전달받은 형식에 맞춰 날짜와 시간 정보를 문자열로 반환
- MySQL Date and TIME Function: https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html

```sql
select
date_format(now(), '%D %y %a %d %m %n %j')
```

# SQL 고급

### CREATE TABLE AS SELECT

- city테이블과 똑같은 city2테이블 생성

```sql
create table city2 as select * from city;

select * from city2;
```

### CREATE DATABASE

- CREATE DATABASE 문은 새로운 데이터베이스를 생성
- USE문으로 새 데이터베이스를 사용

```sql
create database joonyeong;

use joonyeoung;
```

### CREATE TABLE(MySQL Workbench)

- 데이터 타입 : https://dev.mysql.com/doc/refman/8.0/en/data-types.html

```sql
select * from test;
```

### CREATE TABLE

```sql
CREATE TABLE test2(
	id INT NOT NULL PRIMARY KEY, //기본적으로 테이블이 PRIMARY라는 인덱스를 가지게 된다
	col1 INT NULL,
	col2 FLOAT NULL,
	col3 VARCHAR(45) NULL
);

SELECT * FROM test2;
```

### ALTER TABLE

- ALTER TABLE문과 함께 ADD 문을 사용하면, 테이블에 컬럼을 추가할 수 있음

```sql
ALTER TABLE test2
add col4 INT NULL;

select * from test2;

desc test2;

alter table test2
modify col4 varchar(20) NULL;

alter table test2
drop col4;
```

- ALTER TABLE 문과 함께 MODIFY 문을 사용하면, 테이블의 컬럼 타입을 변경할 수 있음

## 인덱스(INDEX)

- 테이블에서 원하는 데이터를 빠르게 찾기 위해 사용
- 일반적으로 데이터를 검색할 때 순서대로 테이블 전체를 검색하므로 데이터가 많으면 많을수록 탐색하는 시간이 늘어남
- 검색과 질의를 할 때 테이블 전체를 읽지 않기 때문에 빠름
- 설정된 컬럼 값을 포함한 데이터의 삽입, 삭제, 수정 작업이 원본 테이블에서 이루어질 경우, 인덱스도 함께 수정되어야 함
- 인덱스가 있는 테이블은 처리 속도가 느려질 수 있으므로 수정보다는 검색이 자주 사용되는 테이블에서 사용하는 것이 좋음

### CREATEINDEX

- CREATEINDEX 문을 사용하여 인덱스를 생성

```sql
CREATE INDEX Col1Idx
on test (col1)
```

### SHOW INDEX

- 인덱스 정보 보기

```sql
SHOW INDEX FROM test;
```

### CREATE UNIQUE INDEX

- 중복 값을 허용하지 않는 인덱스

```sql
CREATE UNIQUE INDEX Col2Idx
on test (col2);

show index from test;
```
