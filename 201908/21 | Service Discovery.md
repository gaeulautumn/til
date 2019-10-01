## Service Discovery Pattern

### What is Service Discovery ?
- MSA 와 같은 분산 환경은 서비스간의 원격 호출로 구성된다. 이 때 원격 호출은 IP, Port 를 이용하는 방식이었는데, 클라우드 환경에서의 오토 스케일링이나 컨테이너 기반의 배포로 인해 IP 가 동적으로 변경되는 일이 잦아졌다. 
그래서 서비스간 호출을 할때 대상 서비스의 위치를 알아낼 수 있는 방법이 필요한데, 이를 Service Discovery 라고 한다.


### Client Side vs Server Side
- Service Discovery 를 구현하는 방법으로는 크게 Client Side, Server Side 두 가지 방법이 있다.

#### Client Side
![Client Side](/images/201908/ServiceDiscovery_1.png)
- Service 간 Discovery 를 위해, 서비스는 인스턴스가 생성될 때 Service Registry 에 자신의 정보를 기록해놓는다. 서비스 클라이언트는 Service Registry 에서 이 서비스의 주소를 찾아 호출한다.

#### Server Side
![Server Side](/images/201908/ServiceDiscovery_2.png)
- Client Service 는 Load Balancer 를 통해 대상 서비스를 호출하고, 이 Load Balancer 에서 Service Registry 를 통해 대상 서비스의 주소를 알아낸 뒤 Routing 한다.

### Service Registry
- 보통 서비스 클라이언트에서는 Service Registry 정보를 캐싱하여 사용한다. 
Service Registry 는 지정된 시간에 한번씩 서비스들에 ping 을 보내 서비스 정보를 갱신한다.
Service Registry 에서는 이미 서비스의 주소를 알고있기 때문에, DNS 서버에 별도 요청할 필요가 없다. 


* Service Registry 를 구현하기 위한 솔루션 : ZooKeeper, etcd
* Service Discovery 를 지원하는 솔루션 : Eureka(Netflix), Consul(Hashcorp)


[참고URL](https://kihoonkim.github.io/2017/01/27/Microservices%20Architecture/Chris%20Richardson-NGINX%20Blog%20Summary/4.%20Service%20Discovery%20in%20a%20MSA/)

