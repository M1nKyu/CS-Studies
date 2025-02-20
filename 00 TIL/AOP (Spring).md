---
created: 2025-02-14 17:14
category:
  - design-pattern
  - spring
tags:
  - TIL
last_modified: 2025-02-20T20:06:00
---
### 🍪 관점 지향 프로그래밍(Aspect-Oriented Programming, AOP)
- 객체 지향 프로그래밍 패러다임을 보완하는 기술.

- 메소드나 객체의 기능을 핵심 관심사(Core Concern)과 공통 관심사(Cross-cutting Concern)로 나누어 보고, 그 관점을 기준으로 모듈화하는 것.
	- `핵심 관심사`: 각 객체가 가져야 할 본래의 기능(핵심 비즈니스 로직).
	- `공통 관심사`: 여러 객체에서 공통적으로 사용되는 기능(로깅, 보안, 트랜잭션 처리 등).

- 여러 개의 클래스에서 반복해서 사용하는 코드가 있다면 해당 코드를 모듈화하여 공통 관심사로 분리한다.

#### 🍬 AOP의 주요 개념
- **Aspect**:
	- 공통 관심사를 코드의 여러 곳에 적용할 수 있도록 모듈화한 것.
	- ex: 로깅, 트랜잭션 관리
- **Join Point**:
	- AOP를 적용할 수 있는 지점
	- ex: 메서드 실행, 메서드 호출 등
- **Advice**: 
	- Join Point에서 실행할 코드.
	- AOP에서 수행하려는 동작을 정의한다.
	- Before(타겟 메서드 실행 전), After(실행 후), Around(실행 전후)
- **Pointcut**:
	- 어떤 Join Point에서 Advice가 실행될 지를 결정하는 조건.
- **Weaving**:
	- Aspect를 실제 코드에 결합하는 과정.
	- 컴파일 타임, 로딩 타임, 런타임 중에 일어날 수 있다.

- ![AOP|400x350](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F994AA3335C1B8C9D28)
	- [이미지 출처](https://engkimbs.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81AOP)
	- 반복해서 쓰는 코드가 Cross-cutting Concern이다.
---
### 🍪 Spring AOP란?
- 스프링 프레임워크에서 제공하는 기능 중 하나로, AOP를 지원하는 기술이다.
#### 🍬 특징
1. 프록시 기반 AOP:
	- Spring AOP는 프록시 패턴을 사용하여 AOP 기능을 구현한다.
	- Spring은 대상 비즈니스 로직이 호출되기 전에 프록시 객체를 사용하여 기능을 추가하거나 수정한다.
2. 런타임 AOP:
	- 런타임에 적용되는 AOP로, 컴파일이나 클래스 로딩 시점에서 변환되지 않는다.
3. 핵심 관심사와 공통 관심사를 분리하여 애플리케이션의 유지보수를 용이하게 한다.

#### 🍬 Spring AOP의 어노테이션
- **@Aspect**
	- AOP의 핵심 기능을 나타내는 클래스에 사용한다.
	- 이 클래스는 Aspect 역할을 한다.
- **@Before**
	- 메서드 실행 전 Advice.
- **@After**
	- 메서드 실행 후 Advice.
- **@Around**
	- 메서드 실행 전후에 동작하는 Advice.
- **@Pointcut**
	- Advice를 실행할 Join Point를 정의하는 데 사용한다.
---
### 🍪 Spring AOP 예제
> 로그를 기록하는 Aspect를 만드는 예제

#### 🍬 의존성
- `spring-boot-starter-aop` 의존성이 필요하다.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
#### 🍬 AOP Aspect 클래스
- 이 클래스는 로깅을 위해 사용할 것이다.
```java
@Aspect  // AOP 기능을 나타내는 클래스
@Component  // Spring의 Bean으로 등록
public class LoggingAspect {

    // 지정한 메서드 실행 전에 로그를 출력하는 Advice
    // `com.example.demo.service` 패키지 내 모든 메서드에 적용된다.
    @Before("execution(* com.example.demo.service.*.*(..))")  
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Executing method: " + joinPoint.getSignature().getName());
    }
}
```
#### 🍬 서비스 클래스 
- 비즈니스 로직을 처리하는 메서드
```java
@Service
public class MyService {

    // 비즈니스 로직을 처리하는 메서드
    public void process() {
        System.out.println("Processing some business logic...");
    }

    public void anotherMethod() {
        System.out.println("Executing another method...");
    }
}
```
#### 🍬 애플리케이션 설정
```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Bean
    public CommandLineRunner demo(MyService myService) {
        return args -> {
            // 서비스 메서드 호출
            myService.process();
            myService.anotherMethod();
        };
    }
}
```
#### 🍬 결과
- 애플리케이션을 실행하면, `MyService`의 메서드가 호출될 때마다 AOP Aspect에 정의된 `logBefore()` 메서드가 실행되어 로그를 출력한다.
```
Executing method: process 📌
Processing some business logic...
Executing method: anotherMethod 📌
Executing another method...
```
---
## 🧙‍♂️ 요약
- AOP는 핵심 관심사와 공통 관심사를 분리하여 코드 중복을 줄이고, 유지보수를 용이하게 만든다.

- 주요 개념과 Spring AOP 어노테이션
	- Aspect: 공통 관심사를 모듈화한 것. (`@Aspect`)
	- Join Point: AOP를 적용할 수 있는 지점. 
	- Advice: Join Point에서 실행될 코드 (`@Before`, `@After`, `@Around`)
	- Pointcut: 어떤 Join Point에서 Advice가 실행될지 결정하는 조건. (`@Pointcut`)
	- Weaving: Aspect를 실제 코드에 결합하는 과정.

- Spring AOP 특징
	- 프록시 기반 AOP
	- 런타임 AOP
	- 핵심 관심사와 공통 관심사의 분리
---
> **참고사이트**
> - [01-스프링 AOP](https://engkimbs.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81AOP)
> - [02-Spring Boot AOP(Aspect-Oriented Programming) 이해하고 설정하기](https://adjh54.tistory.com/133)
