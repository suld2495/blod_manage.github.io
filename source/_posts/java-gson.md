---
title: gson을 이용한 입출력
date: 2019-12-19 00:57:27
categories:
- java
- input / output
---

## 1. json 출력

```java
Map<String, Object> map = new HashMap<String, Object>();
map.put("id", "아이언맨");
map.put("name", "어벤저스");

Gson gson = new Gson();

String path = "C:\\test\\test1.json";

BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(path), "UTF-8"));
writer.write(gson.toJson(map, Map.class));
writer.close();
```

## 2. json 입력

```java
Gson gson = new Gson();

String path = "C:\\test\\test1.json";

BufferedReader reader = new BufferedReader(new InputStreamReader(new FileInputStream(path), "UTF-8"));

Map<String, Object> map = gson.fromJson(reader, Map.class);

System.out.println(map);
```

> json의 입력은 Gson을 사용하면 간편하게 처리할수 있다