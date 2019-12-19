---
title: vue-extends
date: 2019-12-19 09:51:55
tags: [vue,extends]
categories:
- vue
- vue 기본문법
---

## component 확장해서 사용하기

extends는 기존 컴포넌트를 확장해서 유사한 컴포넌트를 만들수 있는 방법입니다.

```js
/* Hello.vue */
<template>
    <div>
        {{ message }}
    </div>
</template>

<script>
export default {
    data() {
        return {
            message: "Hello extends 사용하기"
        }
    },
    created() {
        console.log('Hello created 호출');
    }
}
</script>

/* HelloWorld.vue */
<script>
import Hello from './Hello.vue';

export default {
  name: 'HelloWorld',
  extends: Hello,
  data() {
    return {
        message: "HelloWorld extends 사용하기"
    }
  },
  created() {
    console.log('HelloWorld created 호출');
  }
}
</script>
```

extends를 사용할 경우 data, computed, methods, filters는 오버라이딩이 된다. 하지만 라이프사이클은 모두 순서대로 호출이 된다.