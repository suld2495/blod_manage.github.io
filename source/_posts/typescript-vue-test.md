---
title: 타입스크립트로 vue 테스트 하기
date: 2020-03-13 16:58:50
tags: [typescript-test]
categories:
- javascript
- TDD
- jest
- typescript
---

## 데이터 테스트 하기
js에서는 데이터를 검증 할때 아래와 같이 작성한다.

```js
expect(wrapper.vm.message).toBe('message');
```

ts에서도 위와 같은 코드를 작성한 후에 테스트를 돌리니 아래와 같은 에러가 발생하였다. 

```js
TypeScript diagnostics (customize using `[jest-config].globals.ts-jest.diagnostics` option):
    src/components/__tests__/HelloWorld.test.ts:12:23 - error TS2339: Property 'message' does not exist on type 'CombinedVueInstance<Vue, object, object, object, Record<never, any>>'.
```

그래서 정상적으로 테스트 하기 위한 방법은 두가지 정도 인거 같다.

<br>

#### 1. vm.$data 사용하기
vm에는 data값을 가지고 있는 $data 객체가 존재한다. 해당 객체를 통해서 검증을 하면 가능하다.

```js
expect(wrapper.vm.$data.message).toBe('message');
```

<br>

#### 2. jest.config.js 수정하기
ts-jest를 사용하면 diagnostics옵션의 디폴트값이 true인데 위 코드 처럼 사용할 경우 에러를 발생시킨다. 이를 false로 설정하면 정상적으로 테스트가 가능하다.

```js
module.exports = {
    /// ...
    globals: {
      'ts-jest': {
        diagnostics: false
      }
    }
}
```