# MySQL

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
