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

#### 21. 업그레이드 된 아이템 구하기
- 아이템의 희귀도가 'RARE'인 아이템들의 모든 다음 업그레이드 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력하는 SQL 문을 작성해 주세요. 이때 결과는 아이템 ID를 기준으로 내림차순 정렬주세요.

``` SQL
 SELECT A.ITEM_ID
      , A.ITEM_NAME
      , A.RARITY
   FROM ITEM_INFO               A
        INNER JOIN ITEM_TREE    B
                ON ( B.ITEM_ID = A.ITEM_ID )
        INNER JOIN ITEM_INFO    C
                ON ( C.ITEM_ID = B.PARENT_ITEM_ID )
  WHERE C.RARITY = 'RARE'
  ORDER BY A.ITEM_ID DESC
```

#### 22. 특정 형질을 가지는 대장균 찾기
- 2번 형질이 보유하지 않으면서 1번이나 3번 형질을 보유하고 있는 대장균 개체의 수(COUNT)를 출력하는 SQL 문을 작성해주세요.
  1번과 3번 형질을 모두 보유하고 있는 경우도 1번이나 3번 형질을 보유하고 있는 경우에 포함합니다.

``` SQL
/*
SELECT COUNT(A.GENOM)  AS 'COUNT'
  FROM (SELECT SUBSTRING(CONV(GENOTYPE, 10, 2), 2, 1)  AS GENOM		-- GENOTYPE을 2진수로 바꾼 후 문자열을 잘라 2번 형질 찾기
	  FROM ECOLI_DATA )  A
 WHERE A.GENOM = 1							-- 2번 형질 Y=1, N=0
*/

-- 비트연산자 사용
SELECT COUNT(GENOTYPE) AS COUNT
  FROM ECOLI_DATA
 WHERE GENOTYPE & 5		-- 00101(1, 3번 형질 보유)
   AND NOT GENOTYPE & 2		-- 00010(2번 형질 보유X)
```

#### 23. Python 개발자 찾기
- DEVELOPER_INFOS 테이블에서 Python 스킬을 가진 개발자의 정보를 조회하려 합니다.
  Python 스킬을 가진 개발자의 ID, 이메일, 이름, 성을 조회하는 SQL 문을 작성해 주세요.
  결과는 ID를 기준으로 오름차순 정렬해 주세요.

```SQL
SELECT ID
     , EMAIL
     , FIRST_NAME
     , LAST_NAME
  FROM DEVELOPER_INFOS
 WHERE SKILL_1 IN ('Python')
    OR SKILL_2 IN ('Python')
    OR SKILL_3 IN ('Python')
 ORDER BY ID
```

#### 24. 잔챙이 잡은 수 구하기
- 잡은 물고기 중 길이가 10cm 이하인 물고기의 수를 출력하는 SQL 문을 작성해주세요.
  물고기의 수를 나타내는 컬럼 명은 FISH_COUNT로 해주세요.

```SQL
SELECT COUNT(ID)  AS FISH_COUNT
  FROM FISH_INFO
 WHERE LENGTH IS NULL
```

#### 25. 가장 큰 물고기 10마리 구하기
- FISH_INFO 테이블에서 가장 큰 물고기 10마리의 ID와 길이를 출력하는 SQL 문을 작성해주세요.
  결과는 길이를 기준으로 내림차순 정렬하고, 길이가 같다면 물고기의 ID에 대해 오름차순 정렬해주세요.
  단, 가장 큰 물고기 10마리 중 길이가 10cm 이하인 경우는 없습니다.
  ID 컬럼명은 ID, 길이 컬럼명은 LENGTH로 해주세요.

```SQL
SELECT ID
     , LENGTH
  FROM FISH_INFO
 ORDER BY LENGTH DESC, ID
 LIMIT 10
```

#### 26. 특정 물고기를 잡은 총 수 구하기
- FISH_INFO 테이블에서 잡은 BASS와 SNAPPER의 수를 출력하는 SQL 문을 작성해주세요. 컬럼명은 'FISH_COUNT`로 해주세요.

```SQL
SELECT COUNT(ID)  AS FISH_COUNT
  FROM FISH_INFO                    A
  LEFT OUTER JOIN FISH_NAME_INFO    B
               ON (B.FISH_TYPE = A.FISH_TYPE)
 WHERE B.FISH_NAME IN ('BASS', 'SNAPPER')
```

#### 27. 연도별 대장균 크기의 편차 구하기 
- 분화된 연도(YEAR), 분화된 연도별 대장균 크기의 편차(YEAR_DEV), 대장균 개체의 ID(ID) 를 출력하는 SQL 문을 작성해주세요.
  분화된 연도별 대장균 크기의 편차는 분화된 연도별 가장 큰 대장균의 크기 - 각 대장균의 크기로 구하며 결과는 연도에 대해 오름차순으로 정렬하고 같은 연도에 대해서는 대장균 크기의 편차에 대해 오름차순으로 정렬해주세요.
```SQL
SELECT YEAR(DIFFERENTIATION_DATE)        AS YEAR
     , MAX(SIZE_OF_COLONY) OVER(PARTITION BY(YEAR(DIFFERENTIATION_DATE))) - SIZE_OF_COLONY  AS YEAR_DEV
     , ID
  FROM ECOLI_DATA
 ORDER BY YEAR, YEAR_DEV
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
####  7. 조건에 맞는 아이템들의 가격의 총합 구하기
- ITEM_INFO 테이블에서 희귀도가 'LEGEND'인 아이템들의 가격의 총합을 구하는 SQL문을 작성해 주세요. 이때 컬럼명은 'TOTAL_PRICE'로 지정해 주세요.

```SQL
 SELECT SUM(PRICE) AS TOTAL_PRICE
   FROM ITEM_INFO
  WHERE RARITY LIKE 'LEGEND'
```

####  8. 물고기 종류 별 대어 찾기
- 물고기 종류 별로 가장 큰 물고기의 ID, 물고기 이름, 길이를 출력하는 SQL 문을 작성해주세요.
  물고기의 ID 컬럼명은 ID, 이름 컬럼명은 FISH_NAME, 길이 컬럼명은 LENGTH로 해주세요.
  결과는 물고기의 ID에 대해 오름차순 정렬해주세요.
  단, 물고기 종류별 가장 큰 물고기는 1마리만 있으며 10cm 이하의 물고기가 가장 큰 경우는 없습니다.

```SQL
SELECT A.ID         AS ID
     , B.FISH_NAME  AS FISH_NAME
     , A.LENGTH     AS LENGTH
  FROM FISH_INFO                A
  INNER JOIN FISH_NAME_INFO     B
          ON (B.FISH_TYPE = A.FISH_TYPE)
 WHERE (B.FISH_NAME, A.LENGTH) IN (SELECT B.FISH_NAME
                                        , MAX(A.LENGTH) AS LENGTH
                                     FROM FISH_INFO                 A
                                     LEFT OUTER JOIN FISH_NAME_INFO B
                                                  ON (B.FISH_TYPE = A.FISH_TYPE)
                                    GROUP BY B.FISH_NAME, A.FISH_TYPE)
 ORDER BY A.ID
```

####  9. 잡은 물고기 중 가장 큰 물고기의 길이 구하기
- FISH_INFO 테이블에서 잡은 물고기 중 가장 큰 물고기의 길이를 'cm' 를 붙여 출력하는 SQL 문을 작성해주세요.
  이 때 컬럼명은 'MAX_LENGTH' 로 지정해주세요.

```SQL
SELECT CONCAT(MAX(LENGTH), 'cm') AS MAX_LENGTH
  FROM FISH_INFO
```

####  10. 연도별 대장균 크기의 편차 구하기 
- 분화된 연도(YEAR), 분화된 연도별 대장균 크기의 편차(YEAR_DEV), 대장균 개체의 ID(ID) 를 출력하는 SQL 문을 작성해주세요.
  분화된 연도별 대장균 크기의 편차는 분화된 연도별 가장 큰 대장균의 크기 - 각 대장균의 크기로 구하며 결과는 연도에 대해 오름차순으로 정렬하고 같은 연도에 대해서는 대장균 크기의 편차에 대해 오름차순으로 정렬해주세요.

```SQL

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

#### 4.있었는데요 없었습니다
- 관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

```SQL
SELECT A.ANIMAL_ID
     , B.NAME
  FROM ANIMAL_INS   A
       INNER JOIN ANIMAL_OUTS   B
               ON ( A.ANIMAL_ID = B.ANIMAL_ID )
 WHERE A.DATETIME >= B.DATETIME
 ORDER BY A.DATETIME
```

#### 5. 오랜 기간 보호한 동물(1)
- 아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

```SQL
SELECT A.NAME
     , A.DATETIME
  FROM ANIMAL_INS   A
  LEFT OUTER JOIN ANIMAL_OUTS   B
               ON ( B.ANIMAL_ID = A.ANIMAL_ID )
 WHERE B.DATETIME IS NULL
 
 ORDER BY A.DATETIME ASC
 LIMIT 3
```

#### 6. 보호소에서 중성화한 동물
- 보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화1되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT A.ANIMAL_ID
     , B.ANIMAL_TYPE
     , A.NAME
  FROM ANIMAL_INS   A
  LEFT OUTER JOIN ANIMAL_OUTS   B
               ON ( B.ANIMAL_ID = A.ANIMAL_ID )
 WHERE A.SEX_UPON_INTAKE  LIKE 'Intact %'
   AND B.SEX_UPON_OUTCOME NOT LIKE 'Intact %'
   
 ORDER BY A.ANIMAL_ID
```

#### 7. 주문량이 많은 아이스크림들 조회하기
- 7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT A.FLAVOR
  FROM FIRST_HALF       A
       INNER JOIN JULY  B
               ON ( A.FLAVOR = B.FLAVOR )
 GROUP BY A.FLAVOR
 ORDER BY SUM(A.TOTAL_ORDER + B.TOTAL_ORDER) DESC LIMIT 3
```

#### 8. 5월 식품들의 총매출 조회하기
- FOOD_PRODUCT와 FOOD_ORDER 테이블에서 생산일자가 2022년 5월인 식품들의 식품 ID, 식품 이름, 총매출을 조회하는 SQL문을 작성해주세요. 이때 결과는 총매출을 기준으로 내림차순 정렬해주시고 총매출이 같다면 식품 ID를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT A.PRODUCT_ID
     , A.PRODUCT_NAME
     , A.PRICE * SUM(B.AMOUNT) AS TOTAL_SALES
  FROM FOOD_PRODUCT             A
  LEFT OUTER JOIN FOOD_ORDER    B
               ON ( B.PRODUCT_ID = A.PRODUCT_ID )
 WHERE B.PRODUCE_DATE LIKE '2022-05-%%'
 GROUP BY A.PRODUCT_ID
 ORDER BY TOTAL_SALES   DESC
        , A.PRODUCT_ID  ASC
```

#### 9. 그룹별 조건에 맞는 식당 목록 출력하기
- MEMBER_PROFILE와 REST_REVIEW 테이블에서 리뷰를 가장 많이 작성한 회원의 리뷰들을 조회하는 SQL문을 작성해주세요. 회원 이름, 리뷰 텍스트, 리뷰 작성일이 출력되도록 작성해주시고, 결과는 리뷰 작성일을 기준으로 오름차순, 리뷰 작성일이 같다면 리뷰 텍스트를 기준으로 오름차순 정렬해주세요.

```SQL

```
--------------------------------------------------------------------------------------------------------------------------------------------------------

### IS NULL
#### 1. 경기도에 위치한 식품창고 목록 출력하기
- FOOD_WAREHOUSE 테이블에서 경기도에 위치한 창고의 ID, 이름, 주소, 냉동시설 여부를 조회하는 SQL문을 작성해주세요. 이때 냉동시설 여부가 NULL인 경우, 'N'으로 출력시켜 주시고 결과는 창고 ID를 기준으로 오름차순 정렬해주세요.
```SQL
SELECT WAREHOUSE_ID
     , WAREHOUSE_NAME
     , ADDRESS
     , (CASE WHEN FREEZER_YN IS NULL THEN 'N'
            ELSE FREEZER_YN 
            END)                AS FREEZER_YN
  FROM FOOD_WAREHOUSE
 WHERE ADDRESS LIKE '%경기도%'
 ORDER BY WAREHOUSE_ID
```

#### 2. 이름이 없는 동물의 아이디
- 동물 보호소에 들어온 동물 중, 이름이 없는 채로 들어온 동물의 ID를 조회하는 SQL 문을 작성해주세요. 단, ID는 오름차순 정렬되어야 합니다.

```SQL
SELECT ANIMAL_ID
  FROM ANIMAL_INS
 WHERE NAME IS NULL
 ORDER BY ANIMAL_ID
```

#### 3. 이름이 있는 동물의 아이디
- 동물 보호소에 들어온 동물 중, 이름이 있는 동물의 ID를 조회하는 SQL 문을 작성해주세요. 단, ID는 오름차순 정렬되어야 합니다.

```SQL
SELECT ANIMAL_ID
  FROM ANIMAL_INS
 WHERE NAME IS NOT NULL
 ORDER BY ANIMAL_ID
```

#### 4. NULL 처리하기
- 입양 게시판에 동물 정보를 게시하려 합니다. 동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 "No name"으로 표시해 주세요.

```SQL
SELECT ANIMAL_TYPE
     , CASE WHEN NAME IS NULL THEN 'No name'
            ELSE NAME
            END
     , SEX_UPON_INTAKE
  FROM ANIMAL_INS
 ORDER BY ANIMAL_ID
```

#### 5. 나이 정보가 없는 회원 수 구하기
- USER_INFO 테이블에서 나이 정보가 없는 회원이 몇 명인지 출력하는 SQL문을 작성해주세요. 이때 컬럼명은 USERS로 지정해주세요.

```SQL
SELECT COUNT(USER_ID) AS USERS
  FROM USER_INFO
 WHERE AGE IS NULL
```

#### 6. ROOT 아이템 구하기
- ROOT 아이템을 찾아 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME)을 출력하는 SQL문을 작성해 주세요. 이때, 결과는 아이템 ID를 기준으로 오름차순 정렬해 주세요.

```SQL
SELECT A.ITEM_ID
     , A.ITEM_NAME
  FROM ITEM_INFO            A
  LEFT OUTER JOIN ITEM_TREE B
               ON (B.ITEM_ID = A.ITEM_ID)
 WHERE B.PARENT_ITEM_ID IS NULL
 ORDER BY A.ITEM_ID
```

#### 7. 업그레이드 할 수 없는 아이템 구하기
- 더 이상 업그레이드할 수 없는 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력하는 SQL 문을 작성해 주세요. 이때 결과는 아이템 ID를 기준으로 내림차순 정렬해 주세요.

```SQL
SELECT A.ITEM_ID
     , A.ITEM_NAME
     , A.RARITY
  FROM ITEM_INFO                A
  LEFT OUTER JOIN ITEM_TREE     B
               ON (B.PARENT_ITEM_ID = A.ITEM_ID)
 WHERE B.PARENT_ITEM_ID IS NULL
 ORDER BY A.ITEM_ID DESC
```

#### 8. 잡은 물고기의 평균 길이 구하기
- 잡은 물고기의 평균 길이를 출력하는 SQL문을 작성해주세요.
  평균 길이를 나타내는 컬럼 명은 AVERAGE_LENGTH로 해주세요.
  평균 길이는 소수점 3째자리에서 반올림하며, 10cm 이하의 물고기들은 10cm 로 취급하여 평균 길이를 구해주세요.

```SQL
SELECT ROUND(AVG(CASE WHEN LENGTH IS NULL THEN 10
                      ELSE LENGTH
                      END
            ), 2)     AS AVERAGE_LENGTH
  FROM FISH_INFO
```

#### 9. 
- 

```SQL

```

--------------------------------------------------------------------------------------------------------------------------------------------------------

### String, Date
#### 1. 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기
- USED_GOODS_BOARD와 USED_GOODS_FILE 테이블에서 조회수가 가장 높은 중고거래 게시물에 대한 첨부파일 경로를 조회하는 SQL문을 작성해주세요.
  첨부파일 경로는 FILE ID를 기준으로 내림차순 정렬해주세요. 기본적인 파일경로는 /home/grep/src/ 이며, 게시글 ID를 기준으로 디렉토리가 구분되고, 파일이름은 파일 ID, 파일 이름, 파일 확장자로 구성되도
  록 출력해주세요. 조회수가 가장 높은 게시물은 하나만 존재합니다.
```SQL
SELECT CONCAT('/home/grep/src/', BOARD_ID, '/', FILE_ID, FILE_NAME, FILE_EXT) AS FILE_PATH
  FROM USED_GOODS_FILE
 WHERE BOARD_ID = (SELECT BOARD_ID
                     FROM USED_GOODS_BOARD
                    ORDER BY VIEWS DESC
                    LIMIT 1)
 ORDER BY FILE_ID DESC
```

#### 2. 루시와 엘라 찾기
- 동물 보호소에 들어온 동물 중 이름이 Lucy, Ella, Pickle, Rogan, Sabrina, Mitty인 동물의 아이디와 이름, 성별 및 중성화 여부를 조회하는 SQL 문을 작성해주세요.
```SQL
SELECT ANIMAL_ID
     , NAME
     , SEX_UPON_INTAKE
  FROM ANIMAL_INS
 WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
```

#### 3. 이름에 el이 들어가는 동물 찾기
- 보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다. 이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다. 동물 보호소에 들어온 동물 이름 중, 이름에 "EL"이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 이름 순으로 조회해주세요. 단, 이름의 대소문자는 구분하지 않습니다.
```SQL
SELECT ANIMAL_ID
     , NAME
  FROM ANIMAL_INS
 WHERE NAME LIKE '%el%'
   AND ANIMAL_TYPE = 'DOG'
 ORDER BY NAME
```

#### 4. 중성화 여부 파악하기
- 보호소의 동물이 중성화되었는지 아닌지 파악하려 합니다. 중성화된 동물은 SEX_UPON_INTAKE 컬럼에 'Neutered' 또는 'Spayed'라는 단어가 들어있습니다.
  동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 중성화가 되어있다면 'O', 아니라면 'X'라고 표시해주세요.
```SQL
SELECT ANIMAL_ID
     , NAME
     , CASE WHEN SEX_UPON_INTAKE LIKE '%Neutered%' THEN 'O'
            WHEN SEX_UPON_INTAKE LIKE '%Spayed%'   THEN 'O'
            ELSE 'X'
            END
       AS '중성화'
  FROM ANIMAL_INS
 ORDER BY ANIMAL_ID
```

#### 5. 오랜 기간 보호한 동물(2)
- 입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.
```SQL
SELECT A.ANIMAL_ID
     , A.NAME
  FROM ANIMAL_OUTS                  A
  JOIN (SELECT B1.ANIMAL_ID                          AS ANIMAL_ID
             , DATEDIFF(B1.DATETIME, A1.DATETIME)    AS PERIOD
          FROM ANIMAL_INS               A1
          LEFT OUTER JOIN ANIMAL_OUTS   B1
                       ON (B1.ANIMAL_ID = A1.ANIMAL_ID)
         ORDER BY PERIOD DESC
         LIMIT 2)                   B
    ON (B.ANIMAL_ID = A.ANIMAL_ID)
```

#### 6. 카테고리 별 상품 개수 구하기
- PRODUCT 테이블에서 상품 카테고리 코드(PRODUCT_CODE 앞 2자리) 별 상품 개수를 출력하는 SQL문을 작성해주세요. 결과는 상품 카테고리 코드를 기준으로 오름차순 정렬해주세요.

```SQL
SELECT LEFT(PRODUCT_CODE, 2) AS CATEGORY
     , COUNT(PRODUCT_CODE) AS PRODUCTS
  FROM PRODUCT
 GROUP BY CATEGORY
```

#### 7. 특정 옵션이 포함된 자동차 리스트 구하기
- CAR_RENTAL_COMPANY_CAR 테이블에서 '네비게이션' 옵션이 포함된 자동차 리스트를 출력하는 SQL문을 작성해주세요. 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

```SQL
SELECT *
  FROM CAR_RENTAL_COMPANY_CAR
 WHERE OPTIONS LIKE '%네비게이션%'
 ORDER BY CAR_ID DESC
```
