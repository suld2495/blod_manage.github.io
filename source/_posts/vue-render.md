---
title: Vue 컴포넌트 작성 방법
date: 2020-01-10 13:31:42
tags: [vue,component,render]
categories:
- vue
- vue component
---

## Vue 컴포넌트 작성

Vue에서는 템플릿을 통해서 HTML을 작성하는 것을 권장하고 있습니다. 하지만 특정 상황에서는 자바스크립트를 이용해야 하는 상황이 있습니다. 그렇기 때문에 템플릿 뿐 아니라 자바스크립트로도 컴포넌트를 작성하는 방법에 대해서 알고 있어야 합니다.

#### 싱글 파일 컴포넌트

template, script, style 3개의 태그를 이용하여 하나의 파일에서 작성 합니다.

```html
<template>
    <div>
        {{ str }}
    </div>
</template>

<script>
    export default {
        name: 'VueSingleFileComponent',
        props: {
            str: {
                type: String
            }
        }
    }
</script>

<style scoped>

</style>
```

###### 사용하기

다른 컴포넌트에서 import를 통해 사용할수 있습니다.

```html
<template>
    <vue-string-file-component/>
</template>

<script>
import VueSingleFileComponent from './VueSingleFileComponent.vue';

export default {
    name: 'VueView',
    components: {
        VueSingleFileComponent
    }
}
</script>

<style>
</style>
```

---

Vue.component를 통해서 등록하면 실제 사용하는 곳에서 import를 할 필요 없이 사용할수 있습니다.

###### main.js

```js
import Vue from 'vue';
import VueSingleFileComponent from './VueSingleFileComponent.vue';

Vue.component('vue-single-file-component', VueSingleFileComponent);
```

###### VueView.vue

컴포넌트를 import 하거나 components 속성을 등록 할 필요 없이 사용가능합니다.

```html
<template>
    <vue-string-file-component/>
</template>

<script>
export default {
    name: 'VueView',
}
</script>

<style>
</style>
```

<br>

#### String으로 컴포넌트 작성

템플릿을 js의 String으로 정의하는 방법입니다. 위 처럼 vue 확장자를 가진 파일을 따로 생성하지 않고 Vue.component에 직접 등록 합니다.

```js
import Vue from 'vue';

Vue.component('vue-string', {
    template: '<div> {{ str }}</div>',
    props: {
        str: {
            type: String
        }
    }
})
```

Vue.component를 사용하기 때문에 import로 호출만 하고 compoents 속성에 따로 등록 하진 않아도 됩니다.

```js
<template>
    <vue-string/>
</template>
import '../components/vue-render/VueString.js';

export default {
    name: 'VueView',
}
```

<br>

#### Render function을 사용하여 컴포넌트 작성

template을 사용하지 않고 render 함수를 이용해서 컴포넌트를 작성 할수 있습니다. 해당 컴포넌트를 import해서 사용방법은 SPA랑 동일 합니다.

```js
export default {
    name: "VueRenderFunc",
    props: {
        str : {
            type: String
        }
    },
    render(h) {
        return h('div', [this.str]);
    }
}
```

<br>

### 결론

기본적으로 SPA 방식으로 작성하는 게 좋다고 생각합니다. js를 활용해야 하는 경우에는 render 함수를 이용하여 작성하는 것도 하나의 선택이 될수 있을거라 생각합니다.