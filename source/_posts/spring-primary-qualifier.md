---
title: Primary, Qualifier 어노테이션
date: 2019-12-24 11:30:54
tags: [bean,primary,qualifier,autowired]
categories:
- spring
- IoC Container
---

##### 예제 소스 코드

```java

<!-- BookStore.class -->

public interface BookStore {
    
}

<!-- NobleBookStore.class -->

@Service
public class NobleBookStore implements BookStore {

}

<!-- EssayBookStore.class -->

@Service
public class EssayBookStore implements BookStore {

}
```

## 중복된 의존주입

동일한 타입의 Bean을 의존 주입 받으려고 하면 문제가 발생합니다(동일한 타입의 Bean 등록에는 문제가 없다).

```java
@Component
public class City {
    @Autowired
    BookStore bookStore;

    public BookStore getBookStore() {
        return bookStore;
    }
}
```

```
Error creating bean with name 'city': Unsatisfied dependency expressed through field 'bookStore'
```

동일한 타입의 Bean이 2개가 존재하기 때문에 위와 같은 에러가 발생합니다.

<br>

## 해결방법

##### @Primary 어노테이션 사용

우선적으로 의존 주입하고 싶은 class에 @Primary 어노테이션을 사용합니다.

```java
@Service
@Primary
public class NobleBookStore implements BookStore {

}
```

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(BeanConfig.class);
        System.out.println(((City)applicationContext.getBean("city")).getBookStore());

    }
}
```

```
kr.co.spring.NobleBookStore@16e7dcfd
```

<br>

##### @Qulifier 어노테이션 사용

@Autowired와 함께 사용하며 사용하고자 하는 빈의 아이디를 적어주면 됩니다(빈의 아이디를 따로 설정하지 않으면 default로 class 명에 첫글자가 소문자인 아이디로 설정된다).

```java
@Component
public class City {
    @Autowired
    @Qualifier("nobleBookStore")
    BookStore bookStore;
}
```

<br>

##### @Qulifier 어노테이션 사용하지 않기

기본적으로 @Autowried는 타입으로 의존주입을 해주지만 타입이 존재하지 않으면 Bean Name으로도 의존주입이 가능합니다. 그래서 굳이 @Qulifier를 사용하지 않아도 사용이 가능합니다. 

아래의 코드를 보면 이해가 가능합니다.

```java
@Component
public class City {
    @Autowired
    BookStore nobleBookStore;   // 기존에 bookStore 대신 nobleBookStore를 사용하과 있다.
}
```

<br>

##### 동일한 타입의 Bean 모두 의존주입 받기

동일한 타입의 Bean들을 모두 의존주입을 받을수가 있습니다.

```java
@Component
public class City {
    @Autowired
    List<BookStore> bookRepoitories;   
}
```