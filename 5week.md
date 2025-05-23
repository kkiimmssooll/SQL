## 5-1. Intro
> 다량의 자료를 연결하는 JOIN

## 5-2. JOIN 이해하기
>서로 다른 데이터 테이블을 연결하는 것

```
ex
트레이너 데이터와 포켓몬 데이터에 겹치는 컬럼이 없음
=> 두 데이터를 연결할 수 있는 공통값이 없음!
=> 트레이너 포켓몬 데이터를 활용해 연결
```
 
- 공통 컬럼(KEY)을 기준으로 새 컬럼을 만들어서 연결
    - 보통 ID값으로 많이 사용
- 여러 테이블 JOIN도 가능
- 테이블 형태를 보고 JOIN 후의 모습을 예상하면 훨씬 쉬움

## 5-3. 다양한 JOIN 방법(LEFT, RIGHT, INNER, CROSS JOIN)
- INNER JOIN
    - 두 테이블의 공통적인 요소만 남김
- LEFT JOIN
    - 왼쪽 테이블을 기준으로 그대로 두고 겹치는 것만 조인
- RIGHT JOIN
    - 오른쪽 테이블을 기준으로 그대로 두고 겹치는 것만 조인
- FULL JOIN
    - 모든 값을 조인하고 없는 값은 NULL
- CROSS JOIN
    - a테이블 로우 * b테이블 로우
    - 데이터가 뻥튀기 될 수 있으니 주의!!
![sql5week1](/git/sql5week_1.png)
![sql5week2](/git/sql5week_2.png)

## 5-4. JOIN 쿼리 작성하기
테이블 확인->기준 테이블 정의->JOIN Key 찾기->결과 예상하기(손, 엑셀로 작성)->쿼리 작성/검증
```
SELECT
    col
FROM table_a AS A
INNER JOIN table_b AS B
ON A.key=B.key
```
- CROSS JOIN을 제외하고는 ON 필수
- 밑에 줄줄 이어서 쓰면 여러개 JOIN 가능
- 전체 컬럼은 t.*/일부 제외는 t.* EXCEPT(제외 컬럼)

## 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분
1. 여러 JOIN 중 어떤 것을 사용해야 할까?
    - 목적에 따라 선택
    - 교집합 : INNER
    - 모두 다 : CROSS
    - 둘다 아니면 LEFT/RIGHT
    - 쿼리 템플릿에 예상 결과를 작성하고 중간 과정을 거치면서!
2. 어떤 Table을 왼쪽에 두고, 어떤 Table이 오른쪽에 가야 할까?
    - 기준 테이블을 왼쪽에 두기
    - 기준에는 기준값 존재
    - 데이터 요소들이 빠짐없이 존재하는지?
3. 여러 Table을 연결할 수 있는 걸까?
    - 개수에 한계는 없지만 헷갈릴 수 있음
4. 컬럼은 모두 다 선택해야 할까?
    - 무엇을 하려고 하는지에 따라 다름
    - 사용하지 않을 컬럼은 선택하지 말자
5. NULL이 뭘까?
    - 값이 없음!
    - 0이나 공백과는 다름
    - JOIN에선 연결할 값이 없는 경우 나타남

## 5-6.
1. 트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리를 작성해주세요.
```
단계별로 하나씩 작성하고 실행하면서 확인하기!
1) status가 Active, Training인 것만을 필터링(WHERE)
2) 필터링 후 pokemon과 조인(서브쿼리 사용)
```
![sql5week3](/git/sql5week_3.png)

2. 각 트레이너가 가진 포켓몬중에서 'Grass' 타입의 포켓몬 수를 계산해주세요(단, 편의를 위해 type1 기준으로 계산해주세요)
![sql5week4](/git/sql5week_4.png)

3. 트레이너의 고향(hometown)과 포켓몬을 포획한 위치(location)를 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산해주세요.
![sql5week5](/git/sql5week_5.png)

4. Master 등급인 트레이너들은 어떤 타입의 포켓몬을 제일 많이 보유하고 있을까요?
![sql5week6](/git/sql5week_6.png)

5. Incheon 출신 트레이너들은 1세대, 2세대 포켓몬을 각각 얼마나 보유하고 있나요?
```
1) trainer와 pokemon의 정보가 필요한데, 얘네 둘을 직접적으로 묶을 key가 없으므로 중간에 trainer_pokemon으로 조인
2) 보유는 status IN ("Active","Training")
```
![sql5week7](/git/sql5week_7.png)

# 수행 인증
![sql5week8](/git/sql5week.png)