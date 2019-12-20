---
title: 뷰 라우터
date: 2019-12-20 09:08:19
tags: [vue,router]
categories:
- vue
- vue 라우터
---

# 뷰 라우터

싱글페이지앱에서는 화면의 새로고침이 없이 페이지가 전환이 되어야 합니다. 그럴때 사용하는 것이 뷰 라우터로써 라우트에 컴포넌트를 맵핑 시켜 놓고 특정주소에서 해당하는 컴포넌트를 렌더링하는 방식으로 싱글페이지앱에서 페이지의 전환을 대신합니다.

### 1. vue-router 구성

### 1-1. vue-router파일 작성

#### 참고사항

vue-router의 실제 코드를 main.js에 작성하게 되면 vue-router 편향적인 파일이 되기 때문에 삼가한다. vue-router만을 위한 파일을 생성하여 작성하도록 하자.


#### vue-router 설치

```
npm install vue-router
```

#### import

```js
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);
```

#### 컴포넌트 생성

렌더링할 컴포넌트 생성

```js
const Foo = {template: '<p>foo</p>'}
const Bar = {template: '<p>bar</p>'}
```

#### VueRouter 생성

path는 주소, component는 해당 주소에서 렌더링할 컴포넌트를 맵핑

```js
let routes = [
    {path: '/foo', component: Foo},
    {path: '/bar', component: Bar}
]

export const router = new VueRouter({
    routes
});
```

### 1-2. main.js 작성

#### Vue 인스턴스 생성

```js
import {router} from '라우터작성 경로';

new Vue({
    router
})
```

#### 뷰 라우터 태그 작성

라우터 설정시에 맵핑했었던 컴포넌트가 router-view에 렌더링 됩니다.

```html
<div id="app">
    <router-view></router-view>
</div>
```

#### 라우터 이동

path부분에 주소를 입력 해주면 해당 라우팅 경로로 이동할수 있습니다(실제론 해당 주소로 컴포넌트를 바꿔서 렌더링 작업을 처리해준다는 것).

```html
<div id="app">
    <router-link to="path"></router-link>
</div>
```

#### redirect

path 경로로 들어왔을 때 리다이렉트 할수 있다.

```js
var routes = [
    {
        path: '/',
        redirect: '/리다이렉트할 경로'
    }
]
```

#### url에 # 제거하기

mode에 history값을 주면 #을 제거 가능하다.

```js
new VueRouter({
	mode : history,
	routes : [
	
	]
})
```

### 2. 심화된 라우팅 구성

#### 중첩된 라우트

중첩된 라우트를 사용하기 위해서는 children 속성을 이용하면 가능합니다.

```js
var routes = [
    {
        path: '/mypage',
        component: {template: '<p>안녕하세요</p><router-view></router-view>'},
        
        // 중첩된 라우트를 사용하기 위해서는 children 속성을 이용하면 가능합니다.
        children: {
            path: 'myBoard',
            component: {template: '<p>내 게시물</p>'}
        }
    }
]

new VueRouter({
    routes
})
```

중첩된 라우트는 화면 구성 컴포넌트의 수가 적으면 유용하지만 한번에 더 많은 컴포넌트를 표시하는데는 한계가 있습니다.

#### 다중 라우트

하나의 주소에 여러개의 컴포넌트와 매핑을 합니다.

```js
<view-router></view-router>
<view-router name="header"></view-router>

var routes = [
    {
        path: '/mypage',
        components: {
            default: {template: '<p>기본 컴포넌트</p>'},        // view-router에 name속성의 값을 설정하지 않으면 default
            header: {template: '<header>헤더 컴포넌트</header>'}    // view-router에 name속성의 값을 키로 하여 컴포넌트를 맵핑
        }
    }
];

new VueRouter({
    routes
});
```

> 상하위 관계가 아닌 동등한 레벨의 여러개의 컴포넌트를 한번에 표시가 가능합니다.
