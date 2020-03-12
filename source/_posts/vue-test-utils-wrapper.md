---
title: Vue Test Utils의 Wrapper 객체
date: 2020-03-07 11:07:40
tags: [wrapper객체]
categories:
- javascript
- TDD
- vue
---

## 프로퍼티

#### 1. vm
`vm`은 Vue 인스턴스이며, wrapper.vm으로 vm의 모든 메소드와 프로퍼티에 접근이 가능하다.

```js
wrapper.vm.count;
wrapper.vm.method();
```

<br>

## 메소드

#### 1. html
Wrapper DOM 노드의 HTML을 문자열로 반환을 해줍니다. 그래서 태그를 포함한 결과를 반환합니다.

```js
expect(wrapper.html()).toBe('<div></div>');
```

<br>

#### 2. text
Wrapper의 text만을 반환해줍니다.

```js
expect(wrapper.text()).toBe('문자열');
```

<br>

#### 3. find
선택자에 해당하는 DOM 노드 또는 해당하는 뷰 컴포넌트 중에 선번째 Wrapper를 반환해줍니다.

- Arguments:
    - {string | Component} selector

- Returns: {Wrapper}

```js
wrapper.find('div');
wrapper.find(Component);
```
<br>

#### 3. findAll
해당 하는 모든 Wrapper를 반환해줍니다.

- Arguments:
    - {string | Component} selector

- Returns: {WrapperArray}

<br>

#### 4. exists
해당 Wrapper가 존재하는지 여부를 반환해줍니다.

```js
expect(wrapper.exists()).toBeTruthy();
```

<br>

#### 5. contains
Wrapper가 매칭되는 element 또는 컴포넌트를 포함하고 있는지 여부를 반환해줍니다.

```js
expect(wrapper.contains('div')).toBeTruthy();
expect(wrapper.contains(Component)).toBeFalsy();
```

<br>

#### 6. setData
data에 값을 할당 할수있습니다.

```js
wrapper.setData({ data키:값 })
```

<br>

#### 7. setProps
props에 값을 할당 할수있습니다.

```js
wrapper.setProps({ prop키:값 })
```

<br>

#### 8. trigger
trigger는 wrapper의 이벤트를 발동시켜줍니다. trigger는 추가적으로 option을 줄수 있습니다.
그리고 trigger의 결과를 테스트 할 때는 wrapper.vm.$nextTick를 이용하여 이벤트가 처리 된 후에 테스트 할수 있습니다.

```js
wrapper.trigger('click');
wrapper.trigger('click', {
    ctrlKey: true   // @click.ctrl을 테스트 할수 있다.
})
```

<br>

#### 9. emitted
wrapper의 vm에 의해 발동되어진 커스텀 이벤트를 포함하는 객체를 반환해 줍니다.

```js
wrapper.vm.$emit('custom');
wrapper.vm.$emit('custom', 123);
wrapper.emitted();
/**
    {
        custom: [[], [123]]
    }
 */
```