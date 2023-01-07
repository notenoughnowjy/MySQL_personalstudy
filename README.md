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
