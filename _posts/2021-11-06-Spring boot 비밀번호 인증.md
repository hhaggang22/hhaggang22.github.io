---
layout: post
---

## Spring boot 에서 SMTP 를 이용하여 비밀번호 인증 메일 구현하기

웨딩 플래너 중개 플랫폼 사이트를 제작 중에, 비밀번호 찾기 기능 중 비밀번호 인증 메일을 구현해야 한다.

현재 가입 단계에서 아직 휴대폰 인증이 구현되지가 않아서 사용자로부터 휴대폰 번호 정보를 받지 못하기 때문에

`이메일 확인 -> 비밀번호 인증 메일 전송 -> 이메일에서 인증 링크 클릭 후 사이트로 재접속 -> 비밀번호 변경`

이 순서로 진행하려고 한다.

---

### SMTP란 무엇인가?

SMTP는 Simple Mail Transfer Protocol의 약자로 인터넷 메일을 송수신하는데에 사용하는 TCP/IP 프로토콜이다.
TCP port는 25번 또는 587번으로 Spring boot의 yml에서 port 설정을 할 때 25번이나 587을 사용하면 된다.

---

### 개발환경

`Spring boot, Intelli J, Mac OS, Gradle, 다음 SMTP 계정, Swagger(테스트)`

---

### 개발과정

1.  build.gradle dependencies 설정

    ```markdown
    implementation group: 'org.springframework.boot', name: 'spring-boot-starter-mail', version: '1.2.0.RELEASE'
    ```

2.  application.yml에 메일 설정 작성

    ```yaml
    //다음 smpt
    //smtp.daum.net (SSL 사용, 포트 465)

    spring:
      mail:
        host: smtp.daum.net
        port: 465
        username: SMTP용 daum 계정
        password: SMTP용 daum 계정 비밀번호
        properties:
          mail:
            smtp:
              starttls:
                enable: true
                required: true
              auth: true
              connectiontimeout: 5000
              timeout: 5000
              writetimeout: 5000
    ```

3.  이메일 Service 생성

    ```java
        @Service
        @RequiredArgsConstructor
        public class EmailSenderService {

            private final JavaMailSender javaMailSender;

            @Async
            public void sendEmail(SimpleMailMessage email) {
                javaMailSender.send(email);
            }

        }

    ```
