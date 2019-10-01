## Spring : Custom Properties Class

Spring Boot 에서는  [application.properties](http://application.properties) 파일로 설정을 관리할 수 있는데, 미리 정의된 property 키 값 말고도 직접 정의하여 사용할 수 있는 방법이 있다.

먼저, 의존성 추가

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
```


1. custom property 클래스 를 작성한다
2. @ConfigurationProperties 어노테이션을 추가한다.  (application.properties 파일에서 사용할 키 지정)
3. property 값을 전달받을 setter , property 참조 시 사용할 getter 추가

```
    import lombok.Data;
    import org.springframework.boot.context.properties.ConfigurationProperties;
    
    @Data
    @ConfigurationProperties("spring.influxdb")
    public class InfluxDBProperties {
    
        private String url;
        private String username;
        private String password;
        private String database;
        private int connectTimeout = 10;
        private int readTimeout = 30;
    
    }
```

4. @EnableConfigurationProperties 을 이용해 사용할 수 있다.

```
    import kr.co.daou.bizmsg.alio.gateway.influxdb.InfluxDBProperties;
    import org.springframework.boot.context.properties.EnableConfigurationProperties;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    @EnableConfigurationProperties(InfluxDBProperties.class)
    public class InfluxDBConfiguration {
        
    }
```