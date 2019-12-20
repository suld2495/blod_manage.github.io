---
title: axios
date: 2019-12-20 10:12:19
tags: axios
categories:
- javascript
- 라이브러리
---

# axios 정의

axios는 HTTP 클라이언트 라이브러리로써, 비동기 방식으로 HTTP 데이터 요청을 실행한다. 내부적으로 직접 XMLHttpRequest를 사용하지 않고 AJAX 호출을 할수있다.
그리고 axios는 Promise 기반 api이다.

## api는 따로 관리한다.

axios를 사용할 vue 페이지에서 직접 `import axios`를 통해서 사용하지 않고 api 파일을 따로 작성하여 해당 메소드를 호출 하는 방식으로 설계를 한다.

## api에서는 직접 then 및 catch 작업을 하지 않는다.

아래와 같이 호출만 하고 Promise를 return 하는 방식으로 작성한다.

```js
function test() {
    return axios.get(...);    
}
```

## API 작성 방법

###### 1. 기본 HTTP Request & Response와 관련된 기본설정을 한다.

```js
const config = {
    baseUrl: 'http://127.0.0.1/test/'
}
```

###### 2. API 함수들을 정리한다.

```js
export const getRoleList = function() {
    return axios.get(...);
}

export const getUserList = function() {
    return axios.get(...);
}
```