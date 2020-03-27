---
title: Jest에서 Before, After 사용하기
date: 2020-03-26 21:41:18
tags: [jest-before,jest-after]
categories:
- javascript
- TDD
- jest
---

## before, after 사용하기

Jest에서는 before와 after를 활용해서 테스트 전후에 실행할 코드를 작성 할 수 있습니다.

<br>

## beforeEach와 afterEach

`beforeEach`는 테스트가 실행 되기전 `afterEach`는 테스트가 실행 된 후 실행이 됩니다. 단어 Each에서도 유추할수 있듯이 각각의 테스트가 실행되기 전후에 모두 실행이된다. 예를 들어 3개의 테스트가 있으면 beforeEach 또한 3번 호출된다.

```js
beforeEach(() => {
    console.log('테스트 전 실행 됨');
});

afterEach(() => {
    console.log('테스트 후 실행 돔');
})

for (; i < 6; i++) {
    test(`테스트${i}`, ((i) => (() => {
        console.log(`테스트${i}`);
    }))(i));
}
```

<br>

## 비동기 사용하기

before, after에서도 비동기를 사용할수 있다. [Jest 비동기](jest-async.md)를 통해서 Jest의 비동기 사용법을 알수 있다. 여기에서도 promise를 반환해주면 비동기를 사용할수 있다.

```js
function asyncFn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('비동기 사용');
            resolve();
        }, 500);
    });
}

beforeEach(() => {
    return asyncFn();
});
```

<br>

async, await를 통해서도 비동기를 사용할수 있다.

```js
beforeEach(async() => {
    await asyncFn();
})
```

<br>

## 오직 한번만 실행 하기

before와 after를 테스트 파일에서 오직 한번 만 사용하기 위해서는 `beforeAll`와 `afterAll`를 사용하면 된다. 

```js
beforeAll(() => {
    console.log('테스트 전 오직 한번만 실행된다.');
});

afterAll(() => {
    console.log('테스트 후 한번만 호출된다.');
});
```

<br>

## describe 내부에서의 before, after 사용하기

describe 내부에서 before와 after를 사용하는 경우에는 describe 내부의 테스트가 호출 될 경우에만 호출된다. 하지만 describe 외부의 before와 after는 describe 내부의 test를 포함한 테스트 파일 전체의 test가 호출 될때 호출이 된다.

<br>

## test.only 사용하기

테스트가 실패할때 가장 먼저 확인 해봐야 할것은 테스트 케이스가 오직 실패하는 케이스만 실행 했을때에도 실패하는지 이다.
이를 위해서 test.only를 사용하면 다른 테스트는 스킵하고 해당 테스트만 실행된다. 이때 스킵되는 테스트는 test.only가 존재하는 테스트 파일에 한해서이다.

```js
test.only('', () => {

});
```