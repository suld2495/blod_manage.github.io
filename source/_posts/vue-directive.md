---
title: 뷰 디렉티브(5)
date: 2019-12-18 23:53:50
tags: vue
category: 
- vue
- vue 기본문법
---

## 1. v-if

v-if는 조건부로 렌더링을 할때 사용을 합니다. 조건부가 참이면 렌더링을 거짓이면 렌더링을 하지 않습니다. 조건을 만족하지 않으면 렌더링 자체를 하지 않기때문에 돔에 추가 되지 않습니다.

```html
<div v-if="type === 'A'">v-if를 사용해보겠습니다.</div>
```

v-else도 사용 가능합니다.

```html
<div v-if="type === 'A'">참이면 실행하세요.</div>    
<div v-else>거짓이면 실행하세요.</div>    
```

v-else-if도 사용가능합니다.

```html
<div v-if="type === 'A'">type이 A이면 실행하세요.</div>   
<div v-else-if="type === 'B'">type이 B이면 실행하세요.</div>   
```

조건에 따라서 화면에 보이거나 숨겨야 하는 경우 v-if를 사용하면 좋습니다. 하지만 토글기능을 사용해야 할 경우에는 렌더링을 다시 해주어야 하기 때문에 v-if 보다는 v-show가 좋을수있습니다. template 태그에서도 사용가능합니다.(template 태그 그 자체는 사용해도 화면에 렌더링 되지 않고 내부 자식 태그들만 렌더링 해줍니다.)

## 2. v-show

v-show는 v-if랑 비슷한 역할을 합니다. v-if랑 다른 건 렌더링은 모두 하지만 숨기고 보이는 건 css의 display none 속성을 사용합니다. v-if와 마찬가지로 조건을 만족하면 화면에 보이고 만족하지 않으면 보이지 않습니다.

```
<div v-show="true">v-if를 사용해보겠습니다.</div>
```

잦은 보이거나 숨기는 기능을 사용하게 된다면 v-show를 사용하면 좋습니다(렌더링을 다시 하는 것보단 css로 none또는 block하는게 더 효율적). v-show는 초기 렌더링 비용이 높기 때문에 유의 해야 합니다.

## 3. v-for

v-for는 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복해서 출력 합니다. 대상은 Array뿐아니라 Object도 가능합니다.

```html
<ul>
    <li v-for="item in list">{{item}}</li>
</ul>
```

for문에 대한 index도 사용 가능합니다.

```html
<ul>
    <li v-for="(item, index) in list">{{index + ' : ' + item}}</li>
</ul>
```

#### 정리

1. 하나의 태그에 두개 이상의 다중 for문은 작성이 불가능합니다.
2. v-if랑 함께 사용은 가능 하나 공식가이드에서는 권고하지 않습니다.
3. for문을 사용할때는 :key 속성을 부여하는게 좋습니다(값은 유일값으로).
4. 컴포넌트에도 v-for를 사용 가능합니다.
5. 2.2 버전 이상부터는 :key 속성을 사용하여야 합니다(유일 값).
6. template 태그에서도 사용 가능합니다.
7. 리스트뿐 아니라 객체도 사용가능 => Collection에서 사용가능

## 4. v-bind

v-bind는 HTML 속성 값에 데이터를 바인딩하거나 하위 컴포넌트의 props 속성에 값을 바인딩 할때 사용합니다. 사용방법은 `v-bind:속성명="데이터"` 또는 축약으로 `:속성명="데이터"`으로도 사용가능합니다.

```html
<div v-bind:id="type"></div>    <!-- 데이터 type의 값이 바인딩 됩니다. -->
<div v-bind:id="'type'"></div>  <!-- type이라는 문자열으로 설정합니다. -->
```

class 속성에 객체를 전달할 수도 있습니다. 객체를 활용하면 여러개의 class를 쉽게 바인딩 할수 있습니다. 객체의 값이 true면 해당 객체의 key명이 class로 지정됩니다.

```html
<div v-bind:class="{active: true}"></div>
<div v-bind:class="{active: true, on: true}"></div>
```

## 5. v-model

v-model은 form에서 주로 사용되는 속성입니다. 폼에 입력한 값을 뷰의 데이터와 바인딩하는 역할을 합니다. 다른 디렉티브와는 다르게 양방향으로 바인딩 되기 때문에 input에 값을 입력하면 뷰의 데이터도 변경이
됩니다. v-model에서 사용하는 태그는 input, select, textarea 등이 있습니다.

```html
<input type="text" v-model="value">
```

여러 체크박스를 하나의 배열로 바인딩 할수 있습니다. 체크를 하게 되면 배열에 하나씩 추가 됩니다.

```html
<input type="checkbox" value="김씨" v-model="checkboxNames">
<input type="checkbox" value="이씨" v-model="checkboxNames">
<input type="checkbox" value="박씨" v-model="checkboxNames">
```

.number 수식어를 사용하면 자동으로 숫자로 형변환은 해줍니다.

```html
<input type="text" v-model.number="num">
```

.trim 수식어를 사용하면 자동으로 trim을 해줍니다.

```html
<input type="text" v-model.trim="value">
```

# 6. v-on

v-on은 이벤트를 감지하는 기능입니다. 사용하는 방법은 `v-on:이벤트명="실행부"`이고 축약은 `@이벤트명="실행부"` 입니다. 이벤트가 발생해서 메서드를 호출하면 파라미터로 아무것도 넘기지 않았다면 event가 넘어옵니다.

```html
<button v-on:click="onClick">클릭</button>  <!-- onClick은 메서드로써 괄호()는 넣지 않아도 됨 -->
```

이벤트를 실행시킬때 파라미터를 넘겨줄수 있습니다.

```html
<button v-on:click="onClick('type')"></button>   <!-- 해당 버튼을 눌렀을 때 onClick 메서드를 호출하면서 type이라는 문자열을 파라미터로 전달 -->
```

사용자 파라미터를 넘겨주면서 이벤트 객체도 넘겨 줄때는 $event를 사용합니다.

```html
<button v-on:click="onClick('type', $event)"></button>
```


.stop 수식어를 통해서 버블링을 막을수 있습니다.

```html
<button v-on:click.stop="onClick">버튼</button>
```

.prevent 수식어를 통해서 `event.preventDefault()` 역할을 대신 할수있습니다.

```html
<form v-on:submit.prevent="onSubmit"></form>
```

.once 수식어는 최대 한번만 실행 가능하도록 설정합니다.

```html
<form v-on:submit.once="onSubmit"></form>
```

키에 대한 수식어를 사용 가능합니다. 종류에는 .enter, .tab, .delete(Delete와 Backspace 모두), .esc, .up, .down, .left, .right가 존재하며 config.keyCodes를 통해서 사용자 정의 키 수식어를 만들수도 있습니다.

```html
<input type="text" v-on:keyup.enter="onSubmit">
```

> 수식어는 체이닝이 됩니다.
> 이벤트는 기본이벤트 뿐 아니라 사용자 정의 이벤트도 사용가능합니다.