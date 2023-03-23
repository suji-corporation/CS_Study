## **데이터베이스 기초 용어**

![image](https://user-images.githubusercontent.com/64777557/227129452-88d77a2e-d2a0-4f3c-a053-3a1df8022f22.png)

-   **릴레이션(Relation)**
    -   관계형 데이터 모델에서 하나의 개체에 관한 데이터를 저장하기 위한 구조 (= 테이블)
    -   스키마와 인스턴스로 이루어짐
-   **스키마(Schema)**
    -   릴레이션의 이름, 각 속성의 이름, 타입, 도메인 등을 정의하는 개념
-   **인스턴스(Instance)**
    -   릴레이션에 저장되는 데이터(튜플)의 집합
-   **속성(Attribute)**
    -   릴레이션의 열(Column)
    -   서로 다른 이름을 가져야 함
-   **튜플(Tuple) / 레코드(Record)**
    -   릴레이션의 행(Row)
    -   중복 불가
-   **차수(Degree)**
    -   하나의 릴레이션에서 속성의 전체 개수
-   **카디널리티(Cardinality)**
    -   하나의 릴레이션에서 튜플의 전체 개수
-   **도메인(Domain)**
    -   릴레이션의 속성이 가질 수 있는 원자값들의 집합

</br></br>

## **키(Key)**
-   릴레이션의 튜플을 식별할 수 있는 단일 속성 혹은 속성들의 집합
-   유일성과 최소성에 따라 키의 종류가 달라짐

#### **유일성**
-   하나의 키값으로 튜플을 유일하게 식별할 수 있는 성질
-   혈액형, 나이 (X) | 주민번호, 학번 (0)

#### **최소성**

-   키를 구성하는 속성들 중 가장 최소로 필요한 속성들로만 키를 구성하는 성질
-   키를 구성하고 있는 속성들이 각 튜플을 구분하는 데 꼭 필요한 속성들로만 구성되어 있는지 확인
-   \[주민번호+나이\] (X) | \[주민번호\] (X)
    -   나이를 키로 설정하지 않아도 주민번호 만으로도 유일성을 만족함

</br></br>

### **키 종류**

<img src="https://user-images.githubusercontent.com/64777557/227129489-077a273d-43a0-4402-bcfa-6900d8a1feac.png" width="600">

#### **슈퍼 키(Super Key)**

![image](https://user-images.githubusercontent.com/64777557/227129543-12f256d9-33ea-401b-94f1-8e7fba981f2e.png)

-   슈퍼키는 유일성의 특성을 만족하는 속성 또는 속성들의 집합
-   어떤 속성을 결합하든 튜플을 식별할 수 있으며 중복 튜블이 발생할 수 없는 경우
-   최소성은 만족하지 않아도 됨
-   Ex) \[학번\], \[주민번호\], \[학번+이름\], \[주민번호+나이\] 등

</br>

#### **후보 키(Candidate Key)**

![image](https://user-images.githubusercontent.com/64777557/227129575-17d51f07-705b-4733-a535-bfbb3b5cab47.png)

-   유일성과 최소성을 모두 만족하는 속성 또는 속성들의 집합
-   최소의 속성을 결합하여 튜플을 식별할 수 있어야 함
-   Ex) \[학번\], \[주민번호\]

</br>

#### **기본 키(Primary Key)**

![image](https://user-images.githubusercontent.com/64777557/227129602-736a51a7-1fb2-4ff2-8ea1-8c723df810ce.png)

-   유일성과 최소성을 만족하는 후보 키 중 다음을 고려하여 하나를 선택한 키
    -   NULL 값을 가질 수 없는 속성이 포함된 후보 키
    -   값이 자주 변경될 수 없는 속성이 포함된 후보 키
    -   단순한 후보 키
-   Ex) \[학번\]

</br>

#### **대체 키(Alternate Key)**

![image](https://user-images.githubusercontent.com/64777557/227129647-1536af6d-7f78-4199-a913-9df032c211eb.png)

-   후보 키가 두개 이상일 경우 키본키로 하나를 지정하고 남은 후보 키 집합
-   보조 키라고도 함
-   Ex) \[주민번호\]

</br>

#### **외래 키(Foreign Key)**

<img src="https://user-images.githubusercontent.com/64777557/227129681-5efe93e3-6a46-440b-9d60-2278434cb986.png" width="600">

-   다른 릴레이션의 기본 키를 그대로 참조하는 속성 또는 속성들의 집합
-   릴레이션 간의 관계를 나타내기 위해 사용
-   외래 키에는 참조 테이블의 기본 키에 존재하지 않는 값을 참조할 수 없음. (참조 무결성 조건)
    -   데이터 추가/삭제 시 항상 외래 키가 참조한 값은 참조 테이블에 존재하도록 고려해야 함
-   Ex) 외래 키: 주문 테이블의 '주문고객' 속성 | 참조 키: 고객 테이블의 '고객ID'

</br></br>

### **⭐️ 참고**

-   [https://velog.io/@yun8565/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%82%A4Key-%EC%A0%95%EB%A6%AC](https://velog.io/@yun8565/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%82%A4Key-%EC%A0%95%EB%A6%AC)
-   [https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%ED%82%A4KEY-%EC%A2%85%EB%A5%98-%F0%9F%95%B5%EF%B8%8F-%EC%A0%95%EB%A6%AC#recentComments](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%ED%82%A4KEY-%EC%A2%85%EB%A5%98-%F0%9F%95%B5%EF%B8%8F-%EC%A0%95%EB%A6%AC#recentComments)
-   [https://ggop-n.tistory.com/78](https://ggop-n.tistory.com/78)
