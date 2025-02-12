---
created: 2025-02-12 15:46
category:
  - network
  - web
tags:
  - TIL
last_modified: 2025-02-12T18:57:00
---
## ⭐CORS (Cross-Origin Resource Sharing)

### 🍪 Origin
- Origin은 프로토콜(Protocol) + 호스트(Host) + 포트(Port)를 기준으로 정의된다.
- ![Origin|600x300](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*8g-wHZd7EmekjtL9BV3RBw.png)
	- [이미지 출처](https://medium.com/@jvito11904/cors%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-f26a3eb5e8a2)

- 동일 출처 예시
	- `https://example.com:443/page1.html`
	- `https://example.com:443/api/data`
	- 같은 프로토콜(`https`) + 도메인(`example.com`) + 포트(`443`)

- 다른 출처(Cross-origin) 예시 -> CORS 필요
	- `https://example.com` → `https://api.example.com` (서브도메인이 다름)
	- `https://example.com` → `http://example.com` (프로토콜이 다름)
	- `https://example.com` → `https://example.com:8080` (포트가 다름)
		- HTTP 서버는 `80` 포트 이용
		- HTTPS 서버는 `443` 포트 이용

> 출처가 다르면 기본적으로 요청이 차단되며, 이를 해결하기 위해 CORS가 필요하다.
---
### 🍪 동일 출처 정책 (Same-Origin Policy, SOP)
> 웹 브라우저의 기본 보안 정책 중 하나.
#### 🍬 SOP 등장 배경
> 초창기 웹 보안 문제 중 하나로 CSRF(사이트 간 요청 위조)가 있다.

- CSRF 공격 예시
	1. 사용자가 본인의 은행 사이트에 로그인한다.
	2. 사용자가 악성 웹 사이트를 열어본다.
	3. 악성 웹 사이트가 사용자의 쿠키 정보를 활용하여 은행 사이트로 위조된 요청을 전송한다
	4. 공격자는 사용자의 계좌에서 자금을 이체하거나 정보를 훔칠 수 있다.
- 이러한 보안 문제를 막기 위해 동일 출처 정책(Same-Origin Policy)가 도입됐다.

#### 🍬 SOP 개념
- 웹 브라우저는 보안상 이유로, 스크립트가 동일한 출처의 리소스만 접근할 수 있도록 제한한다.
- A 웹사이트에서 실행된 JavaScript가 B 웹사이트의 데이터를 가져오는 것을 기본적으로 차단한다.
- 동일한 출처(Origin)에서만 리소스 요청이 가능하도록 제한하는 것이 핵심이다.

- SOP를 확인하는 주체는 브라우저이다.
	- 만약 SOP를 위반하는 요청을 클라이언트에서 보내면, 서버가 정상적으로 응답하여도 브라우저는 그 응답을 처리하지 않는다.

> 동일 출처 정책은 강력한 보안 기능이지만, 유연성이 부족하다.
	- 신뢰할 수 있는 외부 API나 서비스와 데이터를 주고받을 때 제한이 걸릴 수 있다. 
	- 이를 해결하기 위해 등장한 것이 CORS.
---
### 🍪 CORS (Cross-Origin Resource Sharing, 교차 출처 리소스 공유)란?
> 다른 출처의 리소스 공유에 대한 허용/비허용 정책
- CORS는 서로 다른 도메인에서 로드된 클라이언트 웹 애플리케이션이 외부 리소스에 접근할 수 있도록 해주는 매커니즘이다.
- ex (CORS가 필요한 경우):
	- 웹 애플리케이션이 외부 API에서 날씨 데이터를 가져온다.
	- 공용 폰트 라이브러리에서 폰트를 불러온다.
	- 동영상 플랫폼 API에서 영상을 가져온다.
> CORS를 통해 클라이언트(웹 브라우저)는 외부 서버가 해당 요청을 허용하는지 먼저 확인 후 데이터를 가졍게 된다.

#### 🍬 CORS 동작 방식
> 웹 브라우저가 외부 서버에 요청을 보낼 때, 다음과 같은 과정이 진행된다.
1. 브라우저가 요청에 `Origin` 헤더를 추가한다.
	- ex: `Origin: https://news.example.com`
2.  서버가 요청을 확인한 후 `Access-Control-Allow-Origin` 헤더를 응답에 포함시킨다.
	- ex `Access-Control-Allow-Origin: https://news.example.com`
3. 브라우저가 응답을 확인한 후 해당 데이터의 공유를 허용한다.

> 만약 서버가 해당 출처(Origin)에서의 요청을 허용하지 않는다면, 브라우저는 CORS 오류를 발생시킨다.

#### 🍬 CORS 설정 예시
- `https://news.example.com` 사이트가 `https://partner-api.com`의 API 데이터를 가져오고 싶다면
- `partner-api.com` 서버는 다음과 같이 CORS 설정을 추가해야 한다.
	- 설정하면 `news.example.com`에서 `partner-api.com` API를 사용할 수 있다.
```http
Access-Control-Allow-Origin: https://news.example.com
```

- 여러 개의 도메인을 허용하려면 쉼표(,)로 구분하거나 `*`를 사용하여 모든 출처를 허용할 수 있다. (보안상 `*`는 권장하지 않는다.)
```http
Access-Control-Allow-Origin: *
```

#### 🍬 CORS의 보안 고려 사항
- `Access-Control-Allow-Origin: *`을 사용하면 모든 출처에서 요청을 허용하므로 보안상 위험이 크다.
- 민감한 정보를 다루는 API는 특정 도메인만 허용하도록 설정해야 한다.
- `Access-Control-Allow-Credentials: true`를 사용할 경우, `*`를 함께 사용하면 오류가 발생한다. (특정 도메인을 지정해야 한다.)
---
### 🍪 CORS 작동 방식 3가지 시나리오
> CORS가 동작하는 방식은 세 가지의 시나리오에 따라 변경된다.
#### 🍬 1. 사전 요청 (Preflight Request)
> 기본적으로 `GET`,`POST`,`HEAD` 요청은 간단한 요청(Simple Request)이므로 바로 요청을 보내면 된다.
> 하지만, 다음과 같은 경우 브라우저는 실제 요청을 보내기 전에 Preflight 요청을 보낸다.
1. `GET`, `POST`, `HEAD` 이외의 HTTP 메서드(OPTIONS)를 사용할 경우 (`PUT`, `DELETE` 등)
2. 특정 헤더를 포함할 때 (`Content-Type`이 `application/json` 등)

- 브라우저는 요청을 보낼 때 한번에 바로 보내지 않고, 먼저 Preflight 요청을 보내 서버와 잘 통신되는지 확인한 후 본 요청을 보낸다.
	- 즉, 요청을 보내기 전 브라우저 스스로 안전한 요청인지 확인.

###### 사전 요청 동작 과정
1. 브라우저가 OPTIONS 요청을 먼저 보낸다.
```http
OPTIONS /data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: DELETE
```
2. 서버가 허용 여부를 응답한다.
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://news.example.com
Access-Control-Allow-Methods: GET, DELETE, HEAD, OPTIONS
Access-Control-Allow-Headers: Content-Type
```
- 이 과정이 끝나면 브라우저는 실제 `DELETE` 요청을 보낸다.

- Preflight Request의 문제점
	- 요청을 보내기 전 Preflight 요청을 보내 보안을 강화하는 목적의 취지는 좋지만,
		- 결국 실제 요청에 걸리는 시간이 늘어 애플리케이션 성능 저하를 발생시킨다.
	- API 호출 수가 많아질수록 서버 요청을 배로 보내게 되므로 비용적인 문제가 발생할 수 있다.
	- (브라우저 캐시를 이용해 Preflight 요청을 캐싱시켜 최적화가 가능하다.)
#### 🍬 단순 요청 (Simple Request)
- Preflight 요청을 생략하고 바로 서버에 직행으로 본 요청을 보낸 후,
	- 서버가 이에 대한 응답 헤더에 `Access-Control-Allow-Origin` 헤더를 보내주면 브라우저가 CORS 정책 위반 여부를 검사하는 방식이다.
###### 단순 요청 가능 조건
1. 요청의 메소드는 GET, HEAD, POST 중 하나.
2. `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width` 헤더일 경우에만 적용된다.
3. Content-Type 헤더가 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 중 하나여야만 한다. 아닐 경우 Preflight Request로 동작한다.

> 까다로운 조건이 많아, 단순 요청이 일어나는 상황은 드물다.
	- 대부분 HTTP API 요청은 `text/xml`이나 `application/json`으로 통신하므로 위 3번에 위반된다.
	- 대부분의 API 요청은 Prefilight Requeest로 이루어진다.

#### 🍬 인증된 요청 (Credentialed Request)
- 쿠키, 인증 토큰, HTTP 인증 등의 자격증명(Credential)을 포함하는 요청
- 브라우저가 서버에 요청을 보낼 때 사용자의 인증 정보(세션, JWT, 쿠키 등)를 함께 전송하는 경우이다.

###### 인증 요청이 필요한 이유
- 일반적인 API 요청에서는 `Access-Control-Allow-Origin: *` 를 사용하여 모든 도메인에서 요청을 허용할 수 있지만,
	- 로그인 상태 유지, 세션 인증, 사용자 식별이 필요한 경우에는 쿠키나 인증 토큰이 포함된 요청이 필요하다.
	- 이러한 요청을 처리하기 위해 서버는 CORS와 관련된 추가적인 설정을 해야 한다.
###### 동작 방식
1. 클라이언트가 요청 시 `credentials:"include"`를 설정한다.
```javascript
fetch("https://api.example.com/user", {
  method: "GET",
  credentials: "include"
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Error:", error)); 
```
- `credentials` 옵션
	- `"omit"` → 쿠키 및 인증 정보 전송 안 함 (기본값)
	- `"same-origin"` → 같은 출처에서만 쿠키 및 인증 정보 포함
	- `"include"` → 다른 출처에서도 인증 정보 포함 가능

1. 서버가 CORS 응답 헤더에서 `Access-Control-Allow-Credentials: true`를 설정한다.
	- 주의: `Access-Control-Allow-Origin: *` 와 `Access-Control-Allow-Credentials: true`를 같이 사용할 수 없다. (특정 도메인을 명시해야 한다.)
```
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Credentials: true
```
---
### 🍪 현업에서의 CORS 관련 고려 사항
#### 🍬 1. 백엔드에서 CORS 설정 필요
- ex: Spring Boot 
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("https://myfrontend.com")  // 특정 도메인 허용
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

#### 🍬 2. 프록시(Proxy) 사용하여 해결
- 개발 환경에서 CORS 문제를 해결할 때, 프론트엔드에서 프록시 서버를 사용하여 우회할 수도 있다.
- ex: React에서 `proxy` 설정을 통해 백엔드 API를 프록시로 요청하면, 브라우저에서 직접 CORS 문제를 발생시키지 않는다.
- `package.json` 설정
```json
"proxy": "http://localhost:8080"
```

#### 🍬 3. API Gateway를 통한 CORS 관리
- AWS API Gateway, Nginx 같은 API Gateway에서 CORS를 설정하는 방법도 사용된다.
- 클라이언트가 직접 서버에 접근하지 않고 Gateway에서 요청을 중개하면, 원본 서버의 CORS 설정을 최소화할 수 있다.
---
## 🧙‍♂️ 요약

> SOP는 보안 강화를 위해 존재하지만, 유연성이 부족함 → 이를 해결하기 위해 CORS가 등장.  
> 서버가 허용한 출처에 한해 외부 리소스 접근 가능하도록 설정해야 함. 

- 출처(Origin): `프로토콜 + 호스트 + 포트`
	- SOP(동일 출처 정책)로 인해 브라우저는 다른 출처의 요청을 기본적으로 차단한다. -> CORS 필요
- CORS
	- SOP의 한계를 해결하기 위한 보안 정책.
	- 서버에서 `Acess-Control-Allow-Origin` 등의 헤더를 설정해 특정 출처에서의 요청을 허용하는 방식이다.

- 요청 방식 3가지
	1. 사전 요청(Preflight Request):
		- `GET`,`POST`, `HEAD`를 제외한 요청 (`PUT`, `DELETE` 등)
		- 서버가 허용하면 실제 요청을 수행한다.
		- 성능 저하 우려 -> 캐싱 가능
	2. 단순 요청(Simple Request):
		- `GET`, `POST`, `HEAD`, 특정 `Content-Type` 조건 충족 시 바로 요청.
	3. 인증된 요청(Credentialed Request):
		- 쿠키 인증 정보 포함 시 `credentials: "include"` 설정 필요.
		- 서버는 `Access-Control-Allow-Credentials: true` 설정 필요.
---
> **참고사이트**
> - [01-AWS](https://aws.amazon.com/what-is/cross-origin-resource-sharing/?nc1=h_ls)
> - [02-Inpa Dev](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)
