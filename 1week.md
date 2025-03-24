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
- '<프로젝트 id>'.'<dataset>'.'<table>'
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

