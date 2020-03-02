---
title: JEST에서 사용하는 흔한 Matchers
date: 2020-03-02 19:41:30
tags: [matcher]
- javascript
- TDD
- jest
- matcher
---

# Matcher란
테스트 코드 작성시에 값들을 다양한 방법으로 테스트 할수 있도록 도와 준다. Jest를 실행 했을 때 실패한 테스트에 대한 에러 메시지를 출력 해 주기 위해서 실패한 matcher를 추적한다.

<br>

## 주로 사용이 되는 Matcher들

<br>

#### 가장 흔하게 사용이 되는 Matcher

###### 1. .toBe
`.toBe`는 값이 일치한지 테스트 할때 사용이 되는 Matcher이다.

```js
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

<br>

###### 2. .not.toBe
`.not.toBe`는 값이 일치하지 않는지 테스트 할때 사용이 되는 Matcher이다.

```js
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
});
```

<br>

###### 3. .toEqual
`.toEqual`은 일반적인 상황에서는 `.toBe`와 동일하게 사용이 된다. 하지만 객체를 테스트 할 때는 다르다. `.toBe`는 일반적으로 Object.is를 사용한 결과와 동일하다. 그렇기 때문에 객체의 값 뿐만아니라 참조또한 같아야 true를 반환한다. `.toEqual`은 객체의 값만을 비교한다.

```js
test('object assignment', () => {
    const data = {one: 1};
    data['two'] = 2;
    expect(data).toEqual({one: 1, two: 2});
});

test('object toBe', () => {
    const data = {one: 1};
    expect(data).toBe(data);     // 성공
    expect(data).toBe({one: 1}); // 실패
})
```

<br>

#### null Matcher
자바스크립트에서는 `null == undefined`이다. 하지만 Jest에서는 오직 null인 경우, 오직 undefined인 경우만 테스트가 가능하다.


```js
test('null', () => {
    const n = null;

    // 성공
    expect(n).toBeNull();
    expect(n).not.toBeTruthy();
    expect(n).toBeFalsy();
    expect(n).toBeDefined();
    expect(n).not.toBeUndefined();

    // 실패
    expect(undefined).toBeDefined(); // null은 되나 undefined는 안된다.
    expect(undefined).not.toBeUndefined(); // null은 되나 undefined는 안된다.
});
```

```js
test('zero', () => {
    const z = 0;
    expect(z).not.toBeNull();
    expect(z).toBeDefined();
    expect(z).not.toBeUndefined();
    expect(z).not.toBeTruthy();
    expect(z).toBeFalsy();
});
```

<br>

#### 숫자 Matcher
숫자 Matcher에서 주의를 해야 할 점은 실수를 비교할때이다. 실수에서는 `.toBe`가 아니라 `.toBeCloseTo`를 사용해야 정확한 테스트가 가능하다.


```js
test('two plus tow', () => {
    const value = 2 + 2;
    expect(value).toBeGreaterThan(3);
    expect(value).toBeGreaterThanOrEqual(3.5);
    expect(value).toBeLessThan(5);
    expect(value).toBeLessThanOrEqual(4.5);

    expect(value).toBe(4);
    expect(value).toEqual(4);
    expect("4").toEqual("4");
});
```

```js
test('adding floating point numbers', () => {
    const value = 0.1 + 0.2;
    // 성공
    expect(value).toBeCloseTo(0.3);

    //실패
    expect(value).toBe(0.3); // 실수를 비교할때는 toBe가 아닌 toBeCloseTo를 써라
})
```

#### 문자열 Matacher
동등 문자열 비교는 `.toBe`를 사용 하면 된다. 만약 정규식으로 문자를 비교하고 싶을때는 `.toMatch`를 사용하면 된다.

```js
test('hello is hello', () => {
    const hello = 'hello';
    expect(hello).toBe('hello');
});

test('there is no I in team', () => {
    expect('team').not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
    expect('Christoph').toMatch(/stop/);
});
```

<br>

#### 배열 Matcher
배열에 특정 값이 포함되었는지 여부를 확인 할때는 `.toContain`을 사용하면 된다.

```js
const shoppingList = [
    'diapers',
    'kleenex',
    'trash bags',
    'paper towels',
    'beer',
];

test('the shopping list has beer on it', () => {
    expect(shoppingList).toContain('beer');
    expect(new Set(shoppingList)).toContain('beer');
});
```

<br>

#### Exception Matcher
만약 특정 기능을 실행했을 때 예외가 발생하는 경우를 테스트 하고 싶을 때는 `.toThrow`를 사용하면 된다.

```js
function compileAndroidCode() {
    throw new Error('you ar using the wrong JDK');
}

test('compiling android goes as expected', () => {
    expect(compileAndroidCode).toThrow();
    expect(compileAndroidCode).toThrow(Error);

    expect(compileAndroidCode).toThrow('you ar using the wrong JDK');
    expect(compileAndroidCode).toThrow(/JDK/);
});
```