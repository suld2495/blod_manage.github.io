---
title: 리스트(List)
date: 2019-12-19 00:51:14
tags: java
categories:
- java
- collection
---

## 1. 리스트의 메서드

### .retainAll(Collection)

List와 Collection에서 중복되는 값만 남기고 모두 삭제한다. 결과는 바로 List에 적용 된다

```
List list1 = new ArrayList();
List list2 = new ArrayList();

list1.add("아이언맨");
list1.add("헐크");

list2.add("아이언맨");
list2.add("윈터솔져");

list1.retainAll(list2);

System.out.println(list1);  // [아이언맨]
```

### .removeAll()

List와 Collection에서 중복되는 내용을 모두 삭제한다. 결과는 바로 List에 적용 된다

```
List list1 = new ArrayList();
List list2 = new ArrayList();

list1.add("아이언맨");
list1.add("헐크");

list2.add("아이언맨");
list2.add("윈터솔져");

list1.removeAll(list2);

System.out.println(list1);  // [헐크]
```

## 2. List와 Array 변환

### List => Array로 변환

```
List<String> list = new ArrayList<String>();

String[] array = list.toArray();
```

### Array => List로 변환

이건 배열 자체적인 기능은 제공하지 않아서 배열 util인 Arrays의 asList메서드를 활용한다

```
String[] array = new String[3];

List<String> list = Arrays.asList(array);
```