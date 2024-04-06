---
title: "프로젝트 시작과 후 함께 고민해야 할 것들"
date: "2022-10-08T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/프로젝트-시작과-후-함께-고민해야-할-것들"
category: "Tech"
tags:
  - "리팩토링"
  - "코드 컨벤션"
description: "최고의 프로젝트를 위한 프론트엔드 라이브러리 선택과 리팩토링에 대하여..."
---

> 그룹웨어 리뉴얼을 위한 프로젝트를 진행하면서 1차 개발에서 아쉬웠던 점을 보완하고자 했다. 개발 기한이 짧다는 이유로 일단 우다다 개발하는 방법이 결국엔 수정과 보완에 시간을 더 쓴다는 것을 경험하고, 2차에서는 시간이 걸리더라도 개발 스펙을 협의하고 수정 방향(리팩토링)도 방향성에 대한 논의가 필요하다고 생각했다.

## 주요 라이브러리

> 프로젝트의 특성, 러닝 커브, 유지보수 등을 고려해 사용할 라이브러리를 선택한다. 라이브러리의 특징을 정확하게 알고 다른 것과 비교하여 선택한 이유가 분명해야 한다.

| 라이브러리                                                                                            | 설명                                                  | 선택 이유                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**React**](https://react.dev/)                                                                       | 사용자 인터페이스를 만들기 위한 JavaScript 라이브러리 | 재사용 가능한 UI 구성 요소를 만들 수 있고, 동적인 웹 페이지를 보다 효율적으로 유지 보수할 수 있다.                                                                                                                        |
| [**TypeScript**](https://www.typescriptlang.org/)                                                     | 자바스크립트에 타입을 부여한 언어                     | 정적 타입을 지원하므로 컴파일 단계에서 오류를 포착할 수 있는 장점이 있다. 또한 명시적 타입 지정으로 코드 가독성을 높이고 예측할 수 있게하며 디버깅하기에 용이하다.                                                        |
| [**Recoil**](https://recoiljs.org/ko/)                                                                | 상태관리 라이브러리                                   | React 전용 상태관리 라이브러리로 손쉬운 전역상태 관리가 가능하며, Redux보다 단순한 상태구조와 작동 방식으로 러닝 커브가 낮다.                                                                                             |
| [**React Query**](https://tanstack.com/query/latest/docs/react/overview)**(TanStack Query )**         | 서버 상태관리 라이브러리                              | API 상태와 관련된 다양한 데이터를 제공하여 복잡한 구현과 설계없이도 계발자가 효율적으로 화면을 구성할 수 있게 한다.                                                                                                       |
| [**Emotion**](https://emotion.sh/docs/introduction)                                                   | css 라이브러리                                        | 러닝커브가 다소 적고, 필요한 코드 주입의 자동화가 잘 되어있다. css props 기능을 이용하여 컴포넌트로 스타일링을 하기에도 용이하다.                                                                                         |
| [**Ant Design**](https://ant.design/components/overview/)**/**[**MUI**](https://mui.com/material-ui/) | UI 프레임워크                                         | 어드민 페이지 개발에 필요한 컴포넌트가 세분화되어있고 완성도가 높다. AntD보다 디자인이 다양하고, 디테일한 기능을 제공한다. 커스텀에 용이하다.<br />*AntD: 관리자 페이지의 table, form 등에 일부 사용``*MUI: 이모티콘 사용 |

## 리팩토링 항목

> 프로젝트 개발 후에는 리팩토링이 필요하다. 프로젝트의 일관성과 향후 유지보수를 위해 항목별 체크리스트를 만들어 각자 맡은 부분에 대한 수정을 진행한다.

- 폴더 구조

  - 공통 컴포넌트(함수)는 2군데 이상 사용하는 컴포넌트(함수) 일 경우를 나타냄

    ```jsx
    - assets
        - styles // 공통 스타일 선언
        - images
    - components
        - common // 공통 컴포넌트
        - Layout // 공통 레이아웃
    	- ...
    - domain
        - Deli
          - comonents // 도메인 관련 컴포넌트
          - hooks // react-query hook
          - Deli.tsx
    	- ...
        - index.ts
    - store // recoil
        - ui 관련(클라이언트) 상태만 관리
    - hooks // react-query 공통 함수
    - utils // 공통 함수
    - constants // 상수
    - pages // router 연결
    - types // 타입 선언
    ```

- 도메인 전용 컴포넌트는 도메인 안에 컴포넌트 폴더에 추가한다.(응집도를 높이기 위해)

- Naming Conventions

  - 컴포넌트의 이름은 Pascal Case를 사용하여 작성한다.
  - 컴포넌트가 아닌 것들은 Camel Case를 사용하여 작성한다.
  - style 파일 이름은 컴포넌트 이름과 동일해야 한다.
  - string props에는 Curly Braces를 사용하지 않는다.
  - JSX에서 Javascript Code를 제거한다.

    ```jsx
    // Bad
    return (
      <ul>
        {posts.map((post) => (
          <li
            onClick={(event) => {
              console.log(event.target, "clicked!");
            }}
            key={post.id}
          >
            {post.title}
          </li>
        ))}
      </ul>
    );

    // Good
    return (
      <ul>
        {posts.map((post) => (
          <li onClick={onClickHandler} key={post.id}>
            {" "}
            {post.title}{" "}
          </li>
        ))}
      </ul>
    );
    ```

  - 컴포넌트에 Chidren이 없는 경우 자체 닫기 태그를 사용한다. 가독성을 높여준다.

- Bug Avoidance

  - undefined나 null 체크는 Optional Chaining을 사용한다.
  - 비동기 호출 등의 이유로 아직 값이 넘어오지 않았을 때 구조분해할당 하는 경우 에러가 발생할 수 있다.

    ```jsx
    // Bad
    const content = undefined;
    const { item } = content; // Uncaught TypeError: Cannot destructure property 'item' of 'content' as it is undefined.

    // Good
    const content = undefined;
    const { item } = content || {};
    console.log(item); // undefined

    const content = undefined;
    const { item } = { ...content };
    console.log(item); // undefine
    ```

  - 불필요한 console.log()를 제거한다.

- 함수 사용

  - 화살표 함수, 함수 표현식 사용 이 부분은 협의가 필요할듯
  - 서버호출함수나 리액트 쿼리 네이밍 통일하기
  - 순수함수

- 상태관리

  - 불필요한 State Lifting과 Props Drilling을 피한다. 부모 컴포넌트에서 비동기 로직을 굳이 처리해줄 필요없다.

- import/export

  - import 순서 지정
  - export할 각 폴더에 대해 index.ts 사용(가져올 때 이름이 반복되지 않음)

- 공통 컴포넌트 만들기

  - modal 상태 및 callback함수를 props driling을 사용하지 않도록 개발
  - antd에서 에러 유발하는 컴포넌트가 어떤 것들이 있을까요?

- Typescript

  - any타입으로 되어있는 것 타입 지정하기

- 로직

  - 등록이나 수정, 삭제는 서버에 사이드 이펙트를 발생시키는 기능은 Alert팝업을 띄워서 확인 작업을 한다.
  - 폼 등록이나 수정의 경우 유효성 검사를 추가한다.

- datePicker
  - ant-design 의 datePicker은 moment를 사용하고 있으므로, dayJs를 사용하여 커스텀 한 컴포넌트를 적용한다.
    - moment - 지원 종료 및 패키지 용량이 큼
    - dayjs - 패키지 용량 작음 계속 지원 중
