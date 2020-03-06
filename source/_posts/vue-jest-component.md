---
title: Vue Test Utils 이용한 컴포넌트 테스트
date: 2020-03-06 20:41:54
tags: [component-test]
categories:
- javascript
- TDD
- jest
---

## new Vue를 이용한 컴포넌트 테스트
아래의 방법은 실제로 컴포넌트를 생성하여 테스트 하는 방법입니다.


```vue
<!-- HelloWorld.js -->

<template>
  <div>{{ message }}</div>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello"
    }
  }
}
</script>

<style>

</style>
```

```js
// helloworld.test.js

import Vue from 'vue';
import HelloWorld from '../components/HelloWorld.vue';

describe(('HelloWorld'), () => {
    it('new Vue를 이용한 테스트', () => {
        const helloWorld = new Vue(HelloWorld).$mount();
        const message = 'Hello';
        expect(helloWorld.message).toBe(message);
    });
});
```

## Vue Test Utils 이용한 컴포넌트 테스트
Vue Test Utils에서 제공 하는 mount 함수를 이용해서 좀더 간편 하게 테스트를 할수 있습니다. 
message에 접근 하기 위해서 vm 프로퍼티를 이용한다는게 다릅니다.

```js
// helloworld.test.js

import { mount } from '@vue/test-utils';
import HelloWorld from '../components/HelloWorld.vue';

describe(('HelloWorld'), () => {
    it('new Vue를 이용한 테스트', () => {
        const wrapper = mount(HelloWorld);
        const message = 'Hello';
        expect(wrapper.vm.message).toBe(message);
    });
});
```