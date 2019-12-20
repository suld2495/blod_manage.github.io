---
title: 라우터 네비게이션 가드
date: 2019-12-20 09:18:25
tags: [vue,navigation_guard]
categories:
- vue
- vue 라우터
---

# 라우터 네비게이션 가드

데이터를 호출 시점은 여러가지가 존재한다. 대표적으로 라이프 사이클 중 하나인 created와 지금 소개할 라우터 네이게이션 가드가 존재한다. 상황에 따라
적절한 위치에서 데이터를 호출 해서 사용 하면 된다.

### 정의

라우터 네비게이션 가드란 vue-router가 제공하는 기능으로 주로 리다이렉션 또는 라우터 이동을 취하여서 네비게이션을 보호하는데 사용한다. 라우터 네비게이션 가드는
created보다 먼저 호출이 된다.

### 사용방법

#### 라우터에서 직접 사용

```js
var routed = {
    routes : [{
        path: 'list',
        name: 'list',
        beforeEnter: (to, from, next) => {
            console.log('to', to);      // 현재 이동하고자 하는 url 정보
            console.log('from', from);  // 현재 url 정보
            console.log('next', next);  // next함수를 호출해야 이동하고자 하는 url로 접속 가능
        }
    }]
}
```
> params 또는 쿼리의 변경에 의해서는 가드가 실행되지 않는다.

### next의 사용

#### 1. next()

네이게이션을 승인 해준다. 라우터의 URL 이동을 허락 해준다.

#### 2. next(false)

네이게이션을 중단한다. from 경로의 URL로 재설정된다.

#### 3. next('/') or next({path : '/'})

다른 위치로 리다이렉션 해 준다.

#### 4. next(error)

next에 전달된 인자가 Error의 인스턴스이면 탐색 중단 및 router.onError()를 이용해 등록 된 콜백에 에러가 전달 된다.

### 가드의 종류

###### 1. 전역 가드 : beforeEnter

###### 2. 글로벌 가드 : beforeResolve, afterEach

###### 3. 내부가드 : beforeRouteEnter, beforeRouteUpdate, beforeRouteLeave

내부가드는 router가 아닌 컴포넌트 안에 직접 네비게이션 가드를 정의할수 있다.

`beforeRouterEnter`는 해당 가드 생성시에 컴포넌트 생성 전이라 this에 접근이 불가능하다.

```js
var component = {
    beforeRouteEnter(to, from, next) {
        next(vm => {
            // 'vm'을 통한 컴포넌트 인스턴스 접근이 가능하다.
        })
    }
}
```

`beforeRouteLeave`는 this에 직접 접근이 가느하다. 사용자가 저장하지 않는 편집 내용을 두고 라우트를 떠나는 것을 방지할때 사용한다.

### 전체 네비게이션 시나리오

```
1) 네비게이션이 트리거됨.
2) 비활성화될 컴포넌트에서 가드를 호출.
3) 전역 beforeEach 가드 호출.
4) 재사용되는 컴포넌트에서 beforeRouteUpdate 가드 호출. (2.2 이상)
5) 라우트 설정에서 beforeEnter 호출.
6) 비동기 라우트 컴포넌트 해결.
7) 활성화된 컴포넌트에서 beforeRouteEnter 호출.
8) 전역 beforeResolve 가드 호출. (2.5이상)
9) 네비게이션 완료.
10) 전역 afterEach 훅 호출.
11) DOM 갱신 트리거 됨.
12) 인스턴스화 된 인스턴스들의 beforeRouteEnter가드에서 next에 전달 된 콜백을 호출합니다.
```








