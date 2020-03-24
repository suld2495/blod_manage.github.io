---
title: 전개 연산자의 사용
date: 2020-03-24 20:29:16
tags: [js-spread, rest]
categories:
- javascript
- ES6
---

## 전개 연산자
전개 연산자는 ES6에서 추가된 문법이다. 사용 방법은 `...값`으로 사용이다. 이 문법을 사용하면, 객체 혹은 배열을 펼칠수있다. 설명만으로는 제대로 이해가 가지 않기 때문에 아래의 예제를 기억하도록 하자.

<br>

#### 배열에서 전개연산자 사용

```js
const arr = [1, 2, 3];
console.log(...arr); // 1, 2, 3

const arr2 = [4, 5, 6];

arr.push(...arr2;)  
```

위의 예제에서 arr3를 만들려면 기존에는 아래와 같은 코드를 사용해야 했다.

```js
const arr = [1, 2, 3];
const arr2 = [4, 5, 6];

Array.prototype.push.apply(arr, arr2);
```

<br>

함수의 매개변수로도 전달이 가능하다

```js
const arr = [1, 2, 3];

function fn(i, j , k) {
    console.log(i, j, k);
}

fn(...arr);
```

<br>

#### 객체에서 사용하기

전개연산자를 통해서 아와 같이 새로운 객체를 생성할수 있다.이를 사용하면JSON.parse(JSON.stringify()))를 더이상 사용할 필요가 없어보인다.

```js
const obj = { name : 'Jone', age : 24 };
const obj2 = {...obj};

console.log(obj2);
```

아래와 같인 특정 key의 값만 가져올수가 있다. 이때 key가 정확히 일치 할때만 가져올수 있다.

```js
const obj = { name : 'Jone', age : 24 };
const { name } = obj;

console.log(name);
```

아래와 같이 작성 하면 Object.assign을 대체 할수 있다. 중복된 key를 가지고 있으면 뒤에 오는 객체의 value로 덮어 씌워진다.

```js
const obj = { name : 'Jone', age : 24};
const obj2 = { name : 'Jane' };
const obj3 = { school : 'high school', born : 'SEOUL' };

const obj4 = {...obj, ...obj2, ...obj3, born : 'ULSAN' };
```

<br>

#### 나머지 연산자

전개 연산자와 나머지 연산자는 다른 연산자 이지만 모양이 동일하다. 나머지 연산자는 앞에 오는 변수에 값을 할당 한 뒤 남은 모든 값을 하나의 배열로 변수에 저장해준다.
나머지 연산자가 중간에 올수 없으며, 나머지 연산자는 항상 하나만 사용이 가능하다.

```js
const [head, ...tail] = [1, 2, 3];

console.log(head);  // 1
console.log(tail);  // [2, 3]
```

함수의 파라미터로도 사용이 가능하다.

```js
function fn(a, b, ...c) {
    console.log(a)
    console.log(b)
    console.log(c)
}

fn(1, 2, 3, 4, 5, 6);
```
