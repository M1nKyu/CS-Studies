---
created: 2025-02-11 17:03
category:
  - network
  - web
tags:
  - TIL
last_modified: 2025-02-11T20:14:00
---
## ⭐ JWT 
### 🍪 JWT (JSON Web Token) 란?
- 인증에 필요한 정보들을 **암호화** 시킨 **JSON 토큰**을 의미한다.
- 주로 **사용자 인증** 및 **정보 교환**을 위해 사용되며, 서버-클라이언트 간 신뢰성을 유지하는 중요한 역할을 한다.
- 웹 상에서 정보를 JSON 형태로 주고받기 위해 표준 규약에 따라 생성한 암호화된 토큰으로, **복잡한 String 형태**로 저장돼있다. 
- JSON 데이터를 **Base64 URL-safe Encode**를 통해 인코딩하여 직력화한 것이다.
- 토큰 내부에는 위변조 방지를 위한, 개인키를 통한 전자서명도 들어있다. 
	- 사용자가 **JWT를 서버로 전송**하면, **서버는 서명을 검증**하는 과정을 거치게 되며 검증이 완료되면 요청한 응답을 돌려준다.
---
### 🍪 JWT의 구성요소
- **헤더**(Header), **페이로드**(Payload), **서명**(Signature)로 구성된다.
- 각 부분은 **Base64로 인코딩**되어 점(.)으로 구분된다.
- ![JWT 형태|600x200](https://velog.velcdn.com/images%2Fhahan%2Fpost%2Fb41e147b-69d0-41ad-9f23-5e1ab8ec35ce%2Fimage.png)
	- [이미지 출처](https://velog.io/@hahan/JWT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
- ![JWT 직력화 형식|600x250](https://cloud.google.com/static/apigee/docs/api-platform/images/jwt-format.png?hl=ko)
	- [이미지 출처](https://cloud.google.com/apigee/docs/api-platform/security/oauth/using-jwt-oauth?hl=ko)
> [JWT.IO](jwt.io) 사이트에서 JWT 토큰을 인코딩/디코딩 해볼 수 있다.
#### 헤더 (Header)
- **토큰 타입**과 사용된 **암호화 알고리즘**(해시 알고리즘) 정보를 담는다.
```json
{
  "alg": "HS256", // 서명 암호화 알고리즘
  "typ": "JWT"    // 토큰 유형
}
```

#### 페이로드 (Payload)
- **사용자의 정보** 및 추가 **데이터**(`Claim`)이 포함된다.
- payload의 내용은 **수정이 가능**하여 더 많은 정보를 추가할 수 있다.
	- **노출과 수정이 가능한 지점**이므로 최소한의 정보만을 담아야 한다.
```json
// 일반적으로 포함되는 클레임
{
  "sub": "1234567890", // 사용자ID, 제목 (Subject)
  "name": "John Doe",  
  "iat": 1516239022,   // 발급 시간 (Issued At)
  "iss": "example",    // 발급자 (Issuer)
  "aud": "example"     // 대상 (Audience)
}
```

#### 서명 (Signature)
- 토큰이 **변조되지 않았음을 검증**하는 서명이다.
- **(헤더 + 페이로드)** 와 서버가 갖고 있는 **유일한 key 값을 합친 것을 암호화**한다.
	- 헤더에서 정의한 알고리즘 방식(alg)을 사용한다.

>- **Header와 Payload**는 **단순히 인코딩된 값**이기 때문에 제 3자가 **복호화 및 조작**할 수 있지만, 
>- Signature는 서버 측에서 관리하는 비밀키가 유출되지 않는 이상 복호화할 수 없다. 
>- 따라서 **Signature는** **토큰의 위변조 여부를 확인**하는데 사용된다.
---
### 🍪 JWT 인증 과정
- ![인증 과정](https://velog.velcdn.com/images%2Fhahan%2Fpost%2Fa2b9ade7-1812-47fc-a07f-f1f00ff5f28d%2Fimage.png)
	- [이미지 출처](https://velog.io/@hahan/JWT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

1. 사용자가 ID/PW를 입력하여 **로그인 요청**을 한다.
2. 서버는 DB에서 사용자 정보를 조회하고 비밀번호를 검증한다.
	- 인증 성공 시, **JWT를 생성**하여 클라이언트에게 **반환**한다.
	- 클라이언트는 JWT를 로컬스토리지, 쿠키, 세션 스토리지 등에 저장한다.
3. 클라이언트가 **JWT를 포함하여 API를 요청**한다.
	- API를 요청할 때, Authorization header에 **Access Token을 담아서 보낸다**.
	- JWT는 **요청마다 서버에 전달**된다. (Stateless, 세션 필요 없음)
4. 서버는 **JWT를 검증**하여 유효한 토큰인지 확인한다.
	- **Signature 검증**: 토큰이 변조되지 않았는지 확인한다.
	- **만료 시간 검증**: 토큰이 유효한 시간 내인지 확인한다.
5. 검증이 통과되면 서버는 요청된 리소스를 반환한다.
	- 클라이언트는 계속해서 동일한 JWT를 사용하여 API 요청을 보낼 수 있다.
6. 클라이언트가 서버에 요청을 했는데, **만약 액세스 토큰의 시간이 만료**되면
	- 클라이언트는 **리프레시 토큰을 이용**해서 서버로부터 **새로운 Access Token을 발급**받는다.
---
### 🍪 토큰 인증의 신뢰성
> 유저 JWT: A(Header) + B(Payload) + C(Signature)일 때, 
	- 임의의 유저가 B를 수정했다고 가정 (B -> B')
1. 다른 유저가 B를 임의로 수정 -> 유저 JWT: **A + B'+ C**
2. 수정한 토큰을 서버에 요청 -> 서버는 **유효성 검사 수행**
	- 유저 JWT: A + B'+ C
	- 검증 후 생성한 JWT: **A + B'+ C'** (**Signature 불일치**)
3. 대조 결과가 일치하지 않아 유저의 정보가 임의로 **조작됨을 알 수 있다**.

> JWT는 **서명(인증)이 목적**이다.
	- Payload 부분은 복호화를 수행하면 그대로 노출된다. 그래서 Payload는 민감한 정보를 넣지 말아야 한다.
	- 토큰의 진짜 목적은 정보 보호가 아닌, 위조 방지이다.
---
### 🍪 JWT 장단점 
#### 🍬 장점
- Header와 Payload로 Signature를 생성 -> 데이터 위변조를 막을 수 있다.
- 인증 정보에 대한 별도의 저장소가 필요없다.
- 클라이언트 인증 정보를 저장하는 세션과 다르게, **서버가 무상태(Stateless)**이다. -> 서버 확장성 우수
- 토큰 기반으로 다른 로그인 시스템에 접근 및 권한 공유가 가능하다. (쿠키와의 차이)
- **모바일 어플리케이션 환경**에서도 잘 동작한다. (**모바일은 세션 사용이 불가능**하다.)

#### 🍬 단점
- Self-Contained: **토큰 자체에 정보**를 담고 있다. 양날의 검이 될 수 있다.
- 정보가 많아질수록 토큰의 길이가 늘어나 **네트워크에 부하**를 줄 수 있다.
- Payload 자체는 암호화가 아닌 Base64 인코딩된 것이므로, **중간에 Payload를 탈취**할 수 있다.
- Stateless 특징을 가지므로, 토큰은 클라이언트 측에서 관리하고 저장한다. 따라서 토큰 자체를 탈취당하면 대처가 어렵다.
---
### 🍪 JWT의 Access Token, Refresh Token
> 현업에서는 **Access Token**, **Refresh Token**으로 이중으로 나누어 인증하는 방식을 현업에서 취한다
	- JWT도 제 3자에게 토큰 탈취의 위험성이 존재하기 때문.

- Access Token과 Refresh Token은 같은 JWT이며, 어디에 저장되고 관리되느냐에 따른 사용의 차이일 뿐이다.
#### 🍬 Access Token
- 클라이언트가 갖고 있는 **실제 유저의 정보가 담긴 토큰**
- 클라이언트에서 요청이 오면 서버에서 해당 토큰에 있는 정보를 활용하여 사용자 정보에 맞게 응답을 한다.
#### 🍬 Refresh Token
- **새로운 Access Token의 발급을 위해 사용**한다.
- 짧은 수명을 가지는 Access Token에 새로운 토큰을 발급해주기 위해 사용한다.
- 보통 **데이터베이스에** 유저 정보와 함께 기록한다.
- **Refresh Token도 만료**된 경우 -> 사용자는 **다시 로그인**해야 한다.

#### 🍬 Access Token과 Refresh Token 비교 (표)

|비교 항목|Access Token|Refresh Token|
|---|---|---|
|**사용 목적**|API 요청 인증용|새로운 Access Token 발급용|
|**유효 기간**|짧음 (15~60분)|길음 (7일~30일)|
|**저장 위치**|클라이언트 (로컬스토리지, 쿠키)|서버(DB, Redis)|
|**보안 위험**|탈취 시 악용 가능|탈취 시 Access Token 발급 가능|
|**사용 방식**|모든 API 요청마다 포함|Access Token 재발급 시만 사용|
|**필요 여부**|항상 필요|Access Token 만료 시 필요|

---
## 🧙‍♂️ 요약
- JWT(JSON Web Token)란?
	- 사용자 인증 및 정보 교환을 위한 **암호화된 JSON 토큰**
	- 서버-클라이언트 간 **무상태(Stateless)** 인증 방식
	- 구성요소: **Header + Payload + Signature**
	- 핵심 목적: **Signature를 통해 토큰 위변조 방지**

- 장점:
	- **세션 저장소 불필요** -> **서버 확장성 우수**
	- 토큰 **위변조 방지**
	- **모바일 환경**에서도 사용 가능

- 단점
	- **Payload 노출** 가능 
	- 토큰 **탈취 시 대응 어려움**
	- 토큰이 길어질 경우 **네트워크 부하 발생** 가능

- 현업에서는 보안 강화를 위해 **Access Token과 Refresh Token을 함께 사용**한다.
---
## 📝 추가 학습
- [[OAuth와 JWT Token]]
---
> **참고사이트**
> - [01-JWT](https://ksh-coding.tistory.com/58)
> - [02-InpaDev(OAuth)](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-OAuth-20-%EA%B0%9C%EB%85%90-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)
> - [03-OAuth](https://ksh-coding.tistory.com/62)
> - [04- OAuthV2 정책을 사용하여 JWT 액세스 토큰을 생성, 확인, 갱신하는 방법](https://cloud.google.com/apigee/docs/api-platform/security/oauth/using-jwt-oauth?hl=ko)
> - [05-Access Token, Refresh Token](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT)
