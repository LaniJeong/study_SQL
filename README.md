# study_SQL
## 프로그래머스>코딩테스트연습
### SELECT
#### 1. 인기있는 아이스크림
- 상반기에 판매된 아이스크림의 맛을 총주문량을 기준으로 내림차순 정렬하고 총주문량이 같다면 출하 번호를 기준으로 오름차순 정렬하여 조회하는 SQL 문을 작성해주세요.

``` SQL
SELECT FLAVOR
  FROM FIRST_HALF
 ORDER BY TOTAL_ORDER DESC
        , SHIPMENT_ID ASC ;
```

#### 2. 모든 레코드 조회하기
- 동물 보호소에 들어온 모든 동물의 정보를 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요.

``` SQL
SELECT ANIMAL_ID
     , ANIMAL_TYPE
     , DATETIME
     , INTAKE_CONDITION
     , NAME
     , SEX_UPON_INTAKE
  FROM ANIMAL_INS
 ORDER BY ANIMAL_ID ;
```

#### 3. 오프라인/온라인 판매 데이터 통합하기
- ONLINE_SALE 테이블과 OFFLINE_SALE 테이블에서 2022년 3월의 오프라인/온라인 상품 판매 데이터의 판매 날짜, 상품ID, 유저ID, 판매량을 출력하는 SQL문을 작성해주세요. OFFLINE_SALE 테이블의 판매 데이터의 USER_ID 값은 NULL 로 표시해주세요. 결과는 판매일을 기준으로 오름차순 정렬해주시고 판매일이 같다면 상품 ID를 기준으로 오름차순, 상품ID까지 같다면 유저 ID를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE  -- DATETIME 의 TYPE을 가진 칼럼의 형식을 수정, 지정해 주는 함수
	   , PRODUCT_ID
	   , USER_ID
	   , SALES_AMOUNT
  FROM ONLINE_SALE
 WHERE SALES_DATE BETWEEN '2022-03-01' AND '2022-03-31'   -- 3월 한달간의 범위 지정
 
 UNION ALL                                                -- 중복된 값들 모두 보여줌
 
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE
     , PRODUCT_ID
     , NULL AS USER_ID                                    -- OFFLINE_SALE 테이블 USER_ID NULL 로 표시
     , SALES_AMOUNT
  FROM OFFLINE_SALE
 WHERE SALES_DATE BETWEEN '2022-03-01' AND '2022-03-31'
 
 ORDER BY SALES_DATE
        , PRODUCT_ID 
        , USER_ID ;                                       -- 오름차순으로
```
