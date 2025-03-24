## 2-3. 데이터 탐색(SELECT, FROM, WHERE)
### SQL 쿼리 구조
```sql
SELECT # 어떤 컬럼을 출력?
Col1 AS new_name,
Col2,
Col3
FROM Dastaset.Table # 어떤 테이블에서 데이터 확인?
WHERE # 원하는 조건?
Col1 = 1
```
- SELECT * : 모든 컬럼 출력
- SELECT * EXCEPT(제외 컬럼)

### SQL 쿼리 작성
- '<프로젝트 id>'.'<데이터셋>'.'<테이블블>'
- 프로젝트 id는 꼭 명시할 필요는 없음(프로젝트가 단일인 경우)
- 프로젝트를 여러개 사용한다면 명시하는 것이 좋음->쿼리를 실행할 때 어떤 프로젝트인지 확인하는 과정이 존재
- 데이터를 활용하고 싶은 목적이 있어야 어떤 컬럼을 선택할지 알 수 있음->항상 목적을 먼저 고민!!
- : 은 쿼리를 구분

### SQL 문법 정리
- FROM : 데이터를 확인할 테이블 명시
    - 이름이 너무 길면 AS '별칭'
- FROM Table1 AS t1
    - WHERE : FROM에 명시된 테이블에 저장된 데이터를 필터링(컬럼 조건 설정)
    - WHERE type1 = 'Fire'
- SELECT : 테이블에 저장되어 있는 컬럼 선택
    - col1 AS '별칭'



## 2-4. SELECT 연습 문제
1. trainer 테이블에 있는 모든 데이터를 보여주는 쿼리 작성
- trainer 테이블에 어떤 데이터가 있는지 확인
- trainer 테이블을 어디에 명시? -> FROM
- 필터링 조건?->모든 데이터->필터링x
```sql
SELECT 
  * 
FROM `bigquery-454009.basic.trainer` 
```
2. trainer 테이블에 있는 트레이너의 name을 출력하는 쿼리 작성
```sql
SELECT
  name
FROM `basic.trainer`
```
3. trainer 테이블에 있는 트레이너의 name, age를 출력하는 쿼리 작성
```sql
SELECT
  name
FROM `basic.trainer`
```
4. trainer의 테이블에서 id가 3인 트레이너의 name, age, hometown을 출력하는 쿼리 작성
```sql
SELECT
  name,
  age,
  hometown
FROM basic.trainer
WHERE id = 3
```
5. pokemon 테이블에서 피카츄의 공격력과 체력을 확인할 수 있는 쿼리 작성
```sql
SELECT
  attack,
  hp
FROM basic.pokemon
WHERE kor_name = "피카츄"
```



## 2-5. 집계(GROUP BY + HAVING + SUM/COUNT)
### GROUP BY
- 같은 값끼리 모아서 그룹화
- DISTINCT : unique
```sql
SELECT
    집계할 컬럼1,
    집계함수(COUNT, MAX, MIN..)
FROM Table
GROUP BY
    집계할 컬럼1
```
1. pokemon 테이블에 있는 포켓몬 수를 구하는 쿼리 작성
```sql
SELECT
    COUNT(id) as cnt
FROM basic.pokemon
```
2. 포켓몬의 수가 세대별로 얼마나 있는지 알 수 있는 쿼리 작성
```sql
SELECT
    generation,
    COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY
    generation
```
3. 포켓몬의 수를 타입 별로 집계하고, 포켓몬의 수가 10 이상인 타입만 남기는 쿼리 작성, 포켓몬의 수가 많은 순으로 정렬
```sql
SELECT
    type1
    COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY
    type1
HAVING cnt>=10
ORDER BY cnt DESC
```
- 일자별/연령대별/특정 타입별/앱 화면별 집계 등에서 쓰임!

### HAVING
- WHERE : Table에 바로 조건 설정
- HAVING : GROUP BY한 후 조건 설정
```sql
SELECT
    컬럼1, 컬럼2,
    COUNT(컬럼1) AS col1_count
FROM <table>
WHERE
    컬럼1>=3
GROUP BY 컬럼1, 컬럼2
HAVING
    col1_count>3
```

### 그외
- 정렬 : ORDER BY(DESC, OSC)
    - 쿼리의 맨 마지막에 작성
- 출력 개수 제한 : LIMIT
    - 쿼리의 맨 마지막에 작성
=======
### SQL 문법 정리
- FROM : 데이터를 확인할 테이블 명시
    - 이름이 너무 길면 AS '별칭'
- FROM Table1 AS t1
    - WHERE : FROM에 명시된 테이블에 저장된 데이터를 필터링(컬럼 조건 설정)
    - WHERE type1 = 'Fire'
- SELECT : 테이블에 저장되어 있는 컬럼 선택
    - col1 AS '별칭'
