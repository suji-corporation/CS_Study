다른 출처로의 리소스 요청을 제한하는 2가지 정책 
1. CORS 
2. SOP 

### 📕 Origin(출처)이란? 
<img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/218381747-b7407a59-e4ed-4f39-b3d6-d3f92f9b48cf.png">

- protocol + host + port == **origin(출처)**
- 출처 : 서버의 위치를 찾아가기 위해 필요한 가장 기본적인 것들을 합쳐놓은 것 
- port는 생략 가능 
   - http, https의 기본 port 번호가 지정되어 있기 때문에
   - **BUT** 출처에 포트 번호가 명시되어 있다면 port까지 모두 일치해야 같은 출처로 인정됨 


❓ **출처 비교는 누가?**        
- 브라우저!
  - 출처를 비교하는 로직은 브라우저에 구현된 스펙 
  - 서버가 요청에 대한 응답을 잘 해줘도 브라우저가 분석했을 때 동일한 출처가 아니면 에러 발생 
  - 응답 데이터에는 문제가 없지만 브라우저가 차단한 것! 

</br>

## SOP 
- Same-Origin Policy 
- `같은 출처에서만 리소스를 공유할 수 있다`
- 동일 서버에 있는 리소스는 자유롭게 가져올 수 있지만 다른 서버의 리소스는 상호작용 불가능 


💡 **SOP이 필요한 이유**
- 출처가 다른 2개의 어플리케이션이 자유롭게 소통하는 것은 꽤 위험함 
- 해커가 개인 정보를 가로챌 수 있음 


</br>

## CORS 
- Cross-Origin Resource Sharing 정책 
- 다른 출처의 리소스 공유에 대한 허용/비허용 정책 
- CORS 정책을 허용하는 리소스에 한해 다른 출처이더라도 받아들이는 것 
- CORS error : 
  - SOP 정책에 따라 다른 출처의 리소스를 차단하면서 발생하는 에러 
  - CORS는 해결책! 
    - 서버 측에서 `Access-Control-Allow-Origin` 헤더에 허용할 출처를 기재하면 됨 
- `SOP을 위반해도 CORS를 따르면 다른 출처의 리소스라도 허용하겠다`

### ⚙️ CORS 기본 동작
1. 클라이언트에서 HTTP 헤더에 Origin 담아서 전달           
          
      <img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/218383941-5ba2074e-b4be-4bc0-975d-d302ac313879.png">

2. 서버는 응답 헤더에 Access-Control-Allow-Origin을 담아 클라이언트로 전달    
   - `Access-Control-Allow-Origin` : 이 리소스를 접근하는 것이 허용된 출처                                                       
                                                                            
      <img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/218385019-14cb9328-2dbd-4d7b-8bb0-4eff705d0359.png">

3. 클라이언트에서 자신이 보냈던 요청의 Origin과 서버가 보내준 Access-Control-Allow-Origin을 비교
  - 비교한 결과가 유효하지 않다면 해당 response를 사용하지 않고 버림 (CORS 에러) 
  - 유효하면 다른 출처의 리소스 가져오는데 문제 ❌

</br>

## CORS 작동 방식 시나리오 
### 1️⃣ Preflight Request
<img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/218386987-7f678088-a970-4971-95f8-3c811c24316e.png">    

- 브라우저는 본 요청을 보내기 전 예비 요청(preflight)을 전송 
- preflight 
  - http 메소드 중 OPTIONS 사용 
  - 본 요청 보내기 전 브라우저 스스로 해당 요청을 보내는게 안전할지 확인하는 작업 
- preflight를 보낸 후 받은 서버의 응답에서 `Access-Control-Allow-Origin`으로 허용되는 출처를 알려줌 
- 서버가 허용해준 origin과 preflight의 origin이 다르면 CORS error 


### 2️⃣ Simple Request 
<img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/218388565-a3f7f433-2b10-45ed-82b0-6faa7170dc24.png">

- 예비 요청 보내지 않고 본 요청 보낸 후 서버의 응답을 받고 CORS 정책 위반 여부 판단 
- 특정 조건을 만족하는 경우에만 예비 요청 생략 가능 
  1. 요청 메소드가 GET or HEAD or POST 
  2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안됨
  3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded, multipart/form-data, text/plain만 허용
- 사실상 모든 조건을 만족하는 것이 쉽지 않음 

### 3️⃣ Credentialed Request 

- 인증된 요청 사용하는 방식 
- 다른 출처 간 통신에 보안 강화하고 싶을 때 사용 
- `credentials` : 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션. 3가지 값 사용 가능 
  1. same-origin(기본값) : 같은 출처 간 요청에만 인증 정보를 담을 수 ⭕ 
  2. include : 모든 요청에 인증 정보 담을 수 ⭕
  3. omit : 모든 요청에 인증 정보 담을 수 ❌
- 요청에 인증 정보가 담겨있는 상태에서 다른 출처 리소스 요청하면 브라우저가 2가지 룰 더 추가해서 CORS 정책 위반 검사 
  1. `Access-Control-Allow-Origin`에는 `*`를 사용할 수 없으며, 명시적인 URL이어야 한다.
  2. 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야 한다. 

</br>

## CORS 에러 해결 방법 
### 1️⃣ Access-Control-Allow-Origin 세팅하기 
- 서버가 하는 일 
- `Access-Control-Allow-Origin` 헤더에 알맞은 값을 세팅하기 
- `*`를 사용하면 당장은 편할 수 있지만 보안적으로 심각한 문제 발생할 수 있음 
- Spring, Express, Django와 같이 이름있는 백엔드 프레임워크의 경우에는 모두 CORS 관련 설정을 위한 세팅이나 미들웨어 라이브러리를 제공

### 2️⃣ Webpack Dev Server로 리버스 프록싱하기
- 프론트가 하는 일 
- 나는 잘 몰라....ㅜㅜ

</br>
</br>

> 참고   
> https://evan-moon.github.io/2020/05/21/about-cors/       
> https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F

