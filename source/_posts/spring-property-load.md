---
title: 프로퍼티 파일 가져오기
date: 2020-01-14 21:26:41
tags: [properties]
categories:
- spring
---

## 프로퍼티 파일 가져오기

##### 스프링을 이용해서 프러퍼티 가져오기

스프링에서 제공하는 Resource와 PropertySource를 통해서 쉽게 프로퍼티 파일을 가져 올수 있습니다.

```java
public class PropertiesTest {
    
    @Test
    public void getProperties() {
        Resource resource = new ClassPathResource("클래스패스 경로");   // prefix로 classpath:를 붙이지 않는다.
        PropertySource propertySource = new ResourcePropertySource(resource);

        propertySource.getProperty("");
    }
}
```