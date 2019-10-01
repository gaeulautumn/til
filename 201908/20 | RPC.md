## 원격 프로시저 호출 (RPC)

### What is RPC ? (Remote Procedure Call)
별도의 원격 제어를 위한 코딩 없이 다른 *주소 공간* 에서 함수나 프로시저를 실행할 수 있게 하는 **프로세스 간 통신** 기술.

![개요](/images/201908/RPC_1.png)

- 프로세스 간 데이터를 주고받을 때 network 통신이 필요하지 않으며, 원격지에 있는 프로그램을 로컬에 있는 것처럼 사용할 수 있다.
-> 분산 네트워크 환경에서 프로그래밍을 *쉽게* 할 수 있게 해줌. (network 프로그래밍을 위한 코드가 불필요)


![mechanism](/images/201908/RPC_2.png)
- IDL(Interface Definition Language) 을 이용하여 서버의 호출 규약 정의
- 함수명, 인자 및 반환값에 대한 데이터형이 정의된 IDL 파일을 rpcgen 컴파일러를 이용하여 stub 코드 생성 (원하는 Language 로)
- stub 코드를 각 프로그램(클라이언트, 서버)에 포함하여 빌드
- 클라이언트에서는 stub 에 정의된 함수를 call 함으로써 원격지에 있는 서버의 함수를 call 할 수 있다.
- stub 코드는 데이터형을 XDR(eXternal Data Representation) 형식으로 변환하여 RPC 호출 실행. (marshalling/unmarshalling)

### 참고 : CORBA 와 RMI
**CORBA**(Common Object Requets Broker Architecture)
로컬/원격을 포괄한 프로그램 객체 간의 메소드 호출 표준화를 위한 규격.
다양한 언어 지원.

**RMI**(Remote Method Invocation)
서로 다른 JVM 간의 메소드 호출 지원. (only Java)

모두 RPC 와 같이 원격 프로그램을 로컬에 있는 것 처럼 사용할 수 있는 기술.


> 하지만 RPC, RMI, CORBA 를 흔히 볼 수는 없다. 왜냐하면 복잡성, 어려움 보안 때문. local call 을 사용할 때는 다음의 가정이 모두 맞다는 보장이 있다. 하지만 remote call 을 하는 환경에서는 그렇지 않으므로, remote call 을 마치 local call 처럼 사용하려면 다음 가정들 중 하나라도 틀렸을 때에 대한 대응이 되어있어야 한다. 하지만 remote call 을 마치 local call 처럼 사용하는 RPC 에서는 그렇게 하지 않으므로 문제가 될 수 있다. (반대로 local call 을 remote call 처럼 사용하는 것은 문제가 되지 않는다. ex.Erlang)

*8 Fallacies of Distributed Computing*
1. The network is reliable.
2. Latency is zero.
3. Bandwidth is infinite.
4. The network is secure.
5. Topology doesn't change.
6. There is one administrator.
7. Transport cost is zero.
8. The network is homogeneous.
[참고](https://stackoverflow.com/questions/3835785/why-has-corba-lost-popularity)


* 더 알아볼 것 : [gRPC](https://corgipan.tistory.com/6)

