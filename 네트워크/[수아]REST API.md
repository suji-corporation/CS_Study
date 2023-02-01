## REST란? 

**REST** : Representational State Transfer    
→ 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것 

- 인터넷과 같은 복잡한 네트워크에서 통신을 관리하기 위한 지침으로 탄생
- 대규모의 고성능 통신 안정적으로 지원할 수 있음
- 쉽게 구현하고 수정할 수 있음 → 모든 api 시스템 파악 & 여러 플랫폼에서 사용 가능
- 웹의 기존 기술 & http 프로토콜 사용

1. `HTTP URI`를 통해 자원을 명시하고 
2. `HTTP Method(POST, GET, PUT, DELETE, PATCH 등)`를 통해 
3. 해당 자원(URI)에 대한 `CRUD Operation`을 적용하는 것 

</br>

## REST 구성 요소

1. `HTTP URI` : 자원 
    - 모든 자원은 고유 ID를 갖고 있고, 자원은 server에 존재
    - 자원을 구별하는 ID == HTTP URI
    - client는 uri를 이용해서 자원을 지정하고, 자원의 상태에 대한 조작을 server에 요청
2. `HTTP METHOD` : 행위 
    - HTTP 프로토콜의 method사용
      - GET, POST, PUT, DELETE 등
3. `표현`(Representation of Resource) 
    - JSON, XML, TEST, RSS : Representaion
    - 하나의 자원은 여러 형태의 representation으로 나타낼 수 있음
    - client가 자원의 상태에 대한 조작 server에 요청하면 representaion 반환

</br>

## REST 특징

1. `Server-client 구조` 
    - server : api 제공, 비즈니스 로직 처리 및 저장, 자원이 있는 쪽
    - client : 사용자 인증, context(세션, 로그인 정보) 등 관리, 자원 요청하는 쪽
    - 서로 간 의존성 ⬇️
2. `Stateless(무상태)`
    - http protocol == stateless protocol → REST == stateless
    - client의 context를 server에 저장 ❌ → 구현이 단순해짐(세션이나 쿠키같은 정보 신경쓰지 않아도 되기 때문) 
    - server는 각각의 요청 별개의 것으로 인식
        - client 요청만 단순 처리
        - DB에 의해 바뀌는 것은 허용
        - server의 처리 방식에 일관성 부여, 부담 ⬇️, 서비스 자유도 ⬆️
3. `Cacheable(캐시 처리 가능)`
    - 웹에서 사용하는 기존의 인프라 그대로 활용 가능 → 그 중 하나가 caching
    - 대량의 요청을 효율적으로 처리하기 위해서 캐시 사용
    - 캐시 사용 → 응답 빨라짐, 전체 응답시간, 성능, 서버의 자원 이용률 ⬆️
4. `Layered System(계층화)` 
    - client는 REST API server만 호출
    - REST server는 다중 계층으로 구성
      - API server는 순수 비즈니스 로직 수행 
      - 그 앞에 보안, 로드밸런싱, 암호화 등을 추가하여 구조상 유연성 줄 수 있음 
5. `Code-On-Demand (optional)`
    - server에서 스크립트를 받아서 client에서 실행
    - 꼭 충족할 필요는 없음
6. `Uniform Interface(인터페이스 일관성)`
    - resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행
    - http protocol 따르는 모든 플랫폼에서 사용 가능 (언어/기술에 종속 ❌)

</br>

## REST 장점 & 단점 

|장점|단점|
|---|---|
|HTTP 프로토콜 그대로 사용 → REST API를 위한 별도의 인프라 구축할 필요 ❌|표준 존재❌|
|HTTP 표준 프로토콜 따르는 모든 플랫폼에서 사용 가능|사용할 수 있는 Method 4가지|
|의도하는 바를 쉽게 파악할 수 있음(REST API의 메시지 명확하기 때문) |구형 브라우저가 지원해주지 못하는 부분 존재 
|서버와 클라이언트 역할 명확하게 분리|쉽게 고칠 수 있는 URL보다 Header가 어렵게 느껴질 수 있음|

</br> 

## REST가 필요한 이유
다양한 클라이언트가 등장하고, 어플리케이션을 분리 및 통합하면서 최근 서버 프로그램들은 다양한 브라우저와 안드로이드, ios등 여러 디바이스들과 통신할 수 있어야 한다. 
이러한 머티 플랫폼에 대한 지원을 위해 서비스 지원에 대한 아키텍처를 만들고 이용하는 방법을 연구한 결과 REST에 관심 갖게 되었다. 

</br>

## REST API

- REST의 원리를 따라 API를 구현한 것
- 두 컴퓨터 시스템이 인터넷을 통해 정보를 안전하게 교환하기 위해 사용하는 `인터페이스`

### REESTful이란? 
- REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어 
- `REST API`를 제공하는 웹 서비스는 `RESTful`하다. 
- **목적** : 이해하기 쉽고 사용하기 쉬운 REST API 만드는 것
- `RESTful`하지 못한 경우 예시
  1. CRUD 기능을 모두 POST로만 처리하는 API
  2. route에 resource, id외의 정보가 들어가는 경우 

</br>

## REST API 특징

- 시스템 분산해서 확장성과 재사용성 높임    
    → 유지보수 및 운용 편리   
- http 지원하는 프로그램 언어로 client, server 구현 가능

</br>

## REST API 설계 규칙 

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
2. 마지막에 슬래시 (/)를 포함하지 않는다.
3. 슬래시는 계층 관계를 나타내는데 사용한다. 
4. 언더바( _ ) 대신 하이폰( - )을 사용한다. 
5. 파일확장자는 URI에 포함하지 않는다.
6. 행위를 포함하지 않는다.


</br>

> 참고 
> https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
> https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
