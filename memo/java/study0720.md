#자바의 정석 

>4. 조건문과 반복문

if 문보다 switch 문이 더 제약이 많지만 switch 문이 더 효율적이다.

~~~
public void ifTest(int score){

    if(score > 90){
        System.out.println("A");
    } else if(score > 80){  // 80 < score <= 90
        System.out.println("B");
    } else if(score > 70){  //70 < score <= 80
        System.out.println("C");
    } else if(score > 60){  //60 < score <= 70
        System.out.println("D");
    }
}
~~~

고의적인 break 생략
~~~
switch (level){
    case 3: //삭제, 쓰기, 읽기 권한
        grantDelete();
    case 2: //쓰기, 읽기 권한
        grantWrite();
    case 1: //읽기 권한
        grantRead();
}
~~~

switch 제약조건 

- 중복 허용 x

- 정수 또는 문자열

~~~
public test(int num, int num2){
    final int ONE = 1;
    switch(num){
        case '1':   //OK 문자 상수(정수 상수 49와 같음)
        case "HI":   //OK JDK1.7부터 허용
        case num2:   //에러 변수 안됨
        case 10.0:   //에러 실수 안됨
    }
}
~~~

이름 붙인 반복문
~~~
@Test
public void outerTest(){
    int i = 0;
    outer:
    while(true){
        for(;;){
            i++;
            System.out.println("==="+i);
            if(i==5){
                System.out.println("for문 끝");
                break;
            }
            if(i==9){
                System.out.println("while 끝");
                break outer;
            }
        }
    }
}
~~~

결과
~~~
===1
===2
===3
===4
===5
for문 끝
===6
===7
===8
===9
while 끝
~~~