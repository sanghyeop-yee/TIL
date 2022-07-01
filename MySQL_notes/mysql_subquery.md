# Database: MySQL Subquery

> Wed Jun 29, 2022

---

[toc]

### 단일 행 (Single-Row) 서브쿼리



```mysql
-- Q. 평균급여보다 많이 받는 사원은?
select * from emp;
select *
from emp
where sal > (select avg(sal) from emp);
```



```mysql
-- Q. Ford 와 같은 부서의 사원 중 평균급여보다 작게 받는 사원은?
select * 
from emp
where deptno = (select deptno from emp where ename = 'Ford')
and sal < (select avg(sal) from emp);
```



```mysql
-- Q, 담당 업무별 급여의 합계와 급여의 평균을 구하된 전체 사원의 평균보다 많이 받는
-- 담당업무를 선택하라.
select job, sum(sal), avg(sal)
from emp
group by job
having avg(sal) > (select avg(sal) from emp);
```



### 다중 행 서브쿼리

하나 이상의 행을 리턴하는 서브쿼리를 다중행 서브쿼리라고 합니다.

복수 행 연산자 (IN, NOT IN, ANY, ALL, EXISTS) 을 사용할 수 있습니다.

* IN: 

```mysql
-- Q. 업무별로 최대 급여를 받는 사람과 같은 급여를 받는 사원의 사원번호와 이름, 업무, 급여를 출력하세요.
select empno, ename, deptno, sal
from emp
where sal in (select max(sal) from emp group by job);
```



* ANY : 1개의 조건에 만족하면 선택

```mysql
select * from emp
where deptno != 20
and 
sal > any (select sal from emp where job='salesman');
```



* ALL: 모든 값이 만족하면 선택

```mysql
select * from emp
where sal > all(select sal from emp where job='salesman');
```



* EXISTS

```mysql
select empno, ename, sal 
from emp e
where exists(select empno from emp where e.empno = mgr);
```



### 다중 열 서브쿼리

```mysql
-- Q. 업무별로 최소 급여를 받는 사원의 사번, 이름, 업무, 부서번호를 출력하세요.
select empno, ename, job, deptno
from emp
where (job,sal) in (select job, min(sal) from emp group by job)
order by job;
```



```mysql
select empno, ename, sal, deptno, comm
from emp
where (sal, deptno) in (select sal, deptno from emp where deptno=30 and comm is not null);
```



```mysql
-- from 절에 서브쿼리
select empno, ename, job, sal 
from (select empno, ename, job, sal, comm, hiredate from emp where deptno=10 or deptno=20) a;
```

