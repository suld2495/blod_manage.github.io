---
title: 뷰 비동기
date: 2020-03-06 21:30:32
tags: [vue-async]
category: 
- vue
- vue 팁
---

## 비동기 갱신 큐
Vue는 비동기 적으로 DOM을 업데이트 합니다. 데이터의 변경이 발견 되면 큐를 열고 같은 이벤트 루프에서 발생한 변경된 모든 데이터를 버퍼에 담습니다. 같은 wather가 여러번 호출 되더라도 큐에서 한번만 푸시 됩니다. 이러한 동작은 불필요한 계산과 렌더링을 방지해줍니다. 그리고 다음 이벤트 루프인 "tick"에서 큐를 모두 비우고 실제 행동을 수행합니다.
<br>
예를 들어 `vm.someData = 'new value'` 라고 설정 할때, Vue는 바로 렌더링을 수행하지 않고 큐가 비워진 후인 다음 "tick"에서 업데이트 됩니다. 대부분의 상황에서는 이러한 작업이 영향을 받지 않지만, DOM state가 업데이트 된 후 작업을 수행하려는 겨우 신경을 써야 할수 있습니다. 데이터가 변경 된 후, DOM에 업데이가 완료되는 것을 기다리려면 데이터가 변경된 직후에 Vue.nextTick(callback)을 사용하면 됩니다. 콜백은 DOM이 업데이트 된 후 호출이 됩니다. 

<br>

#### nextTick 사용하기 
아래의 코드를 실행해보면 data가 변경 되더라도 실제로 바로 렌더링되지 않는 다는 것을 확인 할수 있습니다.
그리고 nextTic 메소드의 콜백함수가 DOM 업데이트가 완료 된 후 실행되었음을 확인할수 있습니다.

```html
<div id="example">{{ message }}</div>
```

```js
var vm = new Vue({
    el: '#example',
    data: {
        message: '123'
    }
})
vm.message = 'new message' 
vm.$el.textContent === 'new message'        // false
Vue.nextTick(function () {
    vm.$el.textContent === 'new message'    // true
})
```

> https://vuejs.org/v2/guide/reactivity.html를 참조하였습니다.