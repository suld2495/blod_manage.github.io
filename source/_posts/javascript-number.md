---
title: 숫자와 관련된 팁
date: 2019-12-19 01:08:07
tags: [javascript,number]
categories:
- javascript
- 유용한팁
- number
---

## 1. 숫자를 천자리마다 콤마찍기

### 사용조건

```
1. 숫자만 가능
2. 숫자를 변수에 저장 후 가능
```

### 소스코드

```js
var number = 123
var result = number.toLocaleString()
```

#### 정리

1. 크롬에서는 작동을 하나 모든 곳에서 통하는 소스코드인지는 아직 확인 불가
1. 심플하게 만들수 있다는 장점
1. 숫자값인 문자열로는 사용불가능하고 필히 숫자 타입으로 바꿔줘야 함
1. 문자열을 대상으로 toLocaleString 메서드를 호출하면 그냥 그대로 나옴
1. 결과값은 당연하게도 `문자열`