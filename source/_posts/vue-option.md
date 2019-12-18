---
title: 뷰 옵션(4)
date: 2019-12-18 21:56:55
tags: vue
category: 
- vue
- vue 기본문법
---

## 1. 인스턴스 생성시에 옵션

### 1) template

해당 하는 타겟에 HTML 요소를 삽입해줍니다.

#### 사용방법

> 루트 태그는 오직 하나만 존재하여야 합니다. 하위 태그는 상관 없습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>안녕하세요</p><button>클릭</button></div>'
    });
</script>
</body>
</html>
```

{% jsfiddle suld2495/2ge6ydkt result,html dark %}

---

### 2) data

뷰 인스턴스의 데이터 값을 설정할때 사용합니다.

#### 사용방법

```
데이터 바인딩 할때 사용이 가능합니다.
template 영역에도 {{}}를 사용해서 데이터 바인딩이 가능합니다.
데이터 타입은 number, string, object, array등 모두 가능합니다.
뷰 인스턴스의 메서드 속성에서 this.data키 로 접근 가능합니다.
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>{{message}}</p><button>{{obj.button}}</button></div>',
        data: {
            message : '안녕하세요',
            obj : {
                button: '클릭'
            }
        }
    });
</script>
</body>
</html>
```

{% jsfiddle suld2495/2ge6ydkt/1 result,html dark %}

---

### 3) methods

말그대로 메서드를 정의 할수있는 속성입니다. 맨 뒤에 s를 잊지 맙시다.

#### 사용방법
```
뷰 인스턴스의 각 메서드 내부에서 this.메서드이름으로 호출 가능합니다
{{}} 내부에서도 메서드를 호출 가능합니다. 이땐 this를 생략하고 메서드이름으로 호출 할수 있습니다.
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>{{message + "" + add()}}</p></div>',
        data: {
            message: '10 곱하기 10은 ',
            num: 10
        },
        methods: {
            add() {
                return this.num * this.num
            }
        }
    });
</script>
</body>
</html>
```

{% jsfiddle suld2495/2ge6ydkt/2 result,html dark %}

---

### 4) computed

복잡한 계산을 처리 한 후에 데이터를 출력해야 하는 경우에 사용할수 있습니다. 메서드를 호출할때랑 다르게 ()를 사용하지 않습니다.

#### 사용용도

데이터를 처리하는 로직을 실행한 후에 화면에 출력 해야 하는 경우 methods 대신에 computed를 사용하도록 합시다.
computed 내부에서 사용한 값의 변화가 발생 할때 재연산해서 결과를 다시 렌더링 해줍니다.

#### 장점

동일한 연산은 반복해서 처리하지 않고 미리 저장해 놓은 값을 불러옵니다.
여러번 호출 되어도 캐싱에 의해서 한번만 처리하기 때문에 속도 상승
methods는 호출 할때마다 실행되기 때문에 computed를 사용할수 있는 경우에는 computed를 사용합시다.

#### 주의 사항

computed 그 자체의 값은 data 처럼 변경이 불가능 하다
computed의 set 메서드를 활용하여 우회해서 값을 변경하는 것이 가능한데 이것 또한 직접 computed의 값을 변경하는게 아닌 computed와 연계되는 대상의 값을 변경하는 방식

```js
data() {
    num1 : 10,
    num2 : 20
},
computed: {
    get sum() {
        return this.num1 + this.num2;
    },
    set sum() {
        this.num2 = 30;
    }
}
```

메소드는 호출 한 횟수만큼 실행되는 것을 알수있다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>{{sum()}}</p><p>{{sum()}}</p></div>',
        data: {
            num1: 10,
            num2: 20
        },
        methods: {
            sum() {
                console.log("sum을 실행합니다.");

                return this.num1 + this.num2;
            }
        }
    });
</script>
</body>
</html>
```

![캡처](https://user-images.githubusercontent.com/42727909/55005557-8bbae380-501f-11e9-8090-88f19767fc23.PNG)


computed는 캐시 기능으로 값의 변화가 없으면 한번만 실행된다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>{{sum}}</p><p>{{sum}}</p></div>',
        data: {
            num1: 10,
            num2: 20
        },
        computed: {
            sum() {
                console.log("sum을 실행합니다.");

                return this.num1 + this.num2;
            }
        }
    });
</script>
</body>
</html>
```

![캡처](https://user-images.githubusercontent.com/42727909/55005692-cb81cb00-501f-11e9-82c1-ab0bcb59e969.PNG)

### 5) watch

computed와 유사한 기능을 제공하지만 데이터 호출과 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합합니다. 값이 변경이 되면 포착하여 해당 메서를 호출해 줍니다.
            
#### 사용방법

data 속성의 값을 메서드의 이름으로 작성하면 해당 data의 값이 변경이 되면 해당 메서드가 호출됩니다.   

#### 사용용도

사용자의 input 값 변경에 따른 로직을 처리할때 사용합니다.
DB등과 같은 비동기 처리 후에 해당 값이 변경 되었을 때 로직을 처리할때 사용합니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
<div id="app"></div>
    
<script src="https://unpkg.com/vue"></script>
<script>
    var app = new Vue({
        el: '#app',
        template: '<div><p>{{num2}}</p><button>{{setNum()}}</button></div>',
        data: {
            num1: 10,
            num2: 20
        },
        methods: {
            setNum() {
                this.num1 = 30;

                return this.num1;
            }
        },
        watch: {
            num1() {
                this.num2 = 40;
            }
        }
    });
</script>
</body>
</html>
```

{% jsfiddle suld2495/2ge6ydkt/4 result,html dark %}