# 미들웨어
## 애플리케이션 미들웨어
app.use()를 통해 전체 애플리케이션에 적용됩니다.

로깅, 인증, 파싱, 응답 구조 통일 등의 역할을 합니다.

next()를 호출하여 다음 미들웨어로 흐름을 넘길 수 있습니다.

js
```
const express = require("express");
const app = express();

// 모든 요청에 대해 실행
app.use((req, res, next) => {
  console.log("요청 시간:", Date.now());
  next();
});

// 특정 경로에만 실행
app.use('/user/:id', (req, res, next) => {
  console.log('Request Type:', req.method);
  next();
});
```
## 라우터 레벨 미들웨어
###라우팅을 기능별로 분리하면 유지보수가 쉬워집니다.

예: 회원, 상품, 관리자 기능을 각각 라우터 파일로 분리

```
// index.js
app.get("/user/login", ...);
app.get("/product/list", ...);

// user.router.js
router.get("/login", ...);

// product.router.js
router.get("/list", ...);

// index.js에서 import 후 사용
app.use("/user", userRouter);
app.use("/product", productRouter);
```

## 오류 처리 미들웨어
에러 처리 미들웨어는 4개의 인수가 필요합니다. (err, req, res, next)

next를 생략하면 일반 미들웨어로 인식됩니다.

```
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```
## 정적 파일 미들웨어 주요 옵션
| 옵션           | 설명                                  | 타입        | 기본값          |
| ------------ | ----------------------------------- | --------- | ------------ |
| dotfiles     | dotfile 제공 여부 (allow, deny, ignore) | 문자열       | "ignore"     |
| etag         | ETag 헤더 사용 여부                       | boolean   | true         |
| extensions   | 확장자 fallback 목록                     | 배열        | \[]          |
| index        | 디렉토리 인덱스 파일 이름                      | 문자열/false | "index.html" |
| lastModified | Last-Modified 헤더 자동 추가              | boolean   | true         |
| maxAge       | 캐시 최대 시간 (ms)                       | 숫자        | 0            |
| redirect     | 디렉토리 요청 시 슬래시 자동 리다이렉트              | boolean   | true         |
| setHeaders   | 응답 헤더 커스터마이징 함수                     | 함수        | -            |


## HTTP 상태 코드
### 성공 (2xx)
| 코드  | 의미              | 예시               |
| --- | --------------- | ---------------- |
| 200 | OK              | 일반적인 GET/POST 응답 |
| 201 | Created         | 회원가입, 글 작성       |
| 204 | No Content      | 삭제 요청 후          |
| 206 | Partial Content | 파일 다운로드 일부 요청    |


### 리다이렉션 (3xx)
| 코드  | 의미                | 예시          |
| --- | ----------------- | ----------- |
| 301 | Moved Permanently | URL 리뉴얼     |
| 302 | Found             | 로그인 후 리다이렉트 |
| 304 | Not Modified      | 브라우저 캐싱 최적화 |


### 클라이언트 오류 (4xx)
| 코드  | 의미                 | 예시             |
| --- | ------------------ | -------------- |
| 400 | Bad Request        | JSON 파싱 실패     |
| 401 | Unauthorized       | 로그인 필요         |
| 403 | Forbidden          | 권한 부족          |
| 404 | Not Found          | 잘못된 URL        |
| 405 | Method Not Allowed | POST만 가능한데 GET |
| 409 | Conflict           | 중복 회원가입        |
| 429 | Too Many Requests  | API 속도 제한      |


### 서버 오류 (5xx)
| 코드  | 의미                    | 예시              |
| --- | --------------------- | --------------- |
| 500 | Internal Server Error | 예상치 못한 서버 예외    |
| 502 | Bad Gateway           | 프록시 문제          |
| 503 | Service Unavailable   | 서버 점검 또는 과부하    |
| 504 | Gateway Timeout       | DB/외부 API 응답 지연 |

