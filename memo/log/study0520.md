#Spring Boot Logging과 Profile 전략

>Logback

자바 오픈소스 로깅 프레임워크, SLF4J 구현체

스프링 부트 기본으로 설정되어있어 따로 설정하지 않아도 된다.

log4j, log4j2 등과 성늘을 비교했을 때에도 logback 이 성능이 좋다.

spring-boot-starter-web 안에 spring-boot-starter-logging 에 구현체가 있다.

Logback 을 이용하여 로깅을 수행하기 위해서 필요한 주요 설정으로는 Logger, Appender, Encoder 의 3가지가 있다.

>Appender

log의 형태를 설정, 로그메시지가 출력될 대상을 결정하는 요소(콘솔에 출력할지, 파일로 출력할지)

appender class의 종류

1.ch.qos.logback.core.ConsoleAppender : 콘솔에 로그 찍음. 로그를 OutputStream에 작성하여 콘솔에 출력되로록 한다.

2.ch.qos.logback.core.FileAppender : 파일에 로그 찍음, 최대 보관 일 수 등 지정할수 있다.

3.ch.qos.logback.core.rolling.RollingFileAppender : 여러개의 파일을 롤링, 순회하면서 로그를 찍는다.

4.ch.qos.logback.classic.net.SMTPAppender : 로그에 메일에 찍어 보낸다.

5.ch.qos.logback.classic.db.DBAppender : DB(데이터베이스)에 로그를 찍는다.

>Logger

-지역설정

-additivity 값은 root설정 상속 유무 설정(default = true)

>Encoder

Appender 에 포함되어 사용자가 지정한 형식으로 표현 될 로그메시지 반환하는 역할 담당 요소

Encoder는 바이트를 소유하고 있는 Appender가 관리하는 OutputStream 에 쓸 시간과 내용을 제어할 수 있다.

FileAppender 와 하위 클래스는 Encoder를 필요로 하고 더이상 layout은 사용하지 않는다.

-application.yml

~~~
logging:
  config: classpath:logback-spring.xml
~~~

으로 하면 logback-spring.xml 에서 로그 설정을 할 수 있다.

~~~
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    
    
    <!--  Appender 시작 -->
    
    <!--  파일을 만드는 부분 -->
    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/adcenter-api.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/old/%d{yyyyMMdd-HH}.log.gz</fileNamePattern>
        </rollingPolicy>
        
        <!--  Encoder는 시작 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS, Asia/Seoul} ${LOG_LEVEL_PATTERN:-%5p} [%class{5} > %method:%line]</pattern>
        </encoder>
        <!--  Encoder는 끝 -->
    </appender>

    <!--  로그를 찍는 부분  -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        
        <!--  Encoder는 시작 -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS, Asia/Seoul} ${LOG_LEVEL_PATTERN:-%5p} [%class{5} > %method:%line]</pattern>
        </encoder>
        <!--  Encoder는 끝 -->
    </appender>
    <!--  Appender 끝 -->
    
    <springProfile name="local">
        <logger name="com.test" level="DEBUG"/>
        <logger name="org.springframework" level="INFO"/>
        <root level="info">
            <appender-ref ref="STDOUT" />
        </root>
    </springProfile>
</configuration>
~~~

여기서 springProfile 내용은 application.yml에 아래와 같이 변환할 수도 있다.

~~~
logging:
  level:
    com:
      test:
        type: DEBUG
    org:
      springframework:
        type: INFO
    root: STDOUT      
~~~


|key|설명|
|-------------|-----------------|
|logging.file|절대 경로로 표현되거나 현재 경로의 상태 경로로 로그파일명을 지정|
|logging.file.path|logging.file 값이 없을때 동작한다. 지정된 경로에 spring 로그 남김|
|logging.file.max-size|로그파일의 사이즈가 지정된 사이즈가 넘으면 파일명에 index 추가한 후 새로운 파일 작성|
|logging.file.max-history| 지정된 일수가 지난 로그를 자동으로 삭제|
|logging.level| ERROR > WARN > INFO > DEBUG > TRACE|

-ERROR : 요청을 처리하는 중 오류가 발생하는 경우 표시

-WARN : 처리 가능한 문제, 향후 시스템 에러의 원인이 될 수 있는 경고성 메세지 표지

-INFO : 상태변경과 같은 정보성 로그 표시

-DEBUG :프로그램을 디버깅하기 위한 정보를 표시

-TRACE : 추적 레벨은 DEBUG보다 훨씬 상세한 정보

>Pattern 

|형식|설명|
|-----------|--------------------------|
|%Logger{length}|Logger name을 length 길이만큼 줄임 |
|%-5level|로그레벨 -5는 출력의 고정폭 값(5글자) |
|%msg | 로그메시지(=%message)|
|${PID:-}|프로세스아이디|
|%d|로그 기록시간|
|%p|로깅 레벨|
|%F|로깅이 발생한 프로그램의 파일명|
|%M|로깅이 발생한 프로그램의 메소드명|
|%I|로깅이 발생한 호출지의 정보|
|%L|로깅이 발생한 호출지의 라인|
|%thread|현재 Thread 명|
|%t|로깅이 발생한 Thread 명|
|%c|로깅이 발생한 카테고리|
|%C|로깅이 발생한 클래스 명|
|%m|로그메시지|
|%n|줄바꿈(new line)|
|%%|%출력|
|%r|애플리케이션 시작 이후 로깅이 발생한 시점까지의 시간(ms)|



참조 : 
공식메뉴얼 http://logback.qos.ch/manual/index.html
스프링부트 doc https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-logging
https://meetup.toast.com/posts/149
https://goddaehee.tistory.com/206
