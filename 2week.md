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