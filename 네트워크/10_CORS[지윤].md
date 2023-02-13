## **SOP(Same Origin Policy)**

-   같은 출처의 HTTP 요청만을 허락하는 정책
-   자원을 요청한 출처와 해당 요청에 응답하는 서버의 출처가 다를 경우 해당 자원을 사용하지 못하도록 제한 -> CORS 정책을 지켰을 시에만 예외적으로 허용
-   SOP 정책이 없을 시 노출된 소스코드를 통해 `CSRF(Cross-Site Request Forgery)`나 `XSS(Cross-Site Scripting)`와 같은 방법으로 공격자의 사용자 정보 탈취 위험이 있음

</br>

#### **※ 출처(Origin)란?**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FXWY7d%2FbtrY7LKgjvJ%2FvSMTMXl4fsYAbEIGHpVSY1%2Fimg.png" width=700>

위와 같은 URL 구조에서 `Protocol + Host + Port`에 해당하는 부분을 뜻한다. 브라우저는 Protocol, Host, Port가 모두 같다면 같은 출처, 셋 중 하나라도 다르다면 다른 출처라고 판단한다.

</br></br>

## **CORS(Cross-Origin Reource Sharing)**

-   교차 출처(다른 출처) 리소스 공유
-   서버측에서 CORS 헤더를 통해서 다른 출처(웹)에서 자원에 접근할 수 있는 권한을 부여하도록 브라우저에게 알려주는 정책
-   출처가 다른 요청이더라도 CORS 헤더를 통해 CORS 정책을 잘 지켰다면 제대로 응답하도록 허용

</br>

### **CORS 동작 방식 (CORS 위반 여부 확인 방법)**

1.  웹 클라이언트 애플리케이션은 HTTP 프로토콜을 통해 다른 출처의 리소스 요청
2.  브라우저는 요청 헤더의 `Origin`이라는 필드에 요청을 보내는 출처를 추가로 담아서 보냄
3.  서버는 응답 헤더에 `Access-Control-Allow-Origin(리소스에 접근이 허용된 출처)`를 담아서 보냄
4.  브라우저는 요청 시 보낸 `Origin`과 응답 헤더 속의 `Access-Control-Allow-Origin`을 비교
    -   `Access-Control-Allow-Origin에 Origin`이 포함된다면 클라이언트로 응답 전송
    -   포함되지 않는다면 브라우저는 해당 응답을 버리고 CORS 정책 위반 메시지를 console에 출력

</br>

### **CORS 시나리오**

CORS의 구체적인 동작 방식은 아래 세 가지의 시나리오에 따라 변경된다. 요청이 어떤 시나리오를 따르는 지에 따라 해결 방법도 달라진다.

#### **1\. Preflight Request**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbUkZOt%2FbtrYYpnLG1N%2FwPtulrAjRIyoYyhDtx2am1%2Fimg.png" width=800>

-   브라우저는 요청을 한번에 보내지 않고 예비 요청(prefight)과 본 요청으로 나누어 두 번 요청 전송
-   예비 요청에는 HTTP 메소드 중 `OPTIONS` 메소드 사용
-   브라우저는 예비 요청으로 통해 본 요청이 안전하게 전달될 수 있을 지 스스로 확인
    -   Javascript의 Fetch API 등을 활용해 브라우저에게 리소스를 받아오라는 명령을 보냄
    -   이에 대해 브라우저는 서버에게 미리 예비 요청을 보냄
    -   서버는 이에 대한 응답 헤더에 현재 자신이 어떤 것(메소드/헤더)를 허용하고 어떤 것을 금지하는지에 대한 정보를 담아 전송
    -   브라우저는 자신이 보낸 예비 요청과 응답 헤더의 허용 정책을 비교한 후 해당 요청이 안전하다고 판단되면 같은 요청(본 요청)을 다시 보냄

#### **2\. Simple Request**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpNy5v%2FbtrYYoWLdwv%2F52nvkm4mmDGqdK4eJCYJ61%2Fimg.png" width=800>

-   예비 요청을 보내지 않고 서버에게 바로 본 요청을 전송한 후 서버가 이에대한 응답 헤더에 `Access-Control-Allow-Origin`을 보내면 이때 브라우저가 CORS 정책 위반 여부를 검사하는 방식
-   Prefight Request와 유사하지만 예비 요청 유무만 다름
-   예비 요청을 생략할 수 있는 경우는 정해져 있음
    -   요청 메소드는 `GET`, `HEAD`, `POST` 중 하나여야 함
    -   `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`를 제외한 헤더는 사용하면 안됨
    -   만약 `Content-Type`를 사용한다면 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용

#### **3\. Credentialed Request**

-   인증된 요청을 사용하는 방법
-   다른 출처 간 통신에서 좀 더 보안을 강화하기 위해 사용
-   `credentials`이라는 옵션을 통해 요청 헤더에 인증과 관련한 정보를 담을 수 있도록 함
    -   `same-origin(기본값)`: 같은 출처 간 요청에만 인증 정보를 담을 수 있다
    -   `include`: 모든 요청에 인증 정보를 담을 수 있다
    -   `omit`: 모든 요청에 인증 정보를 담지 않는다
-   `credentials` 옵션에 `same-origin`이나 `include`와 같은 옵션을 사용하여 인증 정보가 포함되었다면 브라우저는 다른 출처의 리소스 요청 시 `Access-Control-Allow-Origin` 확인 뿐만 아니라 추가적인 검사 조건을 추가
    -   서버 응답 헤더에는 `Access-Control-Allow-Origin`의 값으로 와일드카드(\*)를 사용할 수 없으며 명시적인 URL을 작성해야한다
    -   서버 응답헤더에 `Access-Control-Allow-Credentials`라는 헤더와 그 값아 true로 설정되어 있어야한다

</br>

### **CORS 해결 방법**

-   **서버에 `Access-Control-Allow-Origin` 세팅하기**
    -   `Access-Control-Allow-Origin: \*`는 모든 출처에서 오는 요청을 허용한다는 뜻으로 당장은 편할지라도 보안상 좋지 않음
    -   `Access-Control-Allow-Origin: https://J1Yun.github.com`과 같이 허용 출처를 명확히 적어주는 것이 좋음
-   **프론트에서 `Webpack Dev Server`로 리버스 프록싱하기**
    -   웹팩과 `webpack-dev-server`에서 제공하는 프록시 기능을 활용하여 CORS 정책 우회
