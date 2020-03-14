---
title: Vue 라우터 테스트
date: 2020-03-13 17:14:15
tags: [jest란, jest설치]
categories:
- javascript
- TDD
- vue-test-utils
---

## vue 라우터 테스트 시에 주의 할점
vue 라우터를 테스트 할 시에 global Vue에 직접 추가를 해서는 안된다. vue 라우터를 설치 할 경우에는 $route와 $router가 vue 프로퍼티에 추가되어진다. 이는 $route와 $router를 mock 해서 테스트 하는 경우에 테스트를 실패하게 만든다.
vue-test-utils에서 제공하는 createLocalVue을 사용하는 것을 권장한다.

```js
import { shallowMount, createLocalVue } from '@vue/test-utils'
import VueRouter from 'vue-router'

const localVue = createLocalVue()
localVue.use(VueRouter)
const router = new VueRouter()

shallowMount(Component, {
    localVue,
    router
})
```

<br>

## router-link, router-view를 사용하는 컴포넌트 테스트 하기
위 두가지 컴포넌트를 사용하는 컴포넌트를 테스트 하는 방법은 두가지가 존재한다.

<br>

#### 1. stubs 사용하기
stubs를 사용해서 자식 컴포넌트를 렌더링하는 것을 피한다.

```js
import { shallowMount } from '@vue/test-utils'

shallowMount(Component, {
    stubs: ['router-link', 'router-view']
});
```

<br>

#### 2. localVue를 사용하기
localVue를 이용해서 실제로 VueRouoter를 추가하는 방식이다.

```js
import { shallowMount, createLocalVue } from '@vue/test-utils'
import VueRouter from 'vue-router'

const localVue = createLocalVue();
localVue.use(VueRouter);

shallowMount(Component, {
    localVue
});
```

<br>

## $route, $router mock 하기
$route, $router 객체를 가지고 컴포넌트 테스트를 해야 할 경우가 있다. 이때는 $route, $router를 mock 하여 사용한다.

```js
import { shallowMount } from '@vue/test-utils'

const $route = {
    path: '/some/path'
}

const wrapper = shallowMount(Component, {
    mocks: {
        $route
    }
});

wrapper.vm.$route.path  // 이렇게 접근이 가능해진다.
```



