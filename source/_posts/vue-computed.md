---
title: computed의 사용
date: 2019-12-19 09:35:26
tags: [vue,computed]
categories:
- vue
- vue 기본문법
---


## computed를 사용하면서 느낀 점

computed는 기본 데이터를 가공 처리 작업 후에 데이터 바인딩을 해주어야 하는 경우에 주로 사용하는 거 같습니다. 기본 데이터가 변경 되면 computed가 실행이 되어서 처리 로직을 실행 한 후에 결과를 변경후 렌더링합니다. 기존 데이터가 변경이 되는 걸 감지한다는 점에 있어서 watch와 비슷한 기능을 하는 것 처럼 보이지만 실제로는 watch를 사용하는 것보다는 computed를 사용하는게 더 적절한 경우가 많다고 생각합니다.

## computed VS watch

watch는 해당 데이터의 변화를 감지하면 메소드가 실행이 됩니다. computed도 내루 로직에서 사용 하는 데이터가 변경이 되면 감지하여 메소드가 실행이 됩니다. 이를 보면 두개의 속성이 비슷하게 사용되는 것 처럼 느낄수 있습니다. 둘 모두를 사용했을 때 같은 결과를 얻어 낼수 있다면 computed를 사용하는 것이 좋다고 생각합니다. watch를 사용 할 때는 주로 사용자의 입력등 비동기인 경우에 사용하는 것이 좋다고 생각합니다.

## computed VS methods

computed와 methods의 가장 큰 차이는 `캐싱`입니다. 두 속성 모두 사용자가 원하는 결과를 낼수 있지만 computed 같은 경우에는 종속 된 대상(실행부에서 사용한 인스턴스 데이터 등)의 값이 변화할 때만 다시 실행을 하게 됩니다. 그렇게 때문에 computed를 사용해도 되는 경우에는 methods 보다는 computed를 사용합시다.

## 팁

### computed는 필수적으로 return 받은 값으로 데이터 바인딩 합니다.

> computed 속성의 메서드에서 await를 썼더니 원하지 않는 결과 도출(당연하게도 await를 메서드에 설정하면 해당 메서드는 Promise를 return하기 때문)

### GET | SET

computed의 프로퍼티는 직접 수정으로 인한 값이 수정 되지 않는다

```js
export default {
    data() {
        return {
            num1: 10,
            num2: 20
        }
    },
    computed: {
        sum() {
            return num1 + num2
        }
    },
    methods: {
        updateSum() {
            this.sum = 100; // 이렇게 직접 값을 주어도 변경 되지 않는다 => 이런식의 코딩은 노노
        }
    }
}
```

> sum 앞에 아무런 키워드를 입력하지 않으면 get이 생략된 것으로써 값을 가져오는 기능을 한다
> sum 앞에 set 키워드를 넣어서 해당 sum을 동작하게 하는 데이터를 변경하는 식으로 코드 작성

```js
export default {
    data() {
        return {
            num1: 10,
            num2: 20
        }
    },
    computed: {
        get sum() {
            return num1 + num2
        },
        set sum() {
            this.num = 10   // sum을 직접 수정하는게 computed 메서드를 동작하게 하는 데이터를 변경하는 식으로 작성
        }
    }
}
```