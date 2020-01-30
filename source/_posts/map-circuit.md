---
title: Map 순회
date: 2020-01-30 10:42:04
tags: java
category:
- collection
---

## Map 순회

Map을 사용하다보면 Map에 저장된 모든 데이터를 순회해야할 경우가 발생합니다. 여러가지 방법 중에서 아래의 3가지에 대해서 알아보았습니다.

Map 순회 방법
1. entrySet()
1. keySet()
1. values()

<br>

#### entrySet 사용하기

entrySet은 key와 value 모두 필요한 경우에 사용하면 좋습니다. 반환값으로 Set을 반환해주기 때문에 for문을 사용해서 순회할수 있습니다.

```java
Map<String, Object> map = new HashMap();

for (Map.Entry<String, Object> entry: map.entrySet()) {
    String key = entry.getKey();
    Object value = entry.getValue();
}
```

<br>

#### keySet 사용하기

keySet은 key 값만 필요한 경우에 사용하면 좋습니다. 마찬가지로 반환값은 Set입니다.

```java
Map<String, Object> map = new HashMap();

for (String key : map.keySet()) {

}
```

<br>

#### values 사용하기

values는 value 값만 필요한 경우에 사용하면 좋습니다. 반환값은 Collection 입니다. Collection 또한 for문으로 순회할수 있습니다.

```
Map<String, Object> map = new HashMap();

for (Object value : map.values) {
    
}
```