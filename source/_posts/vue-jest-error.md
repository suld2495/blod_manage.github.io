---
title: Vue Jest 에러
date: 2020-03-06 20:06:39
tags: [jest에러]
categories:
- javascript
- TDD
- jest
---

## 파일 import 불가
Jest에서 Import를 사용 하는 경우 아래와 같이 에러가 발생한다.

```js
Test suite failed to run

Jest encountered an unexpected token

This usually means that you are trying to import a file which Jest cannot parse, e.g. it's not plain JavaScript.

By default, if Jest sees a Babel config, it will use that to transform your files, ignoring "node_modules".

Here's what you can do:
    • To have some of your "node_modules" files transformed, you can specify a custom "transformIgnorePatterns" in your config.
    • If you need a custom transformation specify a "transform" option in your config.
    • If you simply want to mock your non-JS modules (e.g. binary assets) you can stub them out with the "moduleNameMapper" config option.        

You'll find more details and examples of these config options in the docs:
https://jestjs.io/docs/en/configuration.html

Details:

D:\GIT 소스코드 저장소\TDD\JEST\vue-jest\vue-jest-start\src\test\helloworld.test.js:1
({"Object.<anonymous>":function(module,exports,require,__dirname,__filename,global,jest){import { mount } from '@vue/test-utils';
```

<br>

#### 1. 해당 라이브러리가 정상적으로 설치 되어 있는지 확인한다.

1) jest
2) vue-jest
3) @babel/core
4) @babel/preset-env
5) babel-jest
6) @vue/test-utils
7) babel-core@^7.0.0-bridge.0 (바벨이 7.0 이상 사용중일때)

<br>

#### 2. jest.config.js
package.json 또는 jest.config.js에 아래와 같이 입력 한다.

```js
module.export = {
    "moduleFileExtensions": [
      "js",
      "json",
      "vue"
    ],
    "transform": {
      "^.+\\.js$": "babel-jest",
      "^.+\\.vue$": "vue-jest"
    }
}
```

#### 3. babel.config.js
package.json 또는 babel.config.js에 아래와 같이 입력 한다.

```js
module.exports = {
    presets: [
        [
        '@babel/preset-env',
        {
            targets: {
            node: 'current',
            },
        },
        ],
    ],
};
```

#### 정리
구글링 결과 Jest에서 Import를 사용하기 위해서는 Babel을 설치해야 하는 듯하다. 
그리고 babel 7 이상을 사용 한다면 `babel-core@^7.0.0-bridge.0`를 설치해 주어야 한다.
