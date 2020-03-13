---
title: 뷰 타입스크립트로 사용할때 테스트를 위한 설치 방법 
date: 2020-03-12 22:00:53
tags: [typescript-jest]
categories:
- javascript
- TDD
- jest
- typescript
---

## 타입스크립트를 사용할때 추가적으로 설치해야할 라이브러리
뷰를 jest를 통해서 테스트 하고자 할때 설치해야할 라이브러리 목록은 [here](https://suld2495.github.io/2020/03/06/vue-jest-error)를 통해서 확인이 가능하다.

1. ts-jest
2. @types/jest

## jest.config.js

```js
module.export = {
    // ...
    "moduleFileExtensions": [
      // ...
      "ts"
    ],
    "transform": {
      // ...
      "^.+\\.tsx?$": "ts-jest"
    },

    // jest는 기본적으로 js만을 테스트 파일로 찾기 때문에 .ts 파일도 테스트 파일로 찾을수 있도록 설정 해준다.
    "testRegex": "(/__tests__/.*|(\\.|/)(test|spec))\\.(jsx?|tsx?)$"
}
```

<br>

## tsconfig.json
types에 jest를 추가해준다.

```js
{
    "compilerOptions": {
        // ...
        "types": [
            // ...
            "jest"
        ],
    }
}
```