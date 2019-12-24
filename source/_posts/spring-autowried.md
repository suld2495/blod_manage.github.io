---
title: Autowried 어노테이션 사용
date: 2019-12-24 10:44:43
tags: [bean,autowired]
categories:
- spring
- IoC Container
---

@Autowried는 Bean으로 등록된 객체를 자동으로 삽입해주는 역할을 합니다. 생성자, 필드, set메소드를 활용 하여 적용이 가능합니다.

##### 예제 Class 등록

해당 포스팅 시에 사용 할 Book 클래스입니다. 아래에서 사용 될 Book 클래스는 아래 소스 코드를 사용하게 됩니다.

```java
package kr.co.spring;

import org.springframework.stereotype.Component;

@Component
public class Book {

}
```

<br>

##### 생성자를 이용한 방법

생성자 메소드 위에 __@Autowired__만 등록 해서 사용 하면 됩니다.

```java
@Component
class BookStore {
    private Book book;

    @Autowired
    public BookStore(Book book) {
        this.book = book;
    }
}
```

{% alert info %}
생성자에서 @Autowired는 생략이 가능합니다.
{% endalert %}


<br>

##### setter를 이용한 방법

setter 메소드 위에 __@Autowired__만 등록 해서 사용 하면 됩니다. 생성자와는 달리 __@Autowired__를 붙여주지 않으면 의존주입이 되지 않습니다.

```java
@Component
class BookStore {
    private Book book;

    @Autowired
    public setBook(Book book) {
        this.book = book;
    }
}
```

setter를 활용하면 Bean을 필수적으로 의존주입 받을지 여부를 선택할수 있습니다. 옵션을 주지 않는 경우 의존주입 받을 Bean이 없는 경우 에러가 발생합니다. 이때 `@Autowired(required = false)`를 사용해서 만약 Bean이 존재하지 않는 경우 의존주입을 받지 않도록 설정이 가능합니다.

{% alert info %}
setter 메소드 이름은 상관이 없습니다.
{% endalert %}

<br>

##### 필드에 직접 사용

필드 위에 __@Autowired__를 등록해서 사용이 가능합니다. 가장 심플하게 코드를 작성할수 있습니다. setter와 마찬가지로 required 옵션을 사용가능합니다.

```java
@Component
class BookStore {
    @Autowired
    private Book book;
}
```

##### 외부 라이브러리를 Bean 등록하고 의존주입

수정이 불가능한 외부라이브러리를 Bean으로 등록하고 해당 Bean에 의존주입까지 해주어야 하는 경우가 있습니다.

```java
<bean id="library" class="kr.co.spring.Library">
    <constructor-arg value="라이브러리"/>
    <property name="location" value="서울"/>
</bean>
```


```java
public class Library {
    private String name;
    private String location;

    public Library(String name) {
        this.name = name;
    }

    public void setLocation(String location) {
        this.location = location;
    }

    @Override
    public String toString() {
        return "Library{" +
                "name='" + name + '\'' +
                ", location='" + location + '\'' +
                '}';
    }
}
```

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring/spring-bean.xml");
        Library library = (Library) applicationContext.getBean("library");
        System.out.println(library.toString());
    }
}
```

```
Library{name='라이브러리', location='서울'}
```

만약 생성자에서 파라미터를 여러개 받는 경우에는 index를 이용하면 됩니다.

```xml

<bean id="library" class="kr.co.spring.Library">
    <constructor-arg index="0" value="1"/>
    <constructor-arg index="1" value="라이브러리"/>

    <property name="location" value="서울"/>
</bean>
```

type 속성을 이용해서도 사용 가능합니다.

```xml
<bean id="library" class="kr.co.spring.Library">
    <constructor-arg value="1" type="int"/>
    <constructor-arg value="라이브러리" type="java.lang.String"/>

    <property name="location" value="서울"/>
</bean>
```

{% alert info %}
예제에서는 value를 사용하였지만 bean을 의존주입 받을 때는 `ref`를 사용하면 됩니다.
{% endalert %}