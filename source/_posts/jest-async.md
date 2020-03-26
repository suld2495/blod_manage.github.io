---
title: Jest로 비동기 테스트 하기
date: 2020-03-26 21:18:04
tags: [jest-async]
categories:
- javascript
- TDD
- jest
---

## 비동기에서 사용할 함수

```js
function fetchData(check) {
    return new Promise((resolve, reject) => {
        typeof check === 'function' ? check() : check ? resolve('peanut butter') : reject('error');
    });
}
```

## 콜백을 이용한 비동기 테스트 해보기

일반적인 상황에서 비동기를 사용하는 보편적인 방법 중 하나는 콜백을 사용하는 것이다. 하지만 Jest에서 테스트 할때는 의도대로 작동 하지 않는다.
Jest 테스트는 콜백을 호출할때까지 기다려주지 않는다. 테스트 함수 내에서 마지막 코드까지 실행을 하게되면 테스트를 완료하게 된다. 그래서 항상 테스트가 통과하게 된다. 테스트 실패하도록 값을 변경하여도 통과하는 것을 확인 할수있다.

```js
test('비동기를 콜백 활용하기 - 실패', () => {
    function callback(data) {
        expect(data).toBe('peanut butter');
    }

    fetchData(callback);
});
```

<br>

## 파라미터 done을 활용한 비동기 테스트

done을 파라미터로 받은 경우에는 Jest에서는 done 함수가 호출 될때까지 테스트를 완료하지 않고 기다린다. 

```js
test('done을 활용하기', (done) => {
    function callback(data) {
        try {
            expect(data).toBe('peanut butter');
            done();
        } catch (e) {
            done(error);
        }
    }

    fetchData(callback);
});
```

<br>

## Promise 활용 하기

비동기로 promise를 사용하는 경우에는 훨씐 간편하게 작성 할수 있다. promise를 리턴해주면 테스트가 정상적으로 작동하는 것을 확인할수 있다. 이때 promise를 리턴하지 않으면 테스트를 수행하기 전에 끝난다.

```js
test('프로미스 활용하기', () => {
    return fetchData(true).then((data) => {
        expect(data).toBe('peanut butter');
    });
})
```

<br>

## Promise Catch 테스트 하기

promise의 reject 또한 테스트 할수 있다. 이경우에는 then 대신에 catch를 활용한다. catch 내부에서 테스트 할 코드를 작성한다. 이때 `expect.assertions(에러 발생한 횟수)`를 테스트 위에 적어주지 않으면 테스트는 정상적으로 실행되지 못하고 완료되어 버린다.

```js
test('프로미스 catch 테스트하기', () => {
    expect.assertions(1);
    return fetchData(false).catch((e) => expect(e).toMatch('error'));
})
```

<br>

## Matchers인 resolves를 활용하기

promise then 대신에 Jest에서 제공하는 Matchers 중에 하나인 resolves를 활용할수도 있다. expect에 promise를 넘겨준다. 이때 여전히 return을 사용하여야 한다.

```js
test('resolves 사용하기', () => {
    return expect(fetchData(true)).resolves.toBe('peanut butter');
});
```

<br>

## Matchers인 rejects를 활용하기

promise catch 대신에 Jest에서 제공하는 Matchers 중에 하나인 rejects를 활용 할수도 있다. 사용 방법은 resolves와 유사하다. 이때 promise catch와 async와는 달리 `expect.assertions`를 사용하지 않아도 된다.

```js
test('rejects 활용하기', () => {
    return expect(fetchData(false)).rejects.toBe('error');
});
```

<br>

## async와 await를 활용하기

es6에서 제공하는 비동기 방법인 async와 await를 활용해서도 가능하다.

```js
test('async와 await를 활용하기', async() => {
    let data = await fetchData(true);
    expect(data).toBe('peanut butter');
});

test('async와 await를 활용하기', async() => {
    expect.assertions(1);

    try {
        await fetchData(false);
    } catch (e) {
        expect(e).toMatch('error');
    }
});

```