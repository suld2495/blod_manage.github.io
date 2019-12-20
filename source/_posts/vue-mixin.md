---
title: Mixin 기본 사용방법
date: 2019-12-20 10:16:13
tags: [vue,mixin]
categories:
- vue
- vue 활용방법
---

# Minxin 기본 사용방법

### 정의

믹스인은 여러 컴포넌트 간에 공통으로 사용하고 있는 로직, 기능들을 재사용하는 방법이다. 믹스인에서 정의할수 있는 재사용 로직은 data, methods, crated 등과 같은 컴포넌트 옵션이다.

### 사용방법

###### 1. js 파일에서 객체를 반환

```js
export default {
    computed() {
        
    },
    
    data() {
        return {
            
        }
    }
}
```

```js
import Mixin from 'Mixin경로'

export default {
    mixins: [Mixin]
}
```
