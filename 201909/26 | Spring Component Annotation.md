## Spring Annotation : Component, Controller, Repository, Service

Spring 에서는 명시적으로 해당 클래스가 Bean 임을 나타내기 위해 다음과 같은 어노테이션을 사용할 수 있다.
그렇다면 이 넷은 어떤 차이가 있을까?

> @Component, @Controller, @Service, @Repository



### 공통점

이들은 모두 스프링이 관리하는 빈을 나타낸다. 

그리고 사실 @Controller, @Service, @Repository 은 @Component 를 내포한다. 다음과 같이 말이다.

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    ...
}
```

결국 @Controller, @Service, @Repository 는 @Component 에 약간의 기능이 추가된 형태다.



### 차이점

#### @Component
해당 클래스가 Spring Component 임을 나타내는 가장 일반적인 목적의 어노테이션이다.

SpringBoot 에서 @ComponentScan 의 스캔 대상이 된다. @Controller, @Service, @Repository 은 원래 @ComponentScan 의 스캔 대상이 아니지만, 앞서 언급했듯 @Component 를 내포하고 있기 때문에 스캔이 된다.


#### @Controller
해당 클래스가 MVC 패턴의 컨트롤러 역할을 한다는 것을 나타낸다. 

Spring 의 DispatcherServlet 은 @Controller 가 붙은 클래스를 감지하여 해당 클래스 내의 @RequestMapping 이 붙은 메소드를 찾는다. @RequestMapping 은 @Controller 클래스 내에서만 사용할 수 있다. (@Component, @Service, @Repository 클래스 내에서는 동작하지 않는다)

> DispatcherServlet : ServletContainer 를 통해 들어오는 모든 HTTP 요청을 중앙집중식으로 처리하여 적절한 세부 컨트롤러로 위임해주는 역할을 함 (url 기반)


#### @Service
비지니스 로직을 담고있는 계층이며, Repository 클래스의 메소드를 호출한다.

그 밖에 특별한 것은 없다. 하지만 미래에 추가될수도...


#### @Repository
Persistence Layer 를 나타내는 어노테이션이다.

Platform 에 종속된 Exception 을 잡아, Spring 에서 정의한 Exception (Unchecked Data Access Exception)으로 다시 던져준다. 이 기능이 동작하게 하려면, PersistenceExceptionTranslationPostProcessor 를 빈으로 등록해주면 된다.

```java
@Bean
public PersistenceExceptionTranslationPostProcessor exceptionTranslation() {
    return new PersistenceExceptionTranslationPostProcessor();
}

```


모두 스프링 빈을 만들기 위함이라는 공통의 기능을 가지고 있지만, 목적에 맞게 사용하는 것이 적절할 것이다.