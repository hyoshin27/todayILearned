#자바의 정석 

>3.연산자(Operator)

1. 단항 > 산술 > 비교 > 논리 > 삼항 > 대입

-단항(++, --, +, -, ~, !)

-산술(*, /, %), (+, -), (<<, >>)

-비교(<, >, <=, >=, instance of), (==, !=)

-논리(&, ^, |, &&, ||)

-삼항(?:)

-대입(=, +=, -=, *=, /=, %=, <<=, >>=, &=, ^=, |=)

2.단항 연산자와 대입 연산자를 제외한 모든 연산의 진행방향은 왼쪽에서 오른쪽이다.

- 3 + 4 -5 : 3 + 4 -> 7 + 5

- x = y = 3 : y = 3 -> x = 3


~~~
char c1 = 'a';
char c2 = c1 +1;  // 컴파일 에러 발생
char c2 = 'a' + 1;  // 컴파일 에러 없음 - 리터럴 간의 연산이기 때문에
~~~

-상수, 리터럴 간의 연산은 실행 과정동안 변하는 값이 아니라 컴파일 시에 컴파일러가 계산해서 그 결과로 대체함

- 컴파일 전 : char c2 = 'a' + 1; 컴파일 후: char c2 = 'b';

~~~
String str1 = "abc";
String str2 = new String("abc");

str1 == "abc";   //true
str2 == "abc";   //false    내용은 같지만 다른 객체이기 때문에
str1.equals("abc"); //true
str2.equals("abc"); //true  객체가 달라도 내용이 같으면 true
str2.equals("ABC"); //false
str2.equalsIgnoreCase("ABC"); //true -대소문자 구분하고 싶지 않으면 equalsIgnoreCase()
~~~
