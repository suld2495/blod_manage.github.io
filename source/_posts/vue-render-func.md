---
title: Render 함수를 이용한 컴포넌트 작성
date: 2020-01-10 14:42:26
tags: [vue,component,render]
categories:
- vue
- vue component
---

## Render 함수를 이용한 컴포넌트 작성

template을 사용하지 않고 render함수를 이용하여 컴포넌트를 작성 할수 있습니다. 이때 createElement 함수를 사용하여 작성합니다. 


###### 기본 구성

컴포넌트 작성시의 차이점은 template 속성을 사용 하는 대신에 render 함수를 사용한다는 차이점 뿐입니다. render함수는 return으로 VNode를 반환해주기만 하면 됩니다. VNode 생성은 createElement함수를 호출하면 반환해 줍니다(this.$createElement로도 동일하게 사용 가능).

```js
export default {
    name: '',
    proprs: {},
    created() {

    },
    data() {
        return {}
    },
    
    // h를 쓰기도 하고 createElement라고 쓰기도 한다.
    render(h) {
        return h('div', ['값']);
    }
}
```

##### createElement 함수 호출

```js
createElement(
    // {String | Object | Function}
    // HTML 태그 이름(사용자가 만든 컴포넌트 태그 이름 포함), 컴포넌트 옵션 또는 함수 중
    // 하나를 반환하는 함수. 필수사항
    'div',

    // {Object}
    // 컴포넌트에서 사용할 속성 값을 설정합니다. 
    // 해당 값은 선택사항 입니다.
    {

    },

    // {String | Array}
    // VNode 자식들
    // 하위 태그를 작성할때 사용 합니다.
    // createElement()를 사용하거나, 문자열을 입력 가능합니다.
    // 해당 값은 선택사항 입니다.
    [
        '문자열',
        createElement('h1', '옵션 사용 없이 바로 문자열')
    ]
)
```

```js
createElement(
    'div',
    {
        props
    },
    [
        '문자열'
    ]
)
```

> 지금까지 확인결과 VNode 자식값을 사용할시에 배열로 넘기자.

##### 데이터 객체

createElement의 두번째 파라미터로 사용 하는 데이터 객체는 컴포넌트에 속성을 정의 할때 사용하게 됩니다.

```js
{
    // v-bind:class와 동일
    'class': {
        foo: true,
        bar: false
    },

    // v-bind:style와 동일
    style: {
        color: 'red'
    },

    // 일반 HTML 속성을 정의할때 사용
    attrs: {
        id: 'foo'
    },

    // DOM 속성을 정의할때 사용
    domProps: {
        innerHTML: 'baz'
    },

    // v-on:click 등과 같은 역할을 합니다.
    on: {
        click: this.handlerClcik
    },

    // 다른 컴포넌트의 slot으로 들어가는 컴포넌트인 경우 슬롯의 이름을 여기에 적어줍니다.
    slot: 'slot-name',
    
    // ref를 지정하고 싶으면 해당 key를 사용합니다.
    ref: 'refValue'
}
```