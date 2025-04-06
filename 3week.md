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