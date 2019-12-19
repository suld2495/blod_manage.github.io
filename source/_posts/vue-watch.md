---
title: watch의 사용
date: 2019-12-19 09:42:07
tags: [vue,computed]
categories:
- vue
- vue 기본문법
---

watch는 데이터에 대한 변화를 감지할 때 주로 사용하게 됩니다.

## 데이터가 Array나 Object인 경우

대상이 array나 Object인 경우에 문제가 발생합니다. object의 프로퍼티나 array의 자식 값의 변화에는 감지를 하지 못합니다. 그래서 set를 이용하여 중첩된 관계에 대한 반응성을 추가 할수 있습니다.
