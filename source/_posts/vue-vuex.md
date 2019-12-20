---
title: vuex의 기본
date: 2019-12-20 10:20:20
tags: [vue,vuex]
category: 
- vue
- vuex
---
# vuex의 설치 및 시작

## 기본구성

### 설치

```
npm i vuex
```

### vuex 구성

라우터와 마찬가지로 main.js에 직접 구성하는 것은 좋지 못하므로 다른 디렉토리를 구축해서 코드 구성은 해당 파일에서 작성하여 import만 해오자.

### vuex의 정의

vuex란 간단히 말해 상태관리도구이다. 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 한다. 그래서 어느 컴포넌트에서나 쉽게 상태에 접근할수 있다.

### simple vuex 코드

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

new Vuex.Store({
    state: {
        array: []
    }
});
```

### main.js

라우터와 마찬가지로 main.js에서 직접 Vuex를 생성하지 않고 따로 작성된 js에서 import 받아와서 사용한다.

```js
import {store} from 'store/index.js';

new Vue({
    render: h => h(App),
    store
}).$mount('#app');
```

## 프로퍼티

##### 1. state

state는 말그대로 상태를 저장하고 있다.

```js
let vuex = {
    state : {
        array: [],
        data: {},
        str: ""
    }
}
```

##### 2. actions

api와의 통신 역할을 담당한다. api 호출 코드를 여기에서 작성 하면 된다. action에 정의 된 메소드라는 것을 표시하기 위해 대문자 스네이크 표기법을 강의에서는 사용했었다. 

```js
var actions = {
    state: {},
    actions: {
        FETCH_DATA(context, param) {
            fetchDataList()
                .then()
                .catch()
        }
    }
}
```

component에서 actions에 정의된 메소드 호출하기

```js
this.$store.disaptch('메소드명');
this.$store.disaptch('메소드명', 값); // 값은 오직 하나만 전달 가능하므로 여러개를 전달하려면 객체를 통해 전달한다. 
```

actions에서 api통신을 한 결과를 state에 전달하여야 하는데 이때 actions 내부에서 바로 state에 접근이 불가능하다. mutations라는 것을 통해서만 가능하다.
actions에 정의된 메소드의 파라미터로 context값이 넘어오는데 context의 commit 메소드를 호출하면 mutation에 전달가능하다.

```js
var actions = {
    state: {},
    actions: {
        FETCH_DATA(context, param) {    // param에 dispatch를 실행할때 넘겼던 값을 받을수 있다.
            fetchDataList()
                .then(({ data }) => {
                    context.commit('mutation메소드명', '전달할 값')    
                })
                .catch()
        }
    }
}
```

##### 3. mutations

actions에서 backend(api)와이 실행 결과를 state에 저장할때 사용되는 중간 연결자이다. actions에서 context.commit으로 실행될 메소드를 정의한다.

```js
var mutations = {
    state: {
        data: {}
    },
    actions: {},
    mutations: {
        SET_DATA(state, data) {
            state.data = data;
        }
    }
}
```

첫번째 파라미터는 state가 넘어오고 두번째 파라미터는 commit 호출시에 넘겨준 파라미터가 넘어온다.

## vue에서 store 접근

##### 1. 가장 원시적인 방법

아래는 자바스크립트에서 사용방법

```js
this.$store.state.프로퍼티
```

아래는 vue의 template에서 사용방법

```vue
<div>{{ this.$store.state.프로퍼티 }}</div>
```

위의 방법은 Vuex를 모듈방식으로 작성하기 시작하면 참조가 너무 길어진다.(a.b.c.d.e.f.g.value)

##### 2. computed를 이용한 방법

```js
var Vue = {
    computed: {
        state() {
            return this.$store.state
        }
    }
}
```

단순하게 vue의 computed를 이용해서 공통된 부븐을 메소드에 저장하는 방법이다.

##### 3. vuex의 mapState를 이용

```js
import { mapState } from 'vuex';

var Vue = {
    computed: {
        ...mapState({
            value: state => state.value
        })
    }
}
```

vuex에서 제공하는 mapState를 활용하는 방법으로 파라미터로 state가 넘어오다보니 위와 마찬가지로 모듈이 많아지게 되면 state.a.b.c.d.value로 길어질수 있다.

##### 4. vuex의 mapGetter를 이용(mapGetters의 배열을 이용하는 방법)

```js
// vuex 파일

new Vuex.Store({
    getters: {
        value(state) {
            return state.value;
        }
    }
})
```

```js
// state 값 사용 할 파일
import { mapGetters } from 'vuex';

var Vue = {
    computed: {
        ...mapGetters(['value'])
    }
}
```

getters는 computed와 비슷한 역할을 한다. 컴포넌트에서는 mapGetter에서 배열을 넘겨준다(이때 배열의 문자열이 vuex의 getters에 정의 된 메소드 이름을 넘겨주면 해당값을 받아온다. getters에 등록 하는 번거로움이 있지만 사용할때는 변수명이 명료해서 보기 좋다.

##### 5. vuex의 mapGetter를 이용(mapGetters의 객체를 이용하는 방법)

vuex 파일의 사용법은 위와 동일하다.

```js
// state 값 사용 할 파일
import { mapGetters } from 'vuex';

var Vue = {
    computed: {
        ...mapGetters({
            myValue: 'value'
        })
    }
}
```

배열을 이용할때는 변수명을 마음대로 지정이 불가능하지만 객체를 이용하면 이름을 컴포넌트에서 마음대로 지정 할수 있다.

##### 모듈화

하나의 파일에서 모든 데이터를 관리하면 알아보기 힘들기 때문에 모듈화 작업을 한다. mutations, actions, state, getters를 따로 파일을 만들어서 관리한다. 상황에 따라 분리 할수도 있고 길지 않다면 하나의 파일에서 관리할수 있고 더 세분화로 분리할수도 있다.