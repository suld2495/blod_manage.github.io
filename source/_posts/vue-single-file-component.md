---
title: 뷰 싱글파일 컴포넌트 체계(7)
date: 2019-12-19 00:46:09
tags: vue
category: 
- vue
- vue 기본문법
---

Vue.component를 사용하는 전역 컴포넌트로는 구조 파악이나 스타일 적용등에 있어서 어려움이 많아서 복잡한 뷰 프로젝트에서 사용하기가 힘듭니다. 그래서 .vue 파일로 프로젝트를 구성하는 방식인 싱글파일컴포넌트를 사용합니다. 1개의 .vue파일은 1개의 컴포넌트가 됩니다.

## 구조

```html
<template>
    
</template>

<script>
export defualt {
    data() {
        return {

        }
    },

    methods: {

    },

    created() {

    }
}
<script>

<style>

</style>
```

template 영역은 HTML 요소가 들어가는 부분입니다. script 영역은 컴포넌트에 대한 속성이 들어가는 부분입니다. style은 HTML 요소에 대한 스타일을 적용 할수 있습니다.