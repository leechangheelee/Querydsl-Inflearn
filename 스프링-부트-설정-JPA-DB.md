## **프로젝트 환경설정**

![image](https://user-images.githubusercontent.com/79301439/188391639-3553cf34-0a9b-4253-ac74-b7129782e131.png)

```yml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/querydsl
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
#        show_sql: true
        format_sql: true

logging.level:
  org.hibernate.SQL: debug
#  org.hibernate.type: trace
```

![image](https://user-images.githubusercontent.com/79301439/188391808-cfbd5a49-9484-418f-908b-47795a5543e8.png)

![image](https://user-images.githubusercontent.com/79301439/188391870-5aa3e768-7869-49e7-aad4-064d88f5caa4.png)

```gradle
...
dependencies {
    ...
    implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.8'
    ...
}
```

![image](https://user-images.githubusercontent.com/79301439/188392096-d9aba5e7-33de-4f0d-90af-7aa16d4202d9.png)
