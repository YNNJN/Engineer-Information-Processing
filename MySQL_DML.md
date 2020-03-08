## MySQL 기본 사용



#### Sql> 프롬프트 상에서

- show databases; DB들의 리스트를 표시하라
- use world; WORLD DB를 사용한다    #World: 샘플 DB
- show tables; WORLD DB의 테이블 리스트를 표시
- desc city; city 테이블의 구조를 표시하라
- Select * from city; city 테이블의 내용을 표시하라



#### GUI -> 쿼리 탭에서

- 그냥 번개 표시는 전체 명령 수행
- 번개 i 표시는 한 줄 씩 수행

#### CLI -> mysql -uroot -p









## SQL의 종류



#### 1. DML

- Data Manipulation Language
- 테이블의 데이터를 CRUD 하는 기능
- SQL문으로는
  - INSERT, DELETE, UPDATE, SELECT
- UPDATE, DELETE하다가 DB 통으로 변경할 수 있으니, 조작 후 SELECT로 검색하여 결과를 확인하는 습관 들일 것



#### 2. DDL

- Data Definition Language
- DB, 테이블의 스키마를 정의, 수정하는 기능
- 테이블 생성, 칼럼 추가, 타입 변경, 각종 제약 조건 지정, 수정 등
- SQL문으로는
  - CREATE, DROP, ALTER



#### 3. DCL

- Data Control Language
- DB나 테이블의 접근 권한이나 CRUD 권한을 정의하는 기능
- 특정 사용자에게 테이블의 조회 권한 허가 / 금지 등
- SQL문으로는
  - GRANT(DB 객체에 권한을 부여함), REVOKE(DB 객체 권한을 취소함)









## 중복성 제거 / 논리연산자 / 결과값 정렬



### DISTINCT 연산자

- SELECT문의 결과값에서 특정 칼럼만 출력할 경우 중복된 값들이 나오는 경우, 이를 제거해서 표시
- 중복되는 값을 제거해서 실제 값이 몇 개가 나오는지 확인할 때 이용
- select distinct 컬럼명1, 컬럼명2, ... from 테이블 명;



### 논리연산자(AND, OR, NOT)

- select * from 테이블명 where (not) 조건1 and/or (not) 조건2 ...;



### 논리연산자(IN, BETWEEN)

- select * from 테이블명 where in ( 조건 );
- select * from 테이블명 where and ( 조건 between and );



### 결과 정렬(ORDER BY)

- select문 결과깞을 특정한 컬럼을 기준으로 오름차순 / 내림차순으로 정렬해서 표시
- select * from 테이블명 where 조건절 order by 컬럼명 asc / desc ...;
- 오름차순이 디폴트, 여러 개의 컬럼을 나열하면 순서대로 정렬됨









## 결과값 일부 조회 / 집합함수 / 유용함수



### 결과값 일부 조회(LIMIT(ROWNUM, TOP))

- sql 쿼리 결과 중 상위 몇 개만 보여주는 쿼리
- select 컬럼명1, 컬럼명2, ... from 테이블명 where 조건절 limit 숫자;
- 대표적인 비표준기능(DBMS종류마다 다름)
  - 오라클 - ROWNUM
  - SQLServer - TOP



### 집합함수

- 테이블 전체 레코드를 대상으로 특정 컬럼을 적용해서 한 개의 값을 리턴하는 함수
- count(), avg(), sum(), min(), max(), first(), last(), ...
- select aggregation_function(컬럼명) from 테이블명 where 조건절;



### 유용함수

- LENGTH(), MID(), UPPER(), LOWER(), ROUND()









## JOIN / 별명(ALIAS) / VIEW



### JOIN(INNER / LEFT / RIGHT / FULL)

- 서로 다른 테이블을 공통 컬럼을 기준으로 합치는 테이블 단위 연산
- 조인의 결과 테이블은 이전 테이블의 컬럼 수의 합과 같음
- select * from 테이블1 join 테이블2 on 테이블1.컬럼명 = 테이블2.컬럼명 ...
- 조인 시 서로 다른 테이블에 같은 컬럼명이 존재하니, 구분 위해 테이블명.컬럼명 으로 표시
- 조인의 종류는 다음과 같음
  - 조인 시 NULL 값을 허용하는 내부조인(불가)과 외부조인(허용)으로 구분
  - INNER JOIN(LEFT JOIN / RIGHT JOIN / FULL JOIN)
  - INNER JOIN: 조인 시 NULL 값을 허용하지 않음 (NULL값을 가진 레코드는 조인 결과에 빠짐)
  - LEFT JOIN: 조인 시 JOIN의 왼쪽 테이블의 NULL 값을 포함해서 표시
  - RIGHT JOIN: 조인 시 JOIN의 오른쪽 테이블의 NULL 값을 포함해서 표시
  - FULL JOIN: MySQL은 지원 x

- NULL 값을 확인하고 싶다면 일반적 JOIN과 LEFT or RIGHT JOIN의 값을 비교
- is NULL / is not NULL



### ALIAS

- SQL 쿼리 결과 생성 시 컬럼명에 대한 별명을 사용해 표시하는 기능
- select 테이블명1.컬럼명1 as 별명1, 테이블명2.컬럼명2 as 별명2 from
- 조인 시 많이 사용됨



### VIEW

- SQL 쿼리의 결과값으루 임시 테이블로 저장해서 사용 가능
- JOIN과 ALIAS의 경우 조회에 사용한 변경이 남아있지 않은 것과 비교됨
- 뷰는 실제 테이블과 동일한 권한을 가짐
- 사용 용도가 끝나면 명시적으로 삭제해야 함(DROP VIEW ...)
- create view 뷰명 as select ...
- view는 반환값 없음 -> select * from sampleView; 로 확인해야









## SELECT문과 INSERT문의 응용



### SELECT INTO

- 쿼리 결과를 새 테이블로 만듦
- create table 테이블명 select * from 테이블명
- 기존에 존재하지 않는 테이블이 새로 생성됨 (일종의 뷰와 동일한 효과)



### INSERT INTO SELECT

- 쿼리 결과를 기존의 테이블에 추가함(기존 테이블이 존재해야)
  - 즉 기존의 insert, select를 하나의 쿼리로 합친 것
- Insert into 테이블명1 select * from 테이블명2 where 조건절....;
- (전제 조건)select하는 테이블과 insert하는 테이블은 동일한 구조를 가져야
- 두 개의 별도 쿼리를 하나로 합침



### CASE ... WHEN ... END

- SQL의 조건문에 해당
- 조건값에 따른 처리를 구분할 수 있음
- CASE WHEN 조건값1 THEN ...
            WHEN 조건값1 THEN ...
            ELSE
  END









## SELECT문에서 LIKE 검색



### LIKE 검색

- 정확한 키워드를 모를 경우 일부 만으로 검색하는 방법
- 와일드카드(%, _)를 사용하여 패턴 매칭
  - select 컬럼명 from 테이블명 where 컬럼명 like 패턴;
  - %: 0~n글자, _: 1글자
- 매칭 위해 DBBMS에 부담이 많으므로 LIKE에 OR과 같은 논리조건자를 중복해서 사용하지 않는 게 좋음
  - (X) select * from 테이블명 where like 컬럼명1 like ... or 컬럼명2 like ...;
  - 이것이 꼭 필요하다면 full text search와 같은 고급 like 패턴을 사용함



### NULL 값

- 해당 칼럼의 값이 없다는 의미 (해당 컬럼이 NULL 값을 허용하는 경우)
- NULL 값을 가지고 있거나 NULL값이 아닌 컬럼을 검색하려면 is NULL / is not NULL
- select count(*) from country where LifeExpectancy is NULL;
- select count(*) from country where LifeExpectancy is not NULL;



### NULL 함수

- 숫자 컬럼을 연산해야 할 때 NULL을 처리해주는 함수
- NULL 값이 나오면 다른 값(주로 0)으로 대체해서 계산에 문제가 없도록 처리
- IFNULL / COALESCE(MySQL), ISNULL(SQL Server), NVL(오라클)
- 숫자 연산 / 집합함수(ex. sum())의 경우 처리가 내장되어있음
- 직접 함수나 쿼리에 넣는 경우는 NULL 함수를 사용해야
- select avg(LifeExpectancy) from country;
- select avg(IFNULL(LifeExpectancy, 0)) from country;
- select count(LifeExpectancy) from country;
- select count(IFNULL(LifeExpectancy, 0)) from country;



### GOUP BY / HAVING

- **GROUP BY**
- 집합 함수와 같이 사용해 그룹별 연산을 적용함
- select 컬럼명, 집합함수명(컬럼명) from 테이블명 group by 컬럼명;
- select CountryCode, count(CountryCode) from city group by CountryCode;
- **HAVING**
- 집합연산 이후 조건절 추가 가능, WHERE 조건절 대체로 사용
- select CountryCode, count(CountryCode) from city group by CountryCode having count(CountryCode) >= 70;









## 서브쿼리, 집합연산



### 서브쿼리

- 쿼리문 내에 또 다른 쿼리문이 있는 형태
  - 서브 쿼리에서 값을 받아 메인 쿼리에서 사용하겠다
- 그리하여 서브쿼리는 메인 쿼리에 포함되는 관계
  - ()를 사용해 감싸는 형태
  - ORDER BY를 사용하지 못함
- 결과값의 형태에 따라 다음의 종류가 있음
  - 단일행(Single Row) 서브쿼리
    - 결과가 레코드 하나인 서브쿼리
    - 일반 연산자 (=, >, <, ...) 사용
  - 다중행(Multi Row) 서브쿼리
    - 결과가 레코드 여러개인 서브쿼리
    - 다중행 연산자 (IN, ALL, ANY, EXISTS) 사용
      - ALL: 여러 개 레코드의 AND효과 (가장 큰 값보다 큰)
        - Population > ALL(select Population...)
      - ANY: 여러 개 레코드의 OR 효과 (가장 작은 값보다 큰)
        - Population > ANY(select Population...)
      - IN / EXISTS
        - '결과값 중에 있는 것 중' 의미
        - IN은 전체 레코드를 스캔하고, EXISTS는 존재 여부만 확인하고 스캔하지 않음(속도 더 빠름)
        - 결과값은 T/F로 반환
  - 다중컬럼(Multi Column) 서브쿼리



### 집합연산

- 각종 집합연산 지원
- 합집합(UNION), 교집합(INTERSECT), 차집합(MINUS)
  - UNION: 두 쿼리의 결과값을 합쳐서 리턴함
    - select 쿼리1 union select 쿼리2 union ...
    - 두 쿼리의 결과 형식이 동일해야(기본적으로 DISTINCT 적용)
    - 다른 테이블이라도 결과값의 형식만 일치하면 됨
  - UNION ALL: 중복을 허용하는 UNION
    - select 쿼리1 union all select 쿼리2 union
  - INTERSECT - 두 쿼리의 결과값 중 공통값을 찾아서 리턴함
    - select 쿼리 1 intersect select 쿼리2
    - 두 쿼리의 결과 형식이 동일해야 함(기본적으로 DISTINCT 적용)
    - 다른 테이블이라도 결과값의 형식만 일치하면 됨
    - MySQL에서 구현하려면
    - select distinct CountryCode from city where LifeExpenctancy >= 80 **and** CountryCode **IN** (select distinct Code from country where Population >= 1000000);
  - MINUS / EXCEPT - 쿼리 1 결과값에서 쿼리 2 결과값을 빼서 리턴함
    - select 쿼리 1 minus select 쿼리 2
    - MySQL에서 구현하려면
    - select distinct CountryCode from city where Population > 5000000 **and** CountryCode **NOT IN** (select distinct CountryCode from city where CountryCode = 'CHN')
- MySQL은 INTERSECT / MINUS는 지원 x