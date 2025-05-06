## 4-4. 날짜 및 시간 데이터 이해하기(2)(EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FORMAT_DATETIME)

- CURRENT_DATETIME([time_zone]) : 현재 DATETIME 출력
    - time_zone을 넣는 것에 유의
- EXTRACT
    - DATETIME에서 특정 부분만 추출
    - EXTRACT(부분, FROM DATETIME "2025-05-06 14:00:00")
    - DAYOFWEEK로 요일 추출
- DATETIME_TRUNC
    - DATE와 HOUR만 남기기(HOUR 기준이라면 14:41:13->14:00:00)
- PARSE_DATETIME
    - 문자열로 저장된 DATETIME을 DATETIME 타입으로 변환
    - PRASE_DATETIME("문자열의 형태", "DATETIME 문자열")
- FORMAT_DATETIME(위와 반대)
    - DATETIME 데이터를 특정 형태의 문자열로 변환
    - FORMAT_DATETIME("문자열의 형태", DATETIME 타입 데이터)
- LAST_DAY
    - 마지막 날을 알고 싶은 경우
    - default 값은 MONTH
- DATETIME_DIFF
    - 두 DATETIME의 차이
    - DATETIME_DIFF(첫 DATETIME, 두번쨰 DATETIME, 단위)
### EXTRACT vs DATETIME_TRUNC
이후 데이터 분석에서 어떻게 활용할지에 따라 다르다
1시간 단위로 수요를 집계해야 하는 경우 DATETIME_TRUNC

## 4-5 시간 데이터 연습 문제(1)
1. 트레이너가 포켓몬을 포획한 날짜(catch_date)를 기준으로, 2023년 1월에 포획한 포켓몬의 수를 계산해주세요. 
- 쿼리 작성 목표/확인 지표 : 포켓몬의 수
- 쿼리 계산 방법 : COUNT
- 데이터 기간 : 2023년 1월
- 사용할 테이블 : trainer_pokemon
- Join KEY :
- 데이터 특징 : catch_date가 UTC 기준인지 KR 기준인지 확인 필요->catch_date와 catch_datetime 컬럼 비교
    - 컬럼의 설명을 꼭 확인한 후에 쿼리를 작성하자!
 
```sql
SELECT
    COUNT(DISTINCT id) AS cnt
FROM basic.trainer_pokemon
WHERE
    EXTRACT(YEAR FROM DATETIME(catch_datetime, "Asia/Seoul"))=2023 # catch_datetime은 TIMESTAMP로 저장->DATETIME으로 변환하는 과정
    AND EXTRACT(MONTH FROM DATETIME(catch_datetime, "Asia/Seoul"))=1
```

2. 배틀이 일어난 시간(battle_datetime)을 기준으로, 오전 6시에서 오후 6시 사이에 일어난 배틀의 수를 계산해주세요.
- 쿼리 작성 목표/확인 지표 : 오전 6시-오후 6시 배틀의 수
- 쿼리 계산 방법 : COUNT
- 데이터 기간 : 오전 6시-오후 6시
- 사용할 테이블 : battle
- Join KEY :
- 데이터 특징 : catch_date가 UTC 기준인지 KR 기준인지 확인 필요->catch_date와 catch_datetime 컬럼 비교
```sql
SELECT
    COUNT(DISTINCT id) AS battle_cnt
FROM basic.battle
WHERE
    EXTRACT(HOUR FROM battle_datetime)>=6
    AND EXTRACT(HOUR FROM battle_datetime)<18
    # EXTRACT(HOUR FROM battle_datetime) BETWEEN 6 and 18도 가능!
```

## 4-5 시간 데이터 연습 문제(2)

3. 각 트레이너별로 그들이 포켓몬을 포획한 첫날(catch_date)을 찾고, 그 날짜를 'DD/MM/YYYY' 형식으로 출력해주세요. (2024-01-01=>01/01/2024)
- 쿼리 작성 목표/확인 지표 : 포획 첫날을 ''DD/MM/YYYY' 형식으로 출력
- 쿼리 계산 방법 : FORMAT_DATETIME
- 데이터 기간 :
- 사용할 테이블 : trainer_pokemon
- Join KEY :
- 데이터 특징 :
```sql
SELECT
    trainer_id,
    FORMAT_DATE("%d/%m/%Y", min_catch_date) AS new_min_catch_date
FROM(
SELECT
    trainer_id,
    MIN(DATE(catch_datetime, "Asia/Seoul")) AS min_catch_date
FROM basic.trainer_pokemon
GROUP BY
    trainer_id
)
ORDER BY
    trainer_id
```

4. 배틀이 일어난 날짜(battle_date)를 기준으로, 요일별로 배틀이 얼마나 자주 일어났는지 계산해주세요.
- 쿼리 작성 목표/확인 지표 : 요일별 배틀
- 쿼리 계산 방법 : 요일별 COUNT
- 데이터 기간 :
- 사용할 테이블 : battle
- Join KEY :
- 데이터 특징 :
```sql
SELECT
    day_of_week,
    COUNT(DISTINCT id) AS battle_cnt
FROM(
SELECT
    *,
    EXTRACT(DAYOFWEEK FROM battle_date) AS day_of_week
FROM basic.battle
)
GROUP BY
    day_of_week
ORDER BY
    day_of_week
```

5. 트레이너가 포켓몬을 처음으로 포획한 날짜와 마지막으로 포획한 날짜의 간격이 큰 순으로 정렬하는 쿼리를 작성해주세요.
- 쿼리 작성 목표/확인 지표 : 처음 날짜와 마지막 날짜 diff 큰 순으로 정렬
- 쿼리 계산 방법 : 마지막 날짜-처음 날짜를 DATETIME_DIFF하고 ORDER BY
- 데이터 기간 :
- 사용할 테이블 : trainer_pokemon
- Join KEY :
- 데이터 특징 : catch_datetime 사용
```sql
SELECT
    *,
    DATETIME_DIFF(max_catch_datetime, min_catch_datetime, DAY) AS diff
FROM(
SELECT
    trainer_id,
    MIN(DATETIME(catch_datetime, "Asia/Seoul")) AS min_catch_datetime,
    MAX(DATETIME(catch_datetime, "Asia/Seoul")) AS max_catch_datetime
FROM basic.trainer_pokemon
GROUP BY
    trainer_id
)
ORDER BY
    diff
```

## 4-6 조건문(CASE WHEN, IF)
- 특정 카테고리를 하나로 합치는 경우
### CASE WHEN
- 여러 조건일 때 사용
```SQL
SELECT
    new_type1,
    COUNT(DISTINCT id) AS pokemon_cnt
SELECT
    *,
    CASE
        WHEN(type1 IN ("Rock", "Ground")) OR (type2 IN ("Rock", "Ground")) THEN "Rock&Ground"
    ELSE type1
    END AS new_type1
FROM basic.pokemon
)
GROUP BY
    new_type1
```
- 순서에 주의해야 함
- 조건1, 조건2에 둘 다 해당하면 앞선 순서를 따름

### IF
- 단일 조건일 때 유용
- IF(조건문, true일 때 값, false일 때 값) AS 새로운 컬럼명

## 4-7 조건문 연습 문제
```sql
SELECT
    new_type1,
    COUNT(DISTINCT id) AS pokemon_cnt
FROM(
SELECT
    *,
    CASE
        WHEN(type1 IN ("Rock", "Ground")) OR (type2 IN ("Rock", "Ground")) THEN "Rock&Ground"
    ELSE type1
    END AS new_type1
FROM basic.pokemon
)
GROUP BY
    new_type1
```

1. 포켓몬의'speed'가 70이상이면 '빠름', 그렇지 않으면 '느림'으로 표시하는 새로운 컬럼'Speed_Category'를 만들어주세요.
- 쿼리 작성 목표/확인 지표 : speed로 구분해서 Speed_Category 만들
- 쿼리 계산 방법 : if
- 데이터 기간 :
- 사용할 테이블 : pokemon
- Join KEY :
- 데이터 특징 : 
```sql
SELECT
    *,
    IF(speed>=70,"빠름","느림") AS Speed_Category
FROM basic.pokemon
```

2. 포켓몬의 'type1'에 따라 'Water','Fire','Electric'타입은 각각 '물','불','전기'로, 그외타입은 '기타'로 분류하는 새로운 컬럼 'type_Korean'을 만들어주세요
- 쿼리 작성 목표/확인 지표 : type1에 따라 type_Korean 만들기
- 쿼리 계산 방법 : CASE WHEN
- 데이터 기간 :
- 사용할 테이블 : pokemon
- Join KEY :
- 데이터 특징 : 
```sql
SELECT 
    id,
    kor_name,
    type1,
    CASE
        WHEN type1 = "Water" THEN "물"
        WHEN type1 = "Fire" THEN "불"
        WHEN type1 = "Electric" THEN "전기"
        ELSE "기타"
    END AS type1_Korean
FROM basic.pokemon
```

3. 각 포켓몬의 총점(total)을 기준으로, 300이하면 'Low', 301에서 500사이면 'Medium', 501 이상이면'High'로 분류해주세요
- 쿼리 작성 목표/확인 지표 : total에 따라 변경
- 쿼리 계산 방법 : CASE WHEN
- 데이터 기간 :
- 사용할 테이블 : pokemon
- Join KEY :
- 데이터 특징 : 

```sql
SELECT
    id,
    kor_name,
    total,
    CASE
        WHEN total>=501 THEN "High"
        WHEN total BETWEEN 300 AND 500 THEN "Medium"
    ELSE "Low"
    END AS total_grade
FROM basic.pokemon
```

4. 각 트레이너의 배지 개수(badge_count)를 기준으로, 5개 이하면'Beginner',6개에서 8개사이면 'Intermediate', 그 이상이면'Advanced'로 분류해주세요.
- 쿼리 작성 목표/확인 지표 : badge_count 값 변경
- 쿼리 계산 방법 : CASE WHEN
- 데이터 기간 :
- 사용할 테이블 : trainer
- Join KEY :
- 데이터 특징 : 

```sql
SELECT
    id,
    name,
    badge_count,
    CASE
        WHEN badge_count>=9 THEN "Advanced"
        WHEN badge_count BETWEEN 6 AND 8 THEN "Intermediate"
    ELSE "Begginer"
    END AS trainer_level
FROM basic.trainer
```

5. 트레이너가 포켓몬을 포획한 날짜(catch_date)가 '2023-01-01'이후이면 'Recent', 그렇지 않으면 'Old'로 분류해주세요
- 쿼리 작성 목표/확인 지표 : 포획 날짜 기준 값 변경
- 쿼리 계산 방법 : IF
- 데이터 기간 :
- 사용할 테이블 : trainer_pokemon
- Join KEY :
- 데이터 특징 : 

```sql
SELECT
    id,
    trainer_id,
    pokemon_id,
    catch_datetime,
    IF(DATE(catch_datetime, "Asia/Seoul")>="2023-01-01", "Recent", "Old") AS recent_or_old
FROM basic.trainer_pokemon
```

## 4-8 정리
1. 숫자
    - 사칙연산
    - SAFE_DIVIDE : 연산이 안되면 NULL 반환
2. 문자
    - CONCAT
    - SPLIT
    - REPLACE
    - TRIM
    - UPPER
3. 시간, 날짜
    - EXTRACT
    - DATETIME_TRUNC
    - PARSE_DATETIME

## 4-9 BigQuery 공식 문서 확인
- 지피티도 최근 문서는 모를수도 있기 때문에 알아두는 것이 좋음
> "기술명+ documentation"으로 검색

> CONCAT 문법을 알고 싶으면 BigQuery CONCAT document라고 구글에 검색

![sql4week](/git/sql4week.png)










