---
title: 라우터 트랜지션
date: 2019-12-20 09:17:04
tags: [vue,transition]
categories:
- vue
- vue 라우터
---

# 라우터 트랜지션

화면 전환시에 좀더 부드럽게 이동하도록 해준다. 실제로 화면전환시에 애니메이션을 넣어주는 건 아니고 화면전환시마다 적절한 class를 부여해주는데
해당 class에 css로 직접 원하는 애니메이션을 작성하면 된다.

```vue
<transition>
    <router-view></router-view>
</transition>
```

1. router-view 태그를 transition 태그로 감싼다.
2. transition은 라우터 트랜지션 이외에도 사용가능하다.