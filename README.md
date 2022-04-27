# interview


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
