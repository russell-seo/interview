

# 카카오 스타일(지그재그 백엔드 면접 질문 내용)

  
  
  <details>
  <summary>Spring @Transactional 동작 원리</summary>
  <div markdown="1"> 
    
   - 먼저 정리해서 얘기하자면
    
      1. 스프링은 @Transactional 어노테이션을 가진 메서드를 발견하면, 다이나믹 프록시를 만든다.
    
      2. 해당 프록시 객체는 `TransactionManager`에게 트랜잭션 동작을 위임하는 코드를 가진다.
    
      3. 트랜잭션 매니저는 아래 코드처럼, JDBC 코드를 통해 트랜잭션을 실행한다.
     
   - JDBC 에서 개발자가 직접 트랜잭션을 관리하는 방법은 한가지 밖에 없다.
      ~~~java
      
        //커넥션 풀에서 DB커넥션을 받아왔다고 가정
        Connection connection = dataSource.getConnection();
        
        try(connection){
          connection.setAutoCommit(false); // 자동 커밋 off
          
          // ...DB작업...
          
          connection.commit(); // 성공시 트랜잭션 커밋
          
          
        }catch (SQLException e) {
        
          connection.rollback(); // 오류 발생시 트랜잭션 롤백
        }
      
      ~~~
      
      우리가 사용하는 Spring 과 하이버네이트에서 제공해주는 `@Transactional` 은 알아서 트랜잭션을 관리해주는 마법의 키워드가 아니다. 추상화해서 사용할 뿐이지 실제는 위 코드처럼 JDBC 트랜잭션을 사용하여 구현한다.
      
  - 스프링의 마술, Transaction Management
  - 스프링의 추상화, PlatformTransactionManager
    - 스프링은 트랜잭션 처리를 TransactionManager 객체를 통해 처리합니다.
    - 구현체는 갈아끼울 수 있게 인터페이스인 PlatformTransactionManage가 주입되어 사용된다.
    ~~~java
    
    public interface PlatformTransactionManager {
    
      TransactionStatus getTransaction(@Nullable TransactionDefinition var1) throws TransactionException;
      
      void commit(TransactionStatus var1) throws TransactionException;
      
      void rollback(TransactionStatus var1) throws TransactionException;
     
    } 
    ~~~
    
    물론 구현체마다 거의 동일한 부분이 있을 수 있어서, 이를 구현하는 AbstractPlatformTxManager가 존재한다.
    
    ![image](https://user-images.githubusercontent.com/79154652/168084942-e1296bb7-e286-4a88-adce-24231f6c5fff.png)

    
    
    참고로 테스트 용도로 쓰이는 인메모리 DB는, DataSrouce를 설정하지 않아도 바로 사용이 가능하다.
    
    이는 SpringBoot 에 있는 @AutoConfiguratio에 의해 DataSource 와 JdbcTemplate가 빈으로 생성되기 때문이다.
    
    물론 해당 인메모리 DB Drive 의존성은 있어야 자동설정 됩니다. 스프링은 마술이 아니다.
    
    생성된 DataSource는 의좃넝으로 주입받아서 확인 할 수 있다.
    
    ~~~java
    
    @Component
    public class TestRunner implements ApplicationRunner {
    
    @Autowired
    DataSource datasource;
    
    @Override
    public void run(ApplicationArguments args) throws Exception{
    
    Connection connection = datasource.getConnection();
    
    ...DB작업...
    
    }
    
    ~~~
    
    사용법 1 - TransactionTemlpate 사용
    
    스프링IoC, AOP를 사용하지 않고, 코드로 직접 사용하는 방법이다. 보통 TransactionTemplate를 사용한다.(이 객체는 내부에 PlatformTransactionManager를 사용하고 있다)
    
    ~~~java
    
    @Service
    public class UserService{
    
        @Autowired
        private TransactionTemplate template;
        
        public Long registerUser(User user){
            Long id = template.execute(status ->{
            
                //SQL 실행
                
                return id;
            
            });
        
        }
    
    }
    
    ~~~
    
   물론 @Trnasactional 이 간편하기 때문에 잘 사용하지 않는 방법이다.
   
   다이나믹 프록시의 원리상, @Transactional 의 경우 클래스나 메서드 단위로 밖에 걸수 없다.
   
   예를 들어 [상품구매 - 이메일발송] 인데, 이메일 발송이 취소되었다고 트랜잭션에 의해 롤백 되면 안된다.
   
   이럴 경우 @Transactional 으로 해결하기엔 애매한 상황이 온다.
   
   
   - 프록시 특성상, 트랜잭션은 외부에서 doInternalTransaction()을 호출할 때만 걸린다.
   - 스프링 트랜잭션은 메서드 시작시 커넥션이 생성되고, 메서드 종료시에 커넥션을 풀에 반환한다.
   - 트랜잭션을 세부적으로 걸고 싶다면 아래와 같이 TransactionalTemplate를 사용하면 된다.

          @Transcational에서 지원하는 옵션은 당연히 Template에도 있습니다.
          propagation : 트랜잭션 전파 규칙 설정
          isolation : 트랜잭션 격리 레벨 설정
          readOnly : 읽기 전용 여부 설정
          rollbackFor : 트랜잭션을 롤백할 예외 타입 설정
          noRollbackFor : 트랜잭션을 롤백하지 않을 예외 타입 설정
          timeout : 트랜잭션 타임아웃 시간 설정
   
   </div>
   </details>

   <details>
  <summary>Checked Exception vs Unchecked Exception</summary>
  <div markdown="1"> 
    
    
  `Checked Exception` 과 `Unchecked Exception`의 차이를 알아보기 전에 먼저 예외가 먼지 알아볼 필요가 있다.
    
   - 예외란?
    
      - 입력값에 대한 처리가 불가능 하거나 프로그램 실행중에 참조된 값이 잘못된 경우 등 정상적으로 프로그램의 흐름에서 어긋나는 것을 말한다.
      - 자바에서 예외는 개발자가 직접 처리할 수 있기 때문에 예외 상황을 미리 예측하여 핸들링 한다.
    
   - Checked Exception
        - RuntimeException을 상속하지 않은 클래스를 Checked Exception 이라고 한다.
        - 예외 발생 시 트랜잭션을 롤백하지 않고 예외를 던져준다.(스프링 트랜잭션이 선언된 메소드나 클래스에서)
    
   - UnChecked Exception
        - RuntimeException을 상속하는 클래스를 UnChecked Exception 이라고 한다.
        - 예외 발생시 트랜잭션을 rollback 한다.(트랜잭션이 선언된 메소드나 클래스에서)
        - Try-catc문을 이용해서 RuntimeException을 잡더라도 롤백이 발생한다.
    
    
 [관련 블로그(우아한형제)](https://techblog.woowahan.com/2606/)
    
 [트랜잭션 롤백 관련 블로그](https://suhwan.dev/2020/01/16/spring-transaction-common-mistakes/)
    
    
   </div>
   </details>
   
   
   
  <details>
  <summary>JWT 장점</summary>
  <div markdown="1"> 
  
    - JWT의 주요한 이점은 사용자 인증에 대한 필요한 모든 정보는 토큰 자체에 포함되어 있어 서버에 별도의 인증 저장소가 필요가 없다.
    
    - 쿠키를 전달하지 않아도 되므로 쿠키의 취약점이 사라진다.
        - 쿠키의 취약점
          - XSS(Cross-Site Scripting) 공격
            - XSS 공격은 자바스크립트가 사용자의 컴퓨터에서 실행된다는 점을 이용한 공격.
          - 스니핑 공격
            - 네트워크의 중간에서 남의 패킷정보를 도청하는 해킹 유형중 하나.
          - 공용 PC에서 쿠키 값 유출
            - 쿠키는 사용자의 하드 디스크에 저장되기 때문에 공용 PC인 경우 쉽게 탈취가 가능
    
    - 토큰만 가지고 사용자 식별 및 유효성 검사를 하므로 서버를 `무상태로(stateless)` 로 관리할 수 있다.
      - DB조회나 캐시 데이터 조회 없이 로직만으로 인증이 가능하여 애플리케이션 서버를 확장하는데 용이하다.
    
    
  </div>
  </details>
  
  
  <details>
  <summary>Redis 내부동작</summary>
  <div markdown="1">
    
   - Redis Single Thread
        - 명령어를 실행하는 코어 부분은 Single Thread
        - 결국은 싱글스레드라 atomic 유지
        - 단일 스레드를 사용하여 불필요한 context Switching 및 lock을 고려할 필요가 없고 deadlock이 없어 성능 소모가 없습니다.
        - 주요 명령어는 O(1) 성능을 보이지만, 데이터가 많을 경우 여러개의 키를 다루는 명렁어가 O(n) 성능을 보인다.
    
   - Redis는 휘발성 Memory에 데이터를 저장하기 때문에 Persistent를 지원하기 위해 RDB와 AOF를 지원한다.
        - RDB Snapshot
            - Redis는 Single Thread로 동작하기 때문에 Snapshot을 뜨는 (SAVE)시점에서는 모든 명령어 수행이 제한 대신 백그라운드에서 스냅샷을 뜨는 (BGSAVE)를 지원합니다.
            - 스냅샷은 RDB에서도 사용하고 있는 어떤 특정 시점의 데이터를 DISK에 옮겨담는 방식을 뜻합니다. Blocking 방식의 SAVVE와 Non-blocking 방식의 BGSAVE 방식이 있다.
        - AOF
            - Redis에 데이터를 저장하기 전에 수행되는 명령어들을 별도로 저장하여 해당 명령어로 persistent를 유지할 수 있도록 해줍니다.
            - Redis의 모든 Write/update 연산 자체를 모두 log 파일에 기록하는 형태이다. 서버가 재시작 할 때 write/update를 순차적으로 재실행, 데이터 복구를 한다.
  </div>
  </details>
  
   <details>
  <summary>Java8 GC(Garbage Collector)</summary>
  <div markdown="1">
    
     - JAVA 8 의 Default GC는 병렬 GC이다.
        
          - Parallel GC
            - 이 방식은 `throughput collector`로도 알려진 방식이다. 이 방식의 목표는 `다른 CPU가 대기 상태로 남아 있는 것을 최소화 하는 것이다`.
    
            - 시리얼 콜렉터와 달리 Young 영역에서의 콜렉션을 병렬로 처리한다. 많은 CPU를 사용하기 떄문에 GC의 부하를 줄이고 어플리케이션의 처리량을 증가시킬 수 있다.
            - Young 영역의 GC를 멀티 스레드 방식으로 사용하기 때문에, Serial GC에 비해 상대적으로 Stop the world가 짧다.
               - Old 영역은 아님
               - Old 영역의 GC는 시리얼 콜렉터와 마찬가지로 Mark-Sweep-Compact 콜렉션 알고리즘을 사용한다. 
    
          > Mark-sweep-compact 알고리즘
          > 1. Old 영역으로 이동된 객체들 중 살아 있는 개체를 식별한다(Mark)
            2. Old 영역의 객체들을 훓는 작업을 수행하여 쓰레기 객체를 식별한다.(Sweep)
            3. 필요없는 객체들을 지우고 살아 있는 객체들을 한 곳으로 모은다.(Compact)
    
     - JAVA 11의 Default 는 G1 GC 이다.
            
           - G1 GC
              - JAVA 9 부터 default GC
              - 현재 GC중 Stop-the-world의 시간이 제일 짧음
              - 어떠한 GC 방식보다 처리 속도가 빠르며 큰 메모리 공간에서 멀티 프로세스 기반으로 운영되는 애플리케이션을 위해 고안되었다.
              - CMS CG를 개선하여 만든 GC로 위에서 살펴본 GC와는 다른구조를 가진다.
           - Heap을 Region이라는 일정한 부분으로 나눠서 메모리를 관리한다.
             - 기존 GC처럼 물리적인 영역으로 나누지 않고, Region(지역)이라는 개념을 새로 도입하여 Heap을 균등하게 여러 개의 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분하여(Eden 지역인지, Survivor 지역인지, Old 지역인지) 객체를 할당한다.
          
            - G1 GC에서는 Eden, Survivor, Old 역할에 더해 Humongous와 Availalbe/Unused라는 2가지 역할을 추가하였다.
            - Humongous는 Region 크기의 50%를 초과하는 객체를 저장하는 Region을 의미하여, Available/Unused는 사용되지 않는 Region을 의미한다.
            - G1 GC의 핵심은 전체 Heap에 대해서 탐색하지 않고 부분적으로 Region 단위로 탐색하여, 가비지가 많은 Region에만 우선적으로 GC를 수행하는 것이다.
            - G1 GC도 다른 가비지 컬렉션과 마찬가지로 2가지 GC(Minor GC, Major GC) 로 나누어 수행된다.
                - Minor GC
                  - 한 지역에 객체를 할당하다가 해당 지역이 꽉 차면 다른 지역에 객체를 할당하고 Minor GC가 실행된다. G1 GC는 각 지역을 추적하고 있기 때문에, 가비지가 가장 많은 지역을 찾아서 Mark and Sweep을 수행한다.
                  - Eden 지역에서 GC가 수행되면 살아남은 객체는 식별(Mark)하고, 메모리를 회수(Sweep)한다. 그리고 살아남은 객체를 다른지역으로 이동시키게 된다. 복제되는 지역은 Available/Unused 지역이면 해당 지역은 이제 Survivor 영역이 되고, Eden 영역은 Available/Unused 지역이 된다.
    
                - Major GC
                  - 시스템이 계속 운영되다가 객체가 너무 많아 빠르게 메모리를 회수 할 수 없을 때 Major GC가 실행된다. 그리고 여기서 G1 GC와 다른 GC의 차이점이 두각을 보인다.
                  
                  - 기존의 다른 GC 알고리즘은 모든 Heap 영역에서 GC가 수행되었으며, 그에 따라 처리 시간이 상당히 오래 걸림. 하지만 G1 GC는 어느 영역에 가비지가 많은지 알고 있기 떄문에 GC를 수행할 지역을 조합하여 해당 지역에 대해서만 GC를 수행한다. 그리고 이러한 작업은 Concurrent 하게 수행되기 때문에 어플리케이션의 지연도 최소화 할 수 있는 것이다.
  </div>
  </details>
  
   <details>
  <summary>AWS S3 업데이트</summary>
  <div markdown="1">
    
  </div>
  </details>
  
   <details>
  <summary>ElasticSearch 노드 설계</summary>
  <div markdown="1">
    
  </div>
  </details>
  
   <details>
  <summary>Redis AOF는 왜 끄나?</summary>
  <div markdown="1">
    
   - AOF 를 끄는 이유에 대해 설명하기 전에 AOF에 대해 먼저 알아보자
    
      - AOF는 Redis 백업을 위해 지원하는 방식이다.
      - AOF(Append Only File) 방식으로 명령이 실행 될 때 마다 해당 명령이 파일에 기록된다.(데이터 손실이 없음)
        - 명령이 실행되면 바로 작성하는게 아니라 버퍼에 두었다가 주기적으로 파일에 쓰는 방식이다.
      - 기본값으로 appendonly.aof 파일에 기록된다.
        - 조회 명령을 제외한 입력/수정/삭제 명령이 실행될 때 마다 버퍼에 기록 후 파일로 동기화 한다.
      - AOF는 수정이 가능하다.
        - text 파일 이므로 수정이 가능
        - 만약 명령어를 잘못 사용하여 데이터를 잘 못하여 모두 날린 경우. 해당 명령어를 AOF 파일에서 제거한 후 Redis를 재부팅 하면 데이터 손실없이 DB를 사용할 수 있다.
      - AOF의 장점은 서버 장애가 발생해도 데이터 유실이 거의없다.
    
    > 모든 명령이 기록되기 때문에 파일 크기가 커지며, 데이터 로딩이 느려질 수 있다. OS의 파일 크기 제한으로 장애가 발생할 수 있다.
      ReWrite를 통해 파일크기를 줄일 수 있으며, ReWrite를 할 경우 파일의 최종 데이터만 남게된다(Rewrite를 예로 들면 insert 가 1000번 발생한다면 마지막 1번의 데이터만 기록에 남게된다)
    
  </div>
  </details>
  
   <details>
  <summary>Redis Ziplist 자료구조에 대해</summary>
  <div markdown="1">
    
 - Ziplist 가 나온 배경
    
   - Redis에서 주로 사용되는 데이터 타입 중에 실 데이터 외에 메모리를 가장 많이 차지 하고 있는 것은?
   - 바로 `포인터` 이다. 포인터만 제거해도 메모리 사용량을 많이 줄일 수 있다. 그럼 포인터를 갖지 않으려면 어떻게 해야 할까?
   - 새 데이터가 생기면 메모리를 할당하고 거기를 가리키는 포인터를 저장하는 것이 일반적이다. 포인터를 가지지 않는 것은 데이터가 생길때 마다 별도의 메모리를 할당하지 않는 것이다.
   - 기존에 할당받은 메모리를 필요한 만큼 늘려서(resize) 저장하는 것이 바로 Ziplist 이다.
    
   - Ziplist 란?
      
      - 기존에 할당받은 메모리를 필요한 만큼 늘려서 저장하는 것이다. 
      
      - `zlbytes,zltail,zllenentry,entry,zlend` 구조로 생겨먹었다.
        - zlbytes, zltail, zllen이 Header 부분이다. 
        - zlbytes 는 ziplist의 전체 크기로 byte 단위이다. 자료형은 unsigned integer 이다. 4bytes
        - zltail은 tail Entry가 시작 offset을 가진다. 이것도 unsigned integer. 4bytes
        - zllength 는 16byte이고 zliplist entry의 갯수이다. 2bytes
        - zlend는 1byte 이고, 255인 경우 ziplist의 끝을 뜻한다.
        - 선형적인 자료구조이기 때문에 O(N)의 시간복잡도를 가진다.
      
      - Header의 바이트수를 모두 합하면 10bytes 이다.
        - 데이터 없이 빈 ziplist 라면 Header 10Bytes와 zlend 1bytes를 더해서 11bytes 이다.
        - 그러므로 각 항목의 값은 zlbytes=11, zltail=10, zllen=0 이다. zltail이 10인 이유는 0부터 시작하므로 10이 엔트리가 시작하는 offset 이기 때문이다.
        - Entry를 제외한 ziplist의 오버헤드는 11Bytes 이므로 다른 데이터 구조에 비해서 매우적다. 포인터는 하나만 저장해도 8Bytes이다.
    
  </div>
  </details>
  
   <details>
  <summary>서버에 올릴때 메모리를 어떻게 잡나?</summary>
  <div markdown="1">
    
  </div>
  </details>
  
   <details>
  <summary>Spring Boot Test Mocking</summary>
  <div markdown="1">
    
  </div>
  </details>
  
   <details>
  <summary>JWT 데이터 암호화 어떻게?</summary>
  <div markdown="1">
    
    - JWT 암호화는 기본적으로 공개 키 암호 방식(PKC, Public Key Cryptograpyh)를 사용하는데
      비대칭 암호 방식을 이용해 공개 키와 비밀 키를 생성하고 이 키들을 상황에 따라 나누어 가지고 통신한다.
    
    - 서명은 비밀 키가 있는 곳에서만 할 수 있고 공개 키를 가진 어느 곳에서나 이 데이터의 서명을 검증할 수 있다.
      
    - 필자는 프로젝트에서 대칭 알고리즘(1개의 secret key를 공유하는 알고리즘) 인 HS256으로 암호화 하였다.
    
    - Signature로 Header와 Payload 데이터를 base64로 인코딩 한 값이 변조되었는지 아닌지 검사한다.
  </div>
  </details>
  
   <details>
  <summary>토큰 만료시 처리 방안, 시간 설정 어떻게?, 어디에서 토큰 처리?</summary>
  <div markdown="1">
    
    - 
    
  </div>
  </details>
  
   <details>
  <summary>JWT Header, Payload, Signature에 대해</summary>
  <div markdown="1">
    
    - JWT
        - Header 는 JWT를 생성하는 알고리즘과 타입으로 구성되어있다.
    
        - Payload 는 데이터들을 담아서 Base64로 인코딩 한다.
          - Payload는 암호화 하지않아 어떤 누구도 데이터를 들여다 볼 수 있다. 그래서 최소한의 정보만을 담아서 인코딩해야한다.
    
        - Signature 은 Header 와 Payload 값의 위조와 변조에 대해 검증하기 위해 사용하는 부분이다.
           - Header 에 지정한 alg 으로 인코딩한다. 이때 비밀키를 사용하여 서명한다.
        

    
    
  </div>
  </details>
  
   <details>
  <summary>JWT 데이터를 왜 굳이 암호화? 어차피 복호화 해야되는데?</summary>
  <div markdown="1">
    
  </div>
  </details>
