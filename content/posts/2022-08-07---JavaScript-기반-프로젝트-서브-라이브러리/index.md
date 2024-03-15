---
title: "JavaScript 기반 프로젝트 서브 라이브러리"
date: "2022-08-07T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/JavaScript-기반-프로젝트-서브-라이브러리"
category: "Tech"
tags:
  - "JavaScript"
  - "craco"
  - "husky"
  - "lodash"
  - "yup"
description: "JavaScript 기반 프로젝트 진행시 주요 라이브러리(JS, CSS 라이브러리) 외 개발에 필요한 추가 라이브러리를 통해 작업 생산성, 효율성을 높일 수 있다."
---

> JS 기반 프로젝트 진행시 주요 라이브러리(JS, CSS 라이브러리) 외 개발에 필요한 추가 라이브러리를 통해 작업 생산성, 효율성을 높일 수 있다.

# [Craco](https://craco.js.org/)

- Create-React-App Configuration Override의 약어로, CRA에 config 설정을 덮어쓰기 위한 패키지
- Webpack의 번거로운 설정을 줄이기 위해 다양한 오버라이딩 패키지들이 등장했으며, 대표적으로 Craco, react-app-rewired 등이 있다.

### /cf/ CRA Webpack 설정

- CRA는 기본적으로 프로젝트 디렉토리를 간결하게 유지하기 위해 웹팩 설정이나 script 폴더들을 숨겨놓는다. 하지만, 이를 커스텀해야 할 경우 eject 명령어를 통해 이를 추출할 수 있다.
- 이렇게 추출된 폴더 및 파일들은 이제 디렉토리 상에 노출되며, 단 eject를 한 번 하면 이전 상태로 돌아갈 수 없다는 단점이 있다.
- eject를 하게 되면 **설정에 관련된 config, scripts 폴더 및 package.json의 dependency, babel 및 jest 설정코드가 드러난다.** 또한, 한 번 eject를 하면 React APP의 **버전업에 대한 구성요소 업그레이드를 할 수 없다**는 단점도 존재한다.

## 설치 및 세팅

1. Craco 최신 버전 설치

```bash
npm i -D @craco/craco
npm i -D @craco/types *타입스크립트 사용시
```

2. 프로젝트 루트 디렉토리에 `craco.config.js` 파일 생성, 덮어쓸 설정(alias, less loader 등) 추가

```jsx
// craco.config.js
module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: "tsconfig",
        baseUrl: "./src",
        tsConfigPath: "tsconfig.paths",
      },
    },
  ],
};
```

3. `package.json` command 수정

```jsx
// package.json

"scripts": {
	"start": "craco start",
	"build": "craco build",
	"test": "craco test",
	"eject": "craco eject"
}
```

# [Husky](https://typicode.github.io/husky/)

- `git-hook`에 따라 원하는 동작을 하게 도와주는 패키지로, git add, commit, push가 시행되기 전이나 후에 원하는 스크립트를 실행시켜준다.

## 설치 및 세팅

1.  자동 설치

```jsx
npx husky-init && npm install
```

2. `package.json` 스크립트 추가

```jsx
// package.json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

## hook 생성

- hook을 실행하거나 새로운 것을 추가 할 때, `husky install` 을 먼저 실행하고, `husky add <File> [cmd]`를 쓴다.

```jsx
npx husky add .husky/pre-commit "npm test"
git add .husky/pre-commit
```

- commit 하기

```jsx
git commit -m "Keep calm and commit"
```

# [Lodash(\_)](https://lodash.com/docs/4.17.15)

- array, collection, date 등 데이터의 구조를 간편하게 함수형으로 다룰 수 있다.
- 특히, JS의 배열 안의 객체들의 값을 핸들링 할때 (배열, 객체 및 문자열 반복 / 복합적인 함수 생성) 코드 몇줄만으로 유기적으로 다룰수가 있어서 매우 유용하다.
- 특히 Frontend 환경에서 서버(DB)에서 받은 데이터를 정렬하고 가공할때 많이 쓰인다.
- Lodash는 밑줄 `_` 기호를 처음에 선언하여 사용된다. `_.(변수)` 로 작성할 경우 lodash wrapper로 변수를 감싸게 되면서 해당 변수에 대한 chaining을 시작한다.

## 설치 및 사용

### **npm 설치**

```jsx
$ npm i -g npm
$ npm i --save lodash
```

### **l사용 방법**

```jsx
// ES6
import _ from "lodash";

// commonJs
var _ = require("lodash");

var myFriend = [
  { name: "kys", job: "developer", age: 27 },
  { name: "cys", job: "webtoons man", age: 27 },
  { name: "yhs", job: "florist", age: 26 },
  { name: "chj", job: "nonghyup man", age: 27 },
  { name: "ghh", job: "coffee man", age: 27 },
  { name: "ldh", job: "kangaroo", age: 27 },
];

// 콜백함수가 처음으로 참이되는 객체를 반환
_.find(myFriend, function (friend) {
  return friend.age < 28;
}); // → { name: 'kys',job:'developer' ,'age': 27}
```

- JS ES6 문법에 비해 실행 속도는 느린 편이나, 함수 확장성이 크다.

```jsx
var myFriend = [
  { name: "kys", job: "developer", age: 27 },
  { name: "cys", job: "webtoons man", age: 27 },
  { name: "yhs", job: "florist", age: 26 },
  { name: "chj", job: "nonghyup man", age: 27 },
];

myFriend.findIndex(function (friend) {
  return friend.age === 26; // 2
});

_.findIndex(myFriend, function (friend) {
  return friend.age === 26; // 2
});
```

- ES6의 findIndex()는 파라미터로 콜백함수만을 반환하지만, lodash의 findIndex()는 객체값 또한 줄 수 있다.

```jsx
// 처음 일치하는 object의 index 값을 반환
_.findIndex(myFriend, { name: "cys", job: "webtoons man", age: 27 });
// -> 1

// 나이가 26인 객체가 처음으로 나오는 index 반환
_.findIndex(myFriend, { age: 26 });
// → 2
```

# [Yup](https://www.npmjs.com/package/yup) + [react-hook-form](https://www.react-hook-form.com/)

- `Yup`은 폼 형식의 데이터에 대한 유효성 검사 및 그 상황에 맞는 에러 메시지를 효율적으로 관리할 수 있는 라이브러리다. 유효성 검사 항목이 많아지는 경우 코드가 길어지고 불필요한 렌더링이 일어나는데, 코드를 줄이고 유지보수 측면에서 효율적이다.
- `react-hook-form` 은 기존 React의 <form>을 통해 입력값을 처리하는 과정에서 생겨나는 비효율적인 과정을 줄여주는 비제어 컴포넌트 방식으로 더 효율적인 input값을 관리할 수 있다.

## 설치 및 사용

```jsx
npm i yup
npm install react-hook-form
```

## yup 스키마 작성

```jsx
import * as yup from "yup";

export const crewRequiredValidation = yup.object({
  // 사번
  crewId: yup
    .string()
    .required("사번을 입력해주세요.")
    .max(10, "사번은 10자리 이하여야 합니다."),
  // 한글 이름
  crewName: yup
    .string()
    .required("이름을 입력해주세요.")
    .max(15, "이름은 15자리 이하여야 합니다."),
  // 영문 이름
  crewNameEn: yup
    .string()
    .required("영문 이름을 입력해주세요.")
    .max(30, "영문 이름은 30자리 이하여야 합니다."),
  //코코네 아이디
  coconeId: yup
    .string()
    .matches(/^[A-Za-z_]+$/, "영어와 특수문자만 입력 가능합니다.")
    .required("코코네 아이디를 입력해주세요.")
    .max(30, "코코네 아이디는 영문 30자리 이하여야 합니다."),
  // 조직원 상태
  activeStatus: yup.string().required("조직원 상태를 선택해 주세요."),
  // 근무형태
  workType: yup.string().required("근무형태를 선택해 주세요."),
  // 브랜치
  branchId: yup.string().required("Branch를 선택해 주세요."),
  // 부서
  deptId: yup.string().required("부서를 선택해 주세요."),
  //직책
  dutyId: yup.string().required("직책을 선택해 주세요."),
  // 직군
  jobKind: yup.string().required("직군을 선택해 주세요."),
  // 세부 직군
  jobPart: yup.string().required("세부직군을 선택해 주세요."),
  // 생년월일
  birthday: yup
    .date()
    .typeError("생년월일을 입력해 주세요.")
    .required("생년월일을 입력해 주세요."),
  // 입사일
  hireDate: yup
    .date()
    .typeError("입사일을 입력해 주세요.")
    .required("입사일을 입력해 주세요."),
  email: yup
    .string()
    .email("이메일 형식에 맞춰 작성해 주세요.")
    .required("이메일을 입력해 주세요."),
});

export const crewOptionalValidation = yup.object({
  // 핸드폰 번호
  phoneNo: yup
    .string()
    .nullable()
    .matches(
      /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/,
      "형식에 맞춰 입력해 주세요.",
    ),
  // 비상 연락처
  emergencyCall: yup
    .string()
    .nullable()
    .matches(
      /(^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$)|(^$)/,
      // /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/,
      "형식에 맞춰 입력해 주세요.",
    ),
  // 겸직
  deptCrewList: yup.array().of(
    yup.object().shape({
      deptId: yup.string().required("부서를 선택해 주세요."),
      dutyId: yup.string().required("직책을 선택해 주세요."),
    }),
  ),
  // 개인 이메일
  privateEmail: yup
    .string()
    .nullable()
    .email("이메일 형식에 맞춰 작성해 주세요."),
});
```

- Yup에 다양한 내장 함수를 사용해 스키마를 작성할 수 있다. 문자열인지 검사해 주는 string(), email인지 혹은 정규식으로 커스텀된 규칙을 사용하는 email()과 matches() 그리고 필수로 들어가야하는 required() 등이 있다.
- 함수 argument로 문자열을 넣으면, 해당 조건에 부합하지 못할 경우 문자열을 에러메시지로 반환시킬 수 있다. matches()는 정규식을 삽입하고, 두번째 argument로 입력한다.

## reac-hook-form의 useForm 호출

```jsx
import { yupResolver } from '@hookform/resolvers/yup';

export default function RequiredForm() {
	const methods = useForm({
		resolver: yupResolver(crewRequiredValidation),
		mode: 'onBlur',
		defaultValues: formData,
	});

	const { handleSubmit, watch, resetField } = methods;

	const onSubmit = (data: Crew) => {
		setFormData({ ...formData, ...data });
		onClickNextStep();
	};

	return (
		<FormProvider {...methods}>
			<ContentWrapper>
				<Row gutter={[24, 10]}>
					<Col span={12}>
						<FormInputRadio
							name="isActive"
							label="사원 ON/OFF"
							options={isActiveOptions}
						/>
					</Col>
					<Col span={12}>
						<FormInputText label="이름" name="crewName" />
					</Col>
					<Col span={12}>
						<FormInputText label="사번" name="crewId" />
					</Col>
					<Col span={12}>
						<FormInputText label="cocone ID" name="coconeId" />
					</Col>
					<Col span={12}>
						<FormInputSelect
							name="workType"
							label="고용형태"
							options={workTypeOptions}
						/>
					</Col>
					....
				</Row>
			</ContentWrapper>
			<ButtonWrapper>
				<Button type="primary" onClick={handleSubmit(onSubmit)}>
					다음
				</Button>
			</ButtonWrapper>
		</FormProvider>
	);
}
```

- react-hook-form의 useForm에서, `yupResolver` 로 스키마를 호출하면 form의 input 값의 검증은 Yup에서 값은 useForm에서 관리하게 된다.
