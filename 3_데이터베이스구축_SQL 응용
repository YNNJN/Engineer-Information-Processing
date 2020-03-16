## 3과목 - 데이터베이스 구축





1. 논리 데이터베이스 설계
2. 물리 데이터베이스 설계
3. SQL 응용
4. SQL 활용
5. 데이터 전환

---





## 3. SQL 응용

### 1. SQL의 개념

- 관계형 데이터베이스의 표준 질의어
- 정의 조작 제어 기능이 있음
- 관계 대수와 관계 해석을 기초로 한 혼합 데이터 언어
- 분류는 다음과 같음
  - DDL
    - SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의 변경 삭제할 때 사용
    - DB 관리자나 설계자가 사용
    - CREATE, ALTER, DROP
  - DML
    - 사용자가 질의어를 통해 데이터를 실제로 처리하는 데 사용
    - DB 사용자와 관시 시스템 간 인터페이스를 제공
    - SELECT, INSERT, DELETE, UPDATE
  - DCL
    - 데이터의 보안, 무결성, 회복, 병행 수행 제어를 정의하는 데 사용
      - 관리자가 데이터 관리를 목적으로 사용
      - COMMIT
      - ROLLBACK
      - GRANT
      - REVOKE





### 2. DDL

- 번역한 결과가 Data Dictionary에 여러개의 테이블로 저장됨

- CREATE SCHEMA 스키마명 AUTHORIZATION 사용자_id;

- CREATE DOMAIN 도메인명 [AS] 데이터_타입

  ​				[DEFAULT 기본값]

  ​				[CONSTRAINT 제약조건명 CHECK (범위값)];

- CREATE TABLE 테이블명

  - 기본 테이블에 포함될 모든 속성에 대해 속성명, 그 속성의 데이터 타입, 기본 값, NOT NULL 여부를 지정
  - ON DELTE 옵션 - 참조 테이블의 튜플이 삭제되었을 때 기본 테이블에 취해야 할 사항을 지정
    - NO ACTION, CASCADE, SET NULL, SET DEFAULT
  - CONSTRAINT 제약 조건명, CHECK(조건식)
    - 제약 조건의 이름을 저장, 이름 저장할 필요 없으면 CHECK 절만 사용하여 제약 조건을 명시

- CHAR vs VARCHAR

- 다른 테이블을 이용한 테이블 정의

  - CREATE TABLE 신규테이블명 AS SELECT 속성명[, 속성명, ...] FROM 기존테이블명;
  - 기존 테이블에서 추출되는 속성의 데이터 타입과 길이, NOT NULL 정의는 신규 테이블에 그대로 적용됨
  - 제약 조건은 적용 x, 신규 테이블 정의 후 ALTER TABLE 명령 사용하여 제약조건을 추가해야

- CREATE VIEW

  - VIEW는 하나 이상의 기본 테이블로부터 유도되는 이름을 갖는 가상 테이블
    - SELECT문을 서브 쿼리로 사용하여 SELECT문의 결과로서 뷰를 생성함
    - 서브쿼리인 SELECT문에는 UNION이나 ORDER BY 절 사용 불가

- CREATE INDEX

  - 인덱스(검색 시간 단축 위해 만든 보조적 데이터 구조)를 정의하는 명령문
  - Clusted Index - 인덱스 키의 순서에 따라 데이터가 정렬되어 저장되는 방식
    - 검색 빠르나, 데이터 삽입 삭제 발생 시 순서 유지 위해 데이터 재정렬 필요
  - Non-Clustered Index - 인덱스의 키 값만 정렬되었을 뿐 실제 데이터는 정렬되지 않는 방식
    - 검색 속도 떨어짐

- ALTER TABLE

  - 테이블에 대한 정의를 변경하는 명령문
  - ADD(새로운 열 추가), ALTER(특정 속성의 티폴트 값 변경), DROP COLUMN(특정 속성을 삭제)

- DROP

  - 스키마, 도메인, 기본 테이블, 뷰 테이블, 인덱스, 제약 조건 제거하는 명령문
  - CASCADE - 제거할 요소를 참조하는 다른 모든 개체를 함께 제거
  - RESTRICTED - 다른 개체가 제거할 요소를 참조중일 때는 제거를 취소함





### 3. DCL

- GRANT 사용자 등급 TO 사용자_ID리스트;
- REVOKE 사용자 등급 FROM 사용자_ID리스트;
  - 사용자 등급은 다음과 같음
  - DBA - 데이터베이스 관리자
  - RESOURCE - 데이터베이스 및 테이블 생성 가능자
  - CONNECT - 단순 사용자
- WITH GRANT OPTION - 부여받은 권한을 다른 사용자에게 다시 부여할 수 있는 권한을 부여함
- GRANT OPTION FOR - 다른 사용자에게 권한을 부여할 수 있는 권한을 취소함
- COMMIT
- ROLLBACK
- SAVEPOINT





### 4. DML

- INSERT INTO 테이블명([속성병1, 속성명2, ...])
- VALUES (데이터1, 데이터2, ...)
  - 대응하는 속성과 데이터는 개수와 타입이 일치해야
  - 기본 테이블의 모든 속성을 사용할 때는 속성명을 생략 가능
  - SELECT 문 사용하여 다른 테이블의 검색 결과를 삽입 가능
  - 날짜 데이터는 숫자로 취급하지만 '' 또는 ##으로 묶어줌
- DELETE FROM 테이블명 [WHERE 조건];
  - 모든 레코드를 삭제하더라도 테이블의 구조는 남아있는 것이 DROP과의 차이
- UPDATE 테이블명 SET 속성명=데이터[, 속성명=데이터, ...] [WHERE 조건];
- 정리하면
  - SELECT~FROM~WHERE
  - INSERT INTO~VALUES~
  - DELETE~FROM~WHERE~
  - UPDATE~SET~WHERE~





### 5. DML - SELECT

- SELECT~[그룹함수]~[WINDOW 함수 OVER(PARTITION BY ORDER BY )]
  FROM~WHERE~[GROUP BY]~[HAVING]~[ORDER BY]
- 그룹함수 - 그룹 별로 속성의 값을 집계할 함수를 기술
  - COUNT, SUM, AVG, MAX, MIN, STDDEV(표준편차), VARIACE
  - ROLLUP - 그룹별 소계를 구함, n+1레벨까지, 하위->상위 레벨 순으로 데이터가 집계됨
  - CUBE - 모든 조합의 그룹별 소계를 구함, n^2 레벨까지, 상위 -> 하위 레벨 순으로 데이터가 집계됨
- Window함수 - GROUP BY 절 이용 않고 속성의 값을 집계할 함수를 기술
  - PARTITION BY - WINDOW 함수가 적용될 범위로 사용할 속성을 지정
  - ORDER BY- PARTITION 안에서 정렬 기준으로 사용할 속성을 지정
  - 종류는 다음과 같음
    - ROW_NUMBER() - 각 레코드에 대한 일련번호 반환
    - RANK() - 윈도우별로 순위를 반환, 공동 순위를 반영함
    - DENSE_RANK() - 윈도우별로 순위를 반환, 공동 순위를 무시함
- GROUP BY 절 - 특정 속성을 기준으로 그룹을 지정함
- HAVING 절 - 그룹에 대한 조건을 지정함
  - 그룹에 대한 조건을 지정할 때는 WHERE가 아닌 HAVING!
- 집합연산자
  - 2개 이상의 테이블의 데이터를 하나로 통합함
  - SELECT~FROM~[UNION | UNION ALL | INTERSECT | EXCEPT]~SELECT~FROM~[ORDER BY 속성명]
    - UNION - 중복된 행은 한 번만 출력
    - UNION ALL - 중복된 행도 그대로 출력





### 6. DML - JOIN

- 연관된 튜플들을 결합하여, 하나의 새로운 릴레이션을 반환함
- INNER JOIN
  - EQUI JOIN: = 비교에 의해 같은 값을 가지는 행을 연결하여 결과를 생성
    - NATURAL JOIN - 중복된 속성을 제거하여 같은 속성을 한 번만 표기하는 방법, 두 테이블에는 이름과 도메인이 같은 속성이 반드시 존재
    - EQUI JOIN에서 연결고리가 되는 공통 속성을 JOIN 속성이라고 함
    - 1. SELECT~FROM~WHERE 테이블명1.속성명 = 테이블명2.속성명;
    - 2. SELECT~FROM 테이블명1 NATURAL JOIN 테이블명2;
    - 3. SELECT~FROM 테이블명1 JOIN 테이블명2 USING(JOIN 속성);
    - CROSS JOIN
      - 가능한 모든 행들의 조합이 표시됨
      - 첫 번째 테이블의 모든 행들은 두 번째 테이블의 모든 행들과 조인됨
      - 첫 번째 테이블의 행수를 두 번째 테이블의 행수로 곱한 만큼의 행을 반환함
      - 조인 조건이 없는 조인이라고 할 수 있음
  - NON-EQUI JOIN = 이외 연산자(>, <) 비교를 사용하는 조인 방법
- OUTER JOIN
  - JOIN 조건에 만족하지 않는 튜플도 결과로 출력하기 위한 JOIN 방법
  - LEFT OUTER JOIN - 왼쪽 릴레이션은 모두, 오른쪽은 연관 있는 튜플만 표시
    - WHERE 테이블명1.속성명 = 테이블명2.속성명(+); 으로 INNER JOIN과 같은 형식으로 사용
  - RIGHT OUTER JOIN - 오른쪽 릴레이션은 모두, 왼쪽은 연관 있는 튜플만 표시
    - WHERE 테이블명1.속성명(+) = 테이블명2.속성명; 으로 INNER JOIN과 같은 형식으로 사용
- SELF JOIN
  - 같은 테이블에서 2개의 속성을 연결하여 EQUI JOIN을 하는 JOIN 방법