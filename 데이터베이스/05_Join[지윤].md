## **조인(Join)**

-   **한 데이터베이스 내의 여러 테이블의 레코드를 조합하여 하나의 열로 표현한 것\\**
-   **데이터베이스에는 서로 관계있는 데이터가 여러 테이블로 나뉘어 저장되있어 조인을 통해 각 테이블을 연결하여 효과적으로 검색**
-   **두 테이블의 조인을 위해서는**기본키(PRIMARY KEY, PK)와 외래키(FOREIGN KEY, FK) **관계로 맺어져야 함**

</br>

#### **조인의 종류**

-   **내부 조인(INNER JOIN)**: 두 테이블을 조인할 때, 두 테이블에 모두 지정한 열의 데이터가 있어야 함
-   **외부 조인(OUTER JOIN)**: 두 테이블을 조인할 때, 1개의 테이블에만 데이터가 있어도 결과가 나옴
-   **상호 조인(CROSS JOIN)**: 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 조인하는 기능
-   **자체 조인(SELF JOIN)**: 자신이 자신과 조인한다는 의미로, 1개의 테이블을 사용

</br>

※ 샘플 데이터베이스 구조는 다음과 같음

![image](https://user-images.githubusercontent.com/64777557/227437639-bad834b1-5b11-48e0-b4cb-4438b855e93e.png)

</br></br>

### **내부 조인(INNER JOIN)**

![image](https://user-images.githubusercontent.com/64777557/227437616-8829c1cb-1866-4d26-8c34-2c3bf2557965.png)

-   가장 흔한 결합 방식이며 기본 조인 형식으로 간주
-   공통적인 부분만 SELECT

#### **명시적 조인 표현(explicit)**

-   Join 키워드와 함께 On 키워드 사용

```
SELECT *
FROM STUDENT_TABLE
INNER JOIN DEPARTMENT_TABLE
ON STUDENT_TABLE.DEPARTMENT_ID = DEPARTMENT_TABLE.DEPARTMENT_ID;
```

#### **암시적 조인 표현(implicit)**

-   From 절에서 테이블을 분리하는 컴마(,)를 사용

```
SELECT *
FROM STUDENT_TABLE, DEPARTMENT_TABLE
WHERE STUDENT_TABLE.DEPARTMENT_ID = DEPARTMENT_TABLE.DEPARTMENT_ID;
```

</br></br>

### ****외부 조인(OUTER JOIN)****

![image](https://user-images.githubusercontent.com/64777557/227437674-44c939fb-dc54-40d9-b474-807d93e54d95.png)

-   조인 대상 테이블에서 특정 테이블의 데이터가 모두 필요한 상황에서 효과적으로 결과 집합을 생성
-   두 테이블이 가지고 있는 전체 부분 SELECT

#### **왼쪽 외부 조인(LEFT OUTER JOIN)**

-   왼쪽 테이블의 모든 데이터를 포함하는 결과 집합을 생성

```
# 공통부분 포함
SELECT *
FROM STUDENT_TABLE S LEFT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID;

# 공통부분 불포함
SELECT *
FROM STUDENT_TABLE S LEFT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE D.DEPARTMENT_ID IS NULL;
```

#### **오른쪽 외부 조인(RIGHT OUTER JOIN)**

-   테이블의 모든 데이터를 포함하는 결과 집합을 생성

```
# 공통부분 포함
SELECT *
FROM STUDENT_TABLE S RIGHT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID;

# 공통부분 불포함
SELECT *
FROM STUDENT_TABLE S RIGHT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE S.DEPARTMENT_ID IS NULL;
```

#### **완전 외부 조인(FULL OUTER JOIN)**

-   두 테이블의 모든 데이터를 포함하는 결과 집합을 생성

```
# 공통부분 포함
-- ORACLE
SELECT *
FROM STUDENT_TABLE S FULL OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID;
-- MYSQL
SELECT *
FROM STUDENT_TABLE S LEFT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
UNION
SELECT *
FROM STUDENT_TABLE S RIGHT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID;

# 공통부분 불포함
-- ORACLE
SELECT *
FROM STUDENT_TABLE S FULL OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE S.DEPARTMENT_ID IS NULL OR D.DEPARTMENT_ID IS NULL;
-- MYSQL
SELECT *
FROM STUDENT_TABLE S LEFT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE D.DEPARTMENT_ID IS NULL
UNION
SELECT *
FROM STUDENT_TABLE S RIGHT OUTER JOIN DEPARTMENT_TABLE D
ON S.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE S.DEPARTMENT_ID IS NULL;
```

</br>

### **상호 조인(CROSS JOIN)**

![image](https://user-images.githubusercontent.com/64777557/227437695-15bcfb88-8f69-4475-af24-1e42f578c181.png)

-   한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 결합시켜 모두 SELECT
-   카티션 곱(CARTESIAN PRODUCT)이라고도 함

```
SELECT *
FROM STUDENT_TABLE S CROSS JOIN DEPARTMENT_TABLE D
```
