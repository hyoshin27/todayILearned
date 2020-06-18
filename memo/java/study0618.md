#CORS (Cross-Origin Resource Sharing)

웹 브라우저에서 외부 도메인 서버와 통신하기 위한 방식을 표준화한 스펙. 
서버와 클라이언트가 정해진 헤더를 통해 서로 요청이나 응답에 반응할지 결정하는 방식으로 교차 출처 자원 공유라는 이름으로 표준화가 되었다.

>preflight request (사전 요청)

요청하려는 URL 이 외부 도메인일 경우 웹 부라우전은 사전요청을 먼저 하게 된다.
실제로 요청하려는 경로와 같은 URL에 대해 options 메서드로 요청을 미리 날보고 요청 권한이 있는지 확인한다.
CORS 요청을 편법 없이 하기 위해서는 클라이언트의 처리만으로 안되고 해당 서버 측에서 추가 처리 사항이 필요하다.

>서버에서 CORS 요청 핸들링 하기

모든 외부 도메인에서 모든 요청을 허용할 경우 처리

-Access-Control-Allow-Origin : *    

-Access-Control-Allow-Methods : GET,POST,PUT,DELETE,OPTIONS

-Access-Control-Mas-Age (얼마나 오랫동안 preflight 요청이 캐싱 될 수 있는지): 3600 

-Access-Control-Allow-Header (실제 요청시 사용할 수 있는 http 헤): Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers, Authorization



