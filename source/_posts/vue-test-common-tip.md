---
title: Vue 테스트를 위한 팁
date: 2020-03-12 19:59:39
tags: [test-tip]]
categories:
- javascript
- TDD
- vue
---

## 1. UI 컴포넌트에서 완벽한 라인 기반 커버리지를 목표로 하지 않는다.

<br>

## 2. 자식 컴포넌트를 간접적으로 테스팅하는 것을 피하라
자식 컴포넌트까지 테스트하게 되면 반복적인 렌더링으로 인하여 테스트를 느리게 만든다. 이를 위하여 자식 컴포넌트를 렌더링 하지 않는 `shalloMount`를 제공해 준다.

<br>

## 3. Wrapper.destory()를 호출해도 beforeDestroy, destroyed가 호출 되지 않는다.
destroy 메소드를 호출하더라도 2개의 라이프 사이클 메소드가 호출 되지 않는다. 추가적으로 각 스펙이 종료되어도 컴포넌트는 destory 되지 않는다. 다음 스펙에서 해당 컴포넌트가 필요 없다면 수동적으로 제거해주어야 한다.
그리고 setInterval, setTimeout를 사용 하는 경우에 스펙이 종료되어도 clear 되지 않기 때문에 이후 스펙에도 필요한 경우가 아니라면 스펙이 종료되기 전에 종료해 주어야 한다.

<br>

## 4. Vue의 비동기
Vue는 비동기적으로 DOM을 업데이트 하기 때문에 DOM의 변경 된 후에 테스트 해야 하는 경우에는 Vue.nextTick(wrapper.vm.$nextTick)을 사용하라.
다음의 메소드들을 사용한 테스트를 하는 경우 비동기라서 next tick을 사용해야 할수도 있다.

1. setChecked
1. setData
1. setSelected
1. setProps
1. setValue
1. trigger

<br>

## 5. emit 이벤트 테스트
wrapper의 vm을 통해서 $emit 메소드를 사용 가능하다.

```js
wrapper.vm.$emit('이벤트명', 파라미터);
```

이렇게 이벤트를 실행 시키면 wrapper의 emitted 메소드를 통해서 실행된 이벤트 리스트를 확인 가능하다.

```js
// emitted()메소드 실행 결과는 아래와 같다.

{
    이벤트명 : [
        [첫번째 이벤트 파라미터],
        [두번째 이벤트 파리미터]
    ]
}
```

```js
/*
    emitted는 아래와 같이 사용할수 있다.
 */

// emit 된 이벤트 존재 여부 확인
expect(wrapper.emitted().이벤트명).toBeTruthy();	
// emit 된 이벤트 총 개수
expect(wrapper.emitted().이벤트명.length).toBe(개수);	
// emit된 이벤트 중 인덱스에 해당 하는 이벤트의 파라미터 : [파리미터]로써 배열로 감싸져 있다.
expect(wrapper.emitted().이벤트명[인덱스]).toBe([파리미터]);	
```

## 6. 자식의 emit 이벤트 테스트하기

```js
import Child from './Child.vue';

// 자식의 이벤트 호출은 아래와 같이 한다.
wrapper.find(Child).vm.$emit('이벤트명', 파라미터);
```

이벤트 호출의 처리 결과를 테스트 해주면 된다.

## 7. data와 props 값 주기
data와 props에 값을 할당하는 방법에는 2가지가 존재한다.

1. mount시에 할당 하기

```js
mount(컴포넌트, {
    data() {
        return {
            data키 : 값
        }
    },
    propData: {
        prop키 : 값
    }
});
```

<br>

2. mount 후에 할당하기

```js
wrapper.setData({ data키 : 값 });
wrapper.setProps({ prop키 : 값 });
```

되도록이면 값이 변경되는 것을 확인하는게 아니라면 set 보다는 mount시에 초기화하자.

<br>

## 8. createLocalVue
vuex, vue-router와 같은 플러그인을 사용하려면 `Vue.use`를 사용하여야 한다. 이는 Globals Vue constructor를 오염시킨다.
해당 기능을 사용하지 않는 테스트이지만 오염된 Vue 때문에 함께 사용하여야 한다.

<br>

이를 방지 하기 위해서 vue test utils에서는 createLocalVue를 제공해준다. 좀더 독립적인 테스트가 가능해졌다.

```js
const localValue = createLocalValue();
mount(컴포넌트, {
    localValue
})
```

<br>

## 10. 전역 property mock 하기
$route, $store, $t와 같은 것들을 mock 하는 것이다.

```js
// 아래 3개는 직접 작성 해주는 곳이다. => 아래의 값일때 어떻게 동작하는지를 테스트 하겠다는 것일듯하다.
// ex) $route 값이 path가 '/' 일때 어떠한 동작을 테스트 한다.
var $route = ...;
var $store = ...;
var $t = ...;

mount(컴포넌트, {
    mocks: {
        $route,
        $store,
        $t
    }
})
```

<br>

## 11. 자식을 렌더링 하지 않기
mount시에 stubs 옵션을 사용하면 자식을 렌더링 하지 않는다.

```js
mount(컴포넌트, {
    stubs: {
        컴포넌트명 : true
    }
})
```

shalloMount를 사용했을 시에 자식이 렌더링 되지 않는 것은 자동적으로 자식 컴포넌트들을 stubs로 등록해주기 때문이다.
그러므로 자식을 렌더링하지 않는 용도일 경우에는 shalloMount를 사용하자.