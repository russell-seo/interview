

# 카카오 스타일(지그재그 백엔드 면접 질문 내용)

  
  ## Spring @Transactional 동작 원리
     
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
    
    스프링IoC, AOP를 사용하지 않고, 코드로 직접 사용하는 
    
