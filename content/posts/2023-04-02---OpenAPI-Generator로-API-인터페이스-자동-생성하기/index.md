---
title: OpenAPI Generator를 활용한 API 인터페이스 자동 생성하기
date: "2023-04-02T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/OpenAPI-Generator로-API-인터페이스-자동-생성하기"
category: "Tech"
tags:
  - "openapi"
  - "typescript"
description: "TypeScript로 프론트엔드 개발 작업시 API 응답 값에 대한 타입 지정이 필요하다. 이때 많은 양의 타입을 자동화해주는 모듈을 사용할 수 있다. OpenAPI Generator는 Swagger에서 제공하는 자동화 도구 중 하나로 클라이언트에서 사용가능한 타입을 자동으로 생성해준다. "
socialImage: "./image.png"
---

> TypeScript로 프론트엔드 개발 작업시 API 응답 값에 대한 타입 지정이 필요하다. 이때 많은 양의 타입을 자동화해주는 모듈을 사용할 수 있다. OpenAPI Generator는 Swagger에서 제공하는 자동화 도구 중 하나로 클라이언트에서 사용가능한 타입을 자동으로 생성해 준다. Swagger와 연동, TypeScript 타입을 생성하는 대표적인 npm 패키지로 `openapi-typescript-codegen` 가 있다.

## **openapi-typescript-codegen**

- OAS(OpenApi Specification)를 기반으로 TypeScript 타입을 생성하는 [npm 패키지](https://www.npmjs.com/package/openapi-typescript-codegen)
- JSON, YAML 파일 및 Swagger에서 설정된 model type 값에서 타입 추론
- 서버 API의 명확한 model type 정의 필요

## 설치 및 실행

```jsx
npm i node-fetch openapi-typescript-codegen -D
```

- `package.json` script 추가

```jsx
{
	"generate:openApi-typescript": "node ./scripts/generate-openapi-types.js",
}
```

### 스크립트 생성

```jsx
const path = require("path");
const fs = require("fs");
const { promisify } = require("util");
const fetch = require("node-fetch");
const OpenAPI = require("openapi-typescript-codegen");

(async function () {
  // yaml or json
  const format = "json";
  // Swagger groups Array
  const groups = [
    "Global-Cocone",
    "Hello-Cocone",
    "Hello-Cocone-Admin",
    "Hello-Cocone-Login",
  ];
  // swagger-url or yaml
  const specURL = ""; // ex) swaggerURL/api-docs?group';

  const accessAsync = promisify(fs.access);
  const mkdirAsync = promisify(fs.mkdir);
  const writeFileAsync = promisify(fs.writeFile);

  const makeDirectory = async (path) => {
    try {
      await accessAsync(path);
    } catch (err) {
      if (err.code === "ENOENT") {
        await mkdirAsync(path, { recursive: true });
      } else {
        throw err;
      }
    }
  };

  try {
    for (let group of groups) {
      const outputPath = path.resolve(
        // 타입이 생성될 경로
        path.join(__dirname, "..", `src/types/${group.toLowerCase()}`),
      );

      const docFilePath = path.join(outputPath, `spec.${format}`);

      const response = await fetch(`${specURL}=${group}`);
      const data = await response.text();

      await makeDirectory(outputPath);
      await writeFileAsync(docFilePath, data);

      await OpenAPI.generate({
        input: docFilePath,
        output: outputPath,
        useOptions: true,
        useUnionTypes: true,
        exportServices: false,
        exportCore: false,
      });
    }
  } catch (error) {
    console.error(error);
  }
})();
```

- `scripts/generate-openapi-types.js`
- /cf/ [script 예시](https://github.com/hw0k-play/openapi-codegen-example/blob/main/scripts/generate-openapi-types.js)

### 실행

```jsx
npm run generate:openApi-typescript
```

### 결과

- `src/types` 폴더 내부에 `groups` 로 분리한 폴더 생성
- models 폴더 내부에 `interface{}`로 타입 정의된 .ts 파일 생성
