---
title: Map 타입
date: 2019-12-19 01:05:48
tags: [javascript,Map]
categories:
- javascript
- es6
---

## 1. Map 생성하기

```js
let map = new Map();
let map = new Map([
    ["key1", "value1"],
    ["key2", "value2"],
    ["key3", "value3"],
    ["key4", "value4"],
])
```

> `new Map([{id: 1},{id: 2},{id: 3},{id: 4}])`이런식으로 작성하려고 하니까 안됨. Map생성시 파라미터는 이차원배열로 넣어야 할듯

## 2. Map에 값 추가하기

```js
let map = new Map();
map.set('key', 'value')
```

> 값을 추가하는 부분에서 Map와 Object의 차이점이 나타남. Map의 경우에는 **Key로 문자열이외에도 모든걸 Key로 가능**

## 3. Map의 값 호출하기

```js
let key = {name: 'name'}
let map = new Map();
map.set(key, 'value')
map.get(key)
```

## 4. Map의 사이즈 확인

```js
let key = {name: 'name'}
let map = new Map();
map.set(key, 'value')

console.log(map.size)
```

## 5. 배열로 만들기

```js
let map = new Map();
map.set('key1', 'value1')
map.set('key2', 'value2')

let array = [...map]
console.log(map)    // map: [['key1', 'value1'], ['key2', 'value2']]
```

> Object와는 다르게 Map은 `[...map]`을 통해서 Array로 만들수 있습니다. 결과값은 `[[key, value], [key, value]]`의 형태로 만들어 진다.

## 6. 순회하기

```js
let map = Map()

for (let [key, value] of map) {

}

map.forEach((key, value) => {
    
})
```

> Object와는 다르게 forof와 forEach를 사용할수 있다.
> Array에서 제공하는 map, filter등에 대한 메서드는 제공하지 않는 것으로 판단된다.

## 7. Iterator 사용하기

```js
let map = new Map()

map.keys()      // key모음
map.values()    // value모음
map.entries()   // key와 value 한쌍의 모음
```

> Iterator이다 보니 for문과 함께 사용하면 좋을 듯

## 기타 메서드

### has : 해당 Key 존재 여부

```js
let map = new Map()
map.set('key', 'value')

console.log(map.has('key')) // true
```

### delete : 해당 Key의 값 삭제
```js
let map = new Map()
map.set('key', 'value')
console.log(map.delete('key'))  // true => 존재했으면 true 없었으면 false

console.log(map.has('key')) // false
```

### clear : 모든 데이터 삭제
```js
let map = new Map()
map.clear()
```