#리팩토링 자바스크립트

>리팩토링이란

코드의 동작을 바꾸지 않고 코드 변경 -> 행동을 유지하면서 품질 향상. 버그를 수정하거나 새로운 기능 작성x

>코드 품질을 좋게 하기위한 원칙

SOLID : 단일 책임(Single responsibility),
        개방/폐쇄(Open/close),
        리스코브의 대체(Liskov substitution),
        인터페이스 분리 (Interface segregation),
        종속성 반전(Dependency inversion)

DRY : 반복하지 말 것 (Don't Re)peat Yourself)
KISS : 정말 간단해야 할 것 (Keep It Simple, Stupid)
GRASP : 일반적인 책임 할당 소프트웨어 패턴(General Responsibility Assignment Software Pattern)
YAGNI : 나중에 필요없어질 것을 고려(Ya Ain't Gonna Need It)

인터페이스를 단순화하기 위한 함수와 모듈 추출
테스트 이용하여 코드의 동작 확인
가능하면 불순한 기능 피하기
변수 및 함수의 이름 잘 짓기