---
title: 웹팩
date: 2019-12-20 10:26:25
tags: [vue,webpack-config]
categories:
- javascript
- webpack
---

# webpack.config.js

webpack.config.js 파일은 웹팩에 대한 기본적인 설정을 하는 파일입니다.

## 심플한 실제 파일 구성

```js
var path = require('path')	// output 속성에서 사용할 노드 path 라이브러리 저장
var webpack = require('webpack')	// 웹팩 플러그인에서 사용할 웹팩 라이브러리를 저장

// 아래부터가 실질적인 웹팩의 설정 내용
module.exports = {
    entry: './src/main.js',	// 웹팩으로 빌드할 파일 지정(해당 파일의 앱의 첫 시작 파일이 되는 것)
    output: {   // output은 build 후에 생성 될 파일들에 대한 설정
        path: path.resolve(__dirname, './dist'),	// 빌드후에 결과 파일 저장 될 디렉토리
        publicPath: '/dist/',		// 이미지 파일 같이 주소의 앞에 prefix로 붙여주어 주소에 접근 하도록 해주는 것
        filename : 'build.js'		// 빌드하고 난 후 결과 파일
    }
    module: {   // 지금까지는 loader를 사용하는 것 정도만 확인 했음
        rules: [
            {
                test: /\.vue$/,	// 로더가 적용 될 대상 파일 지정(정규식)
                loader: 'vue-loader' 	// loader는 사용할 로더 지정
            },
            {
                test: /\.js$/
                loader: 'babel-loader',
                exclude: /node_moules/  // 해당 디렉토리에서의 탐색은 제외하겠다
            }
        ]
    },
    resolve: {
        alias: {	// 해당 하는 경로를 @로 대신 사용 가능하다, 여러개 편의성있게 등록 가능하다
            '@': path.resolve(__dirname, 'src/')
        },
        extensions: ['*', '.js', '.vue', '.json']	// 해당하는 확장자들을 import하게 도와주는 역할
    },
    devtool: '#eval-source-map'	// 웹팩으로 빌드된 파일로 웹앱 구동시에 개발자 도구에서 사용할 디버깅 방식 지정
}
```       

## 웹팩 설정 구성에 사용 되는 용어 정리

### 1. entry

```
entry : 웹팩을 통해서 build를 할 대상 파일을 지정하는 속성
    - 해당 파일에는 전체 애플리케이션 로직과 필요한 라이브러리를 로딩하는 로직이 들어가게 됩니다.
```

### 2. output

```
output : 웹팩으로 빌드한 결과물의 위치와 파일이름 등 세부옵션을 설정하는 속성
```

### 3. loader

```
loader : 웹팩으로 빌드할때 html, css 파일등을 자바스크립트로 변환하기 위해 필요한 설정을 정의
```

### 4. plugin

```
plugin : 웹팩으로 빌드 하고 나온 결과물에 대해 추가기능을 제공하는 속성
    - 한마디로 필수옵션은 아니지만 빌드할때 혹은 결과물에 추가적인 기능을 적용하고 싶을 때 사용
```

### 5. resolve

```
resolve : 웹팩으로 빌드할때 해당 파일이 어떻게 해석되는지 정의하는 속성
    - 예) 라이브러리 로딩시 버전은 어떤걸로 하고, 경로는 어디로 하고 등에 관한 것
```
