## 2-6. 연습문제 풀이
- 테이블
- 조건
- 컬럼
-집계

### 1.
```sql
SELECT
    COUNT(id) AS cnt
FROM basic.pokemon
WHERE
    type2 IS NULL
```

### 2.
```sql
SELECT
    type1,
    COUNT(id) AS cnt
FROM basic.pokemon
WHERE
  type2 IS NULL
GROUP BY
  type1
ORDER BY cnt DESC
```

### 3.
```sql
SELECT
  type1,
  COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY
  type1
```
- DISTINCT는 DAU에 자주 사용

### 4. 
```sql
SELECT
  is_legendary,
  COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY
  is_legendary
```
- GROUP BY/ORDER BY 1 -> SELECT의 첫 컬럼을 의미

### 5. 
```sql
SELECT
  name,
  COUNT(name) AS cnt
FROM basic.trainer
GROUP BY
  name
HAVING
  cnt>=2
```

### 6.
```sql
SELECT
  *
FROM basic.trainer
WHERE
  name = 'Iris'
```

### 7.
```sql
SELECT
  *
FROM basic.trainer
WHERE
  (name = 'Iris')
  or (name = 'Whitney')
  or (name = 'Cynthia') --괄호가 없으면 꼬일 수 있음
```
```sql
SELECT
  *
FROM basic.trainer
WHERE
  name in ('Iris','Cynthia','Whitney')
```

### 8. 
```sql
SELECT
 COUNT(id) as cnt
FROM basic.trainer
```

### 9.
```sql
SELECT
  generation,
  COUNT(id) as cnt
FROM basic.pokemon
```

### 10.
```sql
SELECT
  generation,
  COUNT(id) as cnt
FROM basic.pokemon
```

### 11.
```sql
SELECT
  type1,
  COUNT(id) as cnt
FROM basic.pokemon
WHERE
  type2 IS NOT NULL
GROUP BY
  type1
ORDER BY cnt DESC
LIMIT 1
```

### 12. 
```sql
SELECT 
  type1,
  COUNT(id) as cnt
FROM basic.pokemon
WHERE
  type2 IS NULL
GROUP BY 
  type1
ORDER BY cnt DESC
LIMIT 1
```

### 13.
```sql
SELECT
  kor_name
FROM basic.pokemon
WHERE
  kor_name LIKE "파%"
```
- 컬럼 LIKE "특정단어%" : %는 앞에도 부틀 수 있고, 뒤에도 붙을 수 있음
- "파%" : 파로 시작하는 단어, "%파" : 파로 끝나는 단어, "%파%" : 파가 들어간 단어

### 14.
```sql
SELECT
  COUNT(id) AS cnt
FROM basic.trainer
WHERE
  badge_count>= 6
```

### 15. 
```sql
SELECT
  trainer_id,
  COUNT(pokemon_id) AS cnt
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
```

### 16.
```sql
SELECT
  trainer_id,
  COUNT(pokemon_id) AS cnt
FROM basic.trainer_pokemon
WHERE
  status = "Released"
GROUP BY
  trainer_id
ORDER BY
  cnt DESC
LIMIT 1
```

### 17.
```sql
SELECT 
  trainer_id,
  COUNTIF(status = 'Released') AS released_cnt,
  COUNT(pokemon_id) AS pokemon_cnt,
  COUNTIF(status = 'Released')/COUNT(pokemon_id) AS released_ratio
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
HAVING
  released_ratio>=0.2
```

## 2-8. GROUP BY ALL
- GROUP BY처럼 컬럼 하나하나 명시하지 않아도 됨

## 3-1 INTRO
- SQL 쿼리 작성 흐름
- 쿼리 작성 템플릿과 생산성 도구
- 오류 디버깅

## 3-2 SQL 쿼리 작성 흐름
- 지표 고민(문제 정의)
- 지표 구체화(분자, 분모 표시)
- 지표 탐색
- 쿼리 작성(테이블 찾기)
- 데이터 정합성 확인
- 쿼리 가독성
- 쿼리 저장

## 3-3 쿼리 작성 템플릿과 생산성 도구
1. 쿼리 작성 목표, 확인할 지표
2. 쿼리 계산 방법
3. 데이터의 기간
4. 사용할 테이블
5. Join key
6. 데이터 특징
- select
- from
- where

- Espanso

![sql2week](/git/sql2week.png)