---
created: 2025-02-14 19:13
category:
  - spring
tags:
  - TIL
last_modified: 2025-02-20T19:11:00
---
### 🍪 서블릿의 한계와 템플릿엔진
- 서블릿은 일일이 문자열로 작성하는 비효율적인 작업이 필요하다.
- 더군다나 String이므로 틀린 부분을 찾아내기도 힘들다.
- 이를 보다 편리하게 작성할 수 있도록 하는 것이 템플릿 엔진이다.
- 템플릿 엔진은 HTML 문서에서 프로그래밍이 필요한 부분만 코드를 적용해서 동적으로 변경할 수 있다.
	- ex: JSP, Thymeleaf, Freemarker ...
### 🍪 MVC 패턴
> Servlet이나 JSP로 작성된 코드에는 비즈니스 코드와 서비스 로직이 한 파일에 포함되어 있다. 이는 유지보수에 매우 불리하다.

- MVC 패턴은 템플릿 엔진에 집중된 로직을 분리하기 위해 등장했다. 
- MVC 는 Model, View, Controller의 약자이며, 
	- MVC 패턴은 하나의 애플리케이션을 구성할 때 구성요소를 이 세가지 역할로 구분한 디자인패턴이다.

#### MVC 패턴의 한계
- 하나의 매핑이 추가될 때마다 Servlet을 생성해야 하고, 매번 중복되는 viewPath로 `forward()`를 호출한다.
	- `forward(): 서버 내부에서 요청을 다른 리소스로 전달하는 방식`
- 경우에 따라 HttpServletRequest, HttpServletResponse를 사용하지 않고 있으며, 이 객체들을 사용하는 코드는 TC를 작성하기도 어렵다.
- 공통 처리가 어렵다.
---
## 🧙‍♂️ 요약
- 서블릿:
	- HTML을 String으로 직접 작성해야 한다. -> 틀린 부분을 찾아내기 힘들다.
	- 이를 편리하게 작성하도록 돕는 것이 템플릿엔진.

- 템플릿 엔진:
	- HTML 문서에서 필요한 부분만 코드를 적용해 동적 변경을 할 수 있도록 돕는다.

- MVC 패턴:
	- 서블릿과 JSP의 비효율적인 코드(뷰와 비즈니스 로직 혼합)를 해결하기 위해 등장했다.
---
> **참고사이트**
> - [01-서블릿의 한계, 템플릿 엔진, MVC 패턴](https://bojalgorism.tistory.com/162)
> - [02-MVC 패턴이 등장한 이유 - 서블릿, JSP 방식의 한계](https://velog.io/@woply/spring-MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4-%EB%93%B1%EC%9E%A5%ED%95%9C-%EC%9D%B4%EC%9C%A0-%EC%84%9C%EB%B8%94%EB%A6%BF-JSP-%EB%B0%A9%EC%8B%9D%EC%9D%98-%ED%95%9C%EA%B3%84)
> - [03](https://velog.io/@choidongkuen/%ED%85%9C%EB%B8%94%EB%A6%BF-%EC%97%94%EC%A7%84-%EB%84%8C-%EB%AD%90%EB%8B%88)
