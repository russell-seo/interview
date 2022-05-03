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
<summary>Parameter 와 Argument의 차이</summary>
<div markdown="1">
  
  - parameter : 함수를 선언할때 사용된 변수
  - argument : 함수가 호출 되었을 때 함수의 파라미터로 넘어오는 실제 값

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
<summary></summary>
<div markdown="1">

</div>
</details>


<details>
<summary></summary>
<div markdown="1">

</div>
</details>


<details>
<summary></summary>
<div markdown="1">

</div>
</details>


<details>
<summary></summary>
<div markdown="1">

</div>
</details>
