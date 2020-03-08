# DDL



## 스키마 정의 / 자료형 / 제약조건



### 스키마 정의

- DB와 테이블의 CRUD(Create, Retrive, Update, Delete)
- 테이블에 대한 정보는 메타데이터로 Data Dictionary에 저장 관리
- CREATE DATABASE 데이터베이스명;
- CREATE TABLE 테이블명(컬럼명1, 데이터타입(크기), 컬럼명2 ...);



### 자료형

- 정수형(부호 있음/ 부호 없음)
  - TINYINT, INT, BIGINT
- 실수형(길이, 소수점 이하 자리수)
  - FLOAT(size, d), DOUBLE(size, d), DECIMAL(size, d)
- 문자열
  - CHAR 고정길이 문자열 (최대 255자), VARCHAR 가변길이 문자열 (최대 255자)
- TEXT 문자열
  - TEXT, MEDIUMTEXT, LONGTEXT
- BLOB(Binary Large Object)    #동영상, 이미지 데이터
  - BLOB, MEDIUMTEXT, LONGTEXT
- 시간 관련
  - DATA, TIME, DATETIME, TIMESTAMP



### 제약조건

- 입력 데이터의 제약 조건을 걸어 해당되지 않는 데이터는 입력되지 않음
- NOT NULL: 데이터가 NULL 값을 받아들이지 않음
- UNIQUE: 테이블에 동일한 값이 입력되어 있을 경우 받아들이지 않음
- PRIMARY KEY: 기본 키 제약조건(UNIQUE, NOT NULL 조건 동반)
- FOREIGN KEY: 외래키 제약조건
- CHECK: 입력값 체크 (MySQL에서는 동작 x)
- DEFAULT: 컬럼 값이 입력되지 않으면 기본값을 입력
- Auto Increment: 자동 증가
  - create table BusinessCard(ID int auto_increment, Name varchar(255), Address varchar(255), Telephone varchar(255), primary key(id));
  - insert into BusinessCard(Name, Address, Telephone) valued ('Bob', 'SEOUL', '123-4567');
  - Select * from BusinessCard









## DB모델링 (중복정보 제거 / 정규형 / 참조무결성)



### 중복정보 제거

- 테이블 간의 정보는 중복되지 않아야
  - 동일 정보가 여러 군데 테이블에 저장되면, 수정에 대한 부담과 무결성(Integrity) 유지가 어려워짐
  - 하나의 정보는 한 군데만
- 이를 위해 정규화를 통해 중복성 제거함
- 중복성 제거 후 필요한 정보는 외래키를 통한 JOIN을 통해 필요한 정보를 구함



### 정규형

- 중복을 제거하기 위한 테이블 정의 규칙
  - 제 1 정규형: 나눌 수 있을 만큼 쪼갤 것
  - 제 2 정규형: 테이블의 컬럼들이 기본키와 직접 연관되는 컬럼만으로 구성할 것
  - 제 3 정규형: 컬럼들 간의 종속관계가 있으면 안 됨



### 참조무결성

- 참조무결성은 외래키의 제약 조건, 즉 외래키(FK)에 적용되는 규칙
- 외래키와 참조되는 원래 테이블의 키와 관계를 명시
- 외래키를 참조하면 원래 테이블에 해당 레코드 값이 반드시 존재해야, 외래키를 따라 갔는데 NULL 값이면 안 됨 (무결성)
- 만약 원래 레코드를 삭제하려면 참조하는 외래키(FK) 값을 먼저 NULL로 만들어야 함
- 외래키 참조 관계가 있을 경우에 레코드 추가 / 삭제시 선후관계를 나타냄









## 스키마 수정, 삭제 (ALTER / DROP)



### 스키마 수정

- 이미 생성된 스키마에 대해 수정할 경우 사용
- 테이블 컬럼 추가 / 삭제 / 수정
  - ALTER TABLE 테이블명 ADD 컬럼명 데이터타입
  - ALTER TABLE 테이블명 DROP 컬럼명
  - ALTER TABLE 테이블명 CHANGE 컬럼명 new_컬럼명 데이터타입 (컬럼명 변경)
  - ALTER TABLE 테이블명 MODIFY 컬럼명 데이터타입 (컬럼타입 변경)
- 기본키 제약조건 추가 / 기본키 제약조건 삭제
  - ALTER TABLE 테이블명 ADD PRIMARY KEY (컬럼명)
  - ALTER TABLE 테이블명 ADD CONSTRAINT 제약이름 UNIQUE (컬럼명1, 컬럼명2)
  - ALTER TABLE 테이블명 DROP PRIMARY KEY
- 외래키 제약조건 추가 / 삭제
  - ALTER TABLE 테이블명 ADD FOREIGN KEY (컬럼명) REFERENCES 원테이블명 (원칼럼명)
  - ALTER TABLE 테이블명 DROP FOREIGN KEY 컬럼명
- 테이블 명
  - ALTER TABLE 테이블명 RENAME new_테이블명;



### 스키마 삭제

- DB 삭제
  - DROP DATABASE 데이터베이스명 -> DB 삭제
- 테이블 삭제
  - DROP TABLE 테이블명 -> 테이블 삭제, 내용과 테이블 전체 삭제
  - DELETE * FROM 테이블명 -> 레코드를 일일히 하나씩 지움, 테이블 스키마는 유지
  - TRUNCATE TABLE 테이블명 -> 테이블 내용만 지움, 테이블 스키마는 유지, 전용 명령어











# DCL



## 권한 설정 / 역할 / 원격 접속



### 접근 권한 설정

- DCL(Data Control Language)
  - 권한 및 역할 설정하는 언어
  - 특정 테이블에 대한 CRUD(Create / Retrieve / Update / Delete) 권한 설정 (로컬 / 랜 / 원격접속)
    - GRANT / REVOKE로 나뉨
    - 주로 DBA(DataBase Administrator)가 설정
- show grants for sampleUser;



### 역할 설정

- 개별 테이블에 대한 CRUD 권한을 삳용자별로 설정하면 경우의 수가 **테이블 수 x 사용자 수의 조합**만큼
- 이 문제를 개선 위해 Role을 정하고, 역할 별 권한을 설정하고 사용자에게 역할을 부여하는 형태로 사용
- 사용자가 여러 개의 롤을 가지는 것이 가능함(관리자, 사용자 등)
- MySQL에는 ROLE 관련 명령 지원 x
- 역할 생성
  - CREATE ROLE 역할명;
- 역할에 대해 권한 설정
  - GRANT CRUD ON 테이블명 TO 역할명;
- 사용자에게 역할 부여
  - GRANT 역할 TO 사용자명;



### 원격 접속

- 사용자를 원격 사용자로 등록
  - GRANT ALL PRIVILEGES ON DB명.테이블명 TO 사용자명 @%'192.168.0.*'
  - IDENTIFIED BY '비밀번호';
- my.ini 수정(bind - address 부분 주석처리)
  - #Instead of skip-networking the default is now to listen only on
  - #localhost which is more compatible and is not less secure
  - **#bind-adress = 127.0.0.1**
- MySQL 서버 재시작
- 방화벽 3306 포트 열기

