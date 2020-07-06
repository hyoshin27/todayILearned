#Restful Api

>REST 란

Representational State Transfer 의 약자 자원의 상태를 주고 받고 변경하는 것

네트워크 상에서 client 와 server 사이의 통신 방식 중 하나

>3요소 

Data, Meta Data, HATEOAS

>장점

HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.

서버와 클라이언트를 명확하게 구분하며 각각 맞는 언어로 따로 구현할 수 있다.(다양한 클라이언트)

서버와 클라이언트의 의존성이 줄어든다.

재사용성이 높다.

의도하는 바를 쉽게 파악할 수 있다.

>단점

예외사항이 있다.(GET 사용시 보안상이나 파라미터의 제약 등)

구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.

>종류

-Create : 생성(POST)

-Read : 조회(GET)

-Update : 수정(PUT)

-Delete : 삭제(DELETE)

-HEAD: header 정보 조회(HEAD)

>HATEOAS(Hypermedia As The Engine Of Application State)

REST 구현의 3단계 

-level3 : hypermedia controls. HATEOAS라는 개념을 통해 해당 자원에 대해 호출 가능한 API에 대한 정보를 자원의 상태를 반영하여 표현

-level2 : http verbs. HTTP 동사를 이용하여 자원에 대해 수행하고자 하는 작업의 종류를 표현

-level1 : resources. 서버가 가진 모든 데이터를 하나의 자원으로 관리하며 고유한 URI를 제공함으로써 클라이언트가 하나의 단일 API 엔드포인트가 아닌 각 자원과 직접 커뮤니케이션할 수 있도록 함

-level0 : the swamp of POX. End-point 자체가 하나의 서비스를 의미합니다. 즉 웹 매커니즘을 전혀 사용하지 않고, HTTP를 단순히 원격 통신을 위한 전송 시스템 으로만 생각하는 것

HATEOAS format : web link, HAL(json, xml)