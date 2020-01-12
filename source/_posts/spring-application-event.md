---
title: 스프링 이벤트 처리
date: 2020-01-12 16:27:37
tags: [ApplicationEvent, 스프링이벤트]
categories:
- spring
- IoC Container
---

# 스프링 이벤트 처리

이벤트를 사용하는 이유는 비지니스 로직과 사이드 이벤트와의 결합도를 낮추기 위해서 사용됩니다. 사이드 로직이 구현된 클래스를 직접 호출하는게 아닌 이벤트 처리를 통해서 결합도를 낮추어 줍니다.

### 스프링 4.2 이전 버전

##### 이벤트 객체 생성

이벤트를 전달하기 위한 객체로써 ApplicationEvent를 상속해서 구현한다. 이벤트 발생시에 전달할 데이터 값을 해당 이벤트 객체에 주입해준다. 이벤트 객체는 Bean으로 등록하지 않는다.

```java
public class MyEvent extends ApplicationEvent {
    private MyDomain myDomain;

    public MyEvent(Object source, MyDomain myDomain) {
        super(source);
        this.myDomain = myDomain;
    }

    public MyDomain getMyDomain() {
        return myDomain;
    }
}
```

<br>

##### 이벤트 핸들러 생성

이벤트가 발생했을 때 호출될 메소드를 정의하는 클래스입니다. ApplicationListener을 구현하고 이벤트 발생시 onApplicationEvent메소드가 호출됩니다. 이벤트 핸들러는 Bean으로 등록 합니다.

```java
@Component
public class MyEventHandler implements ApplicationListener<MyEvent> {
    @Override
    public void onApplicationEvent(MyEvent myEvent) {
        System.out.println("이벤트 받았다. 데이터는 " + myEvent.getMyDomain());
    }
}
```

<br>

##### 이벤트 퍼블리싱

스프링에서 제공하는 ApplicationEventPublisher를 이용해서 이벤트를 퍼블리싱 할수 있습니다. ApplicationEventPublisher는 스프링에서 Bean으로 등록 되어 있어 @Autowired로 바로 사용할수 있습니다. publishEvent 메소드를 통해서 퍼블리싱 하면 등록 된 리스너 객체가 호출되게 됩니다.

```java
@Component
public class EventService {
    @Autowired
    private ApplicationEventPublisher publisher;

    public void run() {
        MyDomain myDomain = new MyDomain();
        publisher.publishEvent(new MyEvent(this, myDomain));
    }
}
```

<br>

### 스프링 4.2 이후 버전(4.2 포함)

##### 이벤트 객체 생성

4.2버전 부터는 이벤트 객체 생성시에 ApplicationEvent를 상속 받을 필요가 없습니다. 스프링 코드가 들어가지 않게 되면서 결합도를 낮춰줍니다.

```java
public class MyEvent {
    private MyDomain myDomain;
    private Object source;

    public MyEvent(Object source, MyDomain myDomain) {
        this.source = source;
        this.myDomain = myDomain;
    }

    public MyDomain getMyDomain() {
        return myDomain;
    }
}
```

<br>

##### 이벤트 핸들러 생성

이벤트 핸들러도 더 이상 ApplicationListener를 구현하지 않아도 됩니다. 이벤트 발생시 호출될 메소드에 @EventListener 어노테이션을 붙여 줍니다.

```java
@Component
public class MyEventHandler {

    @EventListener
    public void onApplicationEvent(MyEvent myEvent) {
        System.out.println("이벤트 받았다. 데이터는 " + myEvent.getMyDomain());
    }
}
```

### 결론

이벤트 객체와 이벤트 핸들러에서 스프링 관련 코드가 사라지면서 결합도가 낮아지게 되었습니다. 
이벤트 리스너가 2개 이상인 경우에는 모두 실행이 되게 됩니다. 이때 순서는 보장 되지 않습니다. 
- @Order을 사용해서 순서를 정할수있습니다.
- @Order 사용시에 숫자가 작을수록 먼저 실행이 됩니다.
- 비동기적으로 실행을 원하면 @Async와 함께 사용합니다. 이때 @Order를 사용하더라도 순서가 보장되지 않습니다.

### 스프링에서 제공되는 이벤트

1. ContextRefreshedEvent : 컨텍스트가 리프레시 될떄 발동
2. ContextClosedEvent: 컨텍스트가 종료될때 발동