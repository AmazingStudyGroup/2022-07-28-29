## 데이터 정의어

### 12-1 객체를 생성, 변경, 삭제하는 데이터 정의어
데이터 정의어(DDL)는 데이터베이스 데이터를 보관하고 관리하기 위해 제공되는 여러 객체(object)의 생성, 변경, 삭제 관련 기능을 수행     

#### 데이터 정의어를 사용할 때 유의점
데이터 정의어는 데이터 조작어(DML)와 달리 명령어를 수행하자마자 데이터베이스에 수행한 내용이 바로 반영(COMMIT)되는 특성이 있다.     
- 데이터 정의어의 종류       

|DDL|기능|
|---|----|
|CREATE|테이블을 생성|
|ALTER|테이블을 변경|
|DROP|테이블을 삭제|

### 12-2 테이블을 생성하는 CREATE
```sql
-- 자료형을 각각 정의하여 새 테이블 생성하기
CREATE TABLE 소유 계정.테이블 이름(
  열1 이름 열1 자료형,
  열2 이름 열2 자료형,
  ...
  열N 이름 열N 자료형
);

-- 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE DEPT_DDL
   AS SELECT * FROM DEPT;
   
-- 다른 테이블의 일부를 복사하여 테이블 새 생성하기
CREATE TABLE EMP_DDL_30
    AS SELECT *
         FROM EMP
        WHERE DEPTNO = 30;
        
-- 다른 테이블의 열 구조만 복사하여 새 테이블 생성하기
CREATE TABLE EMPDEPT_DDL
   AS SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE,
             E.SAL, E.CMM, D.DEPTNO, D.DNAME, D.LOC
        FROM EMP E, DEPT D
       WHERE 1 <> 1;
```

### 12-3 테이블을 변경하는 ALTER
```SQL
-- ALTER 명령어로 HP 열 추가하기
ALTER TABLE EMP_ALTER
  ADD HP VARCHAR2(20);
  
-- ALTER 명령어로 HP 열 이름을 TEL로 변경하기
ALTER TABLE EMP_ALTER
  RENAME COLUMN HP TO TEL;

-- ALTER 명령어로 EMPNO 열 길이 변경하기
ALTER TABLE EMP_ALTER
MODIFY EMPNO NUMBER(5);

-- ALTER 명령어로 TEL열 삭제하기
ALTER TABLE EMP_ALTER
DROP COLUMN TEL;
```
### 12-4 테이블 이름을 변경하는 RENAME
```SQL
-- 테이블 이름 변경하기
RENAME EMP_ALTER TO EMP_RENAME;
```
### 12-5 테이블의 데이터를 삭제하는 TRUNCATE
특정 테이브의 모든 데이터를 삭제한다. 데이터 구조에는 영향을 주지 않는다.    
```SQL
-- EMP_RENAME  테이블의 전체 데이터 삭제하기
TRUNCATE TABLE EMP_RENAME;
```
> TRUNCATE 사용시 유의점        
> TRUNCATE는 데이터 정의어이기 때문에 ROLLBACK이 되지 않는다. 

### 12-6 데이블을 삭제하는 DROP
```SQL
DROP TABLE EMP_RENAME;
```

### 잊기 전에 한번 더
```SQL
-- Q1
CREATE TABLE EMP_HW ( 
     EMPNO    NUMBER(4), 
     ENAME    VARCHAR2(10), 
     JOB      VARCHAR2(9), 
     MGR      NUMBER(4), 
     HIREDATE DATE, 
     SAL      NUMBER(7, 2), 
     COMM     NUMBER(7, 2), 
     DEPTNO   NUMBER(2) 
); 

-- Q2
ALTER TABLE EMP_HW 
  ADD BIGO VARCHAR2(20);
  
-- Q3
ALTER TABLE EMP_HW 
MODIFY BIGO VARCHAR2(30); 

-- Q4
ALTER TABLE EMP_HW 
RENAME COLUMN BIGO TO REMARK; 

-- Q5
INSERT INTO EMP_HW 
SELECT EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO, NULL 
  FROM EMP; 

-- Q6
DROP TABLE EMP_HW;
```
