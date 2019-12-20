---
title: 스프링 테스트
date: 2019-12-20 17:43:23
tags: [junit,spring-test]
categories:
- java
- junit
---

## Spring-Test

스프링은 Junit을 이용하는 테스트 컨텍스트 프레임워크를 제공한다. 이를 이용하면 어노테이션을 이용하여 간편하게 컨텍스트를 사용할수 있다. 해당 방법이 없이 Bean을 사용하려면 직접 ApplicationContext를 생성하는 번거로운 작업을 진행하여야 하나 이로 인해 편하게 작업을 진행 할수 있다.

## @ContextConfiguration
