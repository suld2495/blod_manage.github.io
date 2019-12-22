---
title: IoC 컨테이너와 빈
date: 2019-12-21 15:57:18
tags: [bean,autowired]
categories:
- spring
- IoC Container
---

## IoC란

Ioc란 Inversion of Control의 약자로써, 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라 주입받아서 사용하는 방법을 일컫는 말입니다.

## 스프링 IoC 컨테이너

컨테이너는 보통 인스턴스의 생명주기를 관리하며, 생성된 인스턴스에게 추가적인 기능을 제공하는 것입니다. 스프링 컨테이너는 어플리케이션 컴포넌트의 중앙 저장소와 같은 역할을 하며 빈 설정 소스로부터 빈 정의를 읽어 들여 빈을 구성 하고 제공하는 역할을 합니다. 

> IoC 컨테이너의 핵심적인 역할을 하는 인터페이스가 BeanFactory이다.

## IoC 컨테이너의 장점

1. 의존주입을 통한 빈 관리가 용이하다.
1. 싱글톤 사용을 할수 있다.
1. 라이프사이클 인터페이스를 제공해준다.
    - @PostConstruct : 메소드 위에 사용하면 빈이 생성될때 실행된다(ApplicationContext에서 제공).  
1. 의존주입 방법을 통해서 테스트 하기 쉬운 코드를 작성할수 있다.
    - Mock 객체를 생성해서 의존성 주입을 할수 있어 테스트 하기에 용의하다.

## Bean

스프링 IoC 컨테이너가 관리하는 객치입니다.

## Bean 등록방법

##### 1. XML을 이용한 방법

bean이라는 태그를 이용해서 Bean 등록을 할수 있다.

```xml
<bean id="인스턴스명" class="클래스경로"/>
```

###### 옵션

1. id : 보통 카멜케이스로 작성을 하며 사용할 인스턴스명을 입력해주면 된다.
2. class : Bean 등록 하려는 클래스의 경로를 입력해주면 된다.
3. scope : 말그대로 스코프를 설정
    - 기본값은 싱글톤
    - 매번 다른 값을 사용하려면 prototype으로 설정
    - 이외에도 request, session 등이 존재
4. autowire

###### 의존주입

property 태그를 이용해서 의존주입 관계를 형성 할수 있습니다. `name`은 의존주입할 set메소드 이름이고, `ref`는 Bean의 id입니다.

```xml
<bean id="book" class="kr.co.Book"/>
<bean id="store" class="kr.co.Store">
    <property name="set메소드이름" ref="의존주입할 Bean의 id"/>
</bean>
```

```java
class Store {
    private Book book;

    public void setBookTest(Book book) {    // property의 name으로 bookTest를 입력해주어야 함
        this.book = book;
    }
}
```

{% alert warning %}
property의 name의 값으로 입력해야 하는 것은 set메소드의 이름이다. set메소드의 인자의 이름이 아니다. __위의 코드로 예를 들면 bookTest를 입력해주어야 한다__.
{% endalert %}

##### 예제 코드

{% tabbed_codeblock %}
    <!-- tab java -->
        <!-- Test.class -->

        public class Test {
            public void test() {
                System.out.println("너는 안녕?");
            }
        }

        <!-- TestController.class -->

        public class TestController {
            private Test test;


            public void test() {
                System.out.println("안녕하세요");
                test.test();
            }

            public void setTest(Test test) {
                this.test = test;
            }
        }
    <!-- endtab -->  
    <!-- tab xml -->
        <!-- spring-bean.xml -->

        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
            http://www.springframework.org/schema/beans 
            http://www.springframework.org/schema/beans/spring-beans.xsd
        ">

            <bean id="test" class="kr.co.spring.Test"></bean>

            <bean id="testController" class="kr.co.spring.TestController">
                <property name="test" ref="test"/>
            </bean>
        </beans>
    <!-- endtab -->
{% endtabbed_codeblock %}

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = 
                new ClassPathXmlApplicationContext("spring/spring-bean.xml");

        TestController testController = 
                (TestController) applicationContext.getBean("testController");

        testController.test();
    }
}
```