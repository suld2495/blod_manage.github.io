---
title: 데이터 바인딩 추상화1
date: 2020-01-09 21:29:51
tags: [PropertyEditor,databinding]
categories:
- spring
- IoC Container
---

## 데이터 바인딩

##### 기술적 관점

프로퍼티 값을 타겟 객체에 설정하는 기능입니다.

##### 사용자 관점

사용자 입력값을 어플리케이션 도메인 모델이 동적으로 변환해 넣어주는 기능입니다. 클라이언트 입력값은 대부분 문자열인데, 그 값을 객체가 가지고 있는 int, long, boolean, date 등 심지어 Event, Book 과 같은 도메인 타입으로 변환해서 넣어주는 기능입니다. MVC할때 클라이언트에서 받은 데이터를 객체에 넣어주는 기능이 해당 기능입니다.

---

## PropertyEditor

스프링 3.0 이전까지 DataBinder가 변환 작업시에 사용하던 인터페이스입니다.

### 단점

##### PropertyEditor는 값을 set 하는 경우 상태를 저장

PropertyEditor가 값을 set하는 경우 쓰레드마다 값을 공유할수 있어 쓰레드-세이프하지 않습니다. 그렇기 때문에 PropertyEditor의 구현체는 여러 쓰레드에서 공유해서 사용하서는 안됩니다.

> Bean으로 등록해서 사용하면 안됩니다.

<br>

### 사용 방법

#### xml에서 빈 등록시 문자열로 넘어온 값을 타입에 맞게 데이터 바인딩 하기

<br>

##### DataBindingBean

데이터 바인딩에서 사용할 샘플 Bean 클래스

```java
public class DataBindingBean {
    private String str;
    private Date date;
    private boolean aBoolean;

    public void setStr(String str) {
        this.str = str;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    public void setaBoolean(boolean aBoolean) {
        this.aBoolean = aBoolean;
    }

    @Override
    public String toString() {
        return "DataBindingBean{" +
                "str='" + str + '\'' +
                ", date=" + date +
                ", aBoolean=" + aBoolean +
                '}';
    }
}
```
<br>

##### spring-bean.xml

DataBindingBean를 Bean으로 등록하기 위한 IoC 컨테이너

```xml
<bean id="dataBindingBean" class="kr.co.spring.DataBindingBean">
    <property name="str">
        <value>안녕하세요</value>
    </property>
    <property name="date">
        <value>2020-01-09</value>
    </property>
    <property name="aBoolean">
        <value>true</value>
    </property>
</bean>
```

##### TestClass

```
public class TestClass {

    @Test
    public void test() {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("spring/spring-bean.xml");
        DataBindingBean dataBindingBean = (DataBindingBean) ctx.getBean("dataBindingBean");
        System.out.println(dataBindingBean.toString());
    }
}
```

위의 코드를 작성 한 후에 실행 해보면 아래와 같이 타입이 맞지 않다고 에러를 발생시킵니다. xml에서 모든 값을 문자열로 넘겨주었으니 타입이 맞지 않다는 에러가 나오는 것이 당연합니다. 이제 문자열로 값이 넘어오더라도 타입게 맞게 데이터 바인딩이 되도록 수정해 보도록 하겠습니다.


```
...Failed to convert property value of type [java.lang.String] to required type [java.util.Date] for property 'date'...
```

### CustomEditorConfigurer 등록하기

---

### 4.0 이전 버전

DataBinding에서 사용이 되는 CustomEditorConfigurer의 변화로 인해서 4.0 이전 버전과 이후 버전의 설정이 변경 되었습니다.

##### spring-bean.xml

4.0 이전 버전에서는 customEditors의 type이 `Map<String, ?>` 이었습니다. 아래와 같이 PropertyEditor를 구현한 클래스들을 Bean으로 등록해 주면 됩니다.

```xml
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
        <map>
            <entry key="java.util.Date">
                <bean class="org.springframework.beans.propertyeditors.CustomDateEditor">
                    <constructor-arg>
                        <bean class="java.text.SimpleDateFormat">
                            <constructor-arg value="yyyy-MM-dd"/>
                        </bean>
                    </constructor-arg>
                    <constructor-arg value="true"/>
                </bean>
            </entry>

            <entry key="java.lang.String">
                <bean class="org.springframework.beans.propertyeditors.StringTrimmerEditor">
                    <constructor-arg value="true"/>
                </bean>
            </entry>

            <entry key="java.lang.Boolean">
                <bean class="org.springframework.beans.propertyeditors.CustomBooleanEditor">
                    <constructor-arg value="true"/>
                </bean>
            </entry>
        </map>
    </property>
</bean>
```

<br>


### 4.0 이후 버전(4.0 포함)

4.0 이후 버전에서는 customEditors의 type이 `Map<Class<?>, Class<? extends PropertyEditor>>`로 바뀌었습니다. 이제는 Bean이 아니라 클래스 경로를 넘겨 주면 됩니다. 

##### spring-bean.xml

```xml
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
        <map>
            <!-- 1. entry 태그의 key와 value 속성을 이용하여 작성 -->
            <entry key="java.util.Date" value="org.springframework.beans.propertyeditors.CustomDateEditor"/>
            
            <!-- 2. value 태그를 이용하여 작성 -->

            <!-- 2가지 모두 사용 가능 -->
            <entry key="java.lang.String">
                <value>org.springframework.beans.propertyeditors.StringTrimmerEditor</value>
            </entry>

            <entry key="java.lang.Boolean">
                <value>org.springframework.beans.propertyeditors.CustomBooleanEditor</value>
            </entry>
        </map>
    </property>
</bean>
```

변화가 되면서 PropertyEditor 구현체가 default 생성자가 없으면 아래처럼 에러가 발생합니다.

```
No default constructor found; nested exception is java.lang.NoSuchMethodException: org.springframework.beans.propertyeditors.CustomDateEditor.<init>()
```

> 위의 방법은 모든 데이터바인딩에 적용이 되는 것임. DataBindingBean를 위해서 작성 된게 아니라 문자열로 넘어온 값을 Date 또는 Boolean으로 데이터바인딩해야 할 모든 상황에서 사용되게 됨. 데이터바인딩에서 사용될 사용자 PropertyEditor 구현체를 등록 하는 절차임. 

<br>

### PropertyEditorRegistrar

PropertyEditorRegistrar를 구현함으로써 PropertyEditor 등록을 커스텀하게 작성할수 있습니다. 이를 이용하면 default 생성자가 없는 경우에도 등록 할수 있습니다.

##### CustomEditorRegistrar

```java
public class CustomEditorRegistrar implements PropertyEditorRegistrar {
    public void registerCustomEditors(PropertyEditorRegistry registry) {
        registry.registerCustomEditor(Date.class, new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), true));
        registry.registerCustomEditor(String.class, new StringTrimmerEditor(true));
        registry.registerCustomEditor(Boolean.class, new CustomBooleanEditor(true));
    }
}
```

```xml
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="propertyEditorRegistrars">
        <list>
            <bean class="kr.co.spring.CustomEditorRegistrar"/>
        </list>
    </property>
</bean>
```

정상적으로 잘 출력이 되는 것을 확인할 수 있습니다.

```
DataBindingBean{str='안녕하세요', date=Thu Jan 09 00:00:00 KST 2020, bool=true}
```
