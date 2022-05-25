# 프로그래머스 SQL 문제



## 01. IS NULL / IS NOT NULL

![1](SQL_TEST_PRACTICE.assets/1.png)

```
<오답 작성 쿼리>
SELECT ANIMAIL_ID
FROM ANIMAIL_INS
WHERE NAME = NULL
----------------------------
여기서 잘못된 점은 ANILMAIL_ID = NULL의 의미는 알겠으나, 이건 ANIMAIL_ID 변수에 NULL를 할당한 것임. 문제에서 요구하는 이름없는 동물을 의미하기 위해선 WHERE NAME IS NULL 로 작성해야 한다. * IS NULL 이 하나의 문법이라고 생각하자.

+ 추가

WHERE NAME IS NOT NULL 으로 작성해야 할 때도 있다. 즉, NAME이 NULL이 아닌 경우를 찾고자 하는 조건을 사용할 때이다. 

<정답 쿼리>

SELECT ANIMAIL_ID
FROM ANIMAIL_INS
WHERE NAME IS NULL
```



## 02. DESC

![1](SQL_TEST_PRACTICE.assets/1-16518325836251.png)

```
<오답 작성 쿼리>
SELECT NAME, DATETIME
FROM ANIMAIL_INS

<정답 쿼리>
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC

쿼리 문제 풀이 연습과 개념이 아직 명확하게 잡히지 않아서 헷갈림. 실수하지말자.
```



## 03. != or <> 

![1](SQL_TEST_PRACTICE.assets/1-16518339763791.png)

```
<작성한 정답 쿼리>

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION not in ('aged')

----------------------------------------

<다른 정답 쿼리>
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != ('aged')

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION <> ('aged')

python 문법 중 아니다를 의미하는 != 를 쿼리에 사용한 것을 확인할 수 있다. 또한 != 이것과 <>은 같은 의미이다.
```



## 04. ORDER BY

![1](SQL_TEST_PRACTICE.assets/1-16518343863752.png)

```
<오답 작성 쿼리>

SELECT animal_id, name, datetime
from animal_ins
order by name asc

-----------------

<정답 쿼리>

SELECT animal_id, name, datetime
from animal_ins
order by name asc , datetime desc

이름을 기준으로 먼저 오름차순으로 하되, 이름 순서가 같을 경우는 보호일을 기준으로 내림차순으로 하라는 문제 의도는 알았으나, 쿼리로 어떻게 짜야하는지 헷갈렸다. 단순히 order by문법으로 먼저 name을 쓴 것이 1차 정렬 기준이고, 만약 1차 기준이 충돌됐을 경우 뒤에있는 data 정렬이 2차기준인 것을 잊지말자.
```



## 05. HAVING

![2](SQL_TEST_PRACTICE.assets/2.png)

```
<작성한 정답 쿼리>

SELECT NAME, COUNT(NAME) AS 'COUNT'
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME) >= 2
ORDER BY NAME


GROUP BY 문법을 사용했을 때는 WHERE 문법을 사용하지 못한다. 이때 WHERE 문법과 동일한 기능을 가진 HAVING 문법을 사용하면 된다. HAVING 문법은 항상 GROUP BY절 뒤에 사용해야한다.
```



## 06. IFNULL(A,B)

![2](SQL_TEST_PRACTICE.assets/2-16524316094871.png)

```
<정답 쿼리>

SELECT ANIMAL_TYPE, IFNULL(NAME,'No name') as NAME , SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAl_ID

IFNULL 문법은 다음과 같다.
IFNULL(A,B): A가 NULL이면 B를, 그렇지 않다면 A를 반환한다.
```



## 07. CASE 문법, IF 문법

![2](SQL_TEST_PRACTICE.assets/2-16524326238212.png)

```
<정답 쿼리 1>

SELECT ANIMAL_ID, NAME, 
CASE
 WHEN SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%'
 THEN 'O'
 ELSE 'X' END as '중성화'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID

<정답 쿼리 2>

SELECT ANIMAL_ID, NAME, IF(SEX_UPON_INTAKE LIKE '%Neutered%' 
                           or 
                           SEX_UPON_INTAKE LIKE '%Spayed%', 'O','X') AS '중성화'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID

------------------------------------
Python 문법 중에 if 문법으로 조건에 맞는 값을 출력하는 것처럼 SQL에서도 CASE와 IF 문법이 있다. Python의 if 문법과 차이점은 or을 하더라도 컬럼명을 다시 적어줘야 한다.

<CASE>
CASE WHEN A THEN B ELSE C END AS '이름' - A에 내가 조건에 해당하는 문장을 넣고, B에 A가 해당된다면 넣어줄 값을, 그렇지 않을 경우는 C값을 넣어주며 END로 닫아준다

<IF>
IF(A B C) AS '이름' - A에 조건을 줄 문장을 넣고, 조건에 해당하는 경우 B값을, 그렇지 않을경우 C값을 할당한다.  
```



## 08. DISTINCT

![2](SQL_TEST_PRACTICE.assets/2-16524521683913.png)

```
<정답 쿼리>

SELECT COUNT(DISTINCT NAME)
FROM ANIAML_INS
WHERE NAME IS NOT NULL


DISTINCT 문법 익숙해야함.
```



## 09. DATE_FORMAT(A, B)

![2](SQL_TEST_PRACTICE.assets/2-16524548343995.png)

```
<정답 쿼리> 

SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS '날짜'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
--------------------------

DATETIME의 기본형은 20160-09-23 14:23:00 같은 형식이다. 이 형식을 위와 같은 년도,달,일 순으로만 설정된 것으로 바꾸려면 DATE_FORMAT(A,B) 문법을 사용해야 한다. A에는 바꾸고자 하는 날짜 데이터, B에는 어떤 형식으로 바꿀 지 설정한다. 이때 대소문자 구분에 따라 형식이 바뀔 수 있다. 

EX) '%Y-%M-%D' -> 2019-APRIL-02
EX) '%Y-%m-%d' -> 2019-4-02 
```



## 10. LEFT,RIGHT JOIN

![3](SQL_TEST_PRACTICE.assets/3.png)

![3](SQL_TEST_PRACTICE.assets/3-16530355289541.png)

![3](SQL_TEST_PRACTICE.assets/3-16530355613522.png)

```
JOIN 문법은 아주 중요하다.
LEFT OUTER JOIN / RIGHT OUTER JOIN / INNER JOIN/ FULL OUTER JOIN / EXCLUSIVE JOIN 으로 4가지 JOIN 개념이 있지만, 보통 자주 사용되는 건 LEFT JOIN이다(RIGHT는 LEFT의 테이블 순서만 다른 것 뿐)

LEFT JOIN을 밴다이어그램으로 표현하면 위와 같다. 즉, A라는 테이블의 레코드는 모두 가져오고, A와 B테이블 레코드 중 겹치는 레코드는 A테이블 옆에 붙인다고 생각하면 편하다. 이 때, A레코드는 다 가져오면서, B레코드와 겹치지 않는 레코드가 있는 경우도 발생하며 이때는 NULL값으로 처리된다.

<정답 쿼리>

select outs.animal_id, outs.name 
from animal_outs as outs left outer join animal_ins as ins on outs.animal_id = ins.animal_id 
where ins.animal_id is null
order by outs.animal_id
```



## 11. INNER JOIN(JOIN)

![3](SQL_TEST_PRACTICE.assets/3-16531359943003.png)

```
INNER JOIN 혹은 JOIN 은 두 테이블에 동시에 있는 것들만 합쳐지는 것이기 때문에 NULL값이 없다.

<정답 쿼리>

SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_INS AS A JOIN ANIMAL_OUTS AS B ON A.ANIMAL_ID = B.ANIMAL_ID
ORDER BY B.DATETIME - A.DATETIME DESC
LIMIT 2
```



## 12. 서브쿼리

![3](SQL_TEST_PRACTICE.assets/3-16531382404444.png)

```
서브쿼리는 python 개념 중 if절 속에 if절이라고 생각하면 쉽다.

<정답 쿼리>

SELECT *
FROM PLACES
where host_id in (select host_id
                 from places
                 group by host_id
                 having count(host_id) >= 2)
order by id
```



# Solvesql 연습 문제



## 01. SUBSTR

![1](SQL_TEST_PRACTICE.assets/1-16533700489472.png)

```
SUBSTR() 함수는 SUBSTR(문자열, 시작위치, 나타낼 개수)의 문법을 통해 특정 문자열을 잘라주는 기능을 가진다.

EX) SUBSTR('KOREA',1,3) -> 'KOR'

<정답 쿼리 2>

SELECT YEAR, UPPER(LEFT(CITY,3))
FROM GAMES
WHERE YEAR >= 2000
ORDER BY CITY
```



## 02. VARIANCE

![1](SQL_TEST_PRACTICE.assets/1-16533781240593.png)

```
표본분산을 구할 수 있는 함수로 VARIANCE가 있다.
```



