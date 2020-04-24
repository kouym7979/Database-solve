## Mysql 표준 기본 문법(집계함수)

### **COUNT로 행 갯수 구하기**

​	->SELECT COUNT(*) FROM 테이블명 WHERE 열명=조건;

​	테이블에 존재하는 조건을 만족하는 모든 행의 갯수를 출력합니다.('*'이것은 전체를 의미합니다.)

​	->SELECT COUNT CAL1, COUNT CAL2 FROM 테이블명;

​	CAL1, CAL2의 갯수를 별도로 세며 해당하는것을 센다

​	NULL은 집계함수가 세지 않습니다.



### **DISTINCT로 중복 제거하기**

​	->SELCET ALL CAL FROM 테이블명;

​	->SELECT CAL FROM 테이블명;

​	ALL 키워드는 기본값을 의미합니다. ->모든 열을 조회

​	->SELECT DISTINCT CAL FROM 테이블명;

​	중복된 값을 제거하여 조회한다.

​	->SELECT COUNT(DISTINCT CAL) FROM 테이블명;

​	중복값을 제거하고 행의 갯수를 센다.



### **SUM으로 합계구하기**

​	->SELECT SUM(CAL) FROM 테이블명;

​	열의 합계를 구하여 출력합니다. NULL값은 제외됩니다.



### **AVG로 평균내기**

​	->SELECT AVG(열명) FROM 테이블명;

​	열의 평균을 구하여 출력한다. 이때, NULL값은 제외됩니다.

​	->SELECT AVG(CASE WHEN CAL1 IS NULL TEHN 0 ELSE CAL1 END) FROM 테이블명;

​	NULL을 0으로 간주하고 싶으면 CASE문을 이용합니다.



### **MIN, MAX로 최솟값, 최댓값 구하기**

​	->SELECT MIN(CAL1) FROM 테이블명;

​	->SELECT MAX(CAL1) FROM 테이블명;



### **GROUP BY로 그룹화 하기**

​	1. GROUP BY는 다른 집계함수와 같이 쓰입니다.

​	2. 집계함수를 잘 쓰기 위해서는 GROUP BY를 적절하게 사용할 줄 알아야합니다.

​	3. GROUP BY에서 지정한 열 이외의 다른 열은 집계함수를 사용하지 않은채 SELECT 구에 지정할 수 없습니다.(어떤 행을 출력해야 하는지 알 수 없기 때문입니다.)

​	->SELECT * FROM 테이블명 GROUP BY CAL1;

​	명시한 CAL1에 대해서 같은 열의 이름을 쓰는 경우 하나의 그룹으로 취급합니다.

​	마치 DISTINCT한것처럼 중복을 제거하여 하나로 보여주는것 같습니다.

​	->SELECT COUNT (CAL1), SUM(CAL2) FROM 테이블명 GROUP BY CAL3;

​	GROUP BY는 다른 집계함수와 쓰여야 그 힘을 발휘한다.

​	CAL3이 같은 경우로 묶어서 COUNT(CAL1), SUM(CAL2)를 계산합니다.



### **HAVING 구로 '집계함수'의 조건걸기**

 1. WHERE 구에서는 집계함수를 사용할 수 없다.

    ->WHERE 구 -> GROUP BY -> SELECT -> ORDER BY 순서로 처리되기 때문이다.

    ->그러므로 ORDER BY 구에서는 HAVING 구 없이 집계함수를 사용할 수 있다.

->SELECT CAL1 FROM 테이블 GROUP BY CAL1 HAVING COUNT(CAL)=1;

CAL1의 COUNT 집계가 1인 경우만을 조회한다.

___

###### 문제 설명

`ANIMAL_INS` 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. `ANIMAL_INS` 테이블 구조는 다음과 같으며, `ANIMAL_ID`, `ANIMAL_TYPE`, `DATETIME`, `INTAKE_CONDITION`, `NAME`, `SEX_UPON_INTAKE`는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

| NAME             | TYPE       | NULLABLE |
| ---------------- | ---------- | -------- |
| ANIMAL_ID        | VARCHAR(N) | FALSE    |
| ANIMAL_TYPE      | VARCHAR(N) | FALSE    |
| DATETIME         | DATETIME   | FALSE    |
| INTAKE_CONDITION | VARCHAR(N) | FALSE    |
| NAME             | VARCHAR(N) | TRUE     |
| SEX_UPON_INTAKE  | VARCHAR(N) | FALSE    |

가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.

##### 예시

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME            | INTAKE_CONDITION | NAME     | SEX_UPON_INTAKE |
| --------- | ----------- | ------------------- | ---------------- | -------- | --------------- |
| A399552   | Dog         | 2013-10-14 15:38:00 | Normal           | Jack     | Neutered Male   |
| A379998   | Dog         | 2013-10-23 11:42:00 | Normal           | Disciple | Intact Male     |
| A370852   | Dog         | 2013-11-03 15:04:00 | Normal           | Katie    | Spayed Female   |
| A403564   | Dog         | 2013-11-18 17:03:00 | Normal           | Anna     | Spayed Female   |

가장 늦게 들어온 동물은 Anna이고, Anna는 2013-11-18 17:03:00에 들어왔습니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| 시간                |
| ------------------- |
| 2013-11-18 17:03:00 |

문제1 들어온 시간 최댓값 구하기

```
SELECT MAX(DATETIME) FROM ANIMAL_INS; 
```

문제2 들어온 시간 최솟값 구하기

```
SELECT MIN(DATETIME) FROM ANIMAL_INS;
```

문제3 보호소에 들어온 동물의 수 구하기

```
SELECT COUNT(ANIMAL_ID) FROM ANIMAL_INS;
```

응용 동물의 종이 '강아지'인 동물의 수

```
SELECT COUNT(ANIMAL_ID) FROM ANIMAL_INS WHERE ANIMAL_TYPE = "DOG";
```

문제4 동물의 이름을 중복을 제거하고 출력하기

```
SELECT  COUNT(DISTINCT NAME) FROM ANIMAL_INS;
```

문제5 고양이와 개는 몇 마리인지 구하기

```
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE;
```

문제6 동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요.

```
SELECT NAME,COUNT(NAME) AS COUNT FROM ANIMAL_INS 
GROUP BY NAME
HAVING COUNT(NAME)>=2
ORDER BY NAME;
```

문제7 보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다. 9시부터 19시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요. 이때 결과는 시간대 순으로 정렬해야 합니다.

```
SELECT HOUR(DATETIME) AS HOUR, COUNT(DATETIME) FROM ANIMAL_OUTS
GROUP BY HOUR
HAVING HOUR BETWEEN 9 AND 19
ORDER BY HOUR;
```

->HOUR을 사용하면 'YYYY-MM-DD'형식의 날짜를 시간별로 나누어서 계산해줍니다.

->BETWEEN은 조건을 걸때 사잇값을 구할경우 ~이상 ~이하로 적용을 해서 계산합니다



