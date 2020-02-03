---
title: SpEL
date: 2020-01-22 20:16:31
tags: [el,spring-expression-language]
categories:
- spring
- IoC Container
---

## Spring Expression language 란?

SpEL이란 런타임시에 객체 그래프를 조회하고 조작하는 표현언어입니다. 스프링 3.0이상부터 지원하여 EL과 비슷하지만 메소드 호출 지원, 문자열 템플릿 기능까지 제공해 줍니다.

## SpEL에서 지원하는 기능

1. Literal expressions
1. Boolean and relational operators
1. Regular expressions
1. Class expressions
1. Accessing properties, arrays, lists, and maps
1. Method invocation
1. Relational operators
1. Assignment
1. Calling constructors
1. Bean references
1. Array construction
1. Inline lists
1. Inline maps
1. Ternary operator
1. Variables
1. User-defined functions
1. Collection projection
1. Collection selection
1. Templated expressions

<br>

## SpelExpressionParser

`SpelExpressionParser` 클래스를 사용해서 리터럴 문자열 표현식을 파싱할수 있습니다. 싱글 쿼테이션으로 둘러싸인 문자열을 파싱 해줍니다.

```java
@Test
public void test1() {
    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("'Hello World'");
    String message = (String) exp.getValue();

    System.out.println(message);
}
```
```
Hello World
```

<br>

SpEL은 메소드를 호출하거나 프로퍼티에 접근하거나 생성자를 호출할수 있습니다. 아래의 예제는 String 메소드 중 하나인 concat을 호출하고 있습니다.

```java
@Test
public void test2() {
    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("'Hello World'.concat('!')");
    String message = (String) exp.getValue();

    System.out.println(message);
}
```
```
Hello World!
```

<br>

SpEL을 사용해서 객체(rootObject라고 부름)의 필드의 값을 가져 올수 있습니다. 그리고 Expression의 getValue의 파라미터 중에 하나인 desiredResultType에 반환받은 타입을 입력하면 위의 예제 처럼 캐스팅을 하지 않아도 됩니다.

```java
public class SpELVO {
    private String name;

    public SpELVO(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

@Test
public void test3() {
    SpELVO spELVO = new SpELVO("intelli");

    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("name");
    String message = (String) exp.getValue(spELVO, String.class);

    System.out.println(message);
}
```
```
intelli
```

## EvaluationContext

`EvaluationContext` 인터페이스는 프로퍼티, 메소드, 필드를 처리하고 타입변환을 수행하는 표현식을 평가할때 사용합니다. 스프링에서는 두개의 구현체를 제공합니다.

<br>

__1. SimpleEvaluationContext__ : SpEL에서 필수적인 기능만 제공합니다.(4.3.15 버전부터 지원)
__2. StandardEvaluationContext__ : SpEL의 모든 기능을 사용할수 있습니다.

지금까지의 예제에서도 내부에서는 EvaluationContext를 사용하고 있습니다. 아래처럼 EvaluationContext를 직접 생성 후에 사용할수도 있습니다.

```java
@Test
public void test4() {
    SpELVO spELVO = new SpELVO("intelli");

    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("name");
    EvaluationContext evaluationContext = new StandardEvaluationContext();
    String message = exp.getValue(evaluationContext, spELVO, String.class);

    System.out.println(message);
}
```

<br>

## Array

SpEL을 이용해서 배열에 접근 할수 있습니다.

```java
public class ArrayList {
    private List<String> names;

    public List<String> getNames() {
        return names;
    }

    public void setNames(List<String> names) {
        this.names = names;
    }
}


@Test
public void test5() {
    ArrayList arrayList = new ArrayList();
    arrayList.setNames(Arrays.asList("woman", "man"));

    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("names[0]");
    EvaluationContext evaluationContext = new StandardEvaluationContext(arrayList); // rootObject를 여기에 바로 쓸수도 있다.
    String message = exp.getValue(evaluationContext, String.class);

    System.out.println(message);
}
```
```
woman
```

<br>

값을 가져오는 것 말고도 값을 변경할수도 있습니다. 유의 해야할 점은 값을 추가해주는게 아닌 변경해주는 것입니다.

```java
@Test
public void test6() {
    ArrayLists arrayLists = new ArrayLists();
    arrayLists.setNames(Arrays.asList("man"));

    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("names[0]");
    EvaluationContext evaluationContext = new StandardEvaluationContext(arrayLists);
    exp.setValue(evaluationContext, "woman");

    System.out.println(arrayLists.getNames().get(0));
}
```

값을 변경해주는 기능은 배열 뿐 아니라 일반 객체에서도 사용 가능합니다. 이때 필드는 public 이거나 setter 메소드가 구현되어 있어야 합니다.

```java
@Test
public void test7() {
    SpELVO spELVO = new SpELVO("jesi");

    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("name");
    EvaluationContext evaluationContext = new StandardEvaluationContext(spELVO);
    exp.setValue(evaluationContext, "sara");

    System.out.println(spELVO.getName());
}
```

<br>

## SpelParserConfiguration

`SpelParserConfiguration`를 사용 하면 Collection에서 index가 null일 경우에 자동으로 생성해주는 기능을 활성화 할수 있습니다. 

아래의 예제에서는 배열에서 index가 null인 요소에 접근하려고 할때 자동으로 배열에 빈값이 추가된것을 확인 할수 있습니다. SpelParserConfiguration 생성자의 두번째 파라미터를 true로 전달하면 기능을 활성화 할수 있습니다(기본값은 두개 모두 false).

```java
@Test
public void test8() {
    ArrayLists arrayLists = new ArrayLists();

    SpelParserConfiguration configuration = new SpelParserConfiguration(false, true);
    ExpressionParser parser = new SpelExpressionParser(configuration);
    Expression expression = parser.parseExpression("names[4]");
    String name = expression.getValue(arrayLists, String.class);

    System.out.println("result : " + name);
    System.out.println(arrayLists.getNames().size());
}
```
```
result : 
5
```

String 뿐 아니라 일반 객체도 자동으로 생성해 줍니다. 이때 자동으로 생성 될 객체는 디폴트 생성자가 존재하여야 합니다.

```java
public class SpELVO {
    private String name;

    public SpELVO() {
    }
}

public class ArrayRefrence {
    private List<SpELVO> spELVOs = new ArrayList<>();

    public List<SpELVO> getSpELVOs() {
        return spELVOs;
    }

    public void setSpELVOs(List<SpELVO> spELVOs) {
        this.spELVOs = spELVOs;
    }
}

@Test
public void test9() {
    ArrayRefrence arrayLists = new ArrayRefrence();

    SpelParserConfiguration configuration = new SpelParserConfiguration(false, true);
    ExpressionParser parser = new SpelExpressionParser(configuration);
    Expression expression = parser.parseExpression("spELVOs[4]");
    String name = expression.getValue(arrayLists, String.class);

    System.out.println("result : " + name);
    System.out.println(arrayLists.getSpELVOs().size());
}
```

## 빈 정의를 정의하는 표현식

XML와 어노테이션 기반의 설정 메타데이터와 SpEL 표현식을 함께 사용할수 있습니다. 표현식을 정의 하기 위한 문법은 `#{ <expression string> }`입니다.

#### XML Configuration

표현식을 사용해서 프로퍼티나 생성자의 전달인자에 값을 할당할수 있습니다.

```xml
<bean id="spelVo" class="kr.co.spring.SpELVO">
    <constructor-arg name="name" value="hihi"/>
</bean>
```

```java
@Test
public void xml() {
    ApplicationContext ctx = new ClassPathXmlApplicationContext("spring/spring-spel.xml");
    SpELVO spelVo= (SpELVO) ctx.getBean("spelVo");
    System.out.println(spelVo.getName());
}
```
```
hihi
```

<br>

#### 어노테이션

`@Value` 어노테이션을 이용해서 SpEL을 사용할수 있습니다. 어노테이션이 위치할수 있는 곳은 필드, 메소드, 메소드나 생성자의 파라미터입니다.

###### 필드에서의 사용

아래의 클래스는 Value 어노테이션을 사용하는 예제로 사용이 됩니다. 아래에서 보듯이 일반 문자열 및 빈으로 등록 된 객체의 메소드도 사용이 가능합니다(빈으로 등록되지 않으면 사용이 안됨).

```java
public class ValueSpEL {
    @Value("hi")
    private String hello;

    @Value("#{seplVo.getName()}")
    private String name;

    public String getName() {
        return name;
    }

    public String getHello() {
        return hello;
    }
}

```

SpELVO와 ValueSpEL 클래스를 빈으로 등록합니다.

```java
@Component
public class WebConfiguration {

    @Bean
    public SpELVO seplVo() {
        return new SpELVO("sara");
    }

    @Bean
    public ValueSpEL valueSpEL() {
        return new ValueSpEL();
    }
}

```

```java
@Test
public void annotationField() {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(WebConfiguration.class);
    ValueSpEL valueSpEL = (ValueSpEL) ctx.getBean("valueSpEL");
    System.out.println(valueSpEL.getHello());
    System.out.println(valueSpEL.getName());
}
```
```
hi
sara
```

> 주의를 할 점은 xml이나 어노테이션 등에서 사용 되는 $와 #의 차이점을 구분할줄 알아야 한다. $는 프로퍼티 문법이고,<br> #는 SpEL 문법이다.


