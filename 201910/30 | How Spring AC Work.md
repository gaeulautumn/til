## How Spring Application Context Work ?

### 어떻게 빈을 등록하고 관리하는가

Application Context 란 스프링의 IoC 컨테이너라고 할 수 있다.

BeanFacroty 인터페이스를 구현하여 확장하고 있으며, 빈 스캔 및 초기화, 생성 등을 담당한다.

이 때, AC (Application Context) 는 기본적으로 Eager Loading 방식을 사용한다. (하지만 Bean 의 scope 가 default(singleton) 가 아닐 경우에는 Lazy Loading 방식을 사용하기도 한다)  


### Bean 스캐닝

스프링은 AC 에 등록 할 빈을 찾기 위해 어떤 패키지를 스캔해야 하는지 알아야 한다.

스프링 부트에선, @SpringBootApplication 이 붙은 클래스가 메인 클래스이고, 해당 클래스가 속한 패키지 및 하위 패키지가 스캔 대상이 된다. (@SpringBootApplication 어노테이션은 @Configuration, @ComponentScan, @EnableAutoConfiguration 등을 포함한다)

Bean 생성 과정

- AC 는 Bean 등록을 위해 메타데이터를 참조한다.
- 메타데이터를 보고 Bean 을 생성한다 (Reflection API 사용)
- 의존성을 주입한다 (DI)

[참고 : spring bean lifecycle](https://javabeginnerstutorial.com/spring-framework-tutorial/java-spring-bean-lifecycle/)