# Mixed Content 에러에 의한 API 호출 불가 문제
작성자: 임국희
## 1. 문제 상황
- Vercel로 배포한 사이트에서 서버로 API 요청을 보낼 시 Mixed Content 에러가 발생

## 2. 원인
- Vercel로 배포된 사이트에서 HTTPS로 리소스를 로드한 후 서버에 HTTP로 요청을 보내면서 브라우저 보안 정책에 따라 차단

## 3. 해결 방안
- 배포된 EC2 서버에 SSL 인증서를 적용해 HTTPS 요청을 받을 수 있도록 설정
