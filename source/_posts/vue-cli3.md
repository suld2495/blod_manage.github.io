---
title: CLI3를 통한 프로젝트 생성(8)
date: 2019-12-19 09:27:35
tags: [vue,cli3]
category: 
- vue
- vue 기본문법
---


### 1. vue-cli 설치

```
npm install -g vue-cli
```

### 2. vue-cli3 설치(최신 버전으로 설치) 

```
npm i -g @vue/cli
```

### 3. vue-cli3를 통한 프로젝트 생성

```
vue create 프로젝트명
```

### 4. 프로젝트 서버 시작

```
npm run serve
```

### 5. eslint 끄기

1. package.json와 동일한 depth에 vue.config.js 만들기
2. 아래 코드 넣기
```
module.exports = {
    lintOnSave: false
}    
```

### 6. cli 2.x vs cli 3.x


1. 명령어
    - 2.x : vue init 프로젝트이름 파일위치
    - 3.x : vue create 프로젝트이름
    
2. 웹팩 설정파일
    - 2.x : 노출되어 있다 => 웹팩 설정 가능
    - 3.x : 노출되어 있지 않다 => 웹팩 설정을 위한 다른 방법을 학습하여야 함

3. 프로젝트 구성
    - 2.x : 깃헙의 템플릿을 다운로드 후 사용하게 됨
    - 3.x : 플러그인 기반으로 기능 추가
    
4. ES6 이해도
    - 2.x : 필요없다
    - 3.x : 필요하다
