---
title: 동적 라우팅
date: 2019-12-20 09:12:10
tags: [vue,dynamic-routing]
categories:
- vue
- vue 라우터
---

# 동적 라우팅

블로그 주소를 `/post/포스트번호`로 설계하여 포스트번호가 동적으로 변화하여야 하는 경우에는 동적으로 라우팅을 설정해야 합니다. 동적으로 주소를 사용할땐 주소 앞에 `:`를 사용합니다.

#### 동적 라우팅 구성

```js
var routes = [
    {
        path: '/post/:postId',
        component: {template: '<p>포스팅입니다.</p>'}
    }
];
```

> `/post/001`, `/post/002` 처럼 동적으로 접근 가능
> 게시판에서 게시글 상세보기 접근할때 해당하는 동적 주소 사용 가능

#### 동적 라우팅으로 구성한 데이터 접근

동적으로 작성된 주소의 데이터는 `this.$route.params.데이터명` 으로 접근이 가능합니다.

```html
<script>
export default {
    created() {
        this.$route.params.postId
    }
}
</script>
```

#### props로 데이터 접근

동적 라우팅으로 구성한 데이터를 props로 가져 올수 있다. `this.$route.params.데이터명`로 하면 라우트와의 의존성이 높아지므로 props를 활용하는게 좋다.

```vuejs
var routes = [
    {
        path: '/post/:postId',
        component: {template: '<p>포스팅입니다.</p>'},
        props: true
    }
];
```

#### URL에 동적 데이터 구성 이외에도 queryString 방식도 이용 가능 하다.

##### 1. url 작성

```
url : /user?id=userId
```

##### 2. 라우터 작성

```
var routes = [{
    path: '/user'
    component: 컴포넌트
}]
```

##### 3. 값 받아오기

```
this.$route.query.id
```

##### 4. props로 받아오기

키가 id 값을 props에 userName이라는 키로 변환하여 보내주고 있다.

```
var routes = [{
    path: '/user'
    component: 컴포넌트,
    props: (route) => ({userName: route.query.id})
}]
```