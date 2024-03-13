---
title: "언젠가 만나게 될 Redux"
date: "2022-08-11T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/언젠가-만나게-될-Redux"
category: "Tech"
tags:
  - "redux"
  - "React"
  - "상태관리"
description: "React 상태관리를 위한 Redux를 알아보자"
socialImage: "./image.png"
---

## Redux 개념

- 애플리케이션 규모가 커지면 데이터의 흐름이 복잡해지기 때문에 **\*애플리케이션 단위의 상태관리**(application level state management)가 필요함

* 애플리케이션 상태관리란?
  앱 수준의 상태를 자유롭게 구성하고 상태 일부분에 변화가 생기면 이를 즉각 반영하여 해당 컴포넌트를 재렌더링하게 하는 기능

- 리덕스란 자바스크립트 상태관리 라이브러리로 Flux(앱 설계 규격중 하나)를 준수한다.
- 리덕스 라이브러리
  - redux: 리덕스 모듈
  - react-redux: 리액트 컴포넌트에서 리덕스를 사용하기 위한 유용한 도구 모음

## Redux 규칙

- 나의 애플리케이션 안에 하나의 스토어 생성
- 상태는 읽기 전용(read-only)
  - 리액트에서 setState를 사용해 업데이트, concat 같은 함수를 사용하여 기존의 배열은 수정하지 않고 새로운 배열을 만들어서 교체하는 방식으로 업데이트를 하는 것처럼 리덕스에서도 기존의 상태는 건들이지 않고 새로운 상태를 생성하여 업데이트 해주는 방식을 권장
  - **리덕스에서는 action, dispatch를 이용해 상태를 수정**한다
- 변화를 일으키는 함수, reducer는 순수한 함수
  - 리듀서 함수는 이전 상태와, 액션 객체를 파라미터로 받는다.
  - 이전의 상태는 절대로 건들이지 않고, 변화를 일으킨 새로운 상태 객체를 만들어서 반환한다.
  - 똑같은 파라미터로 호출된 리듀서 함수는 항상 똑같은 결과값을 반환한다.

## Core Principles of Redux

![image.png](./image.png)

### 1) View

- 유저가 바라보는 화면으로 컴포넌트 등 UI 구성 요소

### 2) Action

- 자바스크립트 객체, Store에 입력된 순서와 내용을 재현
- Store에 담긴 데이터를 변형시킬 수 있는 유일한 방법으로 `store.dispatch` 함수의 인자로 담겨 'reducer에 store를 어떻게 변경시킬 것인가'에 대한 정보 제공
- action type 속성이 반드시 있는 객체로, reducer에 switch/case문 구현시 사용

```javascript
const rootReducer = (state: AppState = initialState, action: LoginActions): AppState => {
	switch (action.type){
		case '액션_타입1' :
			return {...state} // 액션타입 1을 반영한 새로운 상태 반환
		case '액션_타입2' :
			return {...state} // 액션타입 1를 반영한 새로운 상태 반환
		}
	return state;
}
```

### 3) **Dispatcher**

- Action 객체를 Reducer에 보내는 역할을 하는 함수로 `store.dispatch()` 형태로 제공됨.
- 기본 dispatch 함수는 반드시 동기적(호출된 순서대로 상태를 변경, 변경된 순서의 내역 관리)으로 처리되어야 하는데,
- 만약 비동기 Action이 필요하다면 비동기 처리가 완료된 이후에 Action을 Dispatch 하거나, 미들웨어를 활용해 비동기 처리를 해줘야 함.

### 4) **Reducer**

- Reducer는 `(previousState, action) => newState`의 형태로 이전 상태값과 액션 객체를 입력으로 받아 새로운 상태값 출력
- `업데이트 되기 이전의 Store를 기반으로 새로 받은 Action에 미리 준비된 로직을 처리해 새로운 Store를 리턴하는 함수.` 즉, 감지된 Action 타입에 따라 이벤트를 처리하는 '이벤트 리스너'로 생각할 수 있음

```js
// Reducer 함수 의미
const rootReducer = (previousState: AppState, action: Action) => {
	const newState =
  return newState
}
```

- 구분

  - `Root Reducer`: `createStore`의 첫 번째 인자로 전달되는 함수로 유일하게 `(state, action) => newState` 형태의 로직 구현
    - `Slice Reducer`: 상태 트리의 일부분만을 업데이트하는 리듀서로 여러 Slice Reducer들이 결합(`combineReducers`)되어 Root Reducer를 구성하게 됨 (\*Linco에서 페이지 기능별 profileSlice, walletSlice 등 사용)

  **:: 유의사항**

- 불변성을 지켜 업데이트 한다.

  - 기존 값을 직접 수정하지 않고 기존 값을 복사하고 새롭게 복사된 값을 덮어쓰는 방식으로 업데이트 한다.

- Reducer 내부에서 비동기나 Side Effect를 처리할 수 없다.

  - ex. `Promise(fetch)`, `Math.random()`, `new Date()`

### 5) **Store**

- 현재의 앱 상태와, 리듀서가 들어가있고, 추가적으로 몇가지 내장 함수들을 포함함
- store에서 관리되고, `store.getState()`로 읽어올 수 있음

```js
// src/index.js

import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";

import { App } from "./App";
import createStore from "./createReduxStore";

const store = createStore();

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>,
);
```

### 6) Middleware

- 비동기 API 호출 등 순수하지 않은 요청(Side Effect)을 처리하거나, Redux Store로 전달되는 Action 등을 로깅하는 장소이며 Reducer가 액션을 처리하기 전에 실행되는 함수.
- 대표적인 Middleware로는 redux-logger, redux-thunk, redux-saga 등

```js
// redux-thunk
export default function thunkMiddleware({ dispatch, getState }) {
  return (next) => (action) =>
    typeof action === "function" ? action(dispatch, getState) : next(action);
}
```

## 코드 구현

1.앱 수준 상태 타입 선언

```js
type user = {
	name: string
	email: string
}

type AppState={
	loggedIn: boolean
	loggedUser: User
}
```

2. 변수 초기화

```js
const initialState: AppState = {
	loggedIn: false,
	loggedUser: {name: '', email: ''}
}
```

3. 리덕스 구현용 디렉토리 및 필요한 파일 만들기

- store 폴더 안에 index.ts, AppState.ts(타입선언), actions.ts, rootReducer.ts(리듀서 ), makeStore.ts(파일 저장소 생성)
