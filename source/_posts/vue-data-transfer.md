---
title: 컴포넌트 간 데이터 전송(9)
date: 2019-12-19 09:29:45
tags: [vue,prop,eventbus]
category: 
- vue
- vue 기본문법
---

# 상위 - 하위 컴포넌트간의 데이터전달

각 컴포넌트는 고유한 유효범위를 가지고 있기 때문에 다른 컴포넌트의 값을 직접 참조할수가 없습니다. 그래서 컴포넌트간의 데이터를 전달 하는 방법을 알아야 합니다.

## 1. 상위 컴포넌트에서 하위 컴포넌트로 값 전달

이 방법은 상위 컴포넌트에서 하위 컴포넌트로 v-bind 디렉티브를 이용해서 값을 전달 할수 있습니다.

### 상위 컴포넌트

상위 컴포넌트에서는 하위 컴포넌트에 v-bind 디렉티브를 이용해서 속성에 값을 전달합니다.

```html
<child-componenet v-bind:msg="message"></child-componenet>

<script>
export default {
    data() {
        return {
            message: '안녕하세요'
        }
    }
}
</script>
```

### 하위 컴포넌트

하위 컴포넌트에서는 상위 컴포넌트에서 속성의 값을 props 속성을 이용하여 전달 받을 수 있습니다.

```html
<script>
export default {
    props: {
        msg: String     // 속성명 : 전달 받을 타입으로 작성
    }
}
</script>
```

## 2. 하위 컴포넌트에서 상위 컴포넌트로 값 전달

하위 컴포넌트에서 상위 컴포넌트로 값을 전달하기 위해서는 이벤트를 활용합니다. 이벤트를 발생시키는 $emit, $on을 이용하여 값을 전달 합니다.

### 하위 컴포넌트

하위 컴포넌트는 상위컴포넌트로 값을 전달 할 상황이 생기면 $emit을 이용하여 이벤트를 발생시키면서 데이터를 전달 합니다.

```html
<button @click="onClick">클릭</button>

<script>
export default {
    data() {
        return {
            msg : '안녕하세요.'
        }
    },

    methods: {
        onClick() {
            this.$emit('@click', this.msg);
        }
    }
}
</script>
```

### 상위 컴포넌트

하위 템플릿에 추가한 이벤트에서는 상위 컴포넌트의 메서드를 호출 할수 있기 때문에 하위 컴포넌트에서 $emit과 함께 전달한 데이터를 상위 컴포넌트로 전달 할수 있습니다.

```html
<child-component @@click="onSubmit"></child-component>

<script>
export default {
    methods: {
        onSubmit(msg) {
            console.log(msg);
        }
    }
}
</script>
```

> 단점 : 트리구조가 복잡하게 얽혀있는 구조인 경우에 하나의 데이터를 변경하려고 해도 연관된 상하위컴포넌트 전체를 변경해야 하기 때문에 유지 보수의 어려움

## 3. 이벤트 버스

상위-하위 컴포넌트 간의 데이터 통신은 상-하위 관계가 복잡하게 이루어져 있을 경우에 연관된 props와 이벤트 발생가 많이 생성이 되어서 하나를 수정해야 한다면 관련된 모든 props와 이벤트를 수정하여야 하기 때문에 유지보수의 어려움이 있습니다. 그래서 이벤트 버스를 활용하면 직접 관계가 없는 컴포넌트끼리 데이터를 전송할수 있습니다.


이벤트 버스 또한 $emit와 $on을 활용 합니다.

### 1) 이벤트 버스 생성

Vue의 프로토 타입으로 $EventBus(이름은 상관 없음) 뷰 인스턴스를 생성합니다(이때 아무런 옵션을 설정하지 않습니다). 이렇게 생성하면 컴포넌트 내부에서 this.$EventBus로 접근이 가능합니다.

```js
import Vue from 'vue'

Vue.prototype.$EventBus = new Vue();
```

### 2) 데이터를 전달 하려는 컴포넌트

```html
<button @click="onClick">클릭</button>

<script>
export default {
    methods: {
        onClick() {
            this.$EventBus.$emit('@click', '안녕하세요');
        }
    }    
}
</script>
```

### 3) 데이터를 전달 받는 컴포넌트
```html
<script>
export default {
    created() {
        this.$EventBus.$on('@click', (msg) => {
            console.log(msg);
        });
    }
}
</script>
```

### 4) 이벤트 해제
```html
<script>
export default {
    methods: {
        created() {
            this.$EventBus.$on('@click', () => {
                console.log('생성')
            });
        }, 
        destroyed() {
            this.$EventBus.$off('@click')
        }
    }
} 
</script>
```

#### 장점
서로 연관이 없는 컴포넌트끼리 직접적으로 쉽게 데이터를 전송하고 받을수 있다.
#### 단점
이벤트 버스를 너무 남용하게 되면 데이터의 흐름을 파악하기 힘들다(이벤트를 emit 했는데 누가 받고 있는지 실제 소스 코드를 확인하기 전까지는 확인이 어렵다).
#### 대안
데이터 중앙집중식 방식을 채용하고 있는 vuex를 사용한다.
#### 사용처
spinner(페이지 전환시에 로딩 처리)는 이벤트 버스가 좋다고 한다. 이벤트 버스는 훅에 선언하는 것이 좋다.
#### 주의
이벤트 버스를 등록 하는 경우 필히 beforeDestory(destroyed)에 $off를 해주어야 한다. 하지 않으면 무한 증식한다.