# Database: MySQL Built-in Functions

> Mon Jun 27, 2022

---

[toc]

접속

`mysql -u root -p`



### 내장 함수 1 - 숫자함수

```mysql
-- 절대값
select abs(-63);

-- ** 올림, 내림
select ceil(78.1), floor(19.9);

-- 반올림
select round(84.84);

-- 반올림 소수이하자리, 반올림 정수자리
select round(253.2654, 2), round(253.845, -2);

-- ** 포멧
select format(25364.5895, 2);

-- ** 나머지
select mod(10, 3), 10%3;

-- 난수 (0-1 사이)
select rand();

-- 난수 (5-10 사이) = 큰수-작은수+1+작은수
select rand()*6+5, floor(rand()*6)+5;

-- 제곱, 루트
select pow(5,3), sqrt(16);
```



### 내장 함수 2 - 문자열 처리 함수

```mysql
-- ASCII
select ascii('A'), char(66);

-- bit = 비트수, char = 바이트수, length = 문자수
SELECT BIT_LENGTH('abc'), CHAR_LENGTH('abc'), LENGTH('abc');
SELECT BIT_LENGTH('가나다'), CHAR_LENGTH('가나다'), LENGTH('가나다');

-- concat 문자연결
SELECT CONCAT(2022, 6, 27), CONCAT_WS('-', 2022, 6, 27);

-- 문자열 고르기
SELECT 
    ELT(3, 'a', 'b', 'c', 'd') AS elt,
    FIELD('b', 'x', 'y', 'b', 'z') field,
    FIND_IN_SET('text', 'text, sample') find_in_set,
    INSTR('abcd', 'c') instr,
    LOCATE('x', 'xyz');

select ename, insert(ename, 2, 2, '***') ename2 from emp order by ename; -- JONES, J***ES
select ename, reverse(ename) from emp; -- JONES, SENOJ
select ename, left(ename, 2), right(ename, 2) from emp; -- JONES, JO, ES
select ename, lcase(ename), ucase('abcd') from emp; -- JONES, jones, ABCD
select ename, lpad(ename, 10, '$'), rpad(ename, 10, '*') from emp; -- $$$$$SMITH, SMITH*****
select ename, lpad(ename, char_length(ename)+1, '$'), rpad(ename, 10, '*') from emp; -- $JONES, JONES*****

-- job 에서 3번째부터 2개 문자 구하기
select substring(job, 3, 2), job from emp; -- NA, MANAGER

-- 문자열에서 기준으로 앞뒤로 가져오기
select substring_index('www.nate.com', '.', 2),
	   substring_index('www.google.com', '.', -2);
       
-- Q. 이름을 글자길이의 50%만큼 출력하고 나머지 문자는 '*'으로 표시하기.
select ename, rpad(substring(ename, 1, floor(char_length(ename)/2)), char_length(ename), '*') from emp;
select ename, floor(char_length(ename)/2) from emp;

-- repeat : 이 문자열을 몇번 반복해줘!
select ename, repeat(ename, 3) from emp;

-- replace : 이 문자열의 이 문자를 이 문자로 바꿔줘!
select replace(ename, 'A', '에이') from emp;

-- LTRIM, RTRIM, TRIM : 왼쪽 / 오른쪽 / 양쪽 공백을 제거해줘!
select trim(" goguma is great ");

-- Q. 이메일에서 아이디와 도메인을 분리하기
select 'myemail@gmail.com', substring_index('myemail@gmail.com', '@', 1), substring_index('myemail@gmail.com', '@', -1);
```



### 내장 함수 3 - 날짜 함수

```mysql
-- 현재시간
select now(), curdate(), curtime(), sysdate();
-- 년 월 일 
select date(now()), year(now()), month(now());
-- 날짜 더하기, 빼기
select adddate(now(), interval 30 day), subdate(now(), interval 30 day);
-- 날짜 차이 : 오늘부터 언제까지 d-day
select datediff(now(), '2021-10-05');
-- 요일, 월 이름, 올해 며칠째
select dayofweek(now()), monthname(now()), dayofyear(now());
-- 마지막날 구하기, 시간을 초로 
select last_day(now()), time_to_sec('2022-06-28 00:10');
```



### 내장 함수 4 - 변환 함수

```mysql
-- dateformat
select date_format(now(), '%W %M %d, %Y');
```



### 그룹 함수

```mysql
select * from emp;
select count(*), count(empno), count(distinct empno) from emp;
select count(comm), count(ifnull(comm,0)) from emp;
select max(sal), min(sal), avg(sal), sum(sal) from emp;

-- 담당업무별 사원수
SELECT 
    job, COUNT(empno), sum(sal), round(avg(sal),2)
FROM
    emp
GROUP BY job;

-- WHERE -> GROUP BY -> HAVING 순으로 쿼리문 작성하기
-- Q. 급여가 2500불 이상인 사원을 부서별 급여의 합계, 평균을 구하된 부서별 급여의 평균이 2800불 이상인 부서만 선택
SELECT 
    deptno, SUM(sal), AVG(sal)
FROM
    emp
WHERE
    sal >= 2500
GROUP BY deptno
HAVING AVG(sal) >= 2900
ORDER BY deptno;


-- Q. 1981년에 입사한 사원의 담당 업무별 사원수, 급여의 평균, 최고 급여, 최저 급여를 구하기.
select job 담당업무, count(empno) 사원수, round(avg(sal), 2) 급여평균, max(sal) 최고급여, min(sal) 최저급여
from emp
where year(hiredate)=1981
group by job
having 급여평균>=2000
order by 급여평균 desc;
```



