---
title: "[문제해결] Next.js 13버전 favicon, icon 생성"
date: "2023-07-18T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/2023-18-07---Next.js-13버전-favicon-icon-생성"
category: "Tech"
tags:
  - "Next.js"
  - "favicon"
  - "icon"
description: "새 프로젝트를 설정하며 Next.js 최신 버전(13.4.7)에서 favicon 생성하기"
---
## 시도했던 방법

1) 12이하 버전과 동일하게 작업
2) /src 디렉토리 안에 /app 폴더 생성 → favicon.ico 폴더 추가
   => 캐시 삭제, 버전 업데이트 사항 확인하였으나 동작하지 않았음

## 13버전에서 favicon, icon 생성 (공식문서 참조)

- favicon
  - /app 디렉토리 최상위에 위치한다.
  - <head> 엘리먼트에서 파일 태그에 따라 자동으로 파일을 찾고 보여준다. → 이전처럼 next/head 태그에 넣을 필요 없음

| File convention | Supported file types              | Valid locations |
| --------------- | --------------------------------- | --------------- |
| favicon         | .ico                              | app/            |
| icon            | .ico, .jpg, .jpeg, .png, .svg | app/**/*        |
| apple-icon      | .jpg, .jpeg, .png               | app/**/*        |

- icon
  - **icon. (icon | jpg | jpeg | png | svg)** 이미지 파일을 추가한다.

#### /app 최상위 디렉토리 안에 favicon.ico 파일을 추가해주기만 하면 된다! → 안됨. 버전 이슈..?

- 여러 아이콘을 생성할 경우 `icon1.png` , `icon2.png` 와 같이 번호를 추가한다.
- **favicon은 /app 안에만 위치**한다. 세분화하고 싶다면 **`icon`** 을 쓴다.
- `<link>`태그의 속성은 아이콘 타입과 파일에서 생성된 메타데이터에 따라 결정된다. (자동) (ex: .png 파일은 `type="image/png", size="32x32" 속성을 갖게됨)
- `브라우저 버그 방지를 위해 favicon.icon에 size="any"` 가 추가되었다.

## 해결 방법

1. `/app을 상위 디렉토리로 생성한다.` (/src안에 /app 생성 x)
2. `/app내 최상위에 icon.ico` 로 파일을 추가한다. (파일명 동일하게)
3. 상위 layout.tsx 파일에 next/types import, 메타데이터 추가

```javascript
export const metadata: Metadata = {
  title: "title",
  icons: {
    icon: "/icon.ico",
  },
}
```
