---
title: CORS 크로스 도메인 이슈
date: 2020-01-13 08:25:57
tags: [cors,response-header]
categories:
- java
- server
---

## CORS란

CORS란 도메인 또는 포트가 다른 서버의 자원을 요청하는 매커니즘을 말합니다. 이때 요청을 할때는 corss-origin HTTP에 의해 요청됩니다.
<br>
이때 요청을 할때는 cross-origin HTTP에 의해 요청됩니다.
<br>
하지만 동일 출처 정책 때문에 CORS 상황이 발생시에 요청한 데이터를 브라우저에서 보안목적으로 차단합니다. 해당 문제를 해결하는 가장 쉬운 방법은 같은 도메인을 사용하는 것입니다. 하지만 요즘에는 각기 다른 용도로 사용되는 경우에는 개별 서버를 구축하기 때문에 이는 해결책이 될수 없습니다.
<br>
다른 해결방법은 서버에서 CORS 효청을 허용해주면 됩니다. 서버로 들어오는 모든 요청에 대해서 CORS 요청을 허용하기 위해서 Filter를 이용합니다. 

```java
public class CORSFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) res;

        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS");
        response.setHeader("Access-Control-Allow-Origin", "3600");
        response.setHeader("Access-Control-Allow-Origin", "x-auth-token, x-requested-with, origin, content-type, accept, sid");

        chain.doFilter(req, res);
    }
}
```

```xml
<filter>
    <filter-name>cors</filter-name>
    <filter-class>kr.co.spring.filter.SimpleCORSFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>cors</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

##### Access-Control-Allow-Methods

POST, PUT, DELETE 등 허용할 ACCESS METHOD를 설정할수 있습니다. 

##### Access-Control-Max-Age

HTTP Request 요청에 앞서 Preflight Request 라는 요청이 발생되는데, 이는 해당 서버에 요청하는 메소드가 실행 가능한지(권한 여부) 확인을 위한 요청입니다. Preflight Reuqest는 OPTIONS 메소드를 통해 서버에 전달됩니다(Allow-Methods 설정에서 OPTIONS를 허용해야 합니다).
<br>
Access-Control-Max-Age는 Preflight Request를 캐시할 시간입니다. 단위는 초입니다. 캐시를 하게 된다면 해당 시간이 지난 뒤에 재 요청을 하게 됩니다.

##### Access-Control-Allow-Origin

허용할 도메인을 설정할수 있습니다. 값은 *로 설정하면 모든 도메인에서 허용하는 것이고, 특정 도메인만 설정할수 있습니다. 