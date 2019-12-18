---
title: 마이바티스에서 foreach 사용하기
date: 2019-12-19 00:48:16
tags: mybatis
category:
- mybatis
---

# foreach 확장

## 1. foreach에서 배열사용하기

기존에는 foreach에서는 List만 사용이 가능하다고 알고 있었는데 배열도 사용가능하다.

### List 사용방법

```
<!-- JAVA -->
List list = new ArrayList();
list.add("value1");
list.add("value2");

Map map = new Map();
map.put("list", list);


<!-- MYBATIS -->
<foreach collection="list" item="item">
    #{item}
</foreach>
```

### 배열 사용방법

```
<!-- JAVA -->
String[] array = {
    "value1",
    "value2"
};

Map map = new Map();
map.put("array", array);


<!-- MYBATIS -->
<foreach collection="array" item="item">
    #{item}
</foreach>
```

> 배열이나 리스트나 사용방법은 동일 하다

## 2. foreach에서 Map 사용하기

foreach에서 Map을 지원하는건 아니다. Map의 메서드들 중 하나인 entrySet을 이용하여 작성한다

```
<!-- JAVA -->
Map paramMap = new Map();
paramMap.put("id", "value");
paramMap.put("name", "value");

Map map = new Map();
map.put("paramMap", paramMap);


<!-- MYBATIS -->
<foreach collection="paramMap.entrySet()" index="key" item="value">
    #{value}
</foreach>
```



```
<!-- WHERE 절에서 사용 -->
Map paramMap = new Map();
paramMap.put("id", "value");
paramMap.put("name", "value");

SELECT *
  FROM TABLE
 WHERE 
<!-- WHERE 절에 사용 -->
<foreach collection="paramMap.entrySet()" index="key" item="value">
    ${key} = #{value}
</foreach>

<!-- UPDATE문에서 활용 -->
Map paramMap = new Map();
paramMap.put("id", "value");
paramMap.put("name", "value");

UPDATE TABLE
   SET
<foreach collection="paramMap.entrySet()" index="key" item="value">
    ${key} = #{value}
</foreach>   
```

> Map을 활용해서 where 조건을 걸때나 update시에 값 변경할때 동적으로 사용가능하다