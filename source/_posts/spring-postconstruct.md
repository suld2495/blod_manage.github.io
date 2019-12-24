---
title: PostConstruct 어노테이션
date: 2019-12-24 11:53:23
tags: [postConstruct,라이프사이클,initializingBean]
categories:
- spring
- IoC Container
---

IoC 컨테이너는 Bean 라이프 사이클을 관리하여 주고 특정한 시점에 Bean에게 이를 알려줄수 있는 메커니즘을 제공합니다. 스프링에서는 주로 init와 destroy 이벤트를 제공합니다.

##### InitializingBean와 DisposableBean 인터페이스

InitializingBean 인터페이스를 활용해서 Bean의 초기화 작업을 진행할수 있습니다. 해당 인터페이스에서 제공하는 afterPropertiesSet메소드를 구현하면 Spring이 Bean의 생성 후에 해당 메소드를 호출해 줍니다.

```java
@Component
public class Book implements InitializingBean {
    @Autowired
    BeanFactory beanFactory;

    public void afterPropertiesSet() throws Exception {
        System.out.println("Bean이 등록되었습니다.");
        System.out.println(beanFactory.getBean("book"));
    }
}

```

```
Bean이 등록되었습니다.
kr.co.spring.Book@3745e5c6
```

DisposableBean 인터페이스에서 제공하는 destroy메소드를 구현하면 Bean의 소멸 직전에 해당 메소드를 호출해 줍니다.

```java
@Component
public class Book implements DisposableBean {

    @Autowired
    BeanFactory beanFactory;

    public void destroy() throws Exception {
        System.out.println("Bean이 삭제됩니다..");
        System.out.println(beanFactory.getBean("book"));
    }
}
```

<br>

##### xml 빈 등록시 사용자 라이프사이클 메소드 정의

xml에서 빈을 수동으로 등록시에 위의 InitializingBean, DisposableBean을 사용하지 않고 사용자 메소드를 정의 가능합니다.

```xml
<bean id="book" class="kr.co.spring.Book" init-method="initBook" destroy-method="destroyBook"/>
```

```java
public class Book {
    public void initBook() {
        System.out.println("빈이 등록되었습니다.");
    }

    public void destoryBook() {
        System.out.println("빈이 삭제되었습니다.");
    }
}
```

<br>

##### @PostConstruct와 @PreDestroy

좀 더 간편하게 어노테이션을 이용해서 위와 동일하게 사용할수 있습니다.

@PostConstruct는 Bean이 생성 된 후에 호출 될 메소드 위에 사용합니다.

```java
@Component
public class Book {
    @Autowired
    BeanFactory beanFactory;

    @PostConstruct
    public void init() throws Exception {
        System.out.println("Bean이 등록되었습니다.");
        System.out.println(beanFactory.getBean("book"));
    }
}
```

```
Bean이 등록되었습니다.
kr.co.spring.Book@644baf4a
```

@PreDestroy는 Bean이 소멸 직전에 호출 될 메소드 위에 사용합니다.


```java
@Component
public class Book {

    @PreDestroy
    public void destroy() throws Exception {
        System.out.println("빈이 삭제되었습니다.");
    }
}
```

<br>

##### 기타

ApplicationContext 클래스도 Bean으로 등록되어 있기 때문에 의존주입 받을수 있습니다. 이 뿐 아니라 ApplicationContext가 상속하고 있는 BeanFactory, ResourceLoader 등도 Bean으로 등록되어 있기 때문에 ApplicationContext를 의존주입 받지 않고 필요한 기능을 가진 Bean을 의존주입해서 사용하는 것이 가시성에 좋습니다(ex : Bean과 관련된 처리를 할때는 BeanFactory사용).