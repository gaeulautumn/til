## 클래스 내 Spring Transaction 적용

Spring 에서는 트랜잭션 적용 방법을 추상화 하여 제공하며, @Transactional 어노테이션을 통해 사용할 수 있다.

이 @Transactional 은 Proxy 로 동작하기 때문에, 같은 클래스 내에 있는 메소드를 호출할 경우 Transaction 이 적용되지 않는다.  

예를 들어, 다음과 같은 인터페이스 및 구현체가 있을 때 외부에서는 구현체 클래스를 직접 콜하게된다.


![적용 전](/images/201909/Transaction_1.png)


Transaction 적용 시, 외부에서 해당 구현체를 콜하면 Spring 에서는 인터페이스를 구현한 Proxy 객체를 생성한다.
(*그렇다고 Transaction 을 적용하기 위해 항상 인터페이스를 생성하고 이를 구현해야 하는 것은 아니다. Spring 에서는 인터페이스가 있을 경우 java 의 다이나믹 프록시를 사용하고, 없을 경우 Cglib 프록시를 사용한다. [참고](http://wonwoo.ml/index.php/post/1576)*)
![적용 후](/images/201909/Transaction_2.png)



이러한 구조 때문에, 클래스 간 호출에는 Transaction 이 적용되지만 클래스 내 메소드 호출에는 적용되지 않는다.

하지만 클래스를 분리하지 않고 Transaction 을 적용하려면 어떻게 할까?
이럴 땐 다음과 같이 할 수 있다.

이 클래스의 메소드는 함수형 인터페이스를 인자로 받아 실행시키는 것 뿐이지만, 해당 메소드에 Transactional 어노테이션을 적용함으로써 외부에서 해당 함수를 콜 할때 Transaction 이 적용되게 된다.
이 때 ```REQUIRES_NEW``` Propagation 정책을 적용해 상위에 Transaction 이 있더라도 새롭게 Transaction 을 생성하도록 했다.

```java
@Component
public class TransactionWrapper {

    @Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
    public void applyTransaction(Runnable runnable) {
        runnable.run();
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
    public <T> T applyTransaction(Supplier<T> supplier) {
        return supplier.get();
    }
}
```


해당 클래스를 콜 하는 곳에서는 다음과 같이 사용할 수 있다.
```java
// Transaction
erpFileNameVO =
        transactionWrapper.applyTransaction(() -> changeApprovalStatusBillIssueTrans(request, masterVO, approvalBillVList, approvalStatus));
```





[참고](http://egloos.zum.com/aretias/v/708477)