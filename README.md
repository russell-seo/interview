# interview
  네카라쿠배 가즈아~

  ## 기술 면접 관련 기록
  
  
<details>
<summary>Spring Bean</summary>
<div markdown="1">       

 - Spring Bean 이란?
    - Spring 에서 사용하는 객체를 의미한다.
    - IoC Container에 의해 등록, 생성, 조회 관계 설정이 되는 객체를 의미한다.
    - Spring IoC Container 에 의해서 관리되고 어플리케이션의 핵심을 이루는 객체들을 스프링에서는 Beans라고 부른다.
      - 빈과 빈 사이의 의존성은 컨테이너가 사용하는 메타데이터 환경설정에 의존한다.
 - Bean의 주요 속성
    - class : 정규화된 자바 클래스 이름
    - id : bean의 고유 식별자
    - scope : 빈 스코프
    - constructor-arg : 생성시 생성자에 전달할 인수
    - property : 생성 시 bean setter 에 전달할 인수
    - init method 와 destory method
</div>
</details>

<details>
<summary>POJO</summary>
<div markdown="1">       

 - POJO 란?
    - 진정한 POJO란 객체지향적인 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트를 말한다.
        - 어떠한 프레임워크에도 의존하지 않는다.
        - 자바를 이용한 테스트에 용이하다.
 - POJO의 두가지 의견
    - 어떠한 프레임워크에도 완전히 의존하지 않는 자바 객체. (어노테이션이 붙은 것도 POJO가 아니란 의견)
    - 자바 객체 안에 코드를 프레임워크를 바꿔도 그대로 재활용 할 수 있으면 POJO(특정 어노테이션이 존재해도 POJO라는 의견)

 - EJB 부터 스프링까지의 역사를 보면 두 번째가 맞는 듯 하다.
    - EJB 시절에는 특정 기능(Service, Controller등) 을 만들기 위해서는 특정 인터페이스나 클래스는 extends 했어야 했다. 따라서 그 시절에는 특정 클래스의 EJB 프레임워크에
      매우 의존적이었으며 기능을 활용하기 위해서 특정 Class를 extends 해야한다는 관점에서 객체지향적 특징을 잃어버리게 되었다.
    - EJB때는 걔네들이 정의해둔 클래스/인터페이스를 상속/구현 -> 그래서 이거에 종속적이지 않는 것들은 POJO라고 부르자고 정한 것.
    - 결론적으로 비지니스 코드가 특정 프레임워크에만 종속적이지 않다면 POJO라고 부른다.(어노테이션은 주석과 같이 마킹한다는 의미에서 코드에 직접적으로 영향을 주지 않으므로 제외)

</div>
</details>


<details>
<summary>스프링 핵심 원칙</summary>
<div markdown="1">       
  - 스프링 핵심 원칙은 세가지이다. IoC/DI, AOP, PSA
    - IoC
      - Inversion Of Control(제어의 역전)을 의미하며, 객체의 생성과 생명주기 관리까지 모든 객체에 대한 제어권을 개발자가 아닌 프레임워크 에게 위임한 것을 의미한다.
      - 객체의 생성 책임을 개발자가 가지는 것이 아니라, 프레임워크에 위임했다(능동 -> 수동)
      - IoC vs DI
        - IoC는 DI한 형태 -> 객체지향에선 DI를 통해 IoC를 구현한다.
  
    - DI
      - DI는 의존관계 주입을 의미한다. 의존관계란 하나의 객체가 다른 객체의 상태에 따라 영향을 받는 것을 의미한다.
      - 스프링에서는 이러한 의존관계를 개발자가 직접관리하지 않고, 스프링 컨테이너에서 관리한다. 의존관계가 필요할 때 마다 스프링 컨테이너에서 개발자 코드안으로 의존성을 주입해 준다.
      - DI는 스프링에서 IoC를 구현한 한가지 방법이며, IoC는 DI를 포함하는 개념이다.
      - 이를 통해 개발자는 객체의 생성, 생명주기 관리, 의존관계 설정 책임을 신경 쓸 필요 없이 자신의 비즈니스 로직에만 집중하여 생산성을 높일 수 있다.
  
  
    - AOP
      - Aspect-Oriented Programming(관점 지향 프로그래밍)을 의미한다.
      - 스프링 DI가 의존성에 대한 주입이라면 AOP는 로직(code)주입 이라고 할 수 있다.
      - 관점 지향은 쉽게 말해 어떤 로직을 핵심적인 관점과 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화 하겠다는 것이다.
        - 핵심적인 관점 : 비즈니스 로직
        - 부가적인 관점 : 핵심 로직을 실행하기 위해서 행해지는 로직( 로깅, 트랜잭션, 캐싱) - 재사용된다.
  
    - PSA
      - Portable Service Abstraction(일관성 있는 서비스 추상화)를 의미한다.
      - 서비스 추상화란, 같은 일을 하는 다수의 기술을 공통 인터페이스로 제어할 수 있게 하는 것을 의미한다.
      - 외부 환경의 변화에 관계없이 일관된 방식으로 기술에 접근할 수 있게 해주는 것을 의미한다.
      - 예시
        - @Cacheable : 캐시대상으로 redis를 사용하던 ehcach를 사용하던 @Cacheable을 처리하는 내부 코드는 변하지 않는다.
        - @Transactional : JPA의 구현체로 Hibernate를 이용하던 다른 구현체를 이용하던 @Transactional을 처리하는 내부 코드를 변경할 필요가 없다.

</div>
</details>


<details>
<summary>Servlet vs Servlet Container</summary>
<div markdown="1"> 
  
    - Servlet
      - Java 로 HTTP 요청 및 응답을 처리하기 위한 표준
      - 서블릿은 클라이언트의 HTTP요청을 받아 비즈니스 로직을 수행하고 적절한 HTTP 응답을 생성하는 자바 객체이다.
      - 웹페이지를 동적으로 생성하는 역할
      - 서블릿은 일반 자바 객체와 달리 서블릿 컨테이너 내에서만 실행된다.
  
    - Servlet Container
      - 클라이언트로 부터 HTTP 요청 메시지를 적절하게 파싱 한 후, 쓰레드를 생성하여 적절한 서블릿을 실행시키고, 서블릿으로부터 응답받은 요청 처리 결과를 이용해 HTTP 응답 메시지를 만들어주는 컴포넌트
      - 웹 서비스에 필요한 다양한 기능을 제공하며, 개발자로 하여금 비즈니스 로직(Servelt 구현) 만 집중할 수 있도록 도와주는 프레임 워크
      - 지원하는 기능
        - tcp/ip 소켓 연결 및 종료(통신 지원)
        - HTTP요청 메시지 파싱 및 응답 메시지 생성
        - 서블릿 생명주기 관리
        - 멀티쓰레딩 지원(요청당 스레드로 처리)
        - 선언적인 보안 관리
        - 대표적인 Servlet Container : tomcat, netty
</div>
</details>

<details>
<summary>Servlet Container 동작 흐름</summary>
<div markdown="1">
      
      - 사용자 요청 파싱
      - 새로운 쓰레드를 생성하고, HttpServeltRequest, HttpServletResposne 생성.
      - 사용자 요청을 분석하여 대응되는 서블릿 검색(DD.xml을 통해 서블릿을 미리 정의해둔다.)
      - 찾은 서블릿의 service() 메소드를 호출함으로써, 비즈니스 로직 처리 위임
      - 서블릿은 클라이언트에게 넘길 응답을 작성. 이때 Response 객체를 사용한다.
      - Servlet Container가 서블릿으로 부터 받은 Response를 적절한 Http response로 만들어 클라이언트에 반환
      - 요청을 처리한 쓰레드는 소멸하거나 쓰레드 풀로 반환.
  
</div>
</details>
