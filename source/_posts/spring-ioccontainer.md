---
title: IoC 컨테이너와 빈
date: 2019-12-21 15:57:18
tags: [bean,application-context]
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

#### 1. XML을 이용한 방법

bean이라는 태그를 이용해서 Bean 등록을 할수 있다.

```xml
<bean id="인스턴스명" class="클래스경로"/>
```

<br>

##### 옵션

1. id : 보통 카멜케이스로 작성을 하며 사용할 인스턴스명을 입력해주면 된다.
2. class : Bean 등록 하려는 클래스의 경로를 입력해주면 된다.
3. scope : 말그대로 스코프를 설정
    - 기본값은 싱글톤
    - 매번 다른 값을 사용하려면 prototype으로 설정
    - 이외에도 request, session 등이 존재
4. autowire

<br>

##### 의존주입

property 태그를 이용해서 의존주입 관계를 형성 할수 있습니다. `name`은 의존주입할 set메소드 이름이고, `ref`는 Bean의 id입니다. 인스턴스가 아닌 경우 ref 대신 `value`를 사용합니다.

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

<br>

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

테스트 코드를 통해서 XML 빈 등록이 잘 되고 있음을 확인할수 있습니다.

```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = 
                new ClassPathXmlApplicationContext("spring/spring-bean.xml");

        TestController testController = 
                (TestController) applicationContext.getBean("testController");

        testController.test();
        System.out.println("등록 된 Bean" + Arrays.toString(applicationContext.getBeanDefinitionNames()));
    }
}
```

```
등록 된 Bean[test, testController]
```

{% alert info %}
// 클래스 패스로 ApplicationContext 생성 (다른 방법도 존재)
ApplicationContext context = new ClassPathXmlApplicationContext("클래스패스경로");  
context.getBeanDefinitionNames();   // 등록된 모든 Bean Name확인
context.getBean("빈Name");  // Bean 가져오기
{% endalert %}

<br>

##### 단점

XML로 작성하는 경우에는 문자열로 작성을 하기 때문에 자동완성기능 등 IDE의 도움을 받는 것에 제한이 되기 때문에 작성하는데에 어려움이 존재 합니다(지원 되는 것도 존재하지만 그래도 자바소스 작성시보다는 불편하다).

<br>

##### component-scan

지금까지는 수동으로 Bean을 등록을 하였었는데 이 방법으로는 수많은 Bean들을 직접 등록하기가 힘듭니다. 그래서 어노테이션을 통해서 좀더 간편하게 Bean등록하는 방법인 component-scan이 등장하게 되었습니다. package를 설정하여 해당 package내에서 @Component 어느테이션이 등록 된 객체를 Bean에 등록해 줍니다. 

{% alert info %}
@Controller, @Service, @Repository 등 @Component 어노테이션을 상속한 어노테이션 또한 자동으로 등록 됩니다.
{% endalert %}

<br>

##### 예제 코드

{% tabbed_codeblock %}
<!-- tab java -->

    package kr.co.spring;

    import org.springframework.stereotype.Component;

    @Component
    public class ComponentScanTest {
        
    }
<!-- endtab -->  
<!-- tab xml -->
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context 
       http://www.springframework.org/schema/context/spring-context.xsd
    ">
        <cotext:component-scan base-package="kr.co.spring"/>
    </beans>
<!-- endtab -->  
{% endtabbed_codeblock %}


```java
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = 
                new ClassPathXmlApplicationContext("spring/spring-bean.xml");

        System.out.println("등록 된 Bean" + Arrays.toString(applicationContext.getBeanDefinitionNames()));
    }
}
```

```
등록 된 Bean[componentScanTest, ...]
```

<br>

##### 클래스 설정파일로 Bean 등록

Spring 3.0 이후 보전부터 자바 소스 코드로도 Bean을 등록 할수 있는 방법이 추가 되었습니다. 자바 소스 코드로 작성하게 되면 IED의 기능을 활용할수 있기 때문에 XML보다 작성이 용이하다는 장점이 있습니다.

자바소스로 Bean 구성방법은 @Configuration과 @Bean으로 쉽게 구성이 가능합니다. @Configuration은 해당 클래스가 설정파일임을 명시해 줍니다. @Bean은 메소드 정의시에 사용하며 해당 메소드가 반환한 객체를 Bean으로 등록해줍니다. default로 메소드이름이 Bean Name이 됩니다.
```java
@Configuration
public class BeanConfig {

    @Bean
    public Test test() {
        return new Test();
    }

    @Bean
    public TestController testController(Test test) {
        TestController testController = new TestController();
        testController.setTest(test);
        return testController;
    }
}
```

```
public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = 
            new AnnotationConfigApplicationContext(BeanConfig.class);   // 클래스설정파일로 Bean등록할때 사용하는 ApplicationContext
        System.out.println("등록 된 Bean" + Arrays.toString(applicationContext.getBeanDefinitionNames()));

    }
}
```

```
등록 된 Bean[..., test, testController]
```

{% blockquote  %}
@Bean을 활용한 DI에는 2가지 방법이 존재 합니다.
1. 파라미터로 Bean 등록 된 객체를 작성하면 자동으로 주입해 줍니다(위의 방법).
2. @Bean을 사용하는 메소드를 호출합니다(이 방법은 같은 클래스 내부에 존재할때만 가능).
{% endblockquote %}

<br>

##### 클래스 설정파일로 component-scan

@Bean을 이용하는 방법 또한 직접 작성을 해야하기 때문에 등록하기가 힘듭니다. XML에서 compoennt-scan을 사용하였듯이 클래스 설정파일에서도 마찬가지로 component-scan을 사용할수 있습니다.

@ComponentScan("base 패키지명")으로 설정파일에 작성해주면 사용이 가능합니다.
```
@Configuration
@ComponentScan("kr.co.spring")
public class BeanConfig {

}
```

```
@Component
public class ComponentScanTest {

}


public class TestControllerTest {

    @Test
    public void test() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(BeanConfig.class);
        System.out.println("등록 된 Bean" + Arrays.toString(applicationContext.getBeanDefinitionNames()));

    }
}
```

```
등록 된 Bean[..., componentScanTest]
```

<br>

##### @ComponentScan 옵션

1. @ComponentScan : 아무런 옵션도 주지 않는 경우로 해당 클래스가 위치한 패키지 내부를 basePackage로 설정합니다. 
2. @ComponentScan(basePackages = "패키지명") : 위의 예제와 같은 경우로 직접 basePackage를 설정합니다.
3. @ComponentScan(basePackageClasses = 클래스명.class) : 특정 클래스가 위치한 패키지를 basePackage로 설정할때 사용합니다.


<br>

##### 스프링부트에서의 Bean 등록

스프링부트에서 제공하는 SpringBootApplication 어노테이션을 사용하면 위에서 작업했었던 component-scan, ApplicationContext 생성과 같은 소스코드를 작성할 필요가 없습니다. 해당 어노테이션이 자동으로 생성을 해 줍니다.

<br>


##### 공부하면서 느낀점

XML로 Bean을 등록 하는 것보다는 IDE의 도움을 받을수 있는 자바소스로 설정하는게 좀더 편하다고 생각한다. 그리고 Bean을 직접 등록해서 사용하는 것보다는 component-scan을 이용하것이 좋을것 같다. 

### 결론

직접 작성 한 클래스는 component-scan을 활용하고, 외부 라이브러리를 사용할시에는 @Bean을 직접 등록 해서 사용하자!