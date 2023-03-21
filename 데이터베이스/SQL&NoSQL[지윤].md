## **관계형 데이터베이스(RDBMS, SQL Database)**

-   고정된 행(row)과 열(column)로 구성된 테이블에 데이터를 저장
-   테이블의 관계가 구조화된 데이터의 모음이기 때문에 구조화된 쿼리 언어(SQL)를 사용하여 데이터를 다룸
-   각 열은 하나의 속성에 대한 정보를 저장하고, 행에는 각 열의 데이터 형식에 맞는 데이터가 저장
-   테이블의 구조와 데이터 타입 등을 사전에 정의한 후 그 형식을 유지하며 데이터 조작
-   테이블 간의 관계를 직관적으로 파악할 수 있음

**장점**

-   명확하게 정의된 스키마, 데이터 무결성 보장
-   관계는 각 데이터를 중복없이 한번만 저장

**단점**

-   데이터 스키마를 변경하기 어려워 유연성이 떨어짐
-   관계를 맺고 있어서 Join 문이 많은 복잡한 쿼리가 만들어질 수 있음
-   대체로 수직적 확장만 가능함

</br></br>

## **비관계형 데이터베이스(NoSQL Database)**

-   Key-Value, 문서 등 관계형 데이터베이스와는 다른 방식으로 데이터를 저장
-   테이블 구조 및 데이터 저장 형식에 대한 제한이 없음
-   테이블 간 관계를 정의하지 않음

**장점**

-   스키마가 없어서 유연함. 언제든지 저장된 데이터를 조정하고 새로운 필드 추가 가능
-   데이터는 애플리케이션이 필요로 하는 형식으로 저장되어 데이터를 읽어오는 속도가 빨름
-   수직 및 수평 확장이 가능해서 애플리케이션이 발생시키는 모든 읽기/쓰기 요청 처리 가능

**단점**

-   유연성으로 인해 데이터 구조 결정을 미루게 될 수 있음
-   데이터 중복이 발생할 수 있으며, 중복된 데이터가 변경 될 경우 모든 컬렉션에 대해 수정해주어야 함

</br>

### **NoSQL Database 유형**

-   **Key-Value 타입 데이터베이스**
    -   속성과 값에 대해 Key-Value 쌍으로 나타내어 데이터를 배열의 형태로 저장
    -   Ex) Redis, Dynamo 등
-   **문서형(Document) 데이터베이스**
    -   데이터를 테이블이 아닌 JSON과 유사한 형식으로 문서화하여 저장
    -   각각의 문서는 하나의 속성에 대한 데이터를 가지고 있고, 컬렉션이라고 하는 그룹으로 묶어서 관리
    -   Ex) MongoDB 등
-   **Wide-Column Store 데이터베이스**
    -   데이터베이스의 열(column)에 대한 데이터를 집중적으로 관리하는 데이터베이스
    -   각 열에는 key-value 형식으로 데이터가 저장되고, 컬럼 패밀리(column families)라고 하는 열의 집합체 단위로 데이터를 처리
    -   하나의 행에 많은 열을 포함할 수 있어서 유연성이 높으며, 데이터 처리에 필요한 열을 유연하게 선택할 수 있어 규모가 큰 데이터 분석에 주로 사용
    -   Ex) Cassandra, HBase 등
-   **그래프(Graph) 데이터베이스**
    -   자료구조의 그래프와 비슷한 형식으로 데이터 간의 관계를 구성하는 데이터베이스
    -   노드(nodes)에 속성별(entities)로 데이터를 저장하고, 각 노드 간의 관계는 선(edge)으로 표현
    -   Ex) Neo4J, InfiniteGraph

</br></br>

## **SQL Database VS NoSQL Database**

![image](https://user-images.githubusercontent.com/64777557/226547803-56f8b73d-b0e6-4d76-ab91-3c937befd16e.png)

-   **데이터 저장(Storage)**
    -   SQL DB는 SQL을 활용해 미리 작성된 스키마를 기반으로 정해진 형식에 따라 테이블에 데이터 저장
    -   NoSQL DB는 key-value, document, wide-column, graph 등의 방식으로 데이터를 저장 
-   **스키마(Schema)**
    -   SQL DB는 데이터 속성별로 열(column)에 대한 정보를 정해 미리 정해 고정된 형식의 스키마가 필요
    -   NoSQL DB는 동적으로 스키마의 형태를 관리
-   **쿼리(Query)**
    -   SQL DB는 SQL과 같이 구조화된 쿼리 언어를 사용go 테이블의 형식과 테이블간의 관계에 맞춰 데이터를 요청
    -   NoSQL DB는 구조화 되지 않은 쿼리 언어로도 데이터 요청이 가능
-   **확장성(Scalability)**
    -   SQL DB은 주로 수직적 확장(높은 성능의 메모리 및 CPU 사용)만 용이
    -   NoSQL DB는 수직적 확장 뿐만 아니라 수평적 확장(서버 증설, 클라우드 서비스 이용)에도 유리

</br>

### **SQL 기반의 관계형 데이터베이스를 사용해야 하는 케이스**

-   데이터 구조가 명확하며 변경 될 여지가 없으며 명확한 스키마가 중요한 경우
-   관계를 맺고 있는 데이터가 자주 변경되는 경우
    -   중복된 데이터가 없어(데이터 무결성) 변경이 용이하기 때문

</br>

### **NoSQL 기반의 ql관계형 데이터베이스를 사용해야 하는 케이스**

-   정확한 데이터 구조를 알 수 없거나 변경 및 확장 될 수 있는 경우
-   데이터 읽기를 자주 하지만, 데이터 변경은 자주 없는 경우
-   많은 양의 데이터를 다뤄야 하는 경우
    -   데이터베이스를 수평적으로 확장할 수 있기 때문

</br></br>

### **⭐️ 참고**

-   [https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-sql-vs-nosql/](https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-sql-vs-nosql/)
-   [https://khj93.tistory.com/entry/Database-RDBMS%EC%99%80-NOSQL-%EC%B0%A8%EC%9D%B4%EC%A0%90](https://khj93.tistory.com/entry/Database-RDBMS%EC%99%80-NOSQL-%EC%B0%A8%EC%9D%B4%EC%A0%90)
-   [https://gyoogle.dev/blog/computer-science/data-base/SQL%20&%20NOSQL.html](https://gyoogle.dev/blog/computer-science/data-base/SQL%20&%20NOSQL.html)
