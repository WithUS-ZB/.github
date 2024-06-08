# There is already 'gatheringController' bean method 에러 발생
작성자: 박강락
## 1. 문제 상황

컨트롤러 코드 작성시 There is already 'gatheringController' bean method 에러 발생

## 2. 원인

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/639d7f4b-a2a8-4e6f-b2f8-75c80ce32576/9297a5c9-9047-4cf2-94c1-8009988184d6/Untitled.png)

## 3. 해결 방안
바보같은 실수

`getGatheringList` 메서드와 `getGathering` 메서드 모두 같은 Get방식인데다가, url도 동일한 상태로 두고 코드를 작성했다. 평소 당연하게 생각한 부분을 캐치 하지 못해 아까운 30분을 버렸다.