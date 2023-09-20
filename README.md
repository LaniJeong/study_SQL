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
#### 7. 과일로 만든 아이스크림 고르기
- 상반기 아이스크림 총주문량이 3,000보다 높으면서 아이스크림의 주 성분이 과일인 아이스크림의 맛을 총주문량이 큰 순서대로 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT B.FLAVOR
  FROM FIRST_HALF   AS A
       LEFT OUTER JOIN ICECREAM_INFO   AS B
         ON (A.FLAVOR = B.FLAVOR)
 WHERE A.TOTAL_ORDER > 3000
   AND B.INGREDIENT_TYPE =  'FRUIT_BASED'
 ORDER BY A.TOTAL_ORDER DESC ;
```

#### 8. 강원도에 위치한 생산공장 목록 출력하기
- FOOD_FACTORY 테이블에서 강원도에 위치한 식품공장의 공장 ID, 공장 이름, 주소를 조회하는 SQL문을 작성해주세요. 이때 결과는 공장 ID를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT FACTORY_ID
     , FACTORY_NAME
     , ADDRESS
  FROM FOOD_FACTORY
 WHERE ADDRESS LIKE '강원도%%'
 ORDER BY FACTORY_ID ASC ;
```

#### 9. 조건에 부합하는 중고거래 댓글 조회하기
- USED_GOODS_BOARD와 USED_GOODS_REPLY 테이블에서 2022년 10월에 작성된 게시글 제목, 게시글 ID, 댓글 ID, 댓글 작성자 ID, 댓글 내용, 댓글 작성일을 조회하는 SQL문을 작성해주세요. 결과는 댓글 작성일을 기준으로 오름차순 정렬해주시고, 댓글 작성일이 같다면 게시글 제목을 기준으로 오름차순 정렬해주세요.

```SQL
SELECT B.TITLE
     , B.BOARD_ID
     , R.REPLY_ID
     , R.WRITER_ID
     , R.CONTENTS
     , DATE_FORMAT(R.CREATED_DATE,'%Y-%m-%d') as CREATED_DATER	-- DATE_FORMAT 꼭 사용
  FROM USED_GOODS_BOARD AS B
       JOIN USED_GOODS_REPLY AS R
         ON (B.BOARD_ID = R.BOARD_ID)
 WHERE B.CREATED_DATE LIKE '2022-10-%%'
 ORDER BY R.CREATED_DATE ASC
        , B.TITLE ASC ;
```

#### 10. 조건에 맞는 도서 리스트 출력하기
- BOOK 테이블에서 2021년에 출판된 '인문' 카테고리에 속하는 도서 리스트를 찾아서 도서 ID(BOOK_ID), 출판일 (PUBLISHED_DATE)을 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.

```SQL
SELECT BOOK_ID
     , DATE_FORMAT(PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
  FROM BOOK
 WHERE PUBLISHED_DATE LIKE '2021-%%-%%'
   AND CATEGORY = '인문'
 ORDER BY PUBLISHED_DATE ASC ;
```

#### 11. 3월에 태어난 여성 회원 목록 출력하기
- MEMBER_PROFILE 테이블에서 생일이 3월인 여성 회원의 ID, 이름, 성별, 생년월일을 조회하는 SQL문을 작성해주세요. 이때 전화번호가 NULL인 경우는 출력대상에서 제외시켜 주시고, 결과는 회원ID를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT MEMBER_ID
     , MEMBER_NAME
     , GENDER
     , DATE_FORMAT(DATE_OF_BIRTH, '%Y-%m-%d') AS DATE_OF_BIRTH
  FROM MEMBER_PROFILE
 WHERE DATE_OF_BIRTH LIKE '%%%%-03-%%'
   AND GENDER = 'W'
   AND TLNO IS NOT NULL
 ORDER BY MEMBER_ID ASC ;
```

#### 12.흉부외과 또는 일반외과 의사 목록 출력하기
- DOCTOR 테이블에서 진료과가 흉부외과(CS)이거나 일반외과(GS)인 의사의 이름, 의사ID, 진료과, 고용일자를 조회하는 SQL문을 작성해주세요. 이때 결과는 고용일자를 기준으로 내림차순 정렬하고, 고용일자가 같다면 이름을 기준으로 오름차순 정렬해주세요.

```SQL
SELECT DR_NAME
     , DR_ID
     , MCDP_CD
     , DATE_FORMAT(HIRE_YMD,'%Y-%m-%d') AS HIRE_YMD
  FROM DOCTOR
 WHERE MCDP_CD IN ('CS', 'GS')	-- IN 사용법 숙지하기
 ORDER BY HIRE_YMD DESC ;
```
#### 13. 평균 일일 대여 요금 구하기
- CAR_RENTAL_COMPANY_CAR 테이블에서 자동차 종류가 'SUV'인 자동차들의 평균 일일 대여 요금을 출력하는 SQL문을 작성해주세요. 이때 평균 일일 대여 요금은 소수 첫 번째 자리에서 반올림하고, 컬럼명은 AVERAGE_FEE 로 지정해주세요.

```SQL
SELECT ROUND(AVG(DAILY_FEE)) AS AVERAGE_FEE	-- ROUND로 소수 첫번째 자리에서 반올림
  FROM CAR_RENTAL_COMPANY_CAR 
 WHERE CAR_TYPE = 'SUV'
```

#### 14. 12세 이하인 여자 환자 목록 출력하기
- PATIENT 테이블에서 12세 이하인 여자환자의 환자이름, 환자번호, 성별코드, 나이, 전화번호를 조회하는 SQL문을 작성해주세요. 이때 전화번호가 없는 경우, 'NONE'으로 출력시켜 주시고 결과는 나이를 기준으로 내림차순 정렬하고, 나이 같다면 환자이름을 기준으로 오름차순 정렬해주세요.

```SQL
SELECT PT_NAME
     , PT_NO
     , GEND_CD
     , AGE
     , COALESCE(TLNO, 'NONE') AS TLNO   -- NULL이 아닌 첫번째 값을 반환
  FROM PATIENT
 WHERE AGE < 13
   AND GEND_CD = 'W'
 ORDER BY AGE DESC
        , PT_NAME ASC ;
```

#### 15. 재구매가 일어난 상품과 회원 리스트 구하기
- ONLINE_SALE 테이블에서 동일한 회원이 동일한 상품을 재구매한 데이터를 구하여, 재구매한 회원 ID와 재구매한 상품 ID를 출력하는 SQL문을 작성해주세요. 결과는 회원 ID를 기준으로 오름차순 정렬해주시고 회원 ID가 같다면 상품 ID를 기준으로 내림차순 정렬해주세요.

```SQL
SELECT USER_ID
     , PRODUCT_ID
  FROM ONLINE_SALE
 GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT (USER_ID) > 1      -- GROUP BY 로 묶어서 조건절(HAVING)사용, AND와 OR로 여러개의 조건 사용 가능
 ORDER BY USER_ID ASC
        , PRODUCT_ID DESC ;
```

#### 16. 역순 정렬하기
- 동물 보호소에 들어온 모든 동물의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 ANIMAL_ID 역순으로 보여주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.

```SQL
SELECT NAME
     , DATETIME
  FROM ANIMAL_INS
 ORDER BY ANIMAL_ID DESC ;
```

#### 17. 동물의 아이디와 이름
- 동물 보호소에 들어온 모든 동물의 아이디와 이름을 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.

```SQL
SELECT ANIMAL_ID
     , NAME
  FROM ANIMAL_INS
 ORDER BY ANIMAL_ID ASC ;
```

#### 18. 여러 기준으로 정렬하기
- 동물 보호소에 들어온 모든 동물의 아이디와 이름, 보호 시작일을 이름 순으로 조회하는 SQL문을 작성해주세요. 단, 이름이 같은 동물 중에서는 보호를 나중에 시작한 동물을 먼저 보여줘야 합니다.

```SQL
SELECT ANIMAL_ID
     , NAME
     , DATETIME
  FROM ANIMAL_INS
 ORDER BY NAME ASC
        , DATETIME DESC ;
```

#### 19. 상위 n개 레코드
- 동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT NAME
  FROM ANIMAL_INS
 ORDER BY DATETIME ASC 
 LIMIT 1;   -- LIMIT 사용법 숙지하기
```

#### 20. 조건에 맞는 회원수 구하기
- USER_INFO 테이블에서 2021년에 가입한 회원 중 나이가 20세 이상 29세 이하인 회원이 몇 명인지 출력하는 SQL문을 작성해주세요.

``` SQL
SELECT COUNT (*)                -- 전체를 세고
  FROM USER_INFO                
 WHERE YEAR(JOINED) = 2021      -- 2021년에 가입한
   AND AGE BETWEEN 20 AND 29    -- 20~29
```

---------------------------------------------------------------------------------------------------------------------------
### SUM,MAX,MIN
#### 1. 가장 비싼 상품 구하기
- PRODUCT 테이블에서 판매 중인 상품 중 가장 높은 판매가를 출력하는 SQL문을 작성해주세요. 이때 컬럼명은 MAX_PRICE로 지정해주세요.

```SQL
SELECT MAX(PRICE) AS MAX_PRICE
  FROM PRODUCT
```

#### 2. 최댓값 구하기
- 가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT MAX(DATETIME) AS '시간'
  FROM ANIMAL_INS
```

#### 3. 최솟값 구하기
- 동물 보호소에 가장 먼저 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT MIN(DATETIME) AS '시간'
  FROM ANIMAL_INS
```

#### 4. 동물 수 구하기
- 동물 보호소에 동물이 몇 마리 들어왔는지 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT COUNT(ANIMAL_ID) AS 'COUNT'
  FROM ANIMAL_INS
```

#### 5. 중복 제거하기
- 동물 보호소에 들어온 동물의 이름은 몇 개인지 조회하는 SQL 문을 작성해주세요. 이때 이름이 NULL인 경우는 집계하지 않으며 중복되는 이름은 하나로 칩니다.

```SQL
SELECT COUNT( DISTINCT NAME) AS 'COUNT' -- 중복제거(DISTINCT) 후 COUNT
  FROM ANIMAL_INS
```

####  6. 가격이 제일 비싼 식품의 정보 출력하기
- FOOD_PRODUCT 테이블에서 가격이 제일 비싼 식품의 식품 ID, 식품 이름, 식품 코드, 식품분류, 식품 가격을 조회하는 SQL문을 작성해주세요.

```SQL
SELECT *
  FROM FOOD_PRODUCT
 ORDER BY PRICE DESC
 LIMIT 1 ;
```
------------------------------------------------------------------------------------------------------------------------------------

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

#### 2. 고양이와 개는 몇 마리 있을까
- 동물 보호소에 들어온 동물 중 고양이와 개가 각각 몇 마리인지 조회하는 SQL문을 작성해주세요. 이때 고양이를 개보다 먼저 조회해주세요.

```SQL
SELECT ANIMAL_TYPE, COUNT(*) AS 'count'

  FROM ANIMAL_INS
  
 GROUP BY ANIMAL_TYPE 
   HAVING ANIMAL_TYPE IN ('Cat', 'Dog')      -- GROUP BY 절의 결과에 조건을 붙이고 싶을 때
 ORDER BY ANIMAL_TYPE ;
```

#### 3. 동명 동물 수 찾기
- 동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.

```SQL
/* ()안의 쿼리를 먼저 실행 */
SELECT *
  FROM ( SELECT NAME, COUNT(NAME) AS 'COUNT' 
           FROM ANIMAL_INS WHERE NAME IS NOT NULL
          GROUP BY NAME 
          ORDER BY NAME ) C
 WHERE COUNT > 1
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### JOIN
#### 1. 상품 별 오프라인 매출 구하기
- PRODUCT 테이블과 OFFLINE_SALE 테이블에서 상품코드 별 매출액(판매가 * 판매량) 합계를 출력하는 SQL문을 작성해주세요. 결과는 매출액을 기준으로 내림차순 정렬해주시고 매출액이 같다면 상품코드를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT A.PRODUCT_CODE
     , SUM(A.PRICE * B.SALES_AMOUNT) AS SALES
  FROM PRODUCT            AS A
       INNER JOIN OFFLINE_SALE  AS B
               ON ( A.PRODUCT_ID = B.PRODUCT_ID )
 GROUP BY A.PRODUCT_ID		-- GROUP BY 이해가 안됨,,,,,,,,
 ORDER BY SALES DESC
        , A.PRODUCT_CODE ASC ;
```

#### 2. 조건에 맞는 도서와 저자 리스트 출력하기
- '경제' 카테고리에 속하는 도서들의 도서 ID(BOOK_ID), 저자명(AUTHOR_NAME), 출판일(PUBLISHED_DATE) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.

```SQL
SELECT BOOK_ID
     , AUTHOR_NAME
     , DATE_FORMAT (PUBLISHED_DATE, "%Y-%m-%d") AS PUBLISHED_DATE
  FROM BOOK AS B
       JOIN AUTHOR AS A 
         ON (B.AUTHOR_ID = A.AUTHOR_ID
        AND B.CATEGORY = '경제') 	-- WHERE을 따로 쓰지 않고 ON에 AND로 함께 작성
 ORDER BY PUBLISHED_DATE ASC; 
```

#### 3. 없어진 기록 찾기
- 천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

```SQL
/* 입양을 간 기록은 있는데(ANIMAL_OUTS이 기준 테이블) 
   보호소에 들어온 기록이 없는(I.ANIMAL_ID IS NULL)
   O.ANIMAL_ID / O.NAME ID순으로 조회 */
SELECT O.ANIMAL_ID
     , O.NAME
  FROM ANIMAL_OUTS AS O
       LEFT JOIN ANIMAL_INS AS I
              ON O.ANIMAL_ID = I.ANIMAL_ID
 WHERE I.ANIMAL_ID IS NULL
 ORDER BY I.ANIMAL_ID 
```

--------------------------------------------------------------------------------------------------------------------------------------------------------

### IS NULL
#### 1. 
-
```SQL

```
