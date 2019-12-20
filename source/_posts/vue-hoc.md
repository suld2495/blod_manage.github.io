---
title: 하이오더컴포넌트
date: 2019-12-20 10:18:10
tags: [vue,hoc]
categories:
- vue
- vue 활용방법
---

# 하이오더컴포넌트 기본사용 방법

## 정의

Mixin와 마찬가지로 컴포넌트의 로직을 재사용하기 위한 기술이다.

## 방법

##### 1. 공통으로 사용할 부분을 js로 따로 만든다.
##### 2. 함수를 하나 선언하고 return 해준다.
##### 3. return에 들어가는 것 : render, 나머지 공통으로 사용되는 부분 작성

```js
import Component from 'component.vue';

export default function hoc(componentName) {
    return {
        name: componentName,    // 해당 이름으로 컴포넌트 생성됨
        computed() {
            
        },
        data() {
            return {
                
            }
        },
        runder(h) {
            return h(Component);
        }
    }
}
```

render 부분은 공통으로 사용되는 템플릿 부분을 사용하기 위한 것이다.

## 단점

HOC를 사용하게 되면 depth가 하나 추가된다. 이는 많이 사용하게 되면 컴포넌트의 깊이가 깊어져서 데이터 통신이 어려워진다.

```
일반구조 : 상위 - 하위
HOC구조 : 상위 - HOC - 하위
```

vue에서는 mixins와 scoped slotㅇ을 활용하고 함수형 프로그래밍이나 컴포넌트 재사용성을 극대화 하고 싶다면 HOC 활용하는것도 좋을거 같다.