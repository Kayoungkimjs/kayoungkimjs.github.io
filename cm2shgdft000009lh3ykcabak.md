---
title: "[snippet] 배열 생성하기"
datePublished: Mon Oct 28 2024 03:52:25 GMT+0000 (Coordinated Universal Time)
cuid: cm2shgdft000009lh3ykcabak
slug: snippet
tags: array

---

### 1\. `Array.from`

```jsx
let num = Array.from({ length: n }, (_, i) => i + 1)
```

* `length`가 `n`인 배열을 만들고, 각 요소를 `1`부터 `n`까지 채워줌
    
* 순회 가능 또는 유사 배열 객체에서 얕게 복사된 새로운 배열 인스턴스 생성
    

### 2\. `Array`와 `fill`, `map`

```jsx
let num = Array(n).fill(0).map((_, i) => i + 1);
```

* `Array(n)`은 `n` 길이의 빈 배열을 생성하고, `fill(0)`로 모든 값을 `0`으로 채운 뒤, `map`을 이용해 `1`부터 `n`까지의 숫자를 채워줌
    

### 3\. `for` 루프

```jsx
let num = [];
for (let i = 1; i <= n; i++) {
  num.push(i);
}
```

* `1`부터 `n`까지 반복하며 `num` 배열에 값을 추가
    

### 4\. `keys()`

* `Array`의 `keys()` 메서드를 활용해 `1`부터 `n`까지의 배열을 생성
    

```jsx
let num = [...Array(n).keys()].map(i => i + 1);
```

* `Array(n).keys()`는 `0`부터 `n - 1`까지의 숫자들이 담긴 `Iterator`를 반환
    
* `map`을 사용해 `1`부터 `n`까지의 숫자로 변환
    

### 각 방법 비교:

* **가독성**: `Array.from`과 `map`을 조합하는 방법이 가장 직관적이고 코드가 간결함
    
* **성능**: 작은 범위의 `n`에 대해서는 성능 차이가 미미하지만, 아주 큰 `n`에서는 `for` 루프 방식이 조금 더 빠를 수 있음
    
* **코드 간결성**: `Array.from`과 `keys()` 방법은 코드가 간결하지만, `for` 루프는 직관적으로 이해하기 쉬운 편