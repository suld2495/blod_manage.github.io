---
title: Scope 어노테이션
date: 2019-12-26 10:25:30
tags: [scope]
categories:
- spring
- IoC Container
---

##### 싱글톤

스프링에서 Scope에 대한 설정을 하지 않고 Bean 등록을 하게 되면 싱글톤으로 등록이 됩니다. 이 경우에는 하나의 bean을 사용하게 됩니다.

테스트용으로만 사용 할 Library 클래스입니다.
```java
@Component
public class Library {

}
```

동일한지 테스트 코드를 실행해 봅니다. 정상 실행이 되는 것을 확인할수 있습니다.
```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(BeanConfig.class);
        Library library1 = (Library) ctx.getBean("library");
        Library library2 = (Library) ctx.getBean("library");

        System.out.println(library1);
        System.out.println(library2);
        assertEquals(library1, library2);
    }
}
```

```
kr.co.spring.Library@6293abcc
kr.co.spring.Library@6293abcc
```

##### 프로토타입

프로토타입으로 설정을 하는 경우 사용할때 마다 새로운 객체를 받습니다. 사용하는 방법은 프로토타입으로 사용할 객체 위에 `@Scope("prototype")`를 추가해주면 됩니다.

```java
@Component
@Scope("prototype")
public class Library {

}
```

동일한 테스트 코드를 실행했을때 이번에는 두개의 객체가 서로 다르다를 에러가 발생하엿습니다.

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(BeanConfig.class);
        Library library1 = (Library) ctx.getBean("library");
        Library library2 = (Library) ctx.getBean("library");

        System.out.println(library1);
        System.out.println(library2);
        assertEquals(library1, library2);
    }
}
```

```
kr.co.spring.Library@2133814f
kr.co.spring.Library@4c15e7fd

java.lang.AssertionError: 
Expected :kr.co.spring.Library@2133814f
Actual   :kr.co.spring.Library@4c15e7fd
```

##### 싱글톤 타입에서 프로토 타입 의존주입

프로토타입 bean에서 싱글톤타입의 bean을 의존주입 받아서 사용할때는 아무런 문제가 발생하지 않습니다. 하지만 반대의 경우에는 개발자의 의도와는 다른 결과를 발생시킬수 있습니다.

```java
<!-- SingletonBean -->
@Component
public class SingletonBean {
    @Autowired
    PrototypeBean prototypeBean;

    public PrototypeBean getPrototypeBean() {
        return prototypeBean;
    }
}

<!-- PrototypeBean -->
@Component
@Scope("prototype")
public class PrototypeBean {

}
```

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(BeanConfig.class);
        SingletonBean singletonBean = (SingletonBean) ctx.getBean("singletonBean");

        System.out.println(singletonBean.getPrototypeBean());
        System.out.println(singletonBean.getPrototypeBean());
    }
}
```

```
kr.co.spring.PrototypeBean@37918c79
kr.co.spring.PrototypeBean@37918c79
```

위의 결과를 보았을때 프로토타입으로 Scope를 설정했지만 동일한 객체인것을 확인 할수 있습니다.

##### 해결방법1. 프록시객체를 의존주입하기

@Scope 어노테이션에 프록시 설정을 하여 해당 프로토타입 빈을 감싸는 프록시 객체를 반환하는 방식을 사용하여 해결할수 있습니다.

```java
@Component
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class PrototypeBean {

}
```

```
kr.co.spring.PrototypeBean@28eaa59a
kr.co.spring.PrototypeBean@3427b02d
```

##### 해결방법2. ObjectProvider 사용

프로토타입 bean을 의존주입 받을 때 ObjectProvider<타입>을 사용 할수 있습니다. 실제 bean을 사용할 때는 getIfAvailable메소드를 이용합니다.

```java
@Component
public class SingletonBean {
    @Autowired
    ObjectProvider<PrototypeBean> prototypeBean;

    public PrototypeBean getPrototypeBean() {
        return prototypeBean.getIfAvailable();
    }
}
```

```
kr.co.spring.PrototypeBean@222114ba
kr.co.spring.PrototypeBean@3d121db3
```

위의 방법의 경우에는 스프링 클래스를 사용하기 때문에 스프링에 의존이 되어 첫번째 방법을 추천.