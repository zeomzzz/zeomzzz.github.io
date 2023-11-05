---
title:  "MySQL 문법 정리"
excerpt: ""

categories:
  - SQL
tags:
  - [DB, MySQL]

toc: true
toc_sticky: true
 
date: 2023-06-23
last_modified_at: 2023-10-22
---

<br>

# **1. 기초 문법**

## **1) 기본 문법**

- 조건문
    - IF문 : IF(조건, True일 때, False일 때)
    - CASE ~ WHEN
        
        ```sql
        CASE
        	WHEN 조건1 THEN 조건1일 때의 반환값
        	WHEN 조건2 THEN 조건2일 때의 반환값
        	ELSE 이 외의 경우의 반환값
        END
        ```

<br> 

## **2) 날짜 관련**

- 날자 포맷 설정 : `DATE_FORMAT(필드명, 형식)`
    - `%Y` : 4자리 년도, `%y` : 두 자리 년도
    - `%M` : 긴 월 (영문), `%b` : 짧은 월(영문), `%m` : 숫자 월(두 자리), `%c` : 숫자 월
    - `%d` : 일자(두 자리), `%e` : 일자
    - `%W` : 긴 요일(영문), `%a` : 짧은 요일(영문)
    - `%I` : 시간(12시간), `%H` : 시간(24시간)
    - `%i` : 분, `%S` : 초
    - `%T` : hh:mm:SS, `%r` : hh:mm:ss AM, PM
    - ex. `DATE_FORMAT(HIRE_YMD, '%Y-%m-%d’)` : 2023-09-08

<br>
<br>

# **2. 그 외 문법**

<br>

- **MAX(IF) :** `SELECT MAX(IF(조건, 1, 0))`
    - 모든 결과가 1이면 1, 0이면 0, 1, 0이 함께 존재하면 1 출력
    - 관련 문제 : [프로그래머스 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/157340)
- **누적합** : `SUM([대상]) OVER(PARTITION BY [단위] ORDER BY [정렬기준])`
    - 그룹하지 않을거면 PARTITION BY 없이 정렬기준만 주면 됨
    - 관련 문제 : [대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151139)

<br>
<br>

---

**참고 자료**

<br>

- [[mysql] DATE_FORMAT - 날짜 형식 설정](https://devjhs.tistory.com/89)
- [[Mysql] Mysql 조건문 - IF 문, CASE 문](https://redcow77.tistory.com/260)
- [[SQL] Window Functions](https://velog.io/@12aeun/SQL-Window-Functions)

<br>