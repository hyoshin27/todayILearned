#자바의 정석 

>5. 배열(Array)

배열이란?

-같은 타입의 여러 변수를 하나의 묶음으로 다루는 것

선언

~~~
int[] score = new int[] {50,60,70,80,90};
int[] score = {50,60,70,80,90}; //new int[] 생략할 수 있음

int[] score;
score = new int[] {50,60,70,80,90}; //ok
score = {50,60,70,80,90}; //에러 new int[] 생략할 수 없음

//같은 예로 int 배열을 매개변수로 받는 함수
int add(int[] arr){}

int result = add(new int[] {50,60,70,80,90});   //ok
int result = add({50,60,70,80,90}); //에러 new int[] 생략할 수 없음
~~~

배열은 괄호 안에 아무것도 넣지 않으면 길이가 0인 배열생성

~~~
//모두 같다. 참조변수의 기본값은 null이지만 길이 0인 배열로 초기화
int[] score = new int[0];
int[] score = new int[];
int[] score = {};
~~~

배열의 복사

-System.arraycopy() 를 사용하면 간단하고 빠르게 복사할 수 있다.

~~~
for(int i =0; i < num.length; i++){ //System.arraycopy(num, 0, newNum, 0, num.length); 와 같다
    newNum[i] = num[i];
}

System.arraycopy(num, 0, newNum, 0, num.length); //num[0]부터 newNum[0]으로 num.length개의 데이터를 복사

~~~

