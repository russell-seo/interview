

# 카카오 스타일(지그재그 백엔드 면접 질문 내용)

  
  
  <details>
  <summary>## Spring @Transactional 동작 원리</summary>
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
