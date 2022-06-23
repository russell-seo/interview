# interview
  네카라쿠배 가즈아~
  
  [카카오스타일 백엔드 면접 질문](https://github.com/russell-seo/interview/blob/main/kakao/style.md)
  
  [오늘 식탁 백엔드 면접 질문](https://github.com/russell-seo/interview/blob/main/todaytable.md)

  ## 기술 면접 관련 기록
  
  
  ## Spring
  
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
<summary>Spring 서버 동작 과정</summary>
<div markdown="1">
  
  - 스프링의 경우
  
        1. 톰캣이 실행된다
        2. ServletContextListener 의 스프링에서 제공하는 구현체인 ContextLoaderListener 에 의해 Application Context가 생성된다.
        3. Application Context 가 생성되는 과정에서, 빈 정의(xml, component scan, @Configuration)에 의해 빈이 생성된다.
        4. Application Context 에 저장된 빈들의 의존 관계가 주입된다.
        5. 빈들의 생명주기에 맞는 메소드가 실행된다.(빈의 초기화 메소드, 소멸 메소드 등)
  
  - 스프링 부트
  
        1. @SpringBootConfiguration
        2. @Component Scan
        3. @EnableAutoConfiguration
</div>
</details>

<details>
<summary>Proxy</summary>
<div markdown="1">
  
  - 일반적으로 Proxy는 실제 Target 의 기능을 대신 수행하면서, 기능을 확장하거나 추가하는 실제 객체를 의미합니다.
    
    - 왜 사용할까?
      - OCP(Open-Closed Principle)을 지키기 위해서 사용한다.
      - 개방 폐쇠 원칙(=OCP)란 소프트웨어는 확장에 대해 열려있어야 하고, 수정에 대해서는 닫혀 있어야 한다라는 원칙 때문이다.
    
    - 어떻게?
      - Proxy를 이용한 런타임 위빙(Runtime Weaving)을 통해서 관심사를 추출할 수 있다.
      - 런타임 위빙 이란?
        - Weaving 은 target 객체를 새로운 proxied 객체로 적용시키는 과정이다.
      
    - Spring AOP 에서는 이러한 기능을 2가지 방법으로 구현하였다.
      
      - JDK Dynamic Proxy
      - CGlib Proxy
  
        - JDK Dynamic Proxy
          - JDK에서 제공하는 Dynamic Proxy는 1.3버전 부터 생긴 기능이다
          - `인터페이스를 기반으로 Proxy를 생성해주는 방식이다`
          - Dynamic Proxy는 Invocation Handler를 상속받아서 실체를 구현하게 되는데, 이 과정에서 `특정 Object 에 대해 Reflection을 사용하기때문에 성능이 조금 떨어지는 크리티컬 한 단점이 있다.`
          
        - CGlib
          - Enhancer를 바탕으로 Proxy를 구현하는 방식이다.
          - Extends(상속)방식을 이용해서 Proxy화 할 메소드를 오버라이딩 하는 방식이다.
          - CGlib은 기본적으로 Byte코드를 조작해서, 바이너리가 만들어지기 때문에 JDK Dynamic Proxy 보다 성능적으로 우세하다.
          - CGlib에서 실제로 핸들링할 Handler 가 필요한데, CGlib 에서는 이를 `MethodInterceptor` 라는 인터페이스로 정의되어 있다.
          - 다만 final 객체 혹은 private 접근자로 된 메소드는 상속이 지원되지 않기 때문에 제약적인 Proxy 구현이 가능하다.
  
</div>
</details>

<details>
<summary>Proxy 패턴의 문제점</summary>
<div markdown="1">
  
  - 위의 Proxy 내용과 이어지는 내용이다.
  
  - Proxy 패턴의 문제점을 알아보자
  
    - 부가적인 기능 추가 때 마다 별도의 프록시를 만들어야 한다.
    - 프록시로 프록시를 감싸는 일 발생
    - 모든 구현체에서 원래 타겟으로 위임하는 코드가 중복해서 발생.
    - 다른 메소드의 부가 기능이 같은 것일 경우 부가 기능의 주옵ㄱ이 발생
      - 이러한 문제를 해결하기 위해 프록시에 해당하는 클래스를 매번 만드는 것이 아닌 동적으로 런타임에 생성해내는 `다이나믹 프록시`가 등장한 것이다.
  
  
</div>
</details>

<details>
<summary>트랜잭션 격리 수준(Isolation Level)</summary>
<div markdown="1">
  
  - 트랜잭션 격리수준 이란?
    - 동시에 여러 트랜잭션이 처리될 때, 트랜잭션끼리 얼마나 서로 고립되어 있는지를 나타내는 것입니다. 즉 간단하게 말해 특정 트랜잭션이 다른 트랜잭션에 변경한 데이터를 볼수 있도록 허용할지 말지를 결정하는 것 입니다.
  
  - READ UNCOMMITTED
  - READ COMMITTED
  - REPEATABLE READ
  - SERIALIZABLE
  
  아래로 내려 갈 수록 고립 정도가 높아지며, 성능이 떨어지는 것이 일반적입니다.
  
  현재 저는 mysql 기반인 MariaDB를 사용 중인데 격리 수준이 디폴트인 `REPEATABLE READ` 입니다. 오라클인 경우에는 `READ COMMITTED`라고 합니다.
  
  1. READ UNCOMMITTED
  
  READ UNCOMMITTE의 격리수준에서는 어떤 트랜잭션 변경 내용이 COMMIT 이나 ROLLBACK과 상관없이 다른 트랜잭션에서 보여집니다.
  
  이 격리수준 에서는 아래와 같은 문제가 발생합니다.
  
    1. A 트랜잭션에서 테이블의 데이터를 수정중인 상태이고 아직 커밋하지 않았습니다.
  
    2. B 트랜잭션에서 A 트랜잭션이 수정 중인 데이터를 조회 함. 이를 Dirty Read라고 한다.
  
    3. A 트랜잭션에서 문제가 발생해 ROLLBACK 함
  
    4. B 트랜잭션은 커밋되지 않은 데이터를 바라보고 로직을 수행한다.
  
  이런식으로 데이터 정합성에 문제가 많으므로 RDBMS 표준에서는 격리수준으로 인정하지 않는다.
  
  2. READ COMMITTED
  
  어떤 트랜잭션의 변경 내용이 COMMIT 되어야만 다른 트랜잭션에서 조회할 수 있다. 오라클 DBMS 에서 기본적으로 사용하고 있고, 온라인 서비스에서 가장 많이 격리되는 수준이다.
  
  여기서는 B 트랜잭션에서 A 트랜잭션에서 커밋이나 롤백하기전 까지는 DIRTY READ가 발생하지 않습니다.(UNDO 영역에 저장된 데이터를 참조)
  
  언뜻보면 정합성 문제가 해결된 것 처럼 보이지만, 여기서 NON-REPEATABLE READ 부정합 문제가 발생할 수 있다.
  
    1. B 트랜잭션에서 10번 상품의 총 투자공모금액을 조회
    2. 100만원이 조회됨
    3. A 트랜잭션에서 10번 상품의 총 투자공모금액을 120만원으로 바꾸고 커밋
    4. B 트랜잭션에서 10번 상품의 총 투자공모금액을 다시 조회
    5. 120만원이 조회됨.
  
  이는 하나의 트랜잭션내에서 동일한 SELECT를 수행했을 경우 항상 같은 결과를 반환해야 하는 REPEATABLE READ 정합성에 어긋나는 것 입니다.
  
  일반적인 웹 어플리케이션에서는 크게 문제되지 않지만, 작업이 금전적인 처리와 연결되어 있다면 문제가 발생할 수 있습니다.
  
  예를 들어 트랜잭션에서 입금/출금 처리가 계속 진행되는 트랜잭션들이 있고, 오늘의 입금 총 합을 보여주는 트랜잭션이 있다고 하면, 총합을 계산하는 SELECT 쿼리는 실행될 때 마다 다른 결과 값을 가져올 것 입니다.
  
  이런 문제가 발생할 수 있기 때문에 격리수준에 의해 실행되는 SQL 문장이 어떤 결과를 출력할 지 정확히 예측하고 있어야 합니다.
  
  
  3. REPEATABLE READ
  
  REPEATABLE READ 격리수준은 간단하게 말해서 트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리수준 이다.
  
  MYSQL DMBS에서 기본적으로 사용하고 있고, 이 격리수준에서는 NON-REPEATABLE READ 부정합이 발생하지 않는다.
  
    1. 10번 트랜잭션이 2번 상품을 조회
    2. 12번 트랜잭션이 2번 상품의 총 투자공모금액을 변경하고 커밋
    3. 10번 트랜잭션이 2번 상품을 다시 조회
    4. 언두 영역에 백업된 데이터를 반환
  
  즉, 자신의 트랜잭션 번호보다 낮은 트랜잭션 번호에서 변경된(+커밋된) 것만 보게 되는 것 입니다.
  
  REPEATABLE READ 격리수준에서는 트랜잭션이 시작된 시점의 데이터를 일간되게 보여주는 것을 보장해야 하기 때문에 한 트랜잭션 실행시간이 길어질수록 해당 시간 만큼 계쏙 멀티 버전을 관리해야 하는 단점이 있다.
  
  하지만 실제로 영향을 미칠 정도로 오래 지속되는 경우는 없어서 READ COMMITTED와 REPEATABLE READ의 성능 차이는 거의 없다고 합니다.
  
  REPEATABLE READ에서 발생할 수 있는 데이터 부정합
  ~~~
  START TRANSACTION; -- transaction id : 1
  select * from Member where name = "sangwon";
  
    START TRANSACTION; -- transaction id : 2
    select * from Member where name = "sangwon";
    update Member SET name = "russell" where name = "sangwon";
    COMMIT;
  
   UPDATE Member SET name = "gucci" where name = "sangwon"; -- 0 row(s) affected
   COMMIT;
  ~~~
  
  이 상황에서 최종 결과는 `name = russell`이 된다. REPEATABLE READ이기 때문에 2번 트랜잭션에서 name = russell 로 변경하고 commit을 하면
  
  name = sangwon의 내용을 `UNDO` 세그먼트에 남겨놔야 합니다. 그래야 1번 트랜잭션이 일관되게 데이터를 보는 것을 보장해 줄 수 있기 때문입니다.
  
  이 상황에서 아래 구문에서 UPDATE문을 실행하게 되는데, UPDATE의 경우 변경을 수행할 로우에 대해 잠금이 필요합니다. 하지만 1번 트랜잭션이 바라보고 있는 
  
  name = sangwon의 경우 레코드데이터가 아닌 UNDO영역의 데이터이고, UNDO영역에 있는 데이터에 대해서는 쓰기 잠금을 걸 수가 없습니다.
  
  그러므로 위에 UPDATE 구문은 레코드에 대해 쓰기 잠금을 시도하려고 하지만 name=junyoung인 레코드는 존재하지 않으므로, 0 row(s) affected가 출력되고 아무 변경도 일어나지 않게 됩니다. 그러므로 최종 결과는 name=russell 입니다.
  
</div>
</details>

<details>
<summary>@Transcational Propagation</summary>
<div markdown="1">
  
  - Propagation
  
    - `REQUIRED` 
      - 기본옵션
      - 부모 트랜잭션이 존재한다면 부모 트랜잭션에 합류, 그렇지 않다면 새로운 트랜잭션을 만든다.
      - `중간에 자식/부모에서 rollback이 발생한다면 자식과 부모 모두 rollback 한다.`
    - `REQUIREDS_NEW`
      - 무조건 새로운 트랜잭션을 만든다.
      - nested한 방식으로 메소드 호출이 이루어지더라도 rollback은 각각 이루어 진다.
    - `MANDATORY`
      - 무조건 부모 트랜잭션에 합류시킨다
      - 부모 트랜잭션이 존재하지 않는다면 예외를 발생시킨다.
    - `SUPPORTS`
      - 메소드가 트랜잭션을 필요로 하지는 않지만, 진행중인 트랜잭션이 존재하면 트랜잭션을 사용한다는 것을 의미한다.
      - 진행중인 트랜잭션이 존재하지 않더라도 메소드는 정상적으로 동작한다.
    - `NESTED`
      - 부모 트랜잭션이 존재하면 부모 트랜잭션에 중첩시키고, 부모 트랜잭션이 존재하지 않는다면 새로운 트랜잭션을 생성한다.
      - 부모 트랜잭션에 예외가 발생하면 자식 트랜잭션도 rollback한다.
      - 자식 트랜잭션에 예외가 발생하더라도 부모 트랜잭션은 rollback하지 않는다. 이때 롤백은 부모 트랜잭션에서 자식 트랜잭션 호출하는 지점까지만 롤백된다. 이후 부모 트랜잭션에서 문제가 없으면 부모 트랜잭션은 끝가지 COMMIT 된다.
      - 현재 트랜잭션이 있으면 중첩 트랜잭션 내에서 실행하고, 그렇지 않으면 REQUIRED 처럼 동작합니다.
     
    - `NEVER`
      - 메소드가 트랜잭션을 필요로 하지 않는다. 만약 진행중인 트랜잭션이 존재하면 익셉션이 발생한다.
  
  
  
</div>
</details>

<details>
<summary>Spring Transaction vs Spring boot Transaction</summary>
<div markdown="1">
  
  
  
  
</div>
</details>
  
  ## JAVA
  
  
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

<details>
<summary>Parameter 와 Argument의 차이</summary>
<div markdown="1">
  
  - parameter : 함수를 선언할때 사용된 변수
  - argument : 함수가 호출 되었을 때 함수의 파라미터로 넘어오는 실제 값

</div>
</details>






<details>
<summary>추상클래스와 인터페이스의 차이</summary>
<div markdown="1">
  
  - 추상클래스
    - 단일 상속만 가능하다.
    - 모든 접근 제어자를 사용할 수 있다.
    - 변수와 상수를 선언 할 수 있다.
    - 추상 메소드와 일반 메소드를 선언할 수 있다.
  - 인터페이스
    - 다중 구현이 가능하다.
    - public 접근 제어자만 가능하다.
    - 상수만 선언 할 수 있다.
    - 추상메소드만 선언 할 수 있다.

</div>
</details>


<details>
<summary>오버라이딩(Overriding) 과 오버로딩(Overloading)</summary>
<div markdown="1">

    - 오버라이딩(Overriding) : 상위 클래스가 가지고 있는 메소드를 하위 클래스에서 재정의 하는 기술
    - 오버로딩(Overloading) : 매개변수의 유형과 개수를 변경하면서 같은 이름의 메소드를 여러 개 사용하는 기술
  
</div>
</details>


<details>
<summary>싱글톤 패턴</summary>
<div markdown="1">
  
  - 싱글톤 패턴이란?
    - 애플리케이션이 시작될 때, 어떤 클래스가 최초 한번만 메모리를 할당(static)하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴
    - 즉 싱글톤 패턴은 하나의 인스턴스만 생성하여 사용하는 디자인패턴
    - 인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용하는 것
  - 왜 쓰나?
    - 먼저 객체를 생성할 때 마다 메모리 영역을 할당받아야 한다. 하지만 한번의 new 를 통해 객체를 생성한다면 메모리 낭비를 방지 할 수 있다.
    - 또 싱글톤으로 구현한 인스턴스는 전역 이므로 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능한 장점이 있다.

  - 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
  - 그래서 스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤(1개만 생성) 으로 관리한다.
  - 지금까지 써 왔던 스프링 빈이 바로 싱글톤 패턴으로 관리되는 빈이다.
 
</div>
</details>


<details>
<summary>call by value vs call by reference</summary>
<div markdown="1">
  
  - call by value(값에 의한 호출)
    - 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시공간이 생성됨(종료 시 해당 공간 사라짐)
    - call by value 호출 방식은 함수 호출 시 전달되는 변수 값을 복사해서 함수 인자로 전달함
    - 이때 복사된 인자는 함수 안에서 지역적으로 사용되기 때문에 local value 속성을 가짐
    - 따라서 함수 안에서 인자 값이 변경되어도, 외부 변수 값은 변경 안됨.
  
  - call by reference(참조에 의한 호출)
    - call by reference 호출 방식은 함수 호출 시 인자로 전달되는 변수의 레퍼런스를 전달함
    - 따라서 함수 안에서 인자 값이 변경되면, 아규먼트로 전달된 객체의 값도 변경됨.

</div>
</details>


<details>
<summary>생성자 주입</summary>
<div markdown="1">

  - 생성자 호출 시점에 딱 1번만 호출되는 것이 보장됩니다.
  - 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없습니다. 따라서 불변하게 설계 할 수 있습니다.
  - 생성자 주입을 사용하면 의존성 주입을 누락하는 것을 방지 할 수 있습니다. 컴파일 오류로 누락을 방지
  - setter 를 사용하면 final이 assgin reference 인데, 이를 변경할 수 있음.
  - 원하는 구현체를 주입할 수 있으며, 순수 자바코드로 테스트를 실행할 수 있습니다.
</div>
</details>

<details>
<summary>생성자 vs 정적 팩토리 메서드</summary>
<div markdown="1">
  
  - 생성자와 정적 팩토리 메서드의 차이는 정적 팩토리 메서드의 장단점으로 알 수 있다.
  - 정적 팩토리 메서드의 장점
    - 이름을 가질 수 있다.
    - 반드시 새로운 객체를 만들 필요가 없다. 불변 객체를 캐싱하거나, Validation을 처리할 수 있다.
    - 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
    - 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    - static 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도된다.
  
 - 단점
    - 상속하려면 public, protected 생성자가 필요하니, 정적 팩토리 메서드만 제공하면 하위 클래스를 만들 수 없다.
    - static 팩토리 메서드는 프로그래머가 찾기 어렵다.

  </div>
</details>

<details>
<summary>String 객체는 객체인데 왜 New 로 선언하지 않는가?</summary>
<div markdown="1">
  
  - `String`은 대표적 불변 객체로, String 상수 풀 영역에서 객체를 관리한다.
  - 즉 상수처럼 이미 선언된 String 객체가 있으면 이 영역에서 가져다 사용하고 없다면 여기에 새롭개 객체를 생성하여 사용한다.

  
</div>
</details>


<details>
<summary>""와 new String("") 차이</summary>
<div markdown="1">
  
  - `""`는 Heap 내의 별도 공간인 String 상수 풀 영역에 문자열을 생성하고, 같은 문자열은 한번만 생성한다.
    - String 상수 풀 영역에 생성되는 String 은 불변이다.
  - `new String()`은 일반 클래스와 동일하게 Heap에 문자열 객체로 생성된다.
  
</div>
</details>

<details>
<summary>JVM</summary>
<div markdown="1">
  
  - JVM 이란?
    - 자바 가상 머신
    - 자바 바이트코드를 실행할 수 있게 해주는 주체
  
  - 특징
    - WORA(Write Once, Run Anywhere)
      - JVM은 플랫폼에 독립적이며, 모든 자바 가상 머신은 자바 가상 머신 규격에 정의된 대로 자바 바이트 코드를 실행한다.(스펙)
      - 모든 자바 프로그램은 CPU나 운영체제의 종류와 상관없이 동일하게 동작한다.
      - 윈도우, 맥, 리눅스등 운영체제에 종속적이지 않다.
  - GC
    - 클래스 인스턴스는 사용자 코드에 의해 명시적으로 생성되고, GC에 의해 자동적으로 소멸된다.
  
</div>
</details>  


<details>
<summary>자바 프로그램의 동작 과정</summary>
<div markdown="1">
  
  1. JAVA 소스 코드 파일(.java)를 JAVA 컴파일러(javac)로 바이트코드(.class)로 변환
  
  2.  JVM 내에 있는 Class Loader가 runtime data area로 바이트 코드 파일을 적재한다.
      - Loading -> Linking -> Initializing
  
  3. JVM 내에 있는 execution engine(Interpreter, JIT Compiler, GC)이 runtime data area에 적재된 바이트 코드를 기계어로 변경해 명령어 단위로 실행한다.
  
</div>
</details>  

<details>
<summary>Thread-Safe란?</summary>
<div markdown="1">
  
  - Thread-Safe란 멀티 스레드 환경에서 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근해도 프로그램의 실행에 문제가 없음을 의미한다.
    - 다른 스레드로 인해 부수효과가 발생하지 않는 것을 의미
    - 두 개의 스레드가 동시에 데이터에 접근하여 변경한다면 계산 결과가 덮어씌워지기 때문에 Thread-Safe하게 만들어줘야한다.
  
  - Thread-Safe 예시
    - `synchronized` 붙은 메서드, `ConcurrentHashMap`등등
  
  - Thread-Safe 하게 만들기 위해선
    - Mutual Exclustion
      - Thread에 Lock 이나 Semaphore를 걸어서 공유하는 자원에는 하나의 thread만 접근 가능하게 만든다.
    
    - Thread-Local
      - 각 스레드에만 접근 가능한 저장소를 사용하여 동시 접근을 막는다.
  
    - 불변 객체
      - 객체 생성 이후에 값을 변경할 수 없도록 만든다.
  
</div>
</details>

<details>
<summary>깊은복사(Deep Copy) vs 얕은복사(Shallow Copy)</summary>
<div markdown="1">
  
  - `깊은 복사(Deep Copy)` 는 `실제 값`을 새로운 메모리 공간에 복사하는 것을 의미
  
  - `얕은 복사(Shallow Copy)`는 `주소 값`을 복사한다는 의미이다.
    
    - `얕은 복사`의 경우 주소 값을 복사하기 때문에, `참조하고 있는 실제 값은 같다.`
    
  ~~~java
  @Data
  private class CopyObject{
    private String name;
    private int age;
  }
  
  
  class Test{
  
    @Test
    void 얕은복사(){
        CopyObject original = new CopyObject("russell", 20);
        CopyObject copy = original; //얕은 복사
  
        copy.setName("mike");
    }
  }
  
  ~~~
  
  위 코드에서는 copy 객체에 set메소드를 통해 이름을 변경했는데, 실제 결과는 original 객체와 copy 객체 모두 값이 변경된다.
  
  `CopyObject copy = original` 의 코드에서 객체의 얕은 복사를 통해 `주소 값`을 변경했기 때문에 참조하고 있는 실제 값은 동일하고, `복사한 객체가 변경된다면 기존의 객체도 변경되는 것이다.`
  
  위 상태에 대한 메모리 구조는 아래와 같다.
  ~~~java
  CopyObject original = new CopyObject("russell", 20);
  CopyObject copy = original;
  ~~~
  
  ![image](https://user-images.githubusercontent.com/79154652/175191628-ccdcfd40-fdb7-4a51-8c84-e26c546c36be.png)

  

  original 인스턴스를 생성하면 Stack 영역에 참조값이, Heap 영역에 실제 값이 저장된다.
  
  그리고 얕은 복사를 통해 객체를 복사했기 때문에 copy 인스턴스는 original 인스턴스가 참조하고 있는 Heap 영역의 참조값을 동일하게 보고 있는 상태가 된다.
  
  그 후 set 메소드를 통해 값을 변경하면 동일한 주소를 참조하고 있기 때문에 아래와 같이 된다.
  
  ![image](https://user-images.githubusercontent.com/79154652/175191883-9a33730a-1f39-4181-88d2-8378ceff2798.png)

  따라서 코드로는 copy 객체의 name 만 변경했지만, `동일한 주소를 참조` 하고 있기 때문에 original의 객체에도 영향을 끼치게 된다.
  
  
  
  - 깊은 복사(Deep Copy)
    - 깊은 복사를 구현하는 방법은 여러가지가 있습니다.
      - Cloneable 인터페이스 구현
      - 복사 생성자
      - 복사 팩토리 등등...
  
    - Cloneable 인터페이스 구현
    ~~~java
       public class CopyObject implements Clonable {
        private String name;
        private int age;
  
    }
  
    @Override
    protected CopyObject clone() throws CloneNotSupportedException {
      return (CopyObject) super.clone();
    }
  
    @Test
    void 깊은복사() throws CloneNotSupportedException {
        CopyObject original = new CopyObject("russell", 20);
        CopyObject copy = original.clone();
        
        copy.setName("mike");
    }
    ~~~
  
    `깊은 복사를 통해 테스트를 진행해보면 얕은 복사와는 달리 original 인스턴스의 값은 변경되지 않는다.`
  
    original = russell;
    
    copy = mike;
  
    위와 같은 결과를 얻을 수 있다.
  
  
  
  - 복사생성자, 복사팩터리
  
  ~~~java
  public class CopyObject{
  
    private String name;
    private int age;
  
  
  /*복사 생성자*/
  public CopyObject(CopyObject original){
    this.name = original.name;
    this.age = original.age;
  }
  
  public static CopyObject copy(CopyObject original){
  
    CopyObject copy = new CopyObject();
    copy.name = original.name;
    copy.age = original.age;
    return copy;
  }
  
  
  
  @Test
  void 깊은복사(){
    CopyObject original = new CopyObject("russell", 20);
    CopyObject constructor = new CopyObject(original);
    CopyObject copyFactory = CopyObject.copy(original);
    
    constrctor.setName("mike");
    copyFactory.setName("jane");
  }
  }
  
  ~~~
  
  위와 같은 코드에서 original, constructor, copyFactory 이름을 찍어 보면 모두 다른 이름이 나오는 것을 볼 수 있다.
  
  ![image](https://user-images.githubusercontent.com/79154652/175193315-b8556850-7b6b-48ff-a9d5-87ecf6671630.png)

    
  
  
</div>
</details>  
  
  ## DB 
  
<details>
<summary>인덱스</summary>
<div markdown="1">

  - 인덱스란?
    - 인덱스는 테이블 에 대한 동작의 속도를 높여주는 자료구조이다.
      - 비유 : DB `인덱스`:`데이터` = 책 `색인` : `페이지 번호(책 내용)`
    - 인덱스는 데이터 저장 성능을 희생하고 데이터의 읽기 속도를 높이는 기능이다.
      - 인덱스는 데이터를 저장할때 항상 정렬해서 저장해야 하므로 저장하는 과정이 느리고 복잡하다. 대신에 정렬되어있는 값을 조회하는 것은 빠르다.
  
  
</div>
</details>

<details>
<summary>RDB는 인덱스는 어떠한 자료구조로 구현하는가?</summary>
<div markdown="1">
  
  - B-Tree 혹은 B+Tree
    - Root, branch, leaf 노드로 나뉘고 스스로 균형을 맞추는 트리이다.
    - 스스로 균형에 맞춰 데이터를 정렬하기 때문에 항상 O(logN)의 조회성능을 유지한다.

</div>
</details>

<details>
<summary>트랜잭션</summary>
<div markdown="1">

  - 트랜잭션이란?
    - 복수의 쿼리를 독립적으로 한 단위로 묶는 것, 더이상 나눌 수 없는 단위 작업
    - 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위
    - ex) 하나의 거래 완성(단위) = 구매 계좌에서 n만원 출금(작은 단위) + 판매자 계좌에서 n만원 출금(작은단위)
  - 트랜잭션의 성질(ACID)
    - Atomicity(원자성) - 단위
      - 원자 : 더이상 쪼갤수 없는 성질
      - 원자성이란 데이터의 변경이 수반되는 일련의 데이터 조작이 전부 성공할지 전부 실패 할지를 보증하는 구조이다.
      - COMMIT OR ROLLBACK
    - Consistency(일관성) - 무결성 제약 조건
      - 트랜잭션이 안전하게 수행된다는 것을 보장한다는 성질
      - 트랜잭션 수행 전/후에 데이터모델의 모든 제약 조건(기본키, 외래키, 도메인, 도메인 제약조건등)을 만족하는 것을 의미
      - ex) 통장의 잔고는 마이너스가 안된다는 제약 조건이 존재한다.
            
            만약 트랜잭션 과정 중 통장의 잔고가 마이너스가되면 롤백 되어 트랜잭션이 종료된다.
    - Isolation(독립성) - 병행 제어
      - 데이터 조작을 복수의 사용자가 동시에 실행해도 각각의 처리가 모순없이 실행 되는 것을 보증한다는 의미
      - 하나의 트랜잭션이 수행중 다른 트랜잭션이 끼어들지 못하도록 보장하는 것(Lock 처리)
    - Durability(지속성) - 영속화
      - 트랜잭션을 완료(COMMIT)를 하고 완료 통지를 받는 시점에서 트랜잭션이 영구적이 되어 그 결과를 잃지 않는 것
      - 컴퓨터가 종료되거나 시스템 장애가 나타나도 계속 저장되는 성질(RAM 이 아닌 SSD에 저장된 상태)
</div>
</details>

<details>
<summary>Index Range Scan 과 Table Full Scan</summary>
<div markdown="1">
  
  - Table Full Scan
    - 순차 I/O 방식과 MultiBlock I/O 방식으로 디스크를 읽어 한 블록에 속한 모든 레코드를 한번에 읽어들이는 방법.
  
  - Index Range Scan
    - 랜덤 I/O 와  Single Block I/O 로 레코드 하나를 읽기 위해 매번 I/O 를 통해 필요한 레코드를 읽는 방법
  
  - 무조건 Index Range Scan 이 좋은 것은 아니다.
    - 조금만 생각해보면 위와 같이 읽을 데이터가 일정량을 넘으면 Index Range Scan의 경우 매 인덱스 마다 데이터를 가져와야 함으로 다량의 디스크 I/O 가 발생하게 된다.
    - 그러므로 더 비효율적일 수도 있다.
    - 다만, 큰 테이블에서 소량 데이터를 검색할 때는 당연히 Index Range Scan이 유용하다.

</div>
</details>

<details>
<summary>DB 정규화</summary>
<div markdown="1">
  
  - 정규화가 생겨난 배경
    - 한 테이블에 여러 엔티티의 속성들을 혼합하게 되면 정보가 중복 저장되며 저장 공간을 낭비하게 된다. 이러한 중복 정보로 인해 갱신 이상이 발생한다.
      - 삽입 이상 : 원하지 않는 데이터가 삽입 될 경우
      - 삭제 이상 : 하나의 자료만 삭제하고 싶지만, 그 자료가 포함된 데이터 전체가 삭제되는 경우
      - 수정 이상 : 일부의 데이터만 갱신되어 정보가 모호해지거나 일관성이 없어져 정확한 정보 파악이 되지 않는 경우.
  - 정규화 란?
     - 중복을 최소화하게 데이터를 구조화하는 프로세스를 정규화
        - 데이터베이스 정규화의 목표는 이상이 있는 관계를 재구성하여 작고 잘 조직된 관계를 생성하는 것에 있다.
     - 제대로 조직되지 않은 테이블들과 관례들을 작고 잘 조직된 테이블과 관계들로 나누는 것.
  - 정규화를 하는 이유
     - 중복 데이터를 제거하기 위함. 중복 데이터가 많으면 데이터끼리의 정합성을 맞추기 어렵다.
      - 이를 통해 무결성을 유지할 수 있다.
     - 데이터 저장을 논리적으로 하기 위함.

</div>
</details>

## JPA

<details>
<summary>JPA N+1 문제</summary>
<div markdown="1">
  
   - JPA N+1 문제란?
      - 쿼리 1번으로 N개의 엔티티를 가져왔는데, 지연로딩으로 인해 N개의 엔티티 개수만큼 추가로 쿼리를 날리는 문제를 말한다.
      - 예를 들어 Member Entity를 조회하는데 Member 가 속한 Team 을 가져와야 하면 Team 테이블에 쿼리를 날린다.
   - 해결 방안
      - fetch join`select m from Member m join fetch m.team`
      - batch size
      - 위 두가지 방법이 있다.

</div>
</details>


## 네트워크

<details>
<summary>Thread Local</summary>
<div markdown="1">
  
    - Thread Local 이란
        - 각 Thread 마다 갖는 독립적인 지역 변수를 의미한다.
        - Java.lang 패키지에서 제공하는 쓰레드 범위 변수. 한 쓰레드에서 공유할 변수.
    - 특징
        - 같은 쓰레드 내에서만 공유
        - 따라서 같은 쓰레드 라면 해당 데이터를 메소드 매개변수로 넘겨줄 필요가 없다.
    - 스프링에서 사용
        - 트랜잭션 매니저에서 transaction Context를 전파하는데 사용된다.
        - SpringSecurit에서는 ThreadLocal을 기본전략으로 SecurityContextHolder 를 사용한다.
</div>
</details>





<details>
<summary>REST API 란?</summary>
<div markdown="1">
  
  - REST란?
    - Representational State Transfer의 약자
    - 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.
      - 자원(Resource)의 표현(Representation)에 의한 상태 전달
  -  REST 구성
    - 자원(Resource) : URI
      - 모든 자원은 URI라는 고유한 ID가 존재하며, 자원은 서버에 존재한다.
    - 행위(Verb) : HTTP Method
      - GET, POST, PUT, DELETE
    - 표현(Representation)
      - 클라이언트가 자원의 상태(정보)에 대한 조작을 요청하면 서버는 이에 적절한 응답을 보낸다.
      - REST 에서 하나의 자원은 JSON, XML등 여러 형태의 Representation(표현)으로 나타내어 질 수 있다.
  
  - REST 제약 조건(이 모든 것을 지켜야 진정한 REST라고 할 수 있다.)
      1. client - server
      2. stateless (무상태성)
      3. cache(캐시)
      4. uniform interface(self-descriptive message, HATEOAS등)
      5. layered system(다중 계층 - 보안, 로드밸런싱, 암호화 계층, 프록시 등등)
      6. code-on-demand(optional)
  - REST AP란?
      - HTTP 통신에서 어떤 차원에 대한 CRUD 요청을 Resource 와 Method로 표현하여 특정한 형태로 전달하는 방식이다.
      - REST 기반의 규칙을 지켜서 설계된 API
      - 개인적으로 모든 것은 클라이언트가 서버의 자원을 더 쉽게 이용할 수 있도록 하기 위함 인듯 하다.

</div>
</details>

<details>
<summary>프로그램 vs 프로세스 vs 쓰레드</summary>
<div markdown="1">
  
  - 프로그램 : 소스 코드가 파일 단위로 저장 장치에 저장되어 있으며, 아직 실행되지 않은 상태를 의미한다.
    - 디스크에 저장되어 있는 실행 가능한 파일
  - 프로세스 : 실행중인 프로그램. 프로그램을 실행하기 위해서 주소 공간, 메모리 등을 운영체제로 부터 할당 받은 상태
    - 프로그램이 실행되어 RAM에 적재되어 실행 중인 상태
    - 여러 개의 쓰레드를 포함할 수 있다.
  - 스레드 : 프로세스의 실행 단위. 같은 프로세스 내에 있는 스레드 끼리는 프로세스의 자원을 공유 할 수 있다.

</div>
</details>

<details>
<summary>프로세스 메모리 구조</summary>
<div markdown="1">
  
  - 프로세스 메모리와 PCB의 차이
    - PCB는 프로세스를 제어하기 위해 운영체제가 저장하는 자료구조이다.(프로세스의 위치 값, PC값 등등)
    - 프로세스 메모리는 그저 프로그램을 실행하는데 필요한 메모리를 저장시켜놓는 공간이다.
  - 프로세스 메모리 구조
    - 코드 영역 : 프로세스가 실행할 코드가 기계어의 형태로 저장 되는 공간.
      - 컴파일 타임에 결정되며 Read-Only이다.
    - 데이터 영역 : 전역 변수, Static 변수 등이 저장된 공간이다.
      - 컴파일 타임에 결정되며 Read-Write(실행 도중 변경 가능)이다.
    - 힙 영역 : 개발자가 관리하는 메모리 영역으로, 동적 할당 할때 사용된다
      - 런타임에 결정되며 개발자에 의해 메모리 공간이 동적으로 할당되고 해제 된다.
    - 스택 영역 : 호출된 함수의 수행을 마치고 복귀할 주소 및 데이터(지역변수, 매개변수, 리턴 값)등 임시로 저장하는 공간.
      - 컴파일 타임에 결정되며, 정해진 크기가 있으므로 초과시 StackOverFlow가 발생한다.

</div>
</details>

<details>
<summary>스레드를 사용하는 이유는?</summary>
<div markdown="1">
  
  - 스레드가 없을 때의 단점은 아래와 같다.
    - 프로세스 간의 컨텍스트 스위칭 오버헤드
      - 프로세스는 프로세스마다의 독립적인 메모리를 가지고 있다. 그러므로 멀티 프로세스로 동작한다면 빈번한 컨텍스트 스위칭으로 인한 성능 저하가 발생한다.
    - 프로세스 사이 통신의 어려움
      - 프로세스들은 독립된 주소공간을 가지고 있기 때문에, 단순한 방법으로 서로의 메모리 공간을 접근 할 수 없다.
      - 공유메모리, 소켓등을 이용해서 접근 해야 한다.
  
  - 스레드를 사용한다면
      - 빠른 컨텍스트 스위치
        - 스케줄링 단위가 프로세스 였던 시절, Context Switching 이 일어날 때 마다 캐시 flush, 캐시 복수 등을 해야했다.
        - 하지만, 스케줄링 단위가 Thread로 되면서 같은 프로세스 내의 Thread들을 Context Switch 할 때는 TCB만 바꾸면 된다.
        - 메모리 상에서의 주소 이동도 필요없다.(프로세스는 주소 이동을 해야함)
      - 스레드간 통신으로 멀티 스레드 구현
        - 스레드는 하나의 프로세스에 여러 개 존재하며, 프로세스의 Heap, Static, Code 영역을 공유한다. 즉 같은 프로세스 내에서 스레드끼리의 통신은 굉장히 빠르고 쉽게 가능하다.

</div>
</details>

<details>
<summary>Context Switching</summary>
<div markdown="1">
  
  - 여러개의 프로세스가 실행되고 있을 때 기존에 실행되던 프로세스를 중단하고 다른 프로세스를 실행하는 것. 즉 CPU에 실행할 프로세스를 교체하는 기술이다.
  - 어떤 하나의 프로세스를 실행하고 있는 상태에서 인터럽트 요청에 의해 다음 우선 순위의 프로세스가 실행되어야 할 때 기존의 프로세스의 상태 또는 레지스터 값(Context)을 저장하고 
    CPU 가 다음 프로세스를 수행하도록 새로운 프로세스의 상태 또는 레지스터 값(Context)을 교체하는 작업
  

</div>
</details>

<details>
<summary>스택을 스레드마다 독립적으로 할당하는 이유</summary>
<div markdown="1">
  
  - 결론부터 말하면 독립적인 실행 흐름을 추가하기 위해선 최소 조건으로 독립된 스택이 필요하기 때문이다.
  - 스택은 함수 호출 시 전달되는 인자로 되돌아갈 주소 값 및 함수 내에서 선언하는 변수 등을 저장하기 위해 사용되는 공간이다.
  - 스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하며, 이는 독립적인 실행 흐름을 의미한다.

</div>
</details>

<details>
<summary>HTTP 프로토콜 이란?</summary>
<div markdown="1">
  
  - HTTP(Hyper Text Transfer Protocol)이란 서버/클라이언트 모델을 따라 데이터를 주고 받기 위한 프로토콜이다. 
  - HTTP는 어플리케이션 레벨의 프로토콜 TCP/IP 위에서 동작한다. HTTP는 상태를 가지고 있지 않는 Stateless 프로토콜 이며 Method, Path, Version, Headers, Body등으로 구성된다.

</div>
</details>

<details>
<summary>쿠키와 세션</summary>
<div markdown="1">
  
  - HTTP 프로토콜 특성이자 약점을 보완하기 위해 쿠키와 세션을 사용한다.
      - 비연결성(connectionless) : 클라이언트가 요청을 한 후 응답을 받으면 그 연결을 끊어 버리는 특징
      - 무상태성(stateless) : 통신이 끝나면 상태를 유지하지 않는 특징
      - 대표적으로 쿠키와 세션등을 사용하지 않으면 지속적인 로그인 환경을 구축할 수 없다(물론 토큰 기반으론 가능)
  - 쿠키란?
    - 쿠키는 클라이언트측 `브라우저 로컬`에 저장되는 키와 값이 들어있는 작은 데이터이다.
    - 브라우저가 종료되어도 쿠키 만료 기간이 있다면 클라이언트(브라우저) 에서 보관하고 있는다.
    - Response Header 에 Set-Cookie 속성을 사용하면 클라이언트에 쿠키를 만들 수 있다.
    - 쿠키는 사용자가 따로 요청하지 않아도 브라우저가 요청시 Request Header 에 넣어서 자동으로 서버에 전송한다.
  - 세션이란?
    - 세션은 쿠키를 기반으로 하며, 서버에서 관리하는 사용자 정보 파일(데이터) 이다.
    - 서버에서는 클라이언트를 구분하기 위해 세션 ID를 부여하며, 웹 브라우저가 서버에 접속해서 브라우저를 종료할 때 까지 인증 상태를 유지한다.
        - 클라이언트가 Reques를 보내면, 해당 서버의 엔진이 클라이언트에게 유일한 세션ID를 부여한다.
    - 예시 : 로그인 정보 저장
  - 쿠키와 세션 차이
    - 사용자 정보 저장 위치 : 쿠키는 클라이언트, 세션은 서버
    - 보안성 : 쿠키는 클라이언트측에 저장하므로 언제든 스니핑 당할 우려가 있으나, 세션은 쿠키를 이용해서 sessionid만 저장 하고 그것을 구분해서 서버를 처리하기 때문에 보안성이 비교적 우수하다.
    - 모든 정보를 세션에 저장하면 좋지만, 서버 자원의 낭비와 속도 때문에 중요하지 않은 정보는 쿠키에 저장하는 것이 좋다.
  

</div>
</details>

## 자료구조

<details>
<summary>Stack</summary>
<div markdown="1">
  
  - 제한적으로 접근할 수 있는 나열 자료구조
  - LIFO(Last In First Out)
    - 스택은 한쪽 끝에서만 자료를 넣거나 뺄 수 있는 선형 구조이다.
  - 스택의 ADT
    - peek() : 스택의 가장 윗 데이터를 반환한다.
    - push() : 자료를 밀어 넣는다해서 push이다. 스택의 가장 위 데이터 추가
    - pop() : 넣어둔 자료를 꺼낸다 해서 pop이라 한다. 스택의 가장 위 데이터 삭제 및 반환
    - empty() : 스택이 비어있는지 확인

</div>
</details>


<details>
<summary>Queue</summary>
<div markdown="1">
  
  - 줄을 서는 것을 뜻한다.
  - 선입선출의 대표적인 자료 구조 형태
  - FIFO(First In First Out)
    - 먼저 집어 넣은 데이터가 먼저 나오는 구조
  - 큐의 ADT
    - enqueue : 큐 맨 뒤에 용소 추가
    - dequeue : 큐 맨 앞의 요소 삭제 + 반환
    - front : 큐의 맨 앞의 요소 반환
    - rear : 큐의 맨 뒤의 요소 반환

</div>
</details>

<details>
<summary>ArrayList vs LinkedList</summary>
<div markdown="1">
  
  - ArrayList : 순차리스트, 배열기반으로 구현된 리스트
      - 배열 기반이므로 리스트 생성시 길이를 정해야 한다 -> 정적할당
      - 배열 기반으로 하는 ArrayList는 중간에 데이터를 추가하거나 삭제하게 된다면 해당 인덱스 뒤의 존재하는 모든 데이터를 복사하여 한칸 씩 밀어줘야 하므로 길이가 정해져 있지 않은 리스트가 필요하다면 비효율적이다.
      - 인덱스조회 : 배열은 하나의 자료형을 그룹핑할 때 사용되는 자료형이다. 즉, 자료형으로 길이가 같으므로 *(기준 자료형 + 인덱스) 를 통해 인덱스로 조회가 굉장히 빠르다. 시간복잡도 O(1)
  
  - LinkedList : 연결리스트, 메모리의 동적 할당을 기반으로 구현된 리스트
      - 노드 연결기반으로 리스트를  생성시 길이가 정해져 있지 않고, 동적으로 길이를 늘릴 수 있다 -> 동적할당
      - 추가 -> LinkedList는 노드를 연결하는 형태이므로, 새로운 데이터를 생성하여 포인터 값만 조정해주면 된다.
      - 삭제 -> 삭제할 노드를 찾았다면 삭제는 노드 간의 포인터만 조절하면 되므로 굉장히 빠르다. 하지만, 삭제할 노드를 찾기 위해 탐색을 해야한다.
      - 인덱스 조회 -> LinkedList는 노드를 연결하는 형태이므로, 메모리 상의 노드의 위치가 일정하지 않다. 즉 For문으로 하나하나 찾아봐야 한다, 시간복잡도 O(n)
  
  
</div>
</details>

## 기타



<details>
<summary>추상 자료형(Abstract Data Type, ADT) vs Data Structure</summary>
<div markdown="1">
  
  ADT란?
  
  - 구현하고자 하는 구조에 대해 구현 방법은 명시하지 않고 자료구조의 특성들과 어떤 Operations 들이 있는지를 설명하는 자료구조의 한가지 형태.
  - 즉, 일종의 `규칙`들의 나열 이라고 쉽게 이해할 수 있다. ADT의 가장 대표적 예로는 스택 과 큐가 있다.

  Data Structure(DS)
  
  - 스택이랑 큐에 대해 어떻게 구현하는지가 추가되면 DS로서 스택과 큐를 설명하게 된 것이다.
  - 결국 큐와 스택이란 것은 Array 또는 Linked List 에 규칙을 설정한 모습이기 때문
  
  예)
  JAVA - Interface -> ADT
       - Class -> DS
  
  자바의 인터페이스는 ADT라고 할 수 있으며 이를 실제로 구현한 Class를 DS라고 볼 수 있다.
  
  스택에 대한 특징 or 설명 -> ADT 관점
  
  스택을 어떤 구현체로 써서 구현 -> DS 
  
</div>
</details>











  








