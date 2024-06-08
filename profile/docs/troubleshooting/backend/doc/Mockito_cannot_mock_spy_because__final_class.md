# Mockito cannot mock/spy because: final class
작성자: 박지은
## 1. 문제 상황

[나의 관련 블로그 글](https://jepa.tistory.com/72#2.%20%EC%84%A4%EC%A0%95%ED%8C%8C%EC%9D%BC%20%EC%B6%94%EA%B0%80-1)

외부 API를 사용하는 service의 테스트 코드를 작성하려다 모킹을 진행하지 못하는 이슈가 있었다.

Cannot mock/spy class net.nurigo.sdk.message.service.DefaultMessageService
Mockito cannot mock/spy because :

- final class

## 2. 원인

final 클래스여서 모킹을 진행하지 못 하는 것 이었다.

## 3. 해결 방안

[https://github.com/mockito/mockito/wiki/What's-new-in-Mockito-2#mock-the-unmockable-opt-in-mocking-of-final-classesmethods](https://github.com/mockito/mockito/wiki/What%27s-new-in-Mockito-2#mock-the-unmockable-opt-in-mocking-of-final-classesmethods)

위의 Mockito2 문서를 참고하여 final 클래스 모킹이 되게 만들었다.

기존 코드로는

```java
SendAuthSmsRequestDto requestDto = new SendAuthSmsRequestDto("010-7777-7777");
```

와 같이 모킹이 되지 않아 직접 객체를 생성해주었다.

이렇게 하면 유료인 api를 직접 호출하게 되면서 문자 메시지가 실제로 발행되고 결제가 이루어지게 되었다.

Mockito2 를 적용하여

- 의존성

  아래의 의존성을 추가해주었다.

    ```java
    testImplementation "org.mockito:mockito-core:1.+"
    ```

- org.mockito.plugins.MockMaker 파일

    ```java
    mock-maker-inline
    ```

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/639d7f4b-a2a8-4e6f-b2f8-75c80ce32576/99da3305-34b4-4984-8b01-8a5fd3a8852e/Untitled.png)

  src/test/resources 하단에 위치해 주었다.

- final class mocking

    ```java
    DefaultMessageService messageService = mock(DefaultMessageService.class);
    ```

  문제가 되는 클래스를 모킹 해주었다.