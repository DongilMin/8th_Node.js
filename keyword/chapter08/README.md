## Swagger

Swagger는 RESTful API를 설계, 빌드, 문서화 및 사용하기 위한 오픈 소스 도구 세트입니다.

### 주요 구성 요소

1. **Swagger Editor**: API 디자인을 위한 브라우저 기반 편집기
2. **Swagger UI**: API 문서를 시각화하고 상호작용할 수 있는 웹 인터페이스
3. **Swagger Codegen**: API 정의에서 서버 스텁 및 클라이언트 SDK 생성 도구

### 장점

- API 시각화 및 문서화 자동화
- 코드와 문서의 일관성 유지
- 테스트 인터페이스 제공
- 다양한 언어 및 프레임워크 지원

### 단점

- 대규모 API의 경우 문서가 복잡해질 수 있음
- 초기 설정에 시간 투자 필요
- 모든 API 시나리오를 완벽하게 문서화하기 어려움

```
const express = require('express');
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Node.js API 프로젝트',
      version: '1.0.0',
    },
  },
  apis: ['./routes/*.js'],
};

const specs = swaggerJsdoc(options);
const app = express();

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(specs));
```

## OpenAPI

OpenAPI는 RESTful API를 위한 언어에 구애받지 않는 인터페이스 명세(Specification)입니다. 2015년에 Swagger Specification이 Linux Foundation의 오픈소스 프로젝트로 기부되면서 OpenAPI Initiative(OAI)로 이관되었고, 이름이 OpenAPI Specification으로 변경되었습니다.

### 특징

- JSON 또는 YAML 형식으로 작성
- API의 전체 구조 정의 (엔드포인트, 작업, 매개변수, 응답 등)
- 언어 및 프레임워크 독립적
- 자동화된 문서 생성 및 코드 생성 지원

```
openapi: 3.0.0
info:
  title: 샘플 API
  version: 1.0.0
paths:
  /users:
    get:
      summary: 사용자 목록 조회
      responses:
        '200':
          description: 성공적인 응답
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
```

## OpenAPI Component

OpenAPI Component는 OpenAPI 명세의 재사용 가능한 부분을 정의하는 메커니즘입니다. OpenAPI 3.0부터 도입된 `components` 섹션에서 API 전체에서 재활용할 수 있는 요소들을 정의합니다.

### 주요 Component 유형

1. **schemas**: 데이터 모델 정의
2. **responses**: 재사용 가능한 응답 정의
3. **parameters**: 공통 매개변수 정의
4. **examples**: 요청/응답 예시
5. **requestBodies**: 요청 본문 정의
6. **headers**: 재사용 가능한 헤더 정의
7. **securitySchemes**: 인증 메커니즘 정의
8. **links**: 응답과 후속 요청 간의 관계 정의
9. **callbacks**: 웹훅 등의 비동기 작업 정의

```
components:
  schemas:
    Error:
      type: object
      properties:
        code:
          type: integer
        message:
          type: string
  responses:
    NotFound:
      description: 리소스를 찾을 수 없음
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

CORS 설정

```
const express = require('express');
const cors = require('cors');
const app = express();

// 기본 CORS 설정
app.use(cors());

// 또는 세부 옵션 지정
app.use(cors({
  origin: 'http://localhost:3000',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
}));

```

Swagger 설정 및 CORS 연동
```
const express = require('express');
const cors = require('cors');
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const app = express();

// CORS 설정
app.use(cors());

// Swagger 명세 정의
const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Node.js API',
      version: '1.0.0',
      description: 'API 문서',
    },
    servers: [
      {
        url: 'http://localhost:3000',
      },
    ],
  },
  apis: ['./routes/*.js'],
};

const swaggerSpec = swaggerJsdoc(swaggerOptions);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));

// API 라우트 예시
app.get('/api/users', (req, res) => {
  res.json([{ id: 1, name: 'User 1' }]);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`서버 실행 중: http://localhost:${PORT}`);
  console.log(`Swagger UI: http://localhost:${PORT}/api-docs`);
});
```