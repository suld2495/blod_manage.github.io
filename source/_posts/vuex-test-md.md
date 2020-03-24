---
title: Vuex를 사용한 테스트
date: 2020-03-14 11:35:32
tags: [vuex-test]
categories:
- javascript
- TDD
- vue-test-utils
---

## Vuex를 사용하는 컴포넌트 테스트

#### actions를 mock 하기
아래의 컴포넌트는 vuex의 actions를 사용하는 코드이다. Vuex를 사용하는 컴포넌트를 테스트시에 주의해야 할 점은 actions가 어떻게 동작하는지는 중요하지 않다. 우리가 사용하고자 하는 action이 실제로 호출되었는지 여부만 테스트 한다.
action이 정확하게 동작하는지 여부는 vuex store 테스트시에 하게 된다.

```vue
<template>
    <div class="text-align-center">
        <input type="text" @input="actionInputIfTrue">
        <button @click="actionClick()">Click</button>    
    </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { mapActions } from 'vuex'

@Component({
    methods: {
        ...mapActions(['actionClick'])
    },
})
export default class Actions extends Vue {
    actionInputIfTrue(event:any):void {
        const inputValue = event.target.value;
        if (inputValue === 'input') {
            this.$store.dispatch('actionInput', { inputValue });
        } 
    }
}
</script>
```

```ts
import { shallowMount, createLocalVue, Wrapper } from '@vue/test-utils';
import Vuex from 'vuex';
import Actions from '../Actions.vue';

const localVue = createLocalVue();

localVue.use(Vuex);

describe('Actions.vue', () => {
    let wrapper:Wrapper<Vue>;
    let actions:any;
    let store:any;

    beforeEach(() => {
        actions = {
            actionClick: jest.fn(),
            actionInput: jest.fn()
        };

        store = new Vuex.Store({
            actions,
        });

        wrapper = shallowMount(VuexComponent, { store, localVue });
    });

    it('dispatches "actionInput" when input event value is "input"', () => {
        const input = wrapper.find('input');
        input.element.value = 'input';
        input.trigger('input');
        expect(actions.actionInput).toHaveBeenCalled();
    });

    it('does not dispatch "actionInput" when event value is not "input"', () => {
        const input = wrapper.find('input');
        input.element.value = 'no input';
        input.trigger('input');
        expect(actions.actionInput).not.toHaveBeenCalled();
    });

    it('calls store action "actionClick" when button is clicked', () => {
        const button = wrapper.find('button');
        button.trigger('click');
        expect(actions.actionClick).toHaveBeenCalled();
    })
});
```

<br>

`첫번째` 주의해야 할 점은 테스트 시에는 localVue를 사용해야 한다는 것이다.
`두번째` 는 actions를 jest의 mock functions를 사용하여 테스트한다. 이를 이용하면 actions를 실제로 구현하지 않아도 호출되었는지 여부를 확인 할수 있다.
`세번째` 는 함수의 호출 여부를 확인 하는 assertion은 `toHaveBeenCalled`를 사용한다.

<br>

#### getters를 mock 하기
getters도 actions와 마찬가지로 어떻게 동작하는지는 중요하지 않다. getters의 결과가 실제로 렌더링 되는지 여부만 확인 해보면 된다.

```vue
<template>
    <div>
        <p v-if="inputValue">{{inputValue}}</p>
        <p v-if="clicks">{{clicks}}</p>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex'

    @Component({
        computed: mapGetters(['clicks', 'inputValue'])
    })
    export default class Getters extends Vue {

    }
</script>
```

```ts
import { shallowMount, createLocalVue, Wrapper } from '@vue/test-utils';
import Vuex from 'vuex';
import Getters from '../Getters.vue';

const localVue = createLocalVue();

localVue.use(Vuex);

describe('Getters.vue', () => {
    let getters:any;
    let store:any;
    let wrapper:Wrapper<Vue>;

    beforeEach(() => {
        getters = {
            clicks: () => 2,
            inputValue: () => 'input'
        };
        store = new Vuex.Store({
            getters
        });
        wrapper = shallowMount(VuexComponent, { store, localVue });
    });
    
    it('Renders "store.getters.inputValue" in first p tag', () => {
        expect(wrapper.find('.input-value').text()).toBe(getters.inputValue());
    });

    it('Renders "store.getters.clicks" in second p tag', () => {
        expect(wrapper.findAll('p').at(1).text()).toBe(getters.clicks().toString());
    });
});
```

<br>

`첫번째` 는 actions와는 달리 getters는 jest의 mock functions를 사용하지 않는다. getters 객체를 작성하지만 로직은 중요하지 않는다.
`두번째` 는 getters의 리턴 값이 렌더링되었는지 여부를 확인 하는 건 wrapper의 text 메소드를 이용한다.

<br>

#### vuex의 모듈 방식 테스트하기
vuex의 모듈 방식을 활용한 테스트도 기존의 테스트랑 별 다를게 없다.

```vue
<template>
    <div>
        <button @click="moduleActionClick()">Click</button>
        <p>{{moduleClicks}}</p>
    </div>
</template>

<script>
import { Vue, Component } from 'vue-property-decorator';
import { mapActions, mapGetters } from 'vuex';

@Component({
    methods: {
        ...mapActions(['moduleActionClick'])
    },
    computed: mapGetters(['moduleClicks'])
})
export default class MyModuleVuex extends Vue {
    
}
</script>
```

```ts
import { shallowMount, createLocalVue, Wrapper } from '@vue/test-utils';
import Vuex from 'vuex';
import MyModuleVuex from '../MyModuleVuex.vue';
import myModule from '../../store/myModule';

const localVue = createLocalVue();

localVue.use(Vuex);

describe('MyModuleVuex', () => {
    let actions:any;
    let state:any;
    let store:any;
    let wrapper:Wrapper<Vue>;

    beforeEach(() => {
        actions = {
            moduleActionClick: jest.fn()
        };
        state = {
            clicks: 2
        };
        store = new Vuex.Store({
            modules: {
                myModule: {
                    state,
                    actions,
                    getters: myModule.getters
                }
            }
        });
        wrapper = shallowMount(MyModuleVuex, { store, localVue });
    });

    it('calls store action "moduleActionClick" when button is clicked', () => {
        wrapper.find('button').trigger('click');
        expect(actions.moduleActionClick).toHaveBeenCalled();
    })

    it('renders "state.clicks" in first p tag', () => {
        expect(wrapper.find('p').text()).toBe(state.clicks.toString());
    });
});
```

<br>

## Vuex Store 테스트하기
지금까지는 Vuex를 사용하는 컴포넌트에 대해서 알아보았다. 그렇기 때문에 실제 Vuex의 내부 동작과는 무관한 테스트였다. 이번에는 Vuex가 정확하게 동작하는지 여부를 테스트 하기 위한 방법이다.
Vuex Store 테스트는 두가지 방법이 있다. 첫번째는 getters, mutations, actions를 독립적으로 테스트 하는 것이다. 두번째는 실제 Vuex Store를 생성하는 방법이다.
아래는 테스트에서 사용할 mutations와 getters 코드이다.


```ts
export default {
    increment(state:any) {
        state.count++;
    }
}
```

```ts
export default {
    evenOrOdd(state:any) {
        return state.count % 2 === 0 ? 'even' : 'odd'
    }
}
```

<br>

#### getters, mutations, actions를 독립적으로 테스트하기
독립적으로 테스트 하는 경우에는 좀더 상세하게 테스트를 진행할수 있다. 테스트가 실패하더라도 어디에서 실패했는지 찾기가 쉽다. 단점으로는 commit, dispatch같은 Vuex의 함수들을 mock 해야할 필요가 있다. 이는 유닛 테스트는 성공할지라도 mock이 정확하지 않기 때문에 실제 production 코드는 실패할수도 있다.

```ts
import mutations from '../mutations';

test('"increment" increments "state.count" by 1', () => {
    const state = { count: 1 };
    mutations.increment(state);
    expect(state.count).toBe(2);
});
```

```ts
import getters from '../getters';

test('"evenOrOdd" returns even if "state.count" is even', () => {
    const state = { count: 2 };
    expect(getters.evenOrOdd(state)).toBe('even');
});

test('"evenOrOdd" returns odd if "state.count" is odd', () => {
    const state = { count: 1 };
    expect(getters.evenOrOdd(state)).toBe('odd');
});
```

<br>

#### store를 사용한 테스트
실제로 Vuex sotre를 사용한 방법이다. 이 테스트의 장점은 Vuex function들을 mock 할 필요가 없다는 것이다. 하지만 테스트 실패시에 어디에서 문제가 발생했는지 찾기가 어렵다.

```ts
import mutations from './mutations';
import getters from './getters';

export default {
    state: {
        count: 0
    },
    mutations,
    getters
}
```

```ts
import { createLocalVue } from '@vue/test-utils';
import Vuex from 'vuex';
import storeConfig from '../store-config';
import { cloneDeep } from 'lodash';

const localVue = createLocalVue();
let store:any;
localVue.use(Vuex);

beforeEach(() => {
    store = new Vuex.Store(cloneDeep(storeConfig));
});

test('increments "count" value when "increment" is committed', () => {
    expect(store.state.count).toBe(0);
    store.commit('increment');
    expect(store.state.count).toBe(1);
})

test('updates "evenOrOdd" getter when "increment" is committed', () => {
    expect(store.getters.evenOrOdd).toBe('even');
    store.commit('increment');
    expect(store.getters.evenOrOdd).toBe('odd');
})
```

해당 테스트에서는 cloneDeep를 사용하고 있는데 이는 각 테스트에 store를 클린하게 사용하기 위해서 이다.

그리고 생각해 보았을 때 상황에 맞추어서 하나씩만 적용 하던가 아니면 두개 모두를 적용해서 테스트 코드를 작성 하면 좋을거 같다.