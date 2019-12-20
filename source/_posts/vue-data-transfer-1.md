---
title: 데이터 흐름 처리
date: 2019-12-20 10:22:21
tags: [vue,vuex]
category: 
- vue
- vuex
---

# 데이터 흐름의 처리

##### 1. vuex를 최대한 활용

```
흐름 : 부모 => actions => API => mutations => state => 자식에서 state 접근
```

이 방법은 vuex를 최대한 활용 하는 방법으로 데이터를 표현하는 곳에서 직접 state에 접근 하는 방법이다. 이때 데이터를 표현하는 곳에서는 actions를 호출하지 않는다.
내 생각에 actions에 대한 결합도가 높아지기 때문인듯하다(해당 컴포넌트의 범용성이 낮아진다).

##### 2. props 활용

```
흐름 : 부모 => actions => API => mutations => state => 부모에서 state에 접근 후 props로 전달 => 자식
```

부모에서 API 호출 및 값을 가져와서 자식에게 props로 넘겨준다.

> 컴포넌트 태그처럼 명시적으로 표현하려면 이걸 쓰자.
