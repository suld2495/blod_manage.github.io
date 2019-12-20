---
title: 라우터 객체
date: 2019-12-20 09:15:24
tags: [vue,$router,$route]
categories:
- vue
- vue 라우터
---

# 라우터 객체

### router 객체

#### 라우터 객체를 이용한 주소변경

앞서서 `<view-link to="">`를 활용한 주소변경에 대해서는 알아보았습니다. 하지만 때론 자바스크립트로 주소변경을 해야 하는 경우가 발생할수 있습니다.
이런 경우에는 `this.$router.push('주소')`을 이용하여 주소를 이동할수 있습니다.

```html
<button @click="onClik">클릭</button>

<script>
export default {
    methods: {
        onClick() {
            this.$router.push('주소');
            this.$router.push({path : '주소'});
            this.$router.push({path : '주소', params: {id: 123}});
            this.$router.push({path : '주소', query: {id: 123}});   // query를 사용하는 경우 주소창에 /주소?id=123으로 출력
        }
    }
}
</script>
```

#### 히스토리 목록에 추가하지 않고 주소 변경

push를 이용하면 히스토리 목록에 추가가 됩니다. 하지만 replace를 이용하면 목록에 추가 되지 않습니다.($router.go 메서드를 이용해서 이전 주소로 이동이 불가능합니다.)

```html
<router-link to="주소" replace></router-link>   <!-- 선언적 방식은 속성으로 replace를 추가 -->

<script>
export default {
    methods: {
        onClick() {
            this.$router.replace('주소');
            this.$router.replace({path : '주소'});
            this.$router.replace({path : '주소', params: {id: 123}});
            this.$router.replace({path : '주소', query: {id: 123}});   // query를 사용하는 경우 주소창에 /주소?id=123으로 출력
        }
    }
}
</script>
```

> $router는 VueRouter인스턴스 입니다.


### route 객체

#### $route.params

동적라우팅으로 데이터를 받거나 push등을 통해서 params에 데이터를 저장한 경우에 `this.$route.params.데이터`를 통해서 데이터를 얻어 올수 있습니다.

```html
<script>
export default {
    created() {
        this.$route.params.데이터
    }
}
</script>
```

#### $route.query

push등을 통해서 query에 데이터를 저장한 경우에 `this.$route.query.데이터`를 통해서 데이터를 얻어 올수 있습니다.

```html
<script>
export default {
    created() {
        this.$route.query.데이터
    }
}
</script>
```