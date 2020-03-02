---
title: JEST 시작
date: 2020-03-02 19:19:42
tags: [jest란, jest설치]
categories:
- javascript
- TDD
- jest
---

## JEST란
JEST는 페이스북에서 개발한 '테스트 프레임워크' 이다. JEST 이전에는 테스트 라이브러리를 조합해서 사용하였으나 JEST는 이름도 프레임워크인만큼 Test Runner와 Test Mathcher 그리고 Test Mock까지 모두 지원하고 있다.

<br>

## JEST 설치
아래의 명령어를 통해서 jest를 설치 할수 있다.

```js
npm i jest
```

<br>

## 첫 테스트 코드 작성
test.js라는 파일을 만들고 아래의 코드를 추가한다. JEST에서 테스트 코드 작성은 아래와 같다.

```js
test('스펙 정의', () => {
    // 테스트할 로직 구현
});
```


```js
test('문자열은 Hello이다.', () => {
    const str = 'Hello'
    expect('Hello').toBe(str);
});
```

<br>

## package.json에서 scripts 수정
package.json을 아래와 같이 수정한후에 npm run test라고 실행하면 테스트를 시작한다.


```js
{
    "scripts" : {
        "test": "jest"
    }
}
```

<br>

## describe와 it을 이용한 테스트 코드 작성
위에서는 test 함수를 통해서 테스트 코드를 작성하는 방법에 대해서 알아보았다. Jest에서는 이 뿐 아니라 describe와 it을 통해서도 작성 가능하다. describe은 일종의 테스트 그룹이고, it은 작은 단위의 테스트이다. it이 위의 test와 같은 단위이다.

```js
describe('', () => {
    it('', () => {
        // 테스트 코드 작성
    })
});
```