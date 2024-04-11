# HTTP

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcdTMRr%2FbtseG58buH9%2FQaOFqg061fZjq6SzN2GJkK%2Fimg.png)

**HTTP 한줄 요약**

- 서버와 클라이언트간 하이퍼텍스트 기반으로 데이터를 송수신 하기 위해 사용하는 프로토콜

> ❓ **하이퍼텍스트(Hyper-Text)란?**
> 
> 텍스트의 일부가 다른 텍스트로 연결된 문서

---


## 📌 HTTP 특징

1. **Server-Client**
   - Client : `Request`를 전송하는 주체
   - Server : 받은 Request에 대한 처리 결과 `Response`를 보내는 주체

2. **Stateless**
   - Stateful vs Stateless : 서버가 클라이언트의 상태를 저장하는가? 라는 관점에 따라 나눠짐 (서버 관점)
       - Stateful : 서버가 클라이언트의 상태를 저장한다. (클라이언트의 존재를 안다.)
       - Stateless : 서버가 클라이언트의 상태를 저장하지 않는다. (클라이언트의 존재를 모른다.)
   - HTTP 은 기본적으로 클라이언트의 상태를 저장하지 않는다 (Stateless)
       - 따라서 서버의 `Scale-Out` 이 좋다.


3. **Connectionless**
   - 클라이언트가 보낸 요청에 대한 응답을 마치면 서버가 `Connection close()`
   - 필요할 때만 데이터를 연결하고 주고 받아 서버의 자원을 매우 `효율적`으로 사용

> ❓ **`Scale-Out` 과 `Scale-Up`**
> 
> **서버의 개수**를 늘린다 -> Scale-Out<br>
> **서버의 사양**을 늘린다 -> Scale-Up

---


## 📌 HTTP Request Method

클라이언트와 서버간 통신에서 클라이언트가 리소스에 수행하길 원하는 행동을 지정하는 것.

### HTTP 메소드 종류

- **주요 메소드** (CRUD Method)

|Method|Detail|Cacheable|Idempotent|Safe|
|:--:|:--:|:--:|:--:|:--:|
|GET|리소스 조회|✅|✅|✅|❌|
|POST|요청 데이터 처리 (보통 리소스 등록)|❌|❌|❌|
|PUT|리소스 전체 수정 (해당 리소스 없을 경우 생성)|❌|✅|❌|
|PATCH|리소스 일부 수정|❌|❌|❌|
|DELETE|리소스 삭제|❌|✅|❌|

- **ETC Method**

|Method|Detail|Cacheable|Idempotent|Safe|
|:--:|:--:|:--:|:--:|:--:|
|HEAD|메세지 헤더 정보 조회|✅|✅|✅|
|OPTIONS|서버에서 제공하는 메소드 정보 조회|❌|✅|✅|
|CONNECT|클라이언트와 대상 서버 간에 SSL/TLS 터널 연결|❌|❌|❌|
|TRACE|요청이 수신되는 경로 조회|❌|✅|✅|


> ❓ **멱등성(Idempotent) 이란?**
>
> 여러번 반복 요청해도 항상 동일한 응답을 반환하는 것을 멱등성이라고 한다.<br>
> <br>
> → 수학적으로 쉽게 설명하면<br>
>  `f(x) = f(f(x)) = f(f(f(x)))` 이 등식을 만족하는 것이 멱등성을 보장한다.
<br>

> ❓ **Method Safe 란?**
>
> 요청을 수행해도 리소스가 변경되지 않는 것을 의미한다.<br>
> → 멱등성과는 다른 개념

> ❓ **Method 캐싱(Caching) 이란?**
>
> 데이터나 응답 결과를 저장하고, 다시 접근할 경우 재사용하는 것을 의미한다.<br>
> → 빠른 처리 가능 👍🏻

---


## 📌 HTTP Message

![](https://devio2023-media.developers.io/wp-content/uploads/2021/08/image-7.png)

서버와 클라이언트 서버 사이에서 데이터가 교환되는 방식.

HTTP Message Type 2가지

1. **Request Message** : 클라이언트가 서버에 어떤 특정 액션을 취하게 하려는 메시지
2. **Response Message** : 받은 요청 메시지에 대한 취한 액션의 답변.

### Request Message

1. **Start Line**
   - 서버가 수행해야할 행위를 지정하는 Method (ex. `POST`)
   - 도메인의 절대 경로, URL 을 나타내는 Request Target (ex. `/users/login.html`)
   - HTTP 버전 (ex. `HTTP/1.1`)

2. **Headers**
   - 클라이언트가 서버에게 전달하는 추가 정보 (ex. `Host`, `Content-Type`)

3. **Body**
   - 리소스를 가져오는 요청에는 포함 되지 않고, 서버에 데이터 전송이 필요한 경우 사용된다.
   - 즉, 모든 Request에 포함되지 않는다.

### Response Message

1. **Status Line**
   - HTTP 버전 정보
   - 요청의 성공 여부를 나타내는 `Status code` (ex. `200`, `404`)
   - 사람이 이해할 수 있게 간결한 `Status Text` (ex. `OK`, `Not Found`)

2. **Headers**
   - HTTP 전송에 필요한 부가 정보들

3. **Body**
   - 실제 전송할 데이터 (JSON, HTML 등)
   - 데이터를 전송할 필요가 없을 경우 body 부분은 공백이 된다.

---


## 📌 HTTP Status Code

특정 HTTP 요청에 대한 성공 혹은 실패 여부를 세자리 숫자로 표현한 것

|Status Code|Detail|
|:--:|:--:|
|1xx (Information)|클라이언트에게 요청 처리 과정의 진행 상태를 알리거나, 추가 정보가 필요함|
|2xx (Success)|서버가 클라이언트의 요청을 성공적으로 완료됨|
|3xx (Redirect)|요청된 페이지가 일시적으로 또는 영구적으로 이동됨|
|4xx (Client Error)|클라이언트의 요청에 오류가 있어 요청을 서버가 처리할 수 없음|
|5xx (Server Error)|클라이언트가 정상적인 요청을 보냈지만, 서버 오류로 인해 요청을 처리 못함|

### 개인적으로 자주 사용되는 Status Code

- 200 (OK) - 요청 처리가 완료됨
- 201 (Create) - 리소스 생성이 완료됨
- 204 (No Contents) - 요청 처리가 완료되었고, 응답할 데이터는 없음
- 400 (Bad Request) - 클라이언트가 잘못된 요청을 보냄
- 401 (Unauthorized) - 클라이언트가 인증되지 않았거나 인증이 거부됨
- 403 (Forbidden) - 클라이언트가 인증 처리가 되었지만, 요청을 보낸 리소스에 대한 권한이 없음

> ❗️ 혼용 주의 401 vs 403
>
> 401(Unauthorized) - 인증 자체가 안되거나 거부된 클라이언트<br>
> 403(Forbidden) - 인증은 완료되었으나 특정 리소스에 대한 권한이 없는 클라이언트<br>
> 
> ex)<br>
> 로그인하지 않은 사용자가 결제를 하려고 한다 → 401(Unauthorized)<br>
> 일반 사용자가 Admin 페이지에 접근하려고 한다 → 403(Forbidden)

---


## 📌 HTTP URI & URL & URN

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/68ca2654-8f07-48e6-855e-88a9e4c9f906/URL-URI-Miessler-2022.png)

- **URI** (Uniform Resource Identifier)
   - 네트워크 상에 존재하는 특정 자원을 식별할 수 있는 문자열

- **URL** (Uniform Resource Locator)
   - 네트워크 상 존재하는 자원의 정확한 위치 정보

- **URN** (Uniform Resource Name)
   - 위치와 상관없이 식별가능한 자원의 고유의 이름
   - 즉, 자원을 영구적으로 식별할 수 있다.

> 📍 URI, URL, URN 의 예시
> 
> ```
> https://JiHongKim98.github.io/posts → (URI, URL)
> 
> https://JiHongKim98.github.io/posts/123 → (URI)
> 
> urn:isbn:0-486-27557-4 → (URI, URN)
> ```

> ❗️ URL의 한계와 URN
>
> <u>리소스의 **위치가 변경**되면 위치 정보(URL)도 **같이 변경** 되어야하는 URL의 한계</u>를<br>
> 극복하기 위해 URN이 나왔지만, 대중적으로 채택되지 못해 아직 잘 사용되지 않는다.

---


## Ref.

[MDN HTTP DOCS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)<br>
[WIKI HTTP DOCS](https://ko.wikipedia.org/wiki/HTTP)<br>
[Cloudflare - HTTP란 무엇입니까?](https://www.cloudflare.com/ko-kr/learning/ddos/glossary/hypertext-transfer-protocol-http/)<br>
[AWS - HTTP와 HTTPS의 차이점은 무엇인가요?](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)<br>
[HTTP 개념 정리](https://ngwoon.github.io/network/2020/08/07/HTTP-HTML/)<br>
[The Real Difference Between a URL and a URI](https://danielmiessler.com/p/difference-between-uri-url)
