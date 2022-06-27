# Database - MySQL

> Mon Jun 27, 2022

---

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
```

