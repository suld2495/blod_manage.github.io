---
title: 함수형 컴포넌트
date: 2020-01-10 15:26:51
tags: [vue,component,render]
categories:
- vue
- vue component
---

## 함수형 컴포넌트

어떠한 상태도 없고 라이프사이클 관련 메소드도 사용하지 않을때 지금까지 사용해왔었던 컴포넌트 생성 방법을 사용한다면 사용하지도 않는 메소드를 추가하게 됩니다. 함수형 컴포넌트를 사용하면 심플하게 작성할수 있습니다.

### 사용방법

##### functional 속성으로 사용

functional 속성을 사용하는 경우 template 태그를 사용할수 없습니다. 이때 render 함수를 사용 합니다.

```html
<script>
export default {
    functional : true,
    render(h, context) {
        // ...
    }
}
</script>
```

### 장점

함수형 컴포넌트는 라이프 사이클 메소드를 가지지 않습니다. 이를 통해서 앱 퍼포먼스 향상 효과를 가질수 있습니다.

###### FunctionalView

```html
<template>
    <div>
        <template v-for="item in list">
            <functional-component :key="item"/>
        </template>
    </div>
</template>

<script>
    import FunctionalComponent from '../components/functional/FunctionalComponent.vue';

    export default {
        components: {
            FunctionalComponent
        },
        data() {
            return {
                list: []
            }
        },
        created() {
            for (let i = 0; i < 1000; i++) {
                this.list.push(i);
            }
        }
    }
</script>

<style scoped>

</style>
```

###### FunctionalComponent

```html
<script>
    export default {
        functional: true,
        render(h) {
           return h('div', '함수형 컴포넌트');
        }
    }
</script>

<style scoped>

</style>
```

함수형 컴포넌트를 1000개 생성하고 있습니다. 하지만 count는 1이라는 것을 확인할수 있습니다.

![](/image/functional.PNG)


###### NoneFunctionalComponent

functional만 지우고 나머지는 위의 코드와 동일 합니다.

```html
<script>
    export default {
        render(h) {
           return h('div', '함수형 컴포넌트');
        }
    }
</script>

<style scoped>

</style>
```

![](/image/nonefunctional.PNG)


<br>

## SPA에서 functional 사용하기

2.5.0+ 이후로는 템플릿 기반의 함수형 컴포넌트를 정의할수 있습니다. template 태그에 functional 속성을 추가하면 동일하게 함수형 컴포넌트를 사용할수 있습니다.

```html
<template functional>
    <div>함수형입니다.</div>
</template>
<script>
    export default {
    }
</script>

<style scoped>

</style>
```