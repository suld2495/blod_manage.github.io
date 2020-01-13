---
title: 톰캣 웹서버 정보 노출
date: 2020-01-13 09:03:53
tags: [tomcat]
categories:
- tomcat
---

## Response 웹서버 정보 노출

서버로 요청을 보낸 후에 받은 Response에서 웹서버 정보가 노출이 됩니다.

```
Server: Apache-Coyote/1.1
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
...
```

## 노출 방지

웹 서버 정보 노출을 방지 하기 위해서는 server.xml의 Connector 설정을 통해 변경할수 있습니다. 태그에 server=""를 추가하고 값으로 노출을 원하는 문자열을 삽입하면 됩니다.

```
<Connector port="8080" server="원하는 문자열">
...
```
