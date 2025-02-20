### Spring MVC

###### MVC2 모델이 기반인 웹 모듈. 스프링 프레임워크에서 MVC2 모델을 좀 더 발전시켜 나옴

##### MVC2 패턴

![MVC2 패턴](<스크린샷 2024-11-25 오후 8.51.24.png>)

- 요청을 하나의 컨트롤러(Servlet)가 먼저 받음
- 서블릿은 요청에 대한 비즈니스 로직을 처리한 후, 이를 JSP 파일에 반영하는 역할을 수행
  > JSP ❓
  > JavaServer Pages의 약자로, Java를 기반으로 한 웹 애플리케이션에서 동적인 웹 페이지를 생성하기 위해 사용되는 기술. HTML 코드 안에 Java 코드를 삽입할 수 있게 해주며, 서버에서 실행되어 클라이언트에게 HTML을 반환함.
- MVC2 패턴에서 JSP는 사용자에게 보여지는 뷰(View)를 담당하며, 서블릿이 처리한 비즈니스 로직의 결과를 표시하는 역할을 함

##### Spring MVC 동작 구조

![Spring MVX](<스크린샷 2024-11-25 오후 8.55.56.png>)

1. 클라이언트가 서버에 요청을 하면, Front Controller인 DispatcherServlet 클래스가 요청을 받음
2. DispatcherServlet은 프로젝트 파일 내의 servlet-context.xml 파일의 @Controller 인자를 통해 등록한 요청 위임 컨트롤러를 찾아 매핑(mapping)된 컨트롤러가 존재하면 @RequestMapping을 통해 요청을 처리할 메소드로 이동함
3. 컨트롤러는 해당 요청을 처리한 Service(서비스)를 받아 비즈니스 로직을 서비스에게 위임
4. Service는 요청에 필요한 작업을 수행하고, 요청에 대해 DB에 접근해야 한다면 DAO에 요청하여 처리를 위임
5. DAO는 DB 정보를 DTO를 통해 받아 서비스에게 전달
6. 서비스는 전달받은 데이터를 컨트롤러에게 전달
7. 컨트롤러는 Model(모델) 객체에게 요청에 맞는 View 정보를 담아 DispatcherServlet에게 전송
8. DispatcherServlet은 ViewResolver에게 전달받은 View 정보를 전달
9. ViewResolver는 응답할 View에 대한 JSP를 찾아 DispatcherServlet에게 전달
10. DispatcherServlet은 응답할 뷰의 Render를 지시하고 뷰는 로직을 처리
11. DispatcherServlet은 클라이언트에게 Rending된 뷰를 응답하며 요청을 마침
