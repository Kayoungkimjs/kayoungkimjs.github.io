---
title: Normalize.css-vs-Reset.css
date: "2023-04-07T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/Normalize.css-vs-Reset.css"
category: "Tech"
tags:
  - "CSS"
description: "정규화(normalizing), 리셋(resetting)을 통해 기본 CSS를 모든 브라우저에 일관되게 적용할 수 있도록 만들어 준다."
---
> CSS를 사용하다보면 브라우저마다 스타일 요소를 다르게 인식하는 문제를 겪는다. 이는 가독성을 위해 브라우저마다 `User-Agent Stylesheet`을 적용하고 있기 때문이다.

정규화(normalizing), 리셋(resetting)을 통해 기본 CSS를 모든 브라우저에 일관되게 적용할 수 있도록 만들어 준다.

1) 스크립트를 직접 작성 혹은 작성된 스크립트를 사용하거나 
2) npm/yarn으로 라이브러리 install 해서 사용하는 방법이 있다.

초창기에는 크로스 브라우징이 중요한 이슈였지만 IE 서비스가 종료되었고 브라우저 스타일이 어느정도 표준화되면서 기존 사용했던 스크립트에도 변화가 필요한 상황이다.

## Reset CSS

- 크로스 브라우저 간 차이를 피하기 위해 브라우저의 기본요소 디자인을 모두 없애(0으로 만들기)는 것
- 여러가지 Reset 버전이 있지만 [에릭마이어의 CSS Reset](https://meyerweb.com/eric/tools/css/reset/)이 가장 유명
```css
/* <http://meyerweb.com/eric/tools/css/reset/>
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
```
> 그러나 CSS의 규모가 커지고, 성능, 효율성 등의 문제로 Reset CSS의 필요성에 대한 회의적인 의견도 있다.

## Normalize CSS
브라우저간 스타일이 표준 브라우저의 스타일과 동일하게 보일 수 있는 방식으로 브라우저의 유용한 기본값은 남겨둠
```css
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */
html {-webkit-text-size-adjust:100%;line-height:1.15}
body {margin:0}
main {display:block}
h1 {font-size:2em;margin:.67em 0}
hr {box-sizing:content-box;height:0;overflow:visible}
pre {font-family:monospace, monospace;font-size:1em}
a {background-color:transparent}
abbr[title] {border-bottom:none;text-decoration:underline;text-decoration:underline dotted}
b, strong {font-weight:bolder}
code, kbd, samp {font-family:monospace, monospace;font-size:1em}
small {font-size:80%}
sub, sup {font-size:75%;line-height:0;position:relative;vertical-align:baseline}
sub {bottom:-.25em}
sup {top:-.5em}
img {border-style:none}
button, input, optgroup, select, textarea {font-family:inherit;font-size:100%;line-height:1.15;margin:0}
button, input {overflow:visible}
button, select {text-transform:none}
[type=button], [type=reset], [type=submit], button {-webkit-appearance:button}
[type=button]::-moz-focus-inner, [type=reset]::-moz-focus-inner, [type=submit]::-moz-focus-inner, button::-moz-focus-inner {border-style:none;padding:0}
[type=button]:-moz-focusring, [type=reset]:-moz-focusring, [type=submit]:-moz-focusring, button:-moz-focusring {outline:1px dotted ButtonText}
fieldset {padding:.35em .75em .625em}
legend {box-sizing:border-box;color:inherit;display:table;max-width:100%;padding:0;white-space:normal}
progress {vertical-align:baseline}
textarea {overflow:auto}
```
## Opinionated Normalize

- Normalize에서 적용되는 요소들 중에도 중복되거나 불필요한 요소가 있고, 커스텀이 필요한 경우가 생겨남.
- Normalize에 새로운 기본 스타일을 적용해 만들거나, 필요한 부분 일부만 사용하는 경우가 많음
- `sanitize.css`, `minireset.css` 등

## 정리

- Reset, Normalize CSS는 프로젝트 성격에 맞게 사용한다.
- 여러 브라우저를 대응해야하는 서비스 페이지의 경우 하드 리셋을 사내 프로그램 등 규모가 작다면 커스텀해서 필요한 부분만 사용하거나 사용하지 않아도 무방(global style에 넣어주는 정도)
- IE11 대응은 필요, 최신 스펙은 지양할 것.