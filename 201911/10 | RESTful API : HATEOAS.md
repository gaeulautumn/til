## RESTful API : HATEOAS


**HATEOAS** Hypermedia as the Engine of Application State

RESTful 아키텍쳐의 제약사항 중 하나로, RESTful 아키텍쳐를 다른 network 아키텍쳐와 차별화 시켜주는 점이기도 하다.

HATEOAS 를 통해, 클라이언트는 서버의 응답으로부터 리소스에 대한 Hypermedia 링크를 얻을 수 있고, 이를 통해 동적으로 서버의 자원을 가져올 수 있다.

예를 들어, 

```
    HTTP GET http://api.domain.com/management/departments/10
```

라는 요청에 대해, 서버는 다음과 같이 응답하게 된다.

```
    {
        "departmentId": 10,
        "departmentName": "Administration",
        "locationId": 1700,
        "managerId": 200,
        "links": [
            {
                "href": "10/employees",
                "rel": "employees",
                "type" : "GET"
            }
        ]
    }
```

이를 통해, 클라이언트와 서버는 사전에 API 종류에 대해 협의할 필요가 없다. (다만, link 를 전달해주는 format 에 대해서는 협의가 필요할 수 있다) 마치 우리가 웹 서핑을 하듯이, 계속해서 서버가 제공해주는 링크를 따라 가기만 하면 된다. 따라서 클라이언트는 API 종류를 하드코딩 해놓지 않아도 되며, 서버는 동적으로 API를 바꿀 수 있다.

여기서 중요한 키워드는 '동적(dynamic)' 이 아닐까 한다.

보통 REST API 를 설계하라고 하면, 설계문서를 만들고, client 에게 전달하여 개발하도록 한다. API 명세가 바뀌면 바뀌었음을 공지하고 client 에게 변경할 수 있는 시간을 준다.

한마디로, REST API 서버는 그 무엇도 마음대로 바꿀 수 없는 상태이고, 이는 RESTful 하다고 할 수 없을 것이다.

HATEOAS 를 준수하면 client 에게 미리 API url 을 알려주지 않음으로써 서버는 동적으로 무엇이든 바꿀 수 있게되고 client 에게 영향을 주지 않는다. 소프트웨어 설계 관점에서 봤을 때도 바람직한 방향이라 할 수 있다.

하지만, 개발 비용과 난이도가 대폭 상승할 것이므로, 상황에 맞게 설계하는 것이 중요할 듯 하다.

- 참고 : Spring 에 HATEOAS 관련 하위 프로젝트가 있다. HATEAOAS 를 준수하는 API 서버를 만들 때 사용하면 유용할 것이다.

[참고url](https://stackoverflow.com/questions/20335967/how-useful-important-is-rest-hateoas-maturity-level-3)