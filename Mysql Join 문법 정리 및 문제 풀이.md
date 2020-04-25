## Mysql Join 문법 정리 및 문제 풀이

### JOIN 종류

[![Visual_SQL_JOINS_V2](http://rapapa.net/wp/wp-content/uploads/2012/06/Visual_SQL_JOINS_V2.png)](http://rapapa.net/wp/wp-content/uploads/2012/06/Visual_SQL_JOINS_V2.png)



**INNER JOIN(이너 조인)**

EX)

```
SELECT *
FROM 첫번째 테이블 이름
INNER JOIN 두번째 테이블 이름//->INNER JOIN을 INNER만 작성해도 동일한 기능을 수행한다.
ON 조건;
```

->ON 대신 WHERE을 사용할 수 있다.

INNER조인은 MYSQL에서는 간략히 JOIN으로 나타낸다. 일반적으로 사용하는 JOIN이다.

핵심은 JOIN뒤에 나오는 ON이다. ON의 역할은 두 테이블이 결합하는 조건을 나타낸다.

두 테이블에서 조건에 맞는 중복된 값을 보여준다.



**LEFT OUTER, RIGHT OUTER 조인**

```
SELECT *
FROM 첫번째 테이블 이름
LEFT JOIN 두번째 테이블 이름 //RIGHT는 LEFT를 RIGHT로만 바꿔서 적으면 된다.
ON 조건;
```

->왼쪽, 오른쪽 중 하나의 테이블 기준으로 합치는 조인이다.

->A LEFT JOIN B 와 B RIGHT JOIN A는 완전히 같은 식이다.

LEFT OUTER대신 LEFT, RIGHT OUTER 대신 RIGHT만 입력해도 같은 기능을 수행한다.



**CROSS JOIN(카티전 조인)**

-> 집합에서 곱의 집합 개념을 나타낸다.

```
SELECT *
FROM 첫번째 테이블 이름
CROSS JOIN 두번째 테이블 이름;
```

-> 곱의 집합을 나타내는 만큼 너무 많은 레코드를 생성할 위험이 있기 때문에 잘 사용되지는 않는다.





프로그래머스 SQL 문제

문제1.천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

```
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS
LEFT JOIN ANIMAL_INS AS INS
ON (INS.ANIMAL_ID = OUTS.ANIMAL_ID)
WHERE INS.ANIMAL_ID IS NULL
ORDER BY OUTS.ANIMAL_ID;
```

출처:https://programmers.co.kr/learn/courses/30/lessons/59042#qna



문제2.관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

```
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS
RIGHT JOIN ANIMAL_INS AS INS
ON (INS.NAME=OUTS.NAME AND INS.ANIMAL_ID =OUTS.ANIMAL_ID)
WHERE OUTS.DATETIME<INS.DATETIME
ORDER BY INS.DATETIME;
```

출처:https://programmers.co.kr/learn/courses/30/lessons/59043



문제3. 아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

```
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS AS INS
LEFT JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.ANIMAL_ID IS NULL
ORDER BY DATETIME LIMIT 3;
```

3마리만 출력하는 것을 LIMIT로 3으로 해주는 것이 중요합니다.

출처:https://programmers.co.kr/learn/courses/30/lessons/59044



문제4.보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화[1](https://programmers.co.kr/learn/courses/30/lessons/59045#fn1)되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.

```
SELECT OUTS.ANIMAL_ID, OUTS.ANIMAL_TYPE, OUTS.NAME
FROM ANIMAL_INS AS INS
RIGHT JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID=OUTS.ANIMAL_ID
WHERE (INS.SEX_UPON_INTAKE='INTACT MALE' OR INS.SEX_UPON_INTAKE='INTACT FEMALE')
AND (OUTS.SEX_UPON_OUTCOME='NEUTERED MALE' OR OUTS.SEX_UPON_OUTCOME='SPAYED FEMALE')
ORDER BY OUTS.ANIMAL_ID;
```

출처:https://programmers.co.kr/learn/courses/30/lessons/59045