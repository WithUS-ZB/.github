# "Unsupported Media Type""Content-Type 'application/octet-stream' is not supported”
작성자: 박강락
## 1. 문제 상황
"Unsupported Media Type"  에러발생
![img_19.png](../img/img_19.png)

## 2. 원인

@RequstParam, @RequstBody 같이 사용해야되는경우
![img_20.png](../img/img_20.png)

@RequestPart 로 변경해서 보낸다.

RequestPart 로 변경하는경우

Body-form-data 형식로 (키-밸류)  데이터를 보내야 하는데

![img_21.png](../img/img_21.png)

addGatheringRequest 쪽의 Value값 부분을 Json 형식으로 받아들이지 못함
![img_22.png](../img/img_22.png)

Content-Type을 Json 형식으로 명시해주면된다.

## 3. 해결 방안

![img_23.png](../img/img_23.png)

수정 후 정상작동