#클린코드

5장 - 형식 맞추기

코드 형식은 의사소통의 일환
적절한 행 길이 유지(작은파일이 이해하기 쉬움)
신문기사처럼 작성(표제-요약-세세한 사실)
개념은 빈행으로 분리
세로 밀집도(연관성 있는 것은 가까이)
변수 선언은 사용하는 위치에 최대한 가까이
가로 형식 맞추기

6장 - 객체와 자료 구조

자료 추상화
디미터의 법칙(자신이 조작하는 객체의 속사정을 몰라야 함다.)
구조체 감추기
객체는 동작을 공개하고 자료를 숨긴다.

7장 - 오류처리

오류코드보다 예외를 사용하라
try-catch-finally문부터 작성하라
미확인 예외를 사용하라(확인된 예외 하위단계에서 코드를 바꾸면 상위단계메서드도 변경되기 때문)
정상흐름 정의
null 반환 금지(null 반환은 일거리를 늘리고 확인비용이 점점 커짐)
오류처리와 프로그램 논리를 분리하면 독립적인 추론이 가능해지고 유지보수성도 높아짐

8장 -경계

학습테스(외부코드를 사용할때는 테스트케이스 먼저 작성하여 외부코드 익히기)
경계에 위치하는 코드는 깔끔하게 분리하고 우리 코드에 의존하는 편이 낫다(외부패키지가 변했을때 위험이 줄어듦)