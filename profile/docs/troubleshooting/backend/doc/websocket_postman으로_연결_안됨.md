# websocket postman으로 연결 안됨
작성자: 박지은
## 1. 문제 상황

ws://localhost:8080/end-point로 websocket연결을 시도했을 때 안되는 현상

## 2. 원인

http와 WebSocket의 Security chain, config는 완전히 독립적이다

웹소켓 서버는 특정 엔드포인트(예: ws://example.com/socket 또는 wss://secure.example.com/socket)를 통해 연결 요청을 받아들여야 한다.

이러한 엔드포인트를 열어두지 않으면 클라이언트가 서버에 접속할 수 없다.

## 3. 해결 방안

/ws을 허용하여 websocket을 클라이언트가 통신할 수 있게한다.

websocket의 시큐리티 설정은 따로 추가로 해준다.