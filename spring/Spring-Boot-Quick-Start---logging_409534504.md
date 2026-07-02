1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Spring](Spring_120848385.md)
4.  [Spring Boot](Spring-Boot_223477765.md)
5.  [Spring Boot 기본](387350721.md)

# Spring Boot Quick Start - logging

Created by Dongwook Han, last modified on 3월 14, 2024

- SLF4j(simple Logging Facade For Java) logging 관리
- 복잡한 로깅 프레임워크를 쉽게 사용할 수 있도록 도와주는 facade
- 복잡한 서브 시스템을 쉽게 사용할 수 있도록 간단하고 통일된 인터페이스 제공
- Spring Boot 에서는 LogBack 으로 로깅
- spring-boot-starter-logging : log starter dependency
  - logback : spring boot 는 기본적으로 logback 사용
  - log4j
  - java util logging

- 기본 로그 레벨 INFO
- 로깅 직접 관리시 logback.xml 추가하여 설정
```xml
xml
  <?xml version="1.0" encoding="UTF-8"?>

  <configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
    <logger name="com.nettoall" level="DEBUG"/>


  <appender name="fileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>src/main/resources/logs/board_log.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <fileNamePattern>
        src/main/resources/logs/myboard.%d{yyyy-mm-dd}.log.gz
      </fileNamePattern>
      <maxHistory>30</maxHistory>
    </rollingPolicy>
    <encoder>
      <pattern>
        %d{yyyy:MM:dd HH:mm:ss.SSS} %s-5level --- [%thread] %logger{35} : %msg %n
      </pattern>
    </encoder>
  </appender>

  ....

  <logger name="com.nettoall" level="warn" additivity="false">
    <appender-ref ref="consoleAppender" />
    <appender-ref ref="fileAppender" />
  </logger>

  <root level="error">
    <appender-ref ref="consoleAppender" />
  </root>

  </configuration>
```

- src/main/resources/ 에 추가
    - spring와 일반 자바 프로그램일 경우 logback.xml 추가
    - Spring Boot 인 경우 **logback-spring.xml** 을 추가

  - spring이 기본으로 제공하는 설정 파일 사용시 include base.xml 설정, 사용하지 않을시 제외한 후 appendder와 loggder 추가
  - appender 와 logger 설정으로 구성됨
  - 로그 메시지 패턴을 지정\<encoder\>\<pattern\>\</pattern\>\</encoder\> 로 설정
    - %d : 시간 (yyyy-MM-dd HH:mm:ss.SSS )
    - %date{format} : 원하는 형태로 시간 정보 출력
    - %logger{length} : Logger 이름, {length}는 최대 자릿수
    - %thread : 현재 스레드 이름
    - %-5level : 로그 레벨. 5는 출력 고정폭 값
    - %msg : 로그 메시지
    - %n : 개행 처리

- default logback 사용시 application.properties에 로그 설정 추가
```properties
properties
  #Logging Setting
  logging.level.com.nettoall=trace   # 패키지별.로그레벨 지정
  logging.file=src/main/resources/board_log.log  # 로그 메시지 파일 출력
  # 2.7.6 부터는 다음과 같이 사용
  #logging.file.name=src/main/resource/board_log.log
```
