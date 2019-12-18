---
title: CLI를 통한 프로젝트 생성(6)
date: 2019-12-19 00:16:35
tags: vue
category: 
- vue
- vue 기본문법
---

CLI는 .vue파일을 웹브라우저가 인식할수 있는 형태의 파일로 변환해주는 웹팩이나 브라우저리파이 같은 도구들이 적용된 초기 프로젝트 구조를 쉽게 구성하게 해주는 역할을 합니다.

### 설치 방법

```
npm install vue-cli -global
```

### 사용방법

1. 고급 웹팩 기능을 활용한 구성입니다.

```
vue init webpack
```

2. 웹팩의 최소 기능만을 구성합니다.

```
vue init webpack-simple
```

3. 고급 브라우저리파이 기능을 활용합니다.

```
vue init browserify	
```

4. 브라우저리아피의 최소기능만을 구성합니다.

```
vue init browserify-simple
```

5. 최소 뷰 기능만이 들어간 HTML 파일 1개를 생성합니다.

```
vue init simple
```

6. 웹팩 기반의 프로그래시브 웹앱기능을 지원합니다.

```
vue init pwa
```

#### 기타

고급기능들은 테스팅, 문법 검사등도 지원합니다.
CLI를 통해 설치 후에 `npm install`을 통해 뷰 애플리케이션 구동을 위한 라이브러리를 다운 받아야 합니다.
웹팩은 자체 서버(노드 서버)를 제공해 주기 때문에 npm run dev를 통해서 실행 가능합니다. 