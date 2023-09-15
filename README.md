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

#### 4. 아픈 동물 찾기
- 동물 보호소에 들어온 동물 중 아픈 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.

``` SQL
SELECT ANIMAL_ID
     , NAME

  FROM ANIMAL_INS
 WHERE INTAKE_CONDITION = 'Sick'
 ORDER BY ANIMAL_ID ;
```

#### 5. 어린 동물 찾기
- 동물 보호소에 들어온 동물 중 젊은 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.

``` SQL
SELECT ANIMAL_ID
     , NAME
  FROM ANIMAL_INS
 WHERE INTAKE_CONDITION != 'Aged'
 ORDER BY ANIMAL_ID ;
```

#### 6. 서울에 위치한 식당 목록 출력하기
- REST_INFO와 REST_REVIEW 테이블에서 서울에 위치한 식당들의 식당 ID, 식당 이름, 음식 종류, 즐겨찾기수, 주소, 리뷰 평균 점수를 조회하는 SQL문을 작성해주세요. 이때 리뷰 평균점수는 소수점 세 번째 자리에서 반올림 해주시고 결과는 평균점수를 기준으로 내림차순 정렬해주시고, 평균점수가 같다면 즐겨찾기수를 기준으로 내림차순 정렬해주세요.

```SQL
SELECT A.REST_ID
     , A.REST_NAME
     , A.FOOD_TYPE
     , A.FAVORITES
     , A.ADDRESS
     , ROUND(AVG(B.REVIEW_SCORE),2) AS SCORE	-- ROUND(컬럼,나타낼 소숫점 자리수) : 반올림
     
  FROM REST_INFO                AS A
       INNER JOIN REST_REVIEW   AS B
               ON ( A.REST_ID = B.REST_ID )
               
 WHERE A.ADDRESS LIKE '서울%'			-- 주소가 서울인
 
 GROUP BY A.REST_ID 				-- REST_ID를 기준으로 그룹화
               
 ORDER BY SCORE DESC, A.FAVORITES DESC ;
```

### GROUP BY
#### 1. 성분으로 구분한 아이스크림 총 주문량
- 상반기 동안 각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회하는 SQL 문을 작성해주세요. 이때 총주문량을 나타내는 컬럼명은 TOTAL_ORDER로 지정해주세요.

```SQL
SELECT B.INGREDIENT_TYPE
     , SUM(A.TOTAL_ORDER)       AS TOTAL_ORDER 
     
  FROM FIRST_HALF               AS A
       INNER JOIN ICECREAM_INFO AS B
               ON ( A.FLAVOR = B.FLAVOR )        

 GROUP BY B.INGREDIENT_TYPE
 ORDER BY TOTAL_ORDER ;
```
