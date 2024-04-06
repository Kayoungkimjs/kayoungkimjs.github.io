---
title: Next.js App Router에서 다국어(i18n) 처리하기
date: "2023-09-09T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/Next.js-App-Router에서-다국어(i18n)-처리하기"
category: "Tech"
tags:
  - "Next.js"
  - "다국어"
  - "i18n"
description: "Next.js에서 다국어를 처리할 수 있는 라이브러리를 통해 App Router에서 다국어 설정하기"
---
## [i18next(internationalization)](https://www.i18next.com/)

- 웹/앱 어플리케이션에 다국어를 지원하는 자바스크립트 프레임워크
- React, Angular, Vue.js를 비롯한 Node.js, Deno, PHP, iOS, Android 등 다양한 플랫폼을 지원한다.
- 유저 locale(지역/언어)을 감지해 사용중인 브라우저 환경의 언어를 제공한다.

### 작동 방식

### 클라이언트 - window.navigator

```jsx
const locale = window.navigator.language
```
navigator.language는 유저가 사용중인 브라우저 UI를 저장하고 브라우저가 직접 세팅한다. UTS Locale Identifiers 형식의 string을 반환한다.

### 서버 - HTTP Header의 Accept-Language
사용자가 웹에 접근했을 때 요청하는 HTTP의 Accept-Language 헤더에 기반하여 감지한다.

## Next.js 다국어 처리 방법

### 사용 라이브러리

- [next-intl](https://next-intl-docs.vercel.app/)
    - Next.js를 위한 번역 라이브러리로 13버전에 생긴 App Router를 사용한 Client, Server 사이드 컴포넌트의 다국어 처리를 도와준다.

* next-i18next는 12버전 이후 업데이트를 제공하지 않는다.

### 설치 및 파일 구조

- 다국어가 적용되는 컴포넌트는 `[locale]` 폴더 안에 위치

```jsx
설치: npm install next-intl

파일 구조
├── messages (1)
│   ├── en.json
│   └── ...
├── middleware.ts (2)
└── app
    └── [locale]
        ├── layout.tsx (3)
        └── page.tsx (4)
```

### 설정 및 사용 - 클라이언트 컴포넌트

### 1. messages/(언어).json 파일 생성

- 로컬 혹은 원격 데이터 JSON 파일로 생성 (e.g en.json, jp.json)

```jsx
{
  "Index": {
    "title": "Hello world!"
  }
}
```
### 2. middleware 설정

- 요청에 따른 locale 설정 및 redirect, rewrites 등 설정

```jsx
import createMiddleware from 'next-intl/middleware';

export default createMiddleware({
  // A list of all locales that are supported
  locales: ['en', 'de'],

  // If this locale is matched, pathnames work without a prefix (e.g. `/about`)
  defaultLocale: 'en'
});

export const config = {
  // Skip all paths that should not be internationalized. This example skips the
  // folders "api", "_next" and all files with an extension (e.g. favicon.ico)
  matcher: ['/((?!api|_next|.*\\..*).*)']
};
```

### 3. app/[locale]/layout.tsx 설정

- layout 문서에 `NextIntlClientProvider` 를 넣어준다.

```jsx
import {NextIntlClientProvider} from 'next-intl';
import {notFound} from 'next/navigation';

export function generateStaticParams() {
  return [{locale: 'en'}, {locale: 'de'}];
}

export default async function LocaleLayout({children, params: {locale}}) {
  let messages;
  try {
    messages = (await import(`../../messages/${locale}.json`)).default;
  } catch (error) {
    notFound();
  }

  return (
    <html lang={locale}>
      <body>
        <NextIntlClientProvider locale={locale} messages={messages}>
          {children}
        </NextIntlClientProvider>
      </body>
    </html>
  );
}
```

### 4. app/[locale]/page.tsx 설정

- 다국어 적용이 필요한 컴포넌트에 useTranslations를 사용한다.

```jsx
'use client';

import {useTranslations} from 'next-intl';

export default function Index() {
  const t = useTranslations('Index');
  return <h1>{t('title')}</h1>;
}
```

### 설정 및 사용 - 서버 컴포넌트

상단 클라이언트 컴포넌트 설정의 1, 2, 4번은 동일함

### + I18n.ts 파일 생성 및 설정

- 컨피그 파일을 설정해 두면 한번의 요청으로 모든 서버 컴포넌트에서 동일하게 호출된다.

```jsx
import {getRequestConfig} from 'next-intl/server';

export default getRequestConfig(async ({locale}) => ({
  messages: (await import(`./messages/${locale}.json`)).default
}));
```

### + next.config.ts 설정

- 컨피그 경로와 플러그인 세팅

```jsx
const withNextIntl = require('next-intl/plugin')(
  // This is the default (also the `src` folder is supported out of the box)
  './i18n.ts'
);

module.exports = withNextIntl({
  // Other Next.js configuration ...
});
```

### + app/[locale]/layout.tsx

```jsx
import {useLocale} from 'next-intl';
import {notFound} from 'next/navigation';

export default function LocaleLayout({children, params}) {
  const locale = useLocale();

  // Show a 404 error if the user requests an unknown locale
  if (params.locale !== locale) {
    notFound();
  }

  return (
    <html lang={locale}>
      <body>{children}</body>
    </html>
  );
}
```
정적 렌더링 할 경우, 1) 클라이언트 컴포넌트 처리 방식 2) 외부 컴포넌트에서 API를 가져오는 방식 3) CDN 캐싱을 통한 방식을 권장함