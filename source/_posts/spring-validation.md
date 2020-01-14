---
title: 스프링 벨리데이션
date: 2020-01-14 19:30:17
tags: [validation]
categories:
- spring
- IoC Container
---

## Validation 추상화

Validator 인터페이스는 어플리케이션에서 사용하는 객체 검증용 인터페이스 입니다. 어떠한 계층과도 관계가 없이 사용할수 있습니다. 주로 웹에서 많이 사용되지만 서비스, 데이터 어디에서도 사용해도 좋습니다.

#### 구현해야 하는 메소드

Validator을 구현하는 클래스는 두개의 메소드를 오버라이딩 해야 합니다.

##### 1. supports

객체를 검증 할 때 Validator가 검증 할수 있는 클래스인지를 판단하는 로직을 구현하는 메소드입니다. 반환값이 true 이면 검증할수 있다고 판단합니다.

##### 2. validate

실제 검증 로직이 이루어지는 메소드입니다.

<br>

##### Validator 클래스 예제

```java
public class EventValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "not empty", "Title must not be null");
    }
}

public class Event {
    private String title;
    private String name;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
<br>

##### ValidationUtils를 사용해서 검증 로직 구현

스프링에서 제공하는 검증 유틸인 ValidationUtils를 사용해서 검증을 할수 있습니다.

아래 코드는 값이 null 이거나 빈값이거나 길이값이 0인 경우 에러로 처리해줍니다.
```java
ValidationUtils.rejectIfEmptyOrWhitespace(Errors인스턴스, 필드명, 에러코드, 에러발생시 출력할 메시지)
```

<br>

##### ValidationUtils를 사용하지 않고 검증 로직 직접 구현

```java
public class EventValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        if (target == null || "".equals(target.getTitle())) {
            errors.reject(필드명, 에러코드명, 에러발생시 출력할 메시지);
        }
    }
}
```

<br>

##### 테스트 코드

```java
public class ValidationTest {

    @Test
    public void testValidation() {
        Event event = new Event();                              // 타겟 객체
        EventValidator eventValidator = new EventValidator();   // 검증 Validator

        // BeanPropertyBindingResult는 Erros와 BindingResult의 구현체로써 보통은 웹에서는 MVC가 해당 객체를 생성하기 때문에 직접 생성할 일은 적다.
        Errors errors = new BeanPropertyBindingResult(event, "event");  

        eventValidator.validate(event, errors);                 // 타겟 객체를 검증

        for (ObjectError error : errors.getAllErrors()) {       // 타겟 객체에서 Validation을 통과 못한 모든 에러를 가져옴(errors.getAllErrors)
            System.out.println("=== error code ===");
            System.out.println(Arrays.toString(error.getCodes()));
            System.out.println(error.getDefaultMessage());
        }
    }
}

class EventValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        Event event = (Event) target;

        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "not empty", "Title must not be null");

        if (event.getName() == null || "".equals(event.getName())) {
            errors.rejectValue("name", "not empty", "Name must not be null");
        }
    }
}
```

<br>

### 어노테이션을 이용한 검증

위의 방식은 복잡한 검증 로직을 구현할 때는 사용하나 빈값체크, Max, Min 값 체크 등 간단한 Validation은 어노테이션을 통해서 검증을 할수 있습니다.

##### 타겟 객체

```java
public class Event {

    Integer id;

    @NotEmpty
    String title;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}
```

###### 테스트 코드 

직접 Validator를 구현하지 않고 어노테이션을 사용해서 간편한 검증 작업을 진행 할수 있습니다.

```java
public class ValidationTest {

    @Test
    public void testValidation() {
        Event event = new Event();
        event.setEmail("aaa2");

        Errors errors = new BeanPropertyBindingResult(event, "event");  

        eventValidator.validate(event, errors);                 

        for (ObjectError error : errors.getAllErrors()) {       
            System.out.println("=== error code ===");
            System.out.println(Arrays.toString(error.getCodes()));
            System.out.println(error.getDefaultMessage());
        }
    }
}
```

##### pom.xml

Validation 어노테이션을 사용하기 위해서는 아래의 두개의 dependency를 추가해 주어야 합니다.

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.1.0.Final</version>
</dependency>

<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>4.3.11.Final</version>
</dependency>
```