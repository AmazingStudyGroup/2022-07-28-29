## 12 - 1 객체를 생성, 변경, 삭제하는 데이터 정의어
<br/>

- 데이터 정의어(DDL: Data Definition Language)는 데이터베이스 데이터를 보관하고 관리하기 위해 제공되는 여러 객체(Object)의 생성•변경•삭제 관련 기능을 수행한다.

>데이터 정의어를 사용할 때 유의점

- 데이터 정의어는 데이터 조작어(DML)와 달리 명령어를 수행하자마자 데이터베이스에 수행한 내용이 바로 반영되는 특성이 있다.
<br/>

## 12 - 2 테이블을 생성하는 CREATE
<br/>

- CREATE문은 오라클 데이터베이스 객체를 생성하는 데 사용하는 명령어이다.
```sql
// 기본 형식
CREATE TABLE 소유 계정.테이블 이름(
	열1 이름 열1 자료형,
    열2 이름 열2 자료형,
    ...
    열N 이름 열N 자료형
);    
```
<br/>

- 테이블 이름 생성 규칙
	1. 테이블 이름은 문자로 시작해야 한다(한글도 가능하며 숫자로 시작할 수 없음).
   ex) EMP90 (O), 90EMP (X)
	2. 테이블 이름은 30byte 이하여야 한다(즉 영어는 30자, 한글은 15자까지 사용 가능).
	3. 같은 사용자 소유의 테이블 이름은 중복될 수 없다(SCOTT 계정에 두 EMP 테이블은 존재할 수 없음).
	4. 테이블 이름은 영문자(한글 가능), 숫자(0-9)와 특수 문자 $, #, _ 를 사용할 수 있다.
  ex) EMP#90_OB
	5. SQL 키워드는 테이블 이름으로 사용할 수 없다(SELECT, FROM 등은 테이블 이름으로 사용 불가).
<br/>

- 열 이름 생성 규칙
	1. 열 이름은 문자로 시작해야 한다.
	2. 열 이름은 30byte 이하여야 한다.
 	3. 한 테이블의 열 이름은 중복될 수 없다(EMP 테이블에 EMPNO 열이 두 개 존재할 수 없음).
 	4. 열 이름은 영문자(한글 가능), 숫자(0-9)와 특수 문자 $, #, _ 를 사용할 수 있다.
 	5. SQL 키워드는 열 이름으로 사용할 수 없다.
    
<br/>

>자료형을 각각 정의하여 새 테이블 생성하기

```sql
// 모든 열의 각 자료형을 정의해서 테이블 생성하기
CREATE TABLE EMP_DDL(
  EMPNO		NUMBER(4),
  ENAME		VARCHAR2(10),
  JOB		VARCHAR2(9),
  MGR		NUMBER(4),
  HIREDATE	DATE,
  SAL		NUMBER(7,2),
  COMM		NUMBER(7,2),
  DEPTNO	NUMBER(2)
);

DESC EMP_DDL; // 열 구조 확인
```
<br/>

>기존 테이블 열 구조와 데이터를 복사하여 새 테이블 생성하기

- 특정 테이블과 같은 열 구조로 테이블을 만들 때 CREATE문에 서브쿼리를 활용하여 테이블을 생성하는 방법을 많이 사용한다. CREATE문에 서브쿼리를 사용할 때 AS 키워드를 함께 쓴다.
```sql
// 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE DEPT_DDL
	AS SELECT * FROM DEPT;
    
DESC DEPT_DDL;
```
<br/>

>기존 테이블 열 구조와 일부 데이터만 복사하여 새 테이블 생성하기

- 특정 테이블과 열 구조는 같되 테이블 전체 데이터가 아닌 일부 데이터만 복사하여 테이블을 만들어야 한다면 서브쿼리에 WHERE절을 사용하여 생성 테이블에 저장될 데이터를 조건식으로 지정할 수 있다.
```sql
// 다른 테이블의 일부를 복사하여 테이블 생성하기
CREATE TABLE EMP_DDL_30
	AS	SELECT *
    	  FROM EMP
         WHERE DEPTNO = 30;
         
SELECT * FROM EMP_DDL_30;
```
<br/>

>기존 테이블의 열 구조만 복사하여 새 테이블 생성하기

- 특정 테이블과 열 구성이 같되 저장 데이터가 없는 빈 테이블을 생성하려면 WHERE절 조건식의 결과 값이 언제나 false가 나오는 방법을 사용할 수 있다. CREATE문에 서브쿼리를 사용하는 방식은 자주 쓰이며 여러 테이블을 조인한 SELECT문도 활용할 수 있다.
```sql
// 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE EMPDEPT_DDL
   AS SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE,
   			 E.SAL, E.COMM, D.DEPTNO, D.DNAME, D.LOC
        FROM EMP E, DEPT D
       WHERE 1 <> 1;
       
SELECT * FROM EMPDEPT_DDL;
```
<br/>

## 12 - 3 테이블을 변경하는 ALTER
<br/>

- ALTER 명령어는 이미 생성된 오라클 데이터베이스 객체를 변경할 때 사용한다. 테이블에 새 열을 추가 또는 삭제하거나 열의 자료형 또는 길이를 변경하는 등 테이블 구조 변경과 관련 된 기능을 수행한다.
```sql
// EMP 테이블을 복사하여 EMP_ALTER 테이블 생성하기
CREATE TABLE EMP_ALTER
	AS SELECT * FROM EMP;
    
SELECT * FROM EMP_ALTER;
```
<br/>

>테이블에 열 추가하는 ADD

- ALTER TABLE 명령어와 ADD 키워드, 추가할 열 이름과 자료형을 명시하면 테이블에 새 열을 추가할 수 있다.
```sql
// ALTER 명령어로 HP 열 추가하기
ALTER TABLE EMP_ALTER
  ADD HP VARCHAR2(20);
```
<br/>

>열 이름을 변경하는 RENAME

- ALTER 명령어에 RENAME 키워드를 사용하면 테이블의 열 이름을 변경할 수 있다.
```sql
// ALTER 명령어로 HP 열 이름을 TEL로 변경하기
ALTER TABLE EMP_ALTER
 RENAME COLUMN HP TO TEL;
```
<br/>

>열의 자료형을 변경하는 MODIFY

- 테이블의 특정 열의 자료형이나 길이를 변경할 때는 다음과 같이 MODIFY 키워드를 사용한다.
```sql
// ALTER 명령어로 EMPNO 열 길이 변경하기
ALTER TABLE EMP_ALTER
MODIFY EMPNO NUMBER(5);
```
<br/>

> 특정 열을 삭제할 때 사용하는 DROP

- 테이블의 특정 열을 삭제할 때 DROP 키워드를 사용한다. 열을 삭제하면 해당 열의 데이터도 함께 삭제되므로 신중하게 사용해야 한다.
```sql
// ALTER 명령어로 TEL열 삭제하기
ALTER TABLE EMP_ALTER
DROP COLUMN TEL;
```
<br/>

## 12 - 4 테이블 이름을 변경하는 RENAME
<br/>

- 테이블 이름을 변경할 때는 RENAME 명령어를 사용한다.
```sql
RENAME EMP_ALTER TO EMP_RENAME;
```
▶︎ 이름을 변경한 후에는 기존 EMP_ALTER 이름을 사용할 수 없다.
<br/>

## 12 - 5 테이블의 데이터를 삭제하는 TRUNCATE
<br/>

- TRUNCATE명령어는 특정 테이블의 모든 데이터를 삭제한다. 데이터만 삭제하므로 테이블 구조에는 영향을 주지 않는다.
```sql
// EMP_RENAME 테이블의 전체 데이터 삭제하기
TRUNCATE TABLE EMP_RENAME;
```
`❗️ TRUNCATE 명렁어를 사용할 때 유의점`
테이블의 데이터 삭제는 데이터 조작어 중 WHERE절을 명시하지 않은 DELETE문의 수행으로도 가능하다. 하지만 TRUNCATE는 데이터 정의어이기 때문에 ROLLBACK이 되지 않는다는 점에서 DELETE문과 다르다. 즉 삭제 이후 복구할 수 없다.
<br/>

## 12 - 6 테이블을 삭제하는 DROP
<br/>

- DROP 명령어는 데이터베이스 객체를 삭제하는 데 사용한다. 테이블이 삭제되므로 테이블에 저장된 데이터도 모두 삭제된다.
```sql
// EMP_RENAME 테이블 삭제하기
DROP TABLE EMP_RENAME;
```

▶︎ 테이블이 삭제되었으므로 이제 EMP_RENAME 테이블을 사용할 수 없다.

---

