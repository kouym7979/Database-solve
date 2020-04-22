## 프로그래머스 Select문 정리 및 문제 풀이

Select문은 Database table에 들어있는 정보를 조회해주는 문장입니다.

   1.모든 필드 조회

​       -> SELECT*FROM 테이블명;

​       이 문장은 모든 열(필드)를 선택하여 모든열의 정보를 조회할 수 있습니다.

2. 특정 필드 조회

   ->SELECT 필드명 FROM 테이블명;

   이 경우 해당 필드의 정보만 조회가 됩니다.

   ->SELECT 필드명1,필드명2 ... FROM 테이블명;

3. WHERE 문법

   ->WHERE은 조건식을 넣어서 조건에 맞는 데이터를 조회할 수 있게 합니다.

   ->SELECT 필드명 FROM 테이블명 WHERE 조건식; 

4. 필드 정렬

   ->SELECT 필드명 FROM 테이블명 ORDER BY 정렬할필드;

   해당 필드를 정렬하고 선택된 필드를 조회합니다.

___

문제`

ANIMAL_INS` 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. `ANIMAL_INS` 테이블 구조는 다음과 같으며, `ANIMAL_ID`, `ANIMAL_TYPE`, `DATETIME`, `INTAKE_CONDITION`, `NAME`, `SEX_UPON_INTAKE`는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

| NAME             | TYPE       | NULLABLE |
| ---------------- | ---------- | -------- |
| ANIMAL_ID        | VARCHAR(N) | FALSE    |
| ANIMAL_TYPE      | VARCHAR(N) | FALSE    |
| DATETIME         | DATETIME   | FALSE    |
| INTAKE_CONDITION | VARCHAR(N) | FALSE    |
| NAME             | VARCHAR(N) | TRUE     |
| SEX_UPON_INTAKE  | VARCHAR(N) | FALSE    |

동물 보호소에 들어온 모든 동물의 정보를 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.

___

```
SELECT  ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS ORDER BY ANIMAL_ID;
```

```
SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID;
```

첫 번째는 특정 필드를 넣어서 문제의 조건에 맞게 ANIMAL_ID에 의한 정렬로 출력하는 것을 활용해 본 것입니다.

두 번째는 요구하는것이 전체이기 때문에 전체를 출력한후 뒤에 정렬을 문제의 조건에 맞게 정렬을 하여 출력한 것입니다.