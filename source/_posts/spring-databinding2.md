---
title: 데이터 바인딩 추상화2
date: 2020-01-19 09:27:07
tags: [converter,formatter,databinding]
categories:
- spring
- IoC Container
---

## 데이터 바인딩 추사황 : Converter와 Formatter

초기 스프링에서는 PropertyEditor를 사용하였습니다. 이후 이를 대체할 Converter, Formatter 인터페이스가 등장하였습니다.

#### Converter

Converter는 S타입을 T타입으로 변환할수 있습니다. PropertyEditor와는 다르게 상태정보가 없기 때문에 쓰레드 세이프합니다. 이로 인하여 빈으로 등록 후 사용하여도 문제가 없습니다.

#### xml을 이용한 Custom Converter 등록하기

###### Converter 만들기

Conterver를 만드는 방법은 간단합니다. Converter<>

```java
public class EventConverter {

    public static class StringToEventConverter implements Converter<String, Event> {

        public Event convert(String source) {
            Event event = new Event();
            event.setId(Integer.parseInt(source));
            return event;
        }
    }

    public static class EventToStringConverter implements Converter<Event, String> {

        public String convert(Event source) {
            return "convert : " + source.getId();
        }
    }
}
```