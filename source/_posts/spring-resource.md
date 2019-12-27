---
title: 스프링 리소스
date: 2019-12-27 12:24:32
tags: [resource]
categories:
- spring
- IoC Container
---

## Resource 추상화

스프링의 __Resource__ 객체는 java.net.URL을 추상화한 인터페이스입니다. Resource 객체는 스프링 내부에서 가장 많이 사용이 되는 인터페이스이며 스프링 IoC 컨테이너가 생성 될때, 컨테이너 설정 정보를 담는 파일들을 가져올때도 사용합니다.

Resource 인터페이스를 통해 추상화한 이유 java.net.URL의 한계로 클래스 패스를 기준으로 리소스를 읽어오는 기능이 존재하지 않기 때문입니다.

<br>

##### 주요 메소드

1. exists
2. isOpen
3. isFile
4. isDirectory
5. getFile(항상 파일로 가져올수 있는 것은 아님)

<br>

##### 구현체

1. UrlResource : URL을 기준으로 리소스를 읽어들이며 기본으로 제공하는 프로토콜에는 __http, https, ftp, file, jar__
2. ClassPathResource : 클래스패스를 기준으로 리소스를 읽어들이며 접두어로 __classapth:__를 사용
3. FileSystemResource : 파일 시스템을 기준으로 읽어들임 
4. ServletContextResource : 웹 어플리케이션 루트에서 상대경로로 리소스를 읽어들임

## ResourceLoader

__ResourceLoader__는 리소스를 읽어오는 기능을 제공하는 인터페이스 입니다. ApplicationContext도 ResourceLoader를 상속하고 있습니다. 기능은 말그대로 리소스를 읽어오는 기능만 제공하고 있습니다.

##### 구현체

1. DefaultResourceLoader : UrlResource(경로가 http, https 등 프로토콜로 시작)와 ClassPathResource(경로가 classapth:로 시작)를 가져올때 사용
2. FileSystemResourceLoader : DefaultResourceLoader를 상속하고 있으며 경로가 /로 시작하는 경우 FileSystemResource를 반환.
3. GenericWebApplicationContext : DefaultResourceLoader를 상속하고 있으며 경로가 /로 시작하는 경우 ServletContextResource를 반환
    - DefaultResourceLoader를 직접 상속 하고 있지는 않음

리소스를 가져오는 코드는 아래와 같습니다.

```java
resourceLoader.getResource("location 문자열");
```

<br>

## Resource의 타입과 ApplicationContext 타입의 관계

Resource의 타입은 location 문자열과 ApplicationContext의 타입에 따라 결정됩니다. 위의 ResourceLoader 구현체를 통해서 Resource를 얻어오는게 아니라 bean으로 등록 된 ApplicationContext를 통해서 Resource를 가져올때는 아래와 같이 적용됩니다.

1. ClassPathXmlApplicationContext => ClassPathResource
2. FileSystemXmlApplicationContext => FileSystemResource
3. WebApplicationContext => ServletContextResource

만약 ApplicationContext의 타입과 상관없이 리소스 타입을 강제하고 싶다면 접두어를 사용하면 됩니다.

1. classpath
2. file
3. http

그래서 좀더 명확한 코드를 작성하기 위해 접두어를 사용해서 Resource를 가져오는 것이 좋습니다.


```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = BeanConfig.class)
public class TestControllerTest {
    @Autowired
    ResourceLoader resourceLoader;

    @Test
    public void test() {
        System.out.println(resourceLoader.getClass());

        Resource resource = resourceLoader.getResource("test.properties");
        System.out.println(resource.getClass());

        ResourceLoader defulatResourceLoader = new DefaultResourceLoader();
        Resource defaultResource = defulatResourceLoader.getResource("classpath:test.properties");
        System.out.println(defaultResource.getClass());
    }
}
```

```
class org.springframework.context.support.GenericApplicationContext
class org.springframework.core.io.DefaultResourceLoader$ClassPathContextResource
class org.springframework.core.io.ClassPathResource
```