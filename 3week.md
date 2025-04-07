## 3-4. 오류를 디버깅하는 방법
- 오류 메세지가 알려주는 것
    - 길잡이 : 현재 작성한 방식으로 하면 답을 얻을 수 없음
    - 문제 진단 : 이 부분이 문제가 됨
- Syntax Error
    - 문법을 지키지 않아 생기는 오류
- 오류 예시
```sql
Number of arguments does not match for aggregate function COUNT
// 집계 함수 COUNT의 인자 수가 일치하지 않음
// COUNT 괄호 안에는 하나의 인자만 들어감
```
```sql
SELECT list expression references column type1 a which is neither grouped not aggregated
// SELECT 목록 식은 다음에서 그룹화되거나 집계되지 않은 열 참조
// GROUP BY가 필요한데 명시하지 않은 경우
```
```sql
Expected end of input but got keyword SELECT
// 입력이 끝날 것으로 예상되었지만 SELECT 키워드가 입력
// 여러 개의 쿼리를 쓸 경우 끝날 때 ;
```
``` sql
Expected end of input but got keyword WHERE at [5:1]
// 입력이 끝날 것으로 예상되었지만 [5:1]에서 WHERE을 얻음
// 기본 템플릿에 LIMIT이 달려서 나옴
// LIMIT을 제일 하단으로 옮기거나 삭제
```
```sql
Expected ")" but got end of script at [8:11]
// ")"가 예상되지만 [8:11]에 스크립트가 끝남
// 괄호를 작성하지 않은 경우
```

## 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)
- SELECT/WHERE에서 데이터 변환 가능
- 데이터 타입에 따라 다양한 함수 존재 
- 데이터 타입이 중요한 이유
    - 보이는 것과 저장된 것의 차이가 존재하기 때문!
    - 엑셀의 빈값이 ""/NULL
    - 1이 숫자1/문자1
    - 2023-12-31이 DATE/문자
- 자료 타입 변경 함수 CAST
```sql
SELECT
CAST(1 AS STRING)
// 숫자 1을 문자 1로 변경
``sql
- SAFE_ : 변환이 실패할 경우 NULL반환
    - SAFE_CAST
- 수학 함수
    - x/y 대신 SAFE_DIVIDE(x,y)


## 4-2. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)
1. CONCAT : 문자열 붙이기
```sql
SELECT
    CONCAT("안녕", "하세요") AS result
// 데이터를 직접 넣어준 것이기 때문에 FROM이 없어도 실행
```
2. SPLIT : 문자열 분리하기
```sql
SELECT
    SPLIT("가, 나, 다, 라", ", ") AS result
// SPLIT(문자열 원본, 나눌 기준이 되는 문자)
```
3. REPLACE : 특정 단어 수정하기
```sql
SELECT
    REPLACE("안녕하세요", "안녕", "실천") AS result
// REPLACE(문자열 원본, 찾은 단어, 바꿀 단어)
```
4. TRIM : 문자열 자르기
```sql
SELECT
    TRIM("안녕하세요", "하세요") AS result
// TRIM(문자열 원본, 자를 단어)
```
5. UPPER : 영어 대문자 변환
```sql
SELECT
    UPPER("abc") AS result
```

## 4-4. 날짜 및 시간 데이터 이해하기(1)(타임존, UTC, Millisecond, TIMESTAMP/DATETIME)
### 시간 -> created_at, updated_at
1. 날짜 및 시간 데이터 타입 파악하기 : DATE, DATETIME, TIMESTAMP
2. 날짜 및 시간 데이터 관련 알면 좋은 내용 : UTC, Millisecond
3. 날짜 및 시간 데이터 타입 변환하기
4. 시간 함수(두 시간의 차이, 특정 부분 추출하기)

### 시간 데이터 다루기
- DATE : 2023-12-31
- DATETIME : 2023-12-31 13:00:00(DATE+TIME)
- TIME : 13:00:00
- TIMEZONE : UTC
- TIMESTAMP : 2023-12-31 13:00:00 UTC(UTC 부터 경과한 시간을 나타내는 값)
- millisecond : 1/1000초
- millisecond->TIMESTAMP->DATETIME
- microsecond : 1/1000ms
```sql
SELECT
    TIMESTAMP_MILLIS(123456789) AS milli_to_timestamp_value,
    TIMESTAMP_MICROS(123456789000) AS micro_to_timestamp_value,
    DATETIME(TIMESTAMP_MICROS(123456789000)) AS datetime_value; # TIMEZONE 누락
    DATETIME(TIMESTAMP_MICROS(123456789000), 'Asia/Seoul') AS datetime_value_asia; # TIMEZONE 포함
```

### TIMESTAMP와 DATETIME 비교
```sql
SELECT
    CURRENT_TIMESTAMP() AS timestamp_col
    DATETIME(CURRENT_TIMESTAMP(), 'Asia/Seoul') AS datetime_col
```
![sql3week](/git/sql3week_1.png)


![sql3week_1](/git/sql3week.png)