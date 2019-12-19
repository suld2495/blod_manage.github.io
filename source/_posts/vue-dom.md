---
title: DOM 요소에 직접 접근
date: 2019-12-19 09:43:18
tags: [vue,dom]
categories:
- vue
- vue 기본문법
---

vue를 사용하게 되면 DOM요소에 대한 관리를 vue가 해주기 때문에 직접 접근할 일이 드물지만, 특별한 케이스에 한해서 DOM 요소에 접근을 해야할 경우가 생길수 있습니다.

## 1. DOM 요소에 접근

```html
<template>
    <div>
        <span ref="message"></span>
    </div>
</template>

<script>
export default {
    created() {
        this.$refs.message.innerHTML = "ref 테스트"
    }
}
</script>
```

> `this.$refs.ref이름` 으로 DOM요소에 접근 가능

## 2. 자식 컴포넌트의 메서드 호출

```html
<template>
    <div>
        <childComponent ref="child"/>
    </div>
</template>

<script>

/* parentComponent */
import childComponent from './childComponent'

export default {
    components: {
        childComponent
    },
    created() {
        this.$refs.child.displayConsole()
    }
}

/* childComponent */
export default {
    methods: {
        displayConsole() {
            console.log('ref 테스트')
        }
    }
}
</script>
```