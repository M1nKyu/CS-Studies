---
created: 2025-02-15 16:31
category: 
tags:
  - TIL
last_modified: 2025-02-17T18:48:00
---
## ⭐ API Gateway
### 🍪 API Gateway의 필요성
- [[마이크로서비스 아키텍처 (MSA)|MSA]]에서 중요한 역할을 한다.
	- 다양한 MSA로 구성된 시스템에서 통합된 접점을 제공하기 때문이다.
- 클라이언트는 여러 마이크로서비스에 대한 요청을 하나의 엔드포인트로 보낼 수 있다.
	- 시스템의 복잡성을 줄이고, 클라이언트-서버 통신을 간소화해준다.
- 인증 및 권한 부여, 요청 및 응답의 변환, 로깅과 같은 크로스 커팅 관심사를 처리하여, 각 마이크로서비스가 비즈니스 로직에만 집중할 수 있도록 지원한다.
- 보안을 강화하고, API 관리를 용이하게 하며, 마이크로서비스 간 결합도를 낮추는 효과가 있다.
---
### 🍪 API Gateway 종류와 선택 기준
> 크게 오픈소스와 상용 제품으로 나눈다.
#### 🍬 오픈소스 
- Kong
- Tyk
- Zuul 등
#### 🍬 상용 제품
- AWS API Gateway
- Azure API Management 등

#### 🍬 선택 시 고려해야할 요소
- 프로젝트 요구사항과 예산
	- 오픈소스는 저렴하지만, 
	- 기업 지원이 필요한 경우 상용제품을 선택하는 것이 좋다.
- 게이트웨이가 제공하는 기능과 확장성
	- 인증 및 권한 부여
	- 요청 변환
	- 로깅 등 확인
- 성능과 안정성
	- 대규모 트래픽을 처리할 수 있는지
	- 장애 복구가 빠른지 등
- 커뮤니티 활성도와 문서의 질
---
### 🍪 API Gateway 주의사항
- API 게이트웨이를 통한 트래픽 증가는 백엔드 시스템에 부하를 줄 수 있으므로, 적절한 로드밸런싱과 스케일링 전략이 필요하다.
- API 게이트웨이의 보안도 매우 중요하다.
	- 인증 및 권한 부여, SSL/TLS, API 키 관리 등 보안 관련 기능을 적절히 구성하고 관리해야 한다.
---
### 🍪 Spring Cloud Gateway란?
> 자바 개발자 사이에서 인기 있는 API 게이트웨이 솔루션 중 하나.
- 비동기 I/O를 기반으로 하며, Netty 서버를 사용한다.
- 고성능을 제공, 리액티브 프로그래밍 모델을 지원한다.
#### 🍬 스프링 클라우드 게이트웨이의 간단한 예제
```java
@SpringBootApplication
    public class GatewayApplication {

        public static void main(String[] args) {
            SpringApplication.run(GatewayApplication.class, args);
        }

        @Bean
        public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
            return builder.routes()
                    .route(r -> r.path("/get")
                            .uri("http://httpbin.org"))
                    .build();
        }
    }

```
- 이 코드는 `/get` 경로로 오는 요청을 `http://httpbin.org`로 라우팅하는 간단한 게이트웨이를 구성한다.

#### 🍬 스프링 클라우드 게이트웨이의 장점
- 스프링 클라우드 게이트웨이를 사용하면 손쉽게 라우팅 규칙을 정의할 수 있다.
- 스프링 클라우드 게이트웨이의 적용은 마이크로서비스 아키텍처의 복잡성을 줄이고, API 관리를 효율적으로 할 수 있게 도와준다.
- 스프링 생태계와의 높은 호환성을 가진다.
---
### Spring Cloud Gateway 예제
> 2개의 마이크로서비스 (`first-service`, `second-service`)는 이미 존재한다고 가정.
#### build.gradle 의존성 추가
```shell
...
ext {
    set('springCloudVersion', "2021.0.4")
}

dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
    }
}
...
```

#### application.yml
- 첫 번째 서비스 (`first-service`): `/first-service/**` 경로로 오는 요청은 `http://localhost:8081/`으로 전달된다.
- 두 번째 서비스 (`second-service`): `/second-service/**` 경로로 오는 요청은 `http://localhost:8082/`으로 전달된다.
```yml
server:
  port: 8000  # Gateway 서비스가 실행될 포트

eureka:
  client:
    register-with-eureka: false  # API Gateway는 Eureka에 등록되지 않음
    fetch-registry: false  # Eureka에서 다른 서비스 정보는 가져오지 않음
    service-url:
      defaultZone: http://localhost:8761/eureka  # Eureka 서버 URL 설정

spring:
  application:
    name: apigateway-service  # API Gateway 서비스의 이름

  cloud:
    gateway:
      routes:
        - id: first-service  # 첫 번째 서비스 라우팅
          uri: http://localhost:8081/  # first-service의 URI
          predicates:
            - Path=/first-service/**  # /first-service/** 경로에 대한 요청 처리
        - id: second-service  # 두 번째 서비스 라우팅
          uri: http://localhost:8082/  # second-service의 URI
          predicates:
            - Path=/second-service/**  # /second-service/** 경로에 대한 요청 처리
```

#### Gateway Service
```java
//@Configuration
public class FilterConfig {

//    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
        
				// `/first-service/**` 경로로 오는 요청은 `http://localhost:8081`로 전달된다.
				// 요청에 `"first-request": "first-request-header"` 헤더를 추가하고, 응답에 `"first-response": "first-response-header"` 헤더를 추가
                .route(r -> r.path("/first-service/**")
                        .filters(f -> f.addRequestHeader("first-request", "first-request-header") // 요청 헤더 추가
                                .addResponseHeader("first-response", "first-response-header")) // 응답 헤더 추가
                        .uri("http://localhost:8081"))
                        
				// `/second-service/**` 경로로 오는 요청은 `http://localhost:8082`로 전달된다.
				// 요청에 `"second-request": "second-request-header"` 헤더를 추가하고, 응답에 `"second-response": "second-response-header"` 헤더를 추가
                .route(r -> r.path("/second-service/**")
                        .filters(f -> f.addRequestHeader("second-request", "second-request-header") // 요청 헤더 추가
                                .addResponseHeader("second-response", "second-response-header")) // 응답 헤더 추가
                        .uri("http://localhost:8082"))
                        
                .build();
    }
}
```
#### 요청하기
- first-service에 요청: `http://localhost:8000/first-service/**`
- second-service에 요청: `http://localhost:8000/second-service/**`

---
### Spring Cloud Gateway 헤더 추가, 수정
> Spring Cloud Gateway에서 헤더를 추가하거나 수정하는 방식은 `filters`를 사용하여 설정할 수 있다.
#### 요청에 헤더 추가 (Add Request Header)
> 동작 흐름
	1. 클라이언트가 `http//<API_GATEWAY>/get` 주소로 요청을 보낸다.
	2. API Gateway는 이 요청을 처리하면서 `role` 헤더를 `hello-api`값으로 추가한다.
	3. 이후, API Gateway는 요청을 `http://httpbin.org/get`으로 전달하고, 이 요청에는 `role=hello-api` 헤더가 포함된다.
	4. 응답이 돌아오면 클라이언트는 원래의 응답을 받게된다.
- Java DSL
```java
// RouteLocator: 여러 라우트를 정의,설정하는 주요 객체
// RouterLocatorBuilder: 라우트를 설정하는 빌더, `build.routes()`로 라우트 성
@Bean
public RouteLocator helloRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
			// route("add-headaer-route", ...): 라우트 ID 지정, 라우트 ID는 고유 식별자
			// r.path("/get"): /get 경로로 들어오는 요청을 처리
            .route("add-header-route", r -> r.path("/get") 
		            // 요청에 role 헤더를 추가하는 필터
                    .filters(f -> f.addRequestHeader("role", "hello-api"))  // 헤더 추가
                    // 요청을 전송할 실제 서비스 URI, 아래 URL로 요청을 전달한다.
                    .uri("http://httpbin.org/get")
            ).build();
}
```
- Yaml 설정
```yaml
spring:
  cloud:
    gateway:
      routes: # 여러 라우트를 정의한다.
        - id: add-header-route # 라우트의 고유 ID
          uri: http://httpbin.org/get # 요청을 보낼 URI
          predicates: # 요청 경로를 정의한다. 경로로 들어오는 요청을 처리한다.
            - Path=/get # 요청 경로
          filters: # 요청을 처리하는 필터
            - AddRequestHeader=role, hello-api  # 헤더 추가
```

#### 헤더 수정 (Modify Header)
- Java DSL
```java
@Bean
public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
            .route("modify-header-route", r -> r.path("/modify")
                    .filters(f -> f.addRequestHeader("user-agent", "custom-agent"))  // 헤더 값 변경
                    .uri("http://httpbin.org/headers")
            ).build();
}
```
- Yaml 
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: add-header-route
          uri: http://httpbin.org/get
          predicates:
            - Path=/get
          filters:
            - AddRequestHeader=role, hello-api  # 헤더 추가
```

#### 응답 헤더 추가 (Add Response Header)
- java DSL
```java
@Bean
public RouteLocator responseHeaderRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
            .route("add-response-header", r -> r.path("/response")
                    .filters(f -> f.addResponseHeader("X-Custom-Header", "CustomValue"))
                    .uri("http://httpbin.org/response-headers")
            ).build();
}
```
- Yaml 설정
```java
spring:
  cloud:
    gateway:
      routes:
        - id: add-response-header
          uri: http://httpbin.org/response-headers
          predicates:
            - Path=/response
          filters:
            - AddResponseHeader=X-Custom-Header, CustomValue  # 응답 헤더 추가
```
#### 헤더 제거하기
- Java DSL
```java
@Bean
public RouteLocator removeHeaderRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
            .route("remove-header-route", r -> r.path("/remove")
                    .filters(f -> f.removeRequestHeader("X-Unwanted-Header"))  // 헤더 제거
                    .uri("http://httpbin.org/headers")
            ).build();
}
```
- Yaml 설정
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: remove-header-route
          uri: http://httpbin.org/headers
          predicates:
            - Path=/remove
          filters:
            - RemoveRequestHeader=X-Unwanted-Header  # 요청 헤더 제거
```

#### TimeOut 설정하기
```yml
spring:
  cloud:
    gateway:
      httpclient:
        connect-timeout: 1000
        response-timeout: 5s
```
---
> **참고사이트**
> - [01-SPRING CLOUD GATEWAY를 이용한 API GATEWAY 구축기](https://saramin.github.io/2022-01-20-spring-cloud-gateway-api-gateway/)
> - [02-API Gateway, MSA의 출입구 ( Spring Cloud Gateway )](https://velog.io/@black_han26/Spring-Cloud-Gateway-%EA%B5%AC%ED%98%84)
> - [03-API 게이트웨이와 Spring Cloud Gateway](https://f-lab.kr/insight/understanding-api-gateway)
> - [04-API Gateway](https://velog.io/@ekxk1234/API-Gateway)
> - [05-Spring Cloud Gateway 생성하기](https://velog.io/@korea3611/Spring-Boot-Spring-Cloud-Gateway-%EB%A7%8C%EB%93%A4%EA%B8%B0-MSA2)