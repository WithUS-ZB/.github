# Access-Control-Expose-Headers
작성자: 박지은
## 1. 문제 상황

헤더로 토큰을 주는데 프론트에서 값을 가져오지 못하는 문제

브라우저의 개발자 도구에서는 추가한 헤더를 확인할 수 있는데, 프론트(Vue.js)에서는 추가한 헤더만 접근이 불가능했다.

## 2. 원인

CORS-safelisted response header라는 것이 있는데,

이 CORS-safelisted response header 에 해당하는 헤더는 CORS 응답인 경우에서 클라이언트의 스크립트에 노출되어도 안전하다고 여겨지는 헤더들이기 때문에 별도의 설정 없이도 스크립트(이 경우 Vue.js)에서 접근 가능하다.

그러나 추가적인 헤더인 경우 CORS-safelisted response header 에 해당하지 않기 때문에 브라우저의 스크립트에서 접근할 수 없었던 것이다.

> **CORS-safelisted response header 에 속하는 헤더들**
>
>
> Cache-Control
>
> Content-Language
>
> Content-Length
>
> Content-Type
>
> Expires
>
> Last-Modified
>
> Pragma
>
> [문서](https://developer.mozilla.org/en-US/docs/Glossary/CORS-safelisted_response_header)
>

## 3. 해결 방안

Access-Control-Expose-Headers 응답 헤더를 사용하면 된다.

Access-Control-Expose-Headers는 cross-origin 요청에 대한 응답인 경우, 해당 응답의 헤더 중에서 브라우저의 스크립트(Java Script...등)가 접근가능한 헤더를 지정하는데 사용한다.

따라서 Spring Boot 에서는 아래 이미지와 같이 `Access-Control-Expose-Headers 응답 헤더를 설정하면 된다.`

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/api/auth/**")
        .allowCredentials(true)
        .allowedOriginPatterns("*")
        .exposedHeaders("Authorization"); // 노출할 사용자 지정 응답 헤더 설정
  }
}
```