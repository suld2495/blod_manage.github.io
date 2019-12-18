---
title: 뷰 시작하기(1)
date: 2019-12-18 21:18:37
tags: vue
category: 
- vue
- vue 기본문법
---

Vue를 구성하는 기본적인 코드이다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app">{{message}}</div>
<div id="div">{{message}}</div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message : '안녕하세요. vue의 세계에 오신걸 환영합니다.'
        }
    });
</script>
</body>
</html>
```

{% jsfiddle suld2495/zh0sn6c3 result,html dark %}

```
1. 먼저 new Vue()를 통해서 vue 인스턴스를 생성 할 수 있습니다.
2. vue 인스턴스를 생성시에 옵션을 줄수있습니다
    - el은 vue와 연결할 돔의 타겟을 설정 합니다.
    - data속성은 vue 인스턴스의 데이터를 설정 할수있습니다.
    - {{}}를 이용하여 vue 인스턴스와 HTML을 바인딩 하여 data속성 중 message의 값을 설정하였습니다.  
      그래서 타겟 영역 안에 있는 {{message}}는 '안녕하세요. vue의 세계에 오신걸 환영합니다.'로 바뀐것을 확인 할수 있습니다.
      하지만 타겟 영영 밖에 있는 곳은 데이터 바인딩 처리가 되지 않아 문자 그대로 {{message}}가 출력 된것을 확인 할수 있습니다.
```