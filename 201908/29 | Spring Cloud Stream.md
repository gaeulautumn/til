## Spring Cloud Stream

### Spring Cloud
스프링 클라우드는 분산 시스템의 공통 패턴을 빠르게 구축할 수 있게 해주는 스프링 프레임워크의 기술이다.
 > spring-cloud-aws  
 spring-cloud-config  
 spring-cloud-netflix  
 spring-cloud-security  
 spring-cloud-stream  
 spring-cloud-zookeeper

[공식문서](https://spring.io/projects/spring-cloud)


### Spring Cloud Stream
그 중 spring cloud stream 은, 메시지 기반 마이크로 서비스를 구현하기 위한 프레임워크다.
rabbitMQ, kafka 등 미들웨어 메시지 시스템과 독립적으로 메시지 시스템을 이용하는 방법을 추상화 해놓았다.

아래처럼 애플리케이션과 메시지 시스템이 직접 연동되는 것이 아닌, Binder 구현체를 두고 통신을 하게된다.
![구성도](/images/201908/SpringCloudStream_1.png)

따라서 애플리케이션에서는 미들웨어 독립적으로 개발을 할 수 있다.

spring cloud stream 에서는, 기존 메시지 시스템의 Pub/Sub 개념을 Source/Sink 로 부른다.  


#### 주요 인터페이스

**Sink** 메시지가 사용되는 대상을 제공하여 메시지 소비자에 대한 규약을 정의.

**Source** 생성 된 메시지가 전송되는 대상을 제공하여 메시지 생성자에 대한 규약을 정의.

**Processor** 메시지 소비 및 생성을 허용하는 두 대상을 노출시켜 싱크 및 소스 에 대한 규악을 캡슐화.  




#### 주요 어노테이션 
```@EnableBinding``` 메세지 브로커와의 연결(Binding) 담당

```@StreamListener``` 메세지 브로커의 이벤트 수신 (Input 인터페이스를 통한)

```@InboundChannelAdapter``` 메세지를 전송받는 채널에 메세지를 내보내기 위한 어댑터 (Outpu 인터페이스를 통한)  



```java
@SpringBootApplication
@EnableBinding(Sink.class)
public class VoteRecordingSinkApplication {
​
  public static void main(String[] args) {
    SpringApplication.run(VoteRecordingSinkApplication.class, args);
  }
​
  @StreamListener(Sink.INPUT)
  public void processVote(Vote vote) {
      votingService.recordVote(vote);
  }
}
