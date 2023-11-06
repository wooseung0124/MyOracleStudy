# 20231019

실행법
: 시작 - 명령 프롬프트 - sqplus - {사용자명 : system, 암호 : oracle}
ㄴ> [시작 - 명령 프롬프트 - sqplus] 이 과정은 컴퓨터 내에 c드라이브 경로를 통해 윈도우한테 알려주는 행위.
ㄴ> 암호가 'oracle'인 이유는 설치했을 당시에 암호를 'oracle'이라고 설정했음
ㄴ> 자동으로 첫 사용자명은 'system'으로 설정됨
ㄴ> 이것이 관리자 명의

사용자 생성법
: SQL> create user (kimgura) identified by (tiger;)
ㄴ> id자리 : (kimgura)
ㄴ> pw자리 : (tiger;)
ㄴ> 괄호 안 넣어도 됨
ㄴ> 임의로 만든거 {사용자명 : kimgura, 암호 : tiger} 

사용자 권한 부여법
SQL> grant resource, connect to kimgura;
ㄴ> resource : 자원 / connect : 접속권한

=============================================================
oracle 사용전 db 기본 개념
1. db는 표(table)형식으로 데이터를 저장한다.
: column(제목, 열: 세로) + row(데이터, 행: 가로)로 구성되는 table

2. 테이블 생성시 필수요소
: column name / column type / 숫자, 문자, 날짜
ㄴ> db에 저장할 수 있는 데이터 : 문자, 숫자, 날짜, 파일
ㄴ> 보통 앞에 3개 위주로 저장함

3. 데이터 관리
: row 관리 : insert(추가), update(수정), delete(삭제)
테이블 정보 가져오기 : select(출력)

=============================================================
프롬프트를 통한 oracle 사용전 참고사항
1. 한글타입 uft-8 이 없다. 때문에 한글1개당 3개를 데이터가 잡아먹는다.

2. 예약어가 존재하기에 table 명 정할 때 조심하자
: ex) user(예약어 존재) => users로 수정

3. 명령어 실행시 대소문자 구분없음
: create table users = CREATE table users
ㄴ> 대신 -는 연산자로 받아들인다. 
ㄴ> 때문에 문법은 스네이크 케이스를 사용한다.

4. 문법 끝에 세미콜론(;) 마무리
: 한줄에 작성하기에 너무 길다면 ;없이 그냥 엔터 눌러서 줄바꿈하면 된다.

4-ex) 수정코드(엔터 누를 경우)
SQL> update users
  2  set addr='에버랜드'
  3  where num=3;

5. 코드 작성중에 오타 발생시
SQL> [오타 코드작성]; => 실행시 오류발생될 경우
SQL> ed;
ㄴ> 실행시 메모장이 활성화되면서 최근(마지막)에 입력한 코드가 자동으로 입력된 채로 나온다.
ㄴ> 메모장에서 코드를 수정한 다음 '파일'-'끝내기'하여 저장을 누른다.
ㄴ> 그럼 이어서 명령 프롬프트으로 옮겨진다. 
SQL> / 
ㄴ> [ / ]버튼을 누르고 엔터를 누르면 수정값이 적용되고 이어서 코드를 작성하면 된다.

=============================================================
oracle을 활용한 데이터 table 사용방법

1. [테이블 만들기]
SQL> create table users(num number, name varchar2(10),addr varchar2(16));
ㄴ> 문법 : create table [테이블명](칼럼1 칼럼1type, 칼럼2 칼럼2type, 칼럼3 칼럼3type, ...);
ㄴ> varchar2(숫자) : oracle에서 정의한 string type, 숫자는 글자수

2. [테이블 삭제]
SQL> drop table users;
ㄴ> 문법 : drop table [테이블명];

3. [테이블 내에 데이터 추가]
SQL> insert into users(num, name, addr) values(1,'김구라','노량진');
ㄴ> 문법 : insert into [테이블명](칼럼1,칼럼2,칼럼3, ...)values(칼럼1의 row1, 칼럼1의 row2, ...);
ㄴ> 문자의 경우 무조건 따옴표(')로만 사용하라.
ㄴ> 명령어 실행시 '임시저장'이 된다.

4. [테이블 내에 데이터 삭제]
SQL> delete from users where num=1;
ㄴ> 문법 : delete from [테이블명] where [어느 row인지 = *조건절(해당 row 선택)];
ㄴ> 명령어 실행시 '임시저장(수정)'이 된다.

5. [테이블 내에 데이터 수정]
SQL> update users set addr='에버랜드' where num=1;
ㄴ> 문법 : update [테이블명] set [해당 row]=['수정후 데이터'] where [어느 row인지 = *조건절];

* 조건절
: 조건절이 true(참)인 row만 수정이 된다. true 인 row가 여러 개면 여러 개의 row가 한 번에 수정된다.

6. [테이블 데이터 완전 저장]
SQL> commit;

7. [테이블 구조파악]
SQL> desc users;
ㄴ> 문법 : desc [테이블명];
ㄴ> 컬럼이 잘 부여되었는지, 혹은 테이블 내에 어떤 컬럼이 있는지 확인하는 용도일 뿐
ㄴ> 데이터 요소가 들어있지는 않다.

=============================================================
table 내에 데이터(row)활용하기
0. [create로 만들 수 있는 3가지 요소]
ㄴ> table, user, sequence(시퀀스)
ㄴ> 밑에 내용 알기 전에 기본 베이스로 알아두자.

1. [기본 출력법]
SQL> select num, name, addr from users;
ㄴ> 문법 : select [컬럼1], [컬럼2], [컬럼3] from [테이블명];
ㄴ> 순서대로 저장된 것처럼 보이겠지만 정렬을 별도로 해줘야 제대로 순서를 
부여해줄 수 있다.

2. [데이터에 id 기능 부여하기]
SQL> create table users(num number primary key, name varchar2(10), addr varchar2(16));
ㄴ> 테이블 생성시 데이터 타입 뒤에 'primary key' 작성하기
ㄴ> 문법 : create table [테이블명]([컬럼명1] [컬럼1 data type] primary key, [컬럼명2] [컬럼2 type], ...);
ㄴ> 데이터 특성상 중복되어서는 안되는 요소 또는 기준을 부여하고 싶을 때 사용한다.

3. [임시 테이블]
SQL> SELECT SYSDATE FROM DUAL;
ㄴ> 문법 : select [함수 혹은 계산] from dual;
ㄴ> 어떤 값을 얻고자 할 때 사용하는 'dual' 테이블
ㄴ> dual 테이블을 이용하여 함수의 값을 리턴(return)받을 수 있다.

4. [시퀀스 선언하기]
SQL> create sequence my_seq;
ㄴ> 문법 : create sequence [시퀀스 함수명];
ㄴ> primary key를 통해 id를 부여 후 자동으로 순서를 기입하고 싶다면 '시퀀스'를 사용한다.

5. [시퀀스 출력하기]
SQL> select my_seq.nextval from dual;
ㄴ> 문법 : select [시퀀스명].nextval from dual;
ㄴ> 가상의 테이블(dual)을 통해 시퀀스가 선언되었는지 출력하여 볼 수 있다.
ㄴ> oracle 내에도 함수가 존재하지만 ()가 없다. [시퀀스명].nextval 내에는 하나의 함수(호출)를
콜한 셈이다.

6. [시퀀스 활용하기]
SQL> insert into users(num, name, addr) values(my_seq.nextval, '이승우','성수동');
ㄴ> 문법 : insert ~ values(my_seq.nextval, 칼럼1의 row2, 칼럼1의 row3, ...);
ㄴ> 주의해야될 점은 시퀀스 호출 이전에 number 타입으로 '1'이 존재하면 안된다.
ㄴ> 안 그러면 무결성 제약 조건(KIMGURA.SYS_C003996)에 위배된다.
ㄴ> 앞서 배운 primary key와 sequence를 잘 사용한다면 중복되지 않은 값이 순차적으로 자동 배열된다.

7. [데이터 순서 나열하고 출력하기]
SQL> SELECT num, name, addr FROM member ORDER BY num DESC;
ㄴ> 문법 : select [칼럼1], [칼럼2], [칼럼3] from [테이블명] order by [어느 칼럼] [desc or asc];
ㄴ> desc [테이블명] 이것과는 조금 다른 것이 order by [칼럼] desc 실행시 데이터를 내림차순으로 나열한다.
ㄴ> 정렬코드{ascend(asc) : 오르막 / descend(desc) : 내리막}

8. [테이블 row 데이터 조작하기]
SQL> SELECT num, name || '님', addr FROM member;
ㄴ> name || '님' 여기까지의 구조 : 기존 데이터 + '문자열 데이터'
ㄴ> + 연산자를 사용하지 않은 이유는 oracle에선 서로 다른 타입을 더 할 수 없다.
ㄴ> 자바스크립트에선 가능하나 oracle에선 +로는 안되니 파이프 파이프(||)로 대체한 것

9. [테이블 칼럼 조작하기]
SQL> SELECT num, name || '님' AS new_name, addr FROM member;
ㄴ> name || '님' AS new_name
ㄴ> [칼럼2] || ['칼럼2의 row 데이터를 일괄적으로 추가할 데이터'] as [변경할 칼럼2의 이름]

10. [원하는 데로 출력하기]
SQL> SELECT '이름은 ' || name || ' 주소는 ' || addr AS info FROM member;
ㄴ> AS info FROM member;
ㄴ> 문법 : as [변경 컬럼] from [테이블명];
ㄴ> 해석 : 테이블에 존재했던 모든 컬럼을 초기화하고 'info'라는 이름의 컬럼1개만 재생성하기

**셀렉트(select)의 진짜 용도**
ㄴ> 빅데이터를 가공하여 필요한 데이터들만 함수를 통해 가져오는 용도이다.
ㄴ> 메모리 상에서만 보여주기식 결과물을 구현할 수 있다. 
ㄴ> db 원본을 보존함과 동시에 사용자에 따라 원하는 방식으로 필요한 정보들만 보여주고 관리 가능.

11. [데이터 모두 호출]
SQL> select * from mumber;

# 20231020

이번 목표 : select 출력 활용하기

============================================
진행전 실습 세팅하기
1. oracle 설치파일 찾아보기(현 설치파일 경로)
C:\oraclexe\app\oracle\product\10.2.0\server\RDBMS\ADMIN
ㄴ> scott 파일 찾기

2. scott 사용자 계정 로그인
C:\Users\acorn>sqlplus scott/tiger;
ㄴ> 비밀번호가 'tiger'로 설정되는 이유는 어제 'kimgura' 계정을 만들 때 사용한 비번이 적용된다.
ㄴ> 용도 : 연습용 테이블이 만들어지고 sample 데이터가 자동 insert되기에 select 연습에 좋다.

3. scott sql 파일 실행하기
SQL> @ C:\oraclexe\app\oracle\product\10.2.0\server\RDBMS\ADMIN\scott.sql
ㄴ> 해당 파일을 명령 프롬프트에 드래그해도 자동 기입된다.

4. scott 파일 내에는 사원 테이블이 존재하기에 세팅준비는 끝났다.

5. 예제 '오라클정리' PDF 화면에 띄워놓고 19P 펼치기

============================================
명령 프롬프트 사이즈 조절법(1회용임)
1. 페이지 사이즈
SQL> set pagesize 20

2. 가로 사이즈
SQL> set linesize 200

============================================
select 기능 복습하기

구조
SQL> select [궁금한 내용] from [테이블명] where [조건절] order by [asc|desc];
ㄴ> [궁금한 내용] : 굳이 row가 아니더라도 궁금한 게 있으면 선택이 가능한 영역
ㄴ> [조건절] : 함수이기에 원하는 요소 선택도 되고 원치 않은 요소도 배제할 수 있다.
ㄴ> [asc | desc] : 오름차순 혹은 내림차순

============================================
연습을 위해 먼저 주어진 예제의 컬럼명을 외워라.

사원 테이블 => emp
사원번호 => EMPNO
사원이름 => ENAME
직책(직업) => JOB
매니저의 사원번호(직속 상관의 번호, KING 제외) => MGR
입사일(날짜 TYPE) => HIREDATE
급여(봉급, 주급) => SAL
성과급(보너스, 영업사원만 해당) => COMM
부서번호 => DEPTNO

부서 테이블 => dept
부서번호 => DEPTNO
부서명 => DNAME
부서위치 => LOC

============================================
컬럼명을 손으로도 외워보자.

[사원 테이블 호출]
SQL> desc emp;

[사원번호 호출]
SQL> select empno from emp;

[사원이름 호출]
SQL> select ename from emp;

[직책 호출]
SQL> select job from emp;

[매니저의 사원번호 호출]
SQL> select mgr from emp;

[입사일 호출]
SQL> select hiredate from emp;

[급여 호출]
SQL> select sal from emp;

[성과급 호출]
SQL> select comm from emp;

[부서번호 호출]
SQL> select deptno from emp;

[부서 테이블 호출]
SQL> desc dept

[부서 테이블 부서번호 호출]
SQL> select deptno from dept;

[부서명 호출]
SQL> select dname from dept;

[부서위치]
SQL> select loc from dept;

============================================
아웃풋을 통해 내가 칼럼명을 외웠는지 테스트하자.

[사원 테이블 호출] = emp

[사원번호 호출] = emptno*

[사원이름 호출] = ename

[직책 호출] = job

[매니저의 사원번호 호출] =mgr

[입사일 호출] = hizedate*

[급여 호출] = sal

[성과급 호출] =comm

[부서번호 호출] dmptno

[부서 테이블 호출] dept

[부서 테이블 부서번호 호출] dmptno

[부서명 호출] dname

[부서위치] loc

============================================
select 형식 특징

1. order by [asc | desc]의 특징 : varchar2 타입 순서
SQL> select job, sal from emp order by job asc, sal desc;

JOB                       SAL
------------------ ----------
ANALYST                  3000
CLERK                    1300
CLERK                     950
CLERK                     800
MANAGER                  2975
MANAGER                  2850
MANAGER                  2450
PRESIDENT                5000
SALESMAN                 1600
SALESMAN                 1500
SALESMAN                 1250
SALESMAN                 1250

ㄴ> job (varchar2 타입)의 경우 단어 첫 스펠링 기준으로 위에서 아래로 순차적이다.
ㄴ> 하지만 두번째 스펠링은 순차적으로 정리되지 않는다. 두번째부터는 묶음 형식이다.

2. oracle 내에 where문 해석방식
SQL> select empno, ename, sal from emp where sal >= 2000;
ㄴ> 해석 : emp 라는 테이블 내에서 where 조건절 2000보다 큰 sal 요소를 
 추려주되 사원번호, 사원이름, 급여 요소만 출력해줄 것. 

3. 칼럼에 별칭 수정시
3-1 ex) SQL> select empno AS "사원번호", ename AS "사원이름" from emp;
ㄴ> as 뒤에 수정할 값을 입력하면 되지만 띄어쓰기 금지
3-2 ex) SQL>SELECT empno 사원번호 , ename 사원이름 FROM emp 
ㄴ> as와 " 모두 생략 가능함

4. 비교 연산자 사용시
oracle의 동일 기호 : =
javascript의 동일 기호 : ==

5. db 데이터 문자열의 경우
: 대소문자 구분해야함

6. row 데이터 빈공간, 'null'

============================================
select의 중요성
ㄴ 인터페이스 과정 : 사용자 <- 자바 <- oracle db
ㄴ db는 정렬 저장이 없다. row가 무작위로 저장된 상태
ㄴ 자바에서 정렬하게 된다면 속도도 느려지고 코딩하기 힘듬
ㄴ select를 알아야 클라이언트까지 순조롭게 전달이 가능함

============================================
select 연산자를 손으로 기억하기

[산술 연산자 +, -, *, / ]
SQL> select sal, sal*1.1 from emp;
ㄴ 문법 : select [궁금한 내용], [칼럼 연산자 식] from [테이블명]
ㄴ [칼럼 연산자 식] : 띄어쓰기 없이 작성하기

[비교 연산자 = , != , > , < , >= , <= ]
SQL>SELECT * FROM emp WHERE sal >= 3000 
ㄴ 문법 : select [궁금한 내용] from [테이블명] where [궁금한 내용] [비교연산자] [조건]

[논리 연산자 AND , OR , NOT]
SQL>SELECT ename, sal FROM emp WHERE deptno = 10 AND sal >= 3000 
ㄴ 문법 : select [궁금한 내용] from [테이블명] where [조건] [논리 연산자] [조건]

============================================
select SQL 연산자를 손으로 기억하기

[OR 연산자 간결하게]
SQL>SELECT empno, ename, deptno FROM emp WHERE deptno IN(10,20)

[비교 연산자를 활용한 or 연산자]
SQL>SELECT empno, sal FROM emp WHERE sal > ANY(1000, 2000, 3000) ;
ㄴ> in연산자는 값을 기준으로, any는 비교 연산자와 함께 활용

[AND 연산자 간결하게]
SQL>SELECT empno, sal FROM emp WHERE sal > ALL(1000, 2000, 3000) 

[A와 B 사이의 데이타 모두 출력]
SQL>SELECT empno, ename, sal FROM emp WHERE sal BETWEEN 1000 AND 2000 

[NULL 인 경우 TRUE]
SQL>SELECT ename, comm FROM emp WHERE comm IS NULL;

[NULL 이 아닌경우 TRUE]
SQL>SELECT ename, comm FROM emp WHERE comm IS NOT NULL;

[빅데이터 내에 존재하면 TRUE]
SQL>SELECT ename, comm FROM emp
 WHERE EXISTS (SELECT ename FROM emp WHERE ename='FORD')

[문자열 비교]
SQL>SELECT ename, deptno FROM emp WHERE ename LIKE 'J%' 
ㄴ ex) 사원이름이 'J' 로 시작하는 사원의 사원이름과 부서번호를 출력하기

SQL>SELECT ename, deptno FROM emp WHERE ename LIKE '%J%' 
ㄴ ex) 사원이름에 'J' 가 포함되는 사원의 이름과 부서번호를 출력하기

SQL>SELECT ename, sal, hiredate FROM emp WHERE ename LIKE '_A%' 
ㄴ ex) 사원이름의 두번째 글자가 'A' 인 사원의 이름,급여,입사일을 출력하기

SQL>SELECT ename, sal, hiredate FROM emp WHERE ename LIKE '%ES' 
ㄴ ex) 사원 이름이 'ES' 로 끝나는 사원의 이름,급여,입사일을 출력하기

SQL> select hiredate, empno from emp where hiredate like '81%';
ㄴ 입사년도가 81년 인 사원들의 입사일과 사원번호를 출력해 보세요

SQL> select ename || '의 직업은 '||job||'입니다.' from emp;

============================================
[단일행 함수 : 하나의 row 당 하나의 결과값을 반환하는 함수]

1. CHR(아스키 코드)
SQL> select chr(65)from dual;

2. concat(칼럼명, '붙일문자')
SQL> select concat(ename,'님')name from emp;

3. 시작문자를 대문자로
SQL> select initcap('hello world')from dual;

4. 문자열을 소문자로
SQL> select lower('HELLO!')from dual;

5. 문자열을 대문자로
SQL> select upper('hello')from dual;

6. lpad('문자열', 전체 자리수, '남은 자리를 체울 문자') => 왼쪽에 체우기
SQL> select lpad('HI',10,'*')from dual;

7.rpad('문자열', 전체 자리수, '남은 자리를 체울 문자') => 오른쪽에 체우기
SQL> select rpad('HELLO',15,'^')FROM DUAL;

8. ltrim('문자열','왼쪽부터 제거할 문자')
select ltrim('ABCD', 'A') from dual;

9. rtrim('문자열','오른쪽부터 제거할 문자')
SQL> select rtrim('ACACBVD','CD')from dual;

10. replace(문자열1, 문자열2, 문자열3)
ㄴ> 문자열1에 있는 문자열 중에서 문자열 2를 찾아 문자열3으로 바꿔준다.
SQL> select replace('Hello mimi', 'mimi','mama')from dual;

11. substr('문자열', n1,n2) => 문자열의 n1번째 위치에서 n2개만큼 문자열 빼오기
SQL> select substr('ABCDEFGHIJ', 3,5)from dual;

# 20231023

date(날짜 type) -> char(문자type) : to_char
char -> date : *to_date

그 외에 시간값을 구하려면? : 
1. 현시점 시간 : sysdate 함수
2. 문자열로 작성 후 날짜형식으로 변경 : to_date
3. 간단한 형식으로 입력한 수를 자동 date type으로 변경

SQL> select to_date('001225', 'yymmdd') from dual;

primary key : not null(빈) + unique(유일)

12개의 row를 가진 테이블을 select 를 가지고 row가 몇개인지
확인하여 총 합을 보여주는 경우 이 경우를 복수행 함수라고 한다.

HAVING절 : 그룹 묶고 추려내기

an-si조인 : join 조건을 따로 작성

# 20231024

SQL> select ename, emp.deptno, dept.deptno, dname from emp, dept 
where emp.deptno=dept.deptno;
ㄴ> join조건

SQL> select ename, e.deptno, d.deptno, dname from emp e, dept d 
where e.deptno=d.deptno;
ㄴ> 테이블명을 간단하게 바꾼 문법
ㄴ> as 기능은 칼럼명 변경 기능이니 차이를 알아둘 것.

SQL> ed
  1  select ename, e.deptno, d.deptno, dname
  2  from emp e, dept d
  3  where e.deptno=d.deptno
  4 and e.deptno >= 20
ㄴ> ed : 코드 수정 명령어
ㄴ> where e.deptno=d.deptno : join조건
ㄴ> and e.deptno >= 20 : 일반조건
ㄴ> 가독성이 좋지 않다. => inner join을 사용해보자.

inner join 의 on과 using 문법

SQL> ed
  1  select ename, e.deptno, d.deptno, dname
  2  from emp e
  3  inner join dept d on e.deptno=d.deptno
  4 where e.deptno >= 20
ㄴ> ANSI JOIN 표현식
ㄴ> from에서 작성한 여러 테이블명을 하나로 정의하기
ㄴ> 그 밑에 inner join을 통해 다른 테이블명을 명시한다.
ㄴ> 그 옆에 on 을 작성하고 같은 데이터로 볼 조건절을 명시한다.
ㄴ> on의 경우 칼럼명이 다를 경우 작성
ㄴ> 칼럼명이 같을 경우 "on e.deptno=d.deptno" => using(deptno)
ㄴ> 참고로 inner 이란 단어는 생략가능함

==================
join의 문법
1. 일반 join
2. ANSI join

join의 종류
1. inner join
2. outer join

=================
onter join
: 두 테이블을 비교했을 때 한쪽 테이블(b테이블)은 해당 데이터가 존재하고
다른 테이블(a테이블)에는 없는 경우라도 모든 테이블을 조건절에 맞게
모든 데이터를 추출하는 join법

예시
a테이블 | b테이블
   칼럼1 | 칼럼1
      1   |    a
      2   |    b
      3   |    c
      4   |    d
      5   |    e
      -   |    f

SQL>SELECT a.칼럼1, b.칼럼1 
FROM a테이블 a, b테이블 b 
WHERE a.칼럼1(+) = b.칼럼1;
ㄴ> onter join 진행시

SQL>SELECT a.칼럼1, b.칼럼1 
FROM a테이블 a
right outer join b테이블 b on a.칼럼1(+) = b.칼럼1;
ㄴ> 오른쪽(right)에 데이터가 존재하다면
ㄴ> 왼쪽에 +를 넣는다. 그럼 null로 row를 맞춰준다.

*nvl 명령어가 있으면 null도 육안으로 보여준다.

==================
단일행 서브쿼리

예제) 'SMITH' 가 근무하는 부서명을 서브쿼리를 이용해서 출력해 보세요
SQL> select deptno from emp where ename = 'SMITH';

    DEPTNO
----------
        20

SQL> select dname from dept where deptno = 20;

DNAME
----------------------------
RESEARCH
ㄴ> 이걸 하나로 만든다. => 서브쿼리

SQL> select dname from dept
  2  where deptno=(select deptno from emp where ename = 'SMITH');
ㄴ> 해석 : 전체코드 => 메인쿼리
ㄴ> 해석 : (select deptno from emp where ename = 'SMITH') => 서브쿼리
ㄴ> 서브쿼리가 먼저 종료된다.
ㄴ> '출력' row가 1줄이라 가능했다.

==================
다중행 서브쿼리
ㄴ> 반드시 필요한 연산자(in, all, any, exist)
ㄴ> 단일행 서브쿼리의 경우 '출력' row가 2줄부터는 오류가 발생됨

ex) in 연산자의 경우 2개의 값으로 출력값을 계산하는데 2개의 출력값을 가지고 있는
서브쿼리로 사용하면 같은 효과를 누릴 수 있다.

SQL> select ename, sal, deptno from emp
  2  where deptno in(select deptno from emp where sal >=3000);

*where 조건절과 having 문법의 차이
where : row를 추려낸다.
having : group을 추려낸다. group by 문법과 세트다.

# 20231025

commit 의 반대말은 rollback, 되돌리기

오늘의 예제
ㄴ> 2개의 명령 프롬프트 실행하기
ㄴ> 서로 다른 프롬프트 내에 동일한 row 내에 서로 다른 데이터를 수정할 경우,
 한쪽에서는 락이 걸려서 수정 및 출력이 되지 않도록 설계가 되어있다.
ㄴ> commit을 한쪽에서 해주지 않는 이상 계속 멈출 것이다

====
정규화 : 수많은 row 정보들을 관리하기 수월하도록 추려낼 수 있는 데이터를
별도의 테이블을 생성하여 관리하는 것

rdbbase : 관계형 데이터베이스

primary key : id 값, 테이블 정보당 1개
fk(외래키) : 바깥 테이블의 pk를 참조하는 것.
관계) pk : 부모 <--참조-- fk : 자식

====
SQL> create table dept2(deptno number(2) constraint dept2_deptno_pk primary key,
  2  dname varchar2(15) default'영업부',
  3  loc char(10)
  4  constraint dept2_loc_ck check(loc in('seoul','busan')));
ㄴ> constraint : 제약조건
ㄴ> default"" : 값이 존재하지 않으면 '영업부'로 설정되는 명령어
ㄴ> check() : 괄호 안에 조건이 true일 경우에만 저장된다.

만약 insert로 deptno에 동일값이 있을 경우는?
1행에 오류:
ORA-00001: 무결성 제약 조건(SCOTT.DEPT2_DEPTNO_PK)에 위배됩니다

만약 insert로 loc에 없는 값을 넣을 경우는?
1행에 오류:
ORA-02290: 체크 제약조건(SCOTT.DEPT2_LOC_CK)이 위배되었습니다

====
SQL> create table emp2
  2  (empno number(4) constraint emp2_empno_pk primary key,
  3  ename varchar2(15) constraint emp2_ename_nn not null,
  4 deptno number(2) constraint emp2_deptno_fk references dept2(deptno));
ㄴ> references : 참조할 테이블 기준잡기

만약 없는 참조값을 부를 경우는?
1행에 오류:
ORA-02291: 무결성 제약조건(SCOTT.EMP2_DEPTNO_FK)이 위배되었습니다- 부모 키가
없습니다

====
constraint
c : check or not null
r : foreign key
p: primary keydr

칼럼을 정의할 때 같이 정의하는 제약조건
칼럼 수준의 제약조건 column level 제약조건 table level 제약조건

STATUS: 상태
ㄴ ENABLED | DISABLED

===========
시퀀스
currval : 현재 섹션에서 사용했던 최근 values

========

http://127.0.0.1:8080/apex
ㄴ>id주소를 활용하여 웹브라우저에 서비스 제공 가능

=========
[페이지 처리를 하기 위한 select 방법]
1. 정렬한다.
2. 행번호(순번을 부여)
3. 행번호를 활용해서 원하는 row 를 select한다.
========
hr계정 사용하기

1. 관리자 계정 로그인
2. SQL> alter user hr account unlock;
3. SQL> alter user hr identified by hr;



