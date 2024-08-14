---
title: "hashnode test"
datePublished: Wed Aug 14 2024 02:07:42 GMT+0000 (Coordinated Universal Time)
cuid: clzt7otwg00000albft6m6z61
slug: hashnode-test
tags: redux-toolkit

---

# Redux + Redux toolkit

## 개념

* Redux is a pattern and library for managing and updating application state, using events called "actions".
    
* It serves as a **centralized store** for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a **predictable fashion**. =&gt; 'action'이라는 이벤트를 사용해 애플리케이션의 상태를 관리, 업데이트하는 라이브러리. 중앙집중식 스토어(단일 저장소)로 예측 가능한 형태로 상태를 업데이트함
    

### Keys

* Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur.
    
* 사용하는 경우
    
* You have large amounts of application state that are needed in many places in the app
    
* The app state is updated frequently over time
    
* The logic to update that state may be complex
    
* The app has a medium or large-sized codebase, and might be worked on by many people =&gt; A single centralized place to contain the global state in your application, and specific patterns to follow when updating that state to make the code predictable.
    

#### State management

* One-way data flow
    
* State: 앱을 구동하는 근본
    
* View: 현재 상태에 기반한 UI의 선언적 설명
    
* Actions: 사용자 입력을 기반으로 앱에서 발생하는 이벤트, 상태 업데이트를 트리거
    

#### Immutable

* In order to update values immutably, your code must make copies of existing objects/arrays, and then modify the copies.
    
* Redux expects that all state updates are done immutably. We'll look at where and how this is important a bit later, as well as some easier ways to write immutable update logic. =&gt; immutable update는 기존 객체나 배열을 복사해서 그 복사본을 수정하는 것
    

### Notes

* recoil과의 차이점 중 하나, 상태관리의 접근 방식. Recoil은 React 컴포넌트 내에서 직접적으로 상태를 관리하고 공유하는데 초점을 둔 반면, redux는 중앙집중화된(Reducer) 상태 저장소와 액션을 통한 상태 변화 관리 중점.
    
* recoil은 컴포넌트간 상태공유를 간편하게 만들고, Redux는 복잡한 애플리케이션의 상태관리와 미들웨어 역할 강조.
    
* redux-toolkit의 등장으로 보일러 플레이트도 많이 줄고, RTK Query를 내장하고 있기 때문에 별도의 데이터 페칭 라이브러리 없이 캐싱, 자동 데이터 리페칭 등의 기능을 사용할 수 있다. slice를 관심사 별 분리하면 코드 스플리팅도 가능
    
* recoil의 set, get이 여러 컴포넌트에서 일어나면 상태 의존도가 높아질수록 오히려 관리가 어려울 수 있다.
    
* 컴포넌트의 상호 의존도를 줄이는 것은 최적화와 관리 측면에서 필요, 꼭 필요한 경우에만 전역 상태관리를 쓸 것.
    
    ![스크린샷 2024-08-04 오후 6 35 58](https://github.com/user-attachments/assets/d5741ba1-4a6d-449f-ac6a-c8577d06d748 align="left")