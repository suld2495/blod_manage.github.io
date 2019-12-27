---
title: MessageSource 클래스
date: 2019-12-27 11:30:15
tags: [message]
categories:
- spring
- IoC Container
---

## MessageSource

스프링 메시지소는 국제화(i18n)을 제공하는 인터페이스입니다. 메시지 설정 파일을 통해서 각 국가에 해당하는 언어로 메세지를 제공할수 있습니다. ApplicationContext는 MessageSource를 구현하고 있습니다.

##### 메시지 설정 파일

메시지 설정 파일은 프로퍼티파일을 사용하며 파일 이름에 __[파일이름]_[언어]_[국가].properties__ 형식으로 파일을 추가해주면 됩니다. 아래와 같이 2개의 파일을 생성하게 되면 인텔리제이에서는 Bundle로 묶이는 것을 확인 할수 있습니다.

```
messages.properties : 기본 메시지       
messages_ko_KR.properties: 한국 메시지
```

{% alert info %}
파일이름이 messages로 시작하지 않아도 된다. 위의 형식만 맞춰주면 된다. 스프링 부트를 쓸 경우에는 messages로 시작하면 자동으로 등록 해준다.
{% endalert %}

##### 메시지 가져오기

위와 같은 형식으로 파일을 생성한 후에 프로퍼티 작성 방식인 key=value 형식으로 값을 입력합니다.

```properties
// messages.properties
greeting=Hello, so good {0}
```

```properties
// messages_ko_KR.properties
greeting=안녕하세요 {0}
```

ReloadableResourceBundleMessageSource를 Bean으로 등록 해 줍니다. 여기에서 basename은 경로를 포함한 파일이름까지 적어주면 됩니다. 예를 들어 클래스패스에서 common/message-common_ko_KR.properties로 구성할 예정이라면 `messageSource.setBasename("classpath:common/message-common")` 이렇게 입력해주면 됩니다.

```java
@Configuration
@ComponentScan
public class BeanConfig {

    @Bean
    public MessageSource messageSource() {
        ReloadableResourceBundleMessageSource messageSource = new ReloadableResourceBundleMessageSource();
        messageSource.setBasename("classpath:messages");
        messageSource.setDefaultEncoding("UTF-8");
        return messageSource;
    }
}
```

위처럼 설정이 완료가 되면 메시지를 가져올 준비가 되었습니다. messageSource.getMessage("이름", new String[]{"파라미터1", "파라미터2.."}, Locale) 순으로 작성해주면 됩니다. 두번째 파라미터인 배열을 넘기면 프로퍼티에서 작성했었던 `{0}`에 값이 설정이 됩니다.

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = BeanConfig.class)
public class TestControllerTest {

    @Autowired
    MessageSource messageSource;

    @Test
    public void test() {
        System.out.println(messageSource.getMessage("greeting", new String[]{"1"}, Locale.KOREA));
    }
}
```

<br>

##### 메시지소스 리로딩

ReloadableResourceBundleMessageSource는 리로딩 기능을 가지고 있습니다. 프로퍼티의 변경을 감지해서 적용 해주는 기능을 가지고 있습니다. 설정은 bean 생성시에 아래 한줄을 추가 해주면 됩니다.

```java
messageSource.setCacheSeconds(60);
```

##### xml에서 MessageSource bean 등록 하기

```xml
<bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    <property name="basenames">
        <list>
            <value>classpath:/messages.properties/message-common</value>
        </list>
    </property>
    <property name="defaultEncoding">
        <value>UTF-8</value>
    </property>
    <property name="cacheSeconds">
        <value>60</value>
    </property>
</bean>
```