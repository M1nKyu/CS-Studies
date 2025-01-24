---
created: 2025-01-24 16:22
category:
  - network
  - web
tags:
  - TIL
last_modified: 2025-01-24T18:32:00
---
> `REST`, `RESTful Architecture`, `RESTful`, `REST API`, `RESTful API`?
## ⭐ RESTful 아키텍처란?
- `Restful 아키텍처`는 **REST(Representational State Transfer)** 원칙과 제약 조건을 따르는 아키텍처 스타일을 의미한다.

- REST 아키텍처 스타일을 따르는 API를 `REST API`라고 하고,
	- REST 아키텍처를 구현하는 웹 서비스를 `RESTful 웹 서비스`라고 한다.
- `RESTful API`라는 용어는 일반적으로 RESTful 웹 API를 나타낸다.
	- 그러나 `REST API`와 `RESTful API`는 같은 의미로 사용할 수 있다.
---
### 🍪 REST (Representational State Transfer)
> 자원(Resource)의 표현(Representation)에 의한 상태 전달

- 웹 서비스 설계에 사용되는 아키텍처 스타일로, 자원을 클라이언트와 서버 간에 효율적으로 전달하기 위한 일련의 원칙과 제약 조건을 정의한다.
- 인터넷의 기본 원리를 기반으로 설계됐으며, HTTP 프로토콜을 효과적으로 활용하는 방식으로 동작한다.

1. HTTP URI를 통해 자원을 명시하고
2. HTTP Method(GET, POST, DELETE, PATCH 등)을 통해 
3. 해당 자원(URI)에 대한 CRUD를 적용한다.

#### 🍬 REST의 구성 요소
1. 자원 (Resource):
	- URI를 통해 식별되는 모든 정보.
	- ex: `https://api.example.com/users/1`
2. HTTP Method (Method):
	- 자원에 대한 동작을 정의한다.
	- `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
3. 표현 (Representation):
	- 특정 타임스탬프의 Resource 상태.
	- JSON, XML 등으로 표현하여 클라이언트와 서버 간 교환된다.
	- ex: `{ "id": 1, "name": "John Doe" }`
#### 🍬 REST의 설계 원칙
1. **일관된 인터페이스 (Uniform Interface)**
	- RESTful 웹 서비스는 일관된 인터페이스를 제공한다. 즉, 서버는 정보를 표준 형식으로 전송한다. (이 정보가 Representation)
	- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
2. **클라이언트-서버 분리**
	- REST API 설계에서 클라이언트 및 서버 애플리케이션은 서로 완전히 독립적이어야 한다.
	- 클라이언트 애플리케이션이 알아야 하는 유일한 정보는 요청된 리소스의 URI이다.
	- 마찬가지로 서버 애플리케이션은 HTTP를 통해 요청된 데이터에 클라이언트 애플리케이션을 전달하는 것 외에는 클라이언트 애플리케이션을 수정해서는 안된다.
3. **무상태성 (Statelessness)**
	- 서버는 클라이언트의 각 요청을 독립적으로 처리한다.
	- 이전 요청과 관계없이 각 요청을 독립적으로 이해하고 처리한다.
4. **계층형 시스템 (Layered System)**
	- 클라이언트는 서버와의 직접적 연결 외에도, 인증된 중간 서버나 보안 계층을 통해 연결될 수 있다.
5. **캐싱 가능 (Cacheability)**
	- RESTful 웹 서비스는 캐시를 지원한다.
	- 클라이언트나 중간 서버는 서버 응답을 캐시하여 응답 시간을 개선할 수 있다.
6. **코드 온디맨드(Code on Demand)**
	- REST API는 일반적으로 정적 리소스를 전송하지만 경우에 따라 응답에 실행 코드가 포함될 수도 있다.
	- 이러한 경우 코드는 온디맨드 방식으로만 실행되어야 한다.
---
### 🍪 REST API
> REST API(RESTful API 또는 RESTful 웹 API라고도 함)

- REST의 특징을 기반으로 서비스 API를 구현한 것
- REST API는 **HTTP 요청**을 통해 통신하여 **CRUD**와 같은 표준 데이터베이스 기능을 수행한다.
- 웹 서비스에서 자원(Resource)를 URI로 식별하고 HTTP 메서드를 통해 자원에 접근하고 조작하는 방식이다.
- 각 요청이 어떤 동작이나 정보를 위한 것인지를 그 요청의 모습 자체로 추론이 가능하다는 특징이 있다.
---
### 🍪 URI 네이밍 규칙
#### 🍬 Resource를 명사로 나타내라.
> RESTful URI가 가리키는 자원은 객체이다. 때문에 동사가 아닌 명사로 칭한다.
> 자원의 4가지 범주(Document, Collection, Store, Controller)에 따라 일관된 네이밍을 사용한다.

- **문서(Document)**:
	- 문서 자원은 DB의 하나의 레코드, 하나의 객체 인스턴스와 유사한 단일 자원의 개념이다.
	- 단수를 사용하여 문서 자원을 표현한다.
```python
http://api.example.com/device-management/managed-devices/{device-id} # `device-management`
http://api.example.com/user-management/users/{id}  # `user-management`
http://api.example.com/user-management/users/admin # `user-management`
```

- **컬렉션(Collection)**:
	- 서버가 관리하는 리소스 디렉터리.
	- 클라이언트에 의해 새로운 리소스 추가가 요청될 수 있다(POST).
	- 복수를 사용하여 컬렉션 자원을 표현한다.
```python
http://api.example.com/device-management/managed-devices   # `managed-devices`
http://api.example.com/user-management/users               # 'users'
http://api.example.com/user-management/users/{id}/accounts # 'user/{id}/accounts'
```

- **스토어(Store)**:
	- 클라이언트가 관리하는 자원 저장소.
	- 클라이언트는 API를 이용하여 자우너을 넣거나 가져올 수 있고 삭제할 수 있다.
	- 복수를 사용하여 스토어를 표현한다.
```python 
http://api.example.com/song-management/users/{id}/playlists # 'playlists'
```

- **컨트롤러(Controller)**: 예외
- 컨트롤러 자원은 인자와 반환 값, 입출력이 있는 실행 가능한 함수와 같다.
- 특정 자원을 가리키는 것이 아닌, 실행인 만큼 예외적으로 동사를 사용한다.
```python
http://api.example.com/cart-management/users/{id}/cart/checkout  # checkout
http://api.example.com/song-management/users/{id}/playlist/play  # play
```

#### 🍬 그 외 규칙
- **계층 관계를 표현하기 위해 URI 경로에 `/`를 사용한다**
```
http://api.example.com/device-management
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices/{id}
http://api.example.com/device-management/managed-devices/{id}/scripts
http://api.example.com/device-management/managed-devices/{id}/scripts/{id}
```
- **URI 마지막에 `/`를 사용하지 않는다**
	- 마지막에 `/`를 사용할 경우 혼동을 줄 수 있다.
```
http://api.example.com/device-management/managed-devices/  ❌
http://api.example.com/device-management/managed-devices   ⭕
```
- **하이픈(`-`)을 사용하여 가독성을 높인다**
```
.../devicemanagement/manageddevices   ❌  
.../device-management/managed-devices ⭕
```
- **언더스코어(`_`)는 사용하지 않는다.**
	- 폰트, 브라우저에 따라 불명확히 표현될 수 있다.
```
.../inventory_management/managed_entities/{id}/install_script_location  ❌  
.../inventory-management/managed-entities/{id}/install-script-location  ⭕
```

- **소문자를 사용한다.**
	- Schem, HOST는 대소문자 구분이 없으나, 이외에는 모두 대소문자가 구분된다.
```
// 1과 2는 동일, 3은 다르다.
http://api.example.org/my-folder/my-doc //1  
HTTP://API.EXAMPLE.ORG/my-folder/my-doc //2  
http://api.example.org/My-Folder/my-doc //3
```

- **파일의 확장자는 사용하지 않는다.**
	- 가독성을 해치고 어떠한 이점도 없다.
```
http://api.example.com/device-management/managed-devices.xml  ❌
http://api.example.com/device-management/managed-devices      ⭕
```

- **CRUD 함수 명을 사용하지 않는다.**
	- URI는 자원을 가리키는 것이다.
	- 어떤 동작을 가리켜서는 안된다.
	- CRUD는 HTTP의 요청 메소드를 이용하여 표현할 수 있다.
```
HTTP GET http://api.example.com/device-management/managed-devices  //모든 디바이스를 요청
HTTP POST http://api.example.com/device-management/managed-devices  //새로운 디바이스를 생성

HTTP GET http://api.example.com/device-management/managed-devices/{id}  //ID를 이용하여 디바이스를 요청
HTTP PUT http://api.example.com/device-management/managed-devices/{id}  //ID를 이용하여 디바이스를 갱신
HTTP DELETE http://api.example.com/device-management/managed-devices/{id}  //ID를 이용하여 디바이스 삭제
```

- **자원의 필터링을 위해서는 쿼리 파라미터를 사용한다.**
	- 자원을 정렬, 필터링하거나 제한된 수량을 요청해야 할 때 신규 API를 생성하려 하지말고, 쿼리 파라미터를 활용한다.
```
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices?region=USA
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ&sort=installation-date
```

#### RESTful?
- REST의 원리를 잘 따르는 시스템이라면, RESTful 용어로 지칭할 수 있다.
---
## 🧙‍♂️ 요약
- **REST란**
	- 웹 서비스 설계에 사용되는 아키텍처 스타일.
	- 구성: Resource + HTTP Method + Representation

- **REST 설계 원칙**
	- 일관된 인터페이스: 표준화된 방식으로 정보 주고받음
	- 클라이언트-서버 분리: 독립적 동작
	- 무상태성: 각 요청 독립적 처리
	- 계층형 시스템: 서버-클라이언트 사이 중간 계층을 둘 수 있음
	- 캐싱 가능: 응답 캐싱 -> 성능 향상
	- 코드 온디맨드: 응답에 실행코드 포함 가능

- **REST API**
	- REST 원칙을 따르는 웹 API.
	- URI과 HTTP Method를 사용하여 자원에 접근하고 조작.

- **RESTful**
	- REST의 원리를 잘 따르는 시스템을 RESTful이라 부를 수 있음.

- **URI 네이밍 규칙**
	- 자원 -> 명사 사용 (예외: Controller -> 동사)
	- 하이픈 사용
	- 소문자 사용
	- 확장자 생략
---
> **참고사이트**
> - [Medium-Rest API Architecture](https://medium.com/@shikha.ritu17/rest-api-architecture-6f1c3c99f0d3)
> - [REST API vs RESTful API](https://m.blog.naver.com/codingbarbie/223233477242)
> - [REST, REST API, RESTful 특징](https://hahahoho5915.tistory.com/54)
> - [RESTful Architecture 시리즈](https://velog.io/@0hoxy/series/RESTful-Architecture)
> - [AWS-REST란?](https://docs.aws.amazon.com/ko_kr/appsync/latest/devguide/what-is-rest.html)
> - [IBM](https://www.ibm.com/kr-ko/topics/rest-apis)
> - [Medium-URI 네이밍 규칙](https://medium.com/tech-pentasecurity/restful-api-%EB%84%A4%EC%9D%B4%EB%B0%8D-7c81bdb9da63)
>




