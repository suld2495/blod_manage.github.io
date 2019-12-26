---
title: Environment
date: 2019-12-26 11:14:11
tags: [environment]
categories:
- spring
- IoC Container
---

##### Environment

ApplicationContext는 EnvironmentCapable 인터페이스를 구현하고 있습니다. 이 인터페이스는 getEnvironment 메소드를 제공하며 호출시 Environment를 반환해 줍니다. Environment 클래스는 프로파일 및 프로퍼티 값과 관련이 있습니다.

##### 프로파일

개발을 하다보면 로컬, 개발, 운영등 각 환경마다 설정을 달리 해주어야 하는 경우가 발생합니다. 이때 각 환경마다 활성화할 Bean을 관리해주는 역할을 하는게 프로파일입니다. 

프로파일을 설정하면 앱 구동시에 설정된 프로파일 active 값에 따라서 해당 bean을 등록 할지 여부를 결정하게 됩니다. 예를 들어 A라는 Bean은 테스트시에만 쓰고 싶다면, 해당 Bean을 테스트 프로파일로 구성하면 테스트시에만 활성화 되게 됩니다.

##### 설정 방법

설정 파일에 클래스에 정의를 하면 해당 설정 파일에서 정의한 모든 Bean을 한번에 정의할수 있습니다.

```
@Configuration
@ComponentScan
@Profile("test")
public class BeanConfig {
    
}
```

메소드로 정의하여 개별적으로 정의 할수도 있습니다.

```java

<!-- DevBean.class -->
@Component
public class DevBean {
}

<!-- LocalBean.class -->
@Component
public class LocalBean {
}

<!-- AllBean.class -->
@Component
public class AllBean {
}

```

```java
@Configuration
public class BeanConfig {

    @Bean
    @Profile("local")
    public LocalBean localBean() {
        return new LocalBean();
    }

    @Bean
    @Profile("dev")
    public DevBean devBean() {
        return new DevBean();
    }

    @Bean
    public AllBean allBean() {
        return new AllBean();
    }
}
```

Profile을 설정하지 않은 allBean와 활성화 프로파일로 설정한 dev만 bean이 등록된것을 확인할수 있습니다.

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = BeanConfig.class)
@ActiveProfiles("dev")
public class TestControllerTest {

    @Autowired
    ApplicationContext ctx;

    @Test
    public void test() {
        System.out.println(Arrays.toString(ctx.getBeanDefinitionNames()));
    }
}
```

```
[...,allBean, devBean]]
```

위의 방법 이외에도 Bean 등록할 클래스에 직접 사용할수도 있습니다.

```java
@Component
@Profile("local")
public class LocalBean {
}
```

##### 프로파일 문자열

지금까지 사용했었던 프로파일 문자열은 사용자가 임의로 만들수 있습니다. 그리고 지금까지는 활성화할 프로파일을 하나만 설정하였지만 연산자를 통해서 한번에 여러개의 프로파일을 활성화할수 있습니다.

!를 문자열 앞에 붙이면 반대의 의미가 됩니다.

```
!dev 는 dev가 아닌 것만 활성화
```

&는 and의 의미를 가지고 있습니다.

```
abc & def는 abc 이면서 def인것만 활성화
```

|는 or의 의미를 가지고 있습니다.

```
abc | def는 abc이거나 def인것만 활성화
```

<br>

##### 프로퍼티

프로퍼티는 다양한 방법으로 정의할수 있는 설정값입니다. 프로퍼티는 `key=value`로 구성이 됩니다.

##### 추가 방법

`@PropertySource("classpath:파일위치")`로 추가가 가능합니다. XML에서도 프로퍼티 등록이 가능합니다. 되도록 @Configuration이 선언된 클래스에 함께 사용하도록 합시다.

