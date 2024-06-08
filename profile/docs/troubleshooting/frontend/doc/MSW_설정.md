# MSW 설정
작성자: 우승찬
## 1. 문제 상황

프론트엔드 개발 도중, 필요한 데이터를 받지 못하는 상황이 발생 하였습니다 개발 초기 단계이기 때문에 API가 준비되지 않는 상황이라 대안이 필요 했습니다.

## 2. 원인
백엔드에서 아직 API가 완성되지 않았습니다

## 3. 해결 방안

대안을 찾던중에 MSW라는 mock service worker라는 API 모킹 라이브러리를 찾아서 도입하게 되었습니다

여기서 주의할 점은 과거 버전에는 rest를 사용하여 사용했지만 MSW가 버전이 올라가면서 http로 통합되어 사용합니다

과거 사용법

```jsx
import { rest } from "msw";

const posts = ["게시글1", "게시글2", "게시글3"];

export const handlers = [
  rest.get("/posts", (req, res, ctx) => {
    return res(ctx.status(200), ctx.json(todos));
  }),
  
  rest.post("/posts", (req, res, ctx) => {
    posts.push(req.body);
    return res(ctx.status(201));
  })
];
```

현재 사용

```jsx
import { http, HttpResponse } from 'msw';
const URL = import.meta.env.VITE_SERVER_URL

export const handlers = [
  //회원정보 얻기
  
  http.get(`${URL}/api/member/detail`, async () => {
    return HttpResponse.json({
      member_id: 1,
      email: 'example1@example.com',
      nickname: '승찬',
      birthDate: '1990-01-01',
      gender: 'MALE',
      phoneNumber: null,
      profileImg: null,
      signup_Path: 'NORMAL',
      signup_Dttm: '2024-04-30T22:57:59.187977',
      membership: 'FREE',
    });
  }),
  
  // 모임 리스트 정보 얻기
  http.get(`${URL}/api/gathering/list`, async () => {
    return HttpResponse.json([
      {
        id: 1,
        nickname: '김옥순',
        title: '꽃꽃이 아줌마 모임',
        kind: 'MEETING',
        like: 10,
        date_st: '2024-05-10',
        date_end: '2024-05-11',
        time: '13:05',
        day: '2024-05-20',
        posted: '2024-05-09',
        category: '꽃꽃이',
        personnel: '8',
        address: '경기 성남시 분당구 서판교로 32',
        address_detail: '자이숲 어린이집',
        location: { lat: '37.1507522209507', lng: '127.088961136958' },
        writer: 1,
        fee: 7000,
        participantSelectionMethod: 'UNLIMITED_APPLICATION',
        participantsType: 'ADULT',
        title_img: 'https://cdn.pixabay.com/photo/2017/02/15/13/40/tulips-2068692_1280.jpg',
        sub_img: ['여기도', '서브', '이미지 주소 넣으면 됩니다'],
        content: '여기는 모임을 하는 내용',
        inn: 5,
      },
      {
        id: 2,
        nickname: '박광덕',
        title: '한강 자전거 갱슅',
        kind: 'MEETING',
        like: 6,
        date_st: '2024-05-10',
        date_end: '2024-05-11',
        time: '14:05',
        day: '2024-05-29',
        posted: '2024-05-02',
        category: '자전거',
        personnel: '5',
        address: '전남 강진군 강진읍 동성로 2',
        address_detail: '자이숲 어린이집',
        location: { lat: '37.1507522209507', lng: '127.088961136958' },
        writer: 2,
        fee: 0,
        participantSelectionMethod: 'FIRST_COME',
        participantsType: 'MINOR',
        title_img: 'https://cdn.pixabay.com/photo/2023/08/27/00/08/cycling-8215968_1280.jpg',
        sub_img: ['여기도', '서브', '이미지 주소 넣으면 됩니다'],
        content: '여기는 모임을 하는 내용',
        inn: 3,
      },
    ]);
  }),
];

```