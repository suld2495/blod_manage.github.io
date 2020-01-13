---
title: 원격 서버 디버깅 
date: 2020-01-13 11:23:40
tags: [remote-debugging]
categories:
- server
---

## 톰캣 원격 서버 디버깅

로컬에서 IDE를 통해서 서버에 접속하면 편하게 디버깅 작업을 진행 할수 있습니다. 하지만 원격 서버에 설치 된 서버를 실행했을 때는 디버깅이 되지 않아 오류가 발생 했을 때 어떤 문제가 발생했는지 정확히 파악하기가 쉽지 않습니다.
<br>
 그래서 톰캣 서버를 사용 하는 경우에 원격 서버 디버깅 하는 방법에 대해서 알아보겠습니다. 

 > 윈도우 서버를 기준으로 작성되었습니다.

<br>

#### 톰캣 설정

catalina.bat 파일을 열어서 아래의 코드를 추가 해줍니다. 아래에서 address는 디버깅 때 사용 할 포트로써 사용가능한 포트 번호를 입력해줍니다. 그리고 접속할수 있도록 인바운드/아웃바운드 규칙에 포트를 추가해줍니다.

```
set JPDA_OPTS=-agentlib:jdwp=transport=dt_socket,address=8888,server=y,suspend=n
```

<br>

#### 톰캣 서버 시작

디버깅을 하기 위해서는 jpda를 이용해야 하기 때문에 톰캣 서버를 시작 할때 아래의 명령어로 시작 합니다.

```
catalina.bat jpda start
```

<br>

#### 인텔리제이 설정

단축키는 Alt + Shift + F10 후에 0번 입력 또는 Run/Debug Configurations 팝업을 오픈합니다.  
<br>
아래 이미지와 같이 remote를 추가해 줍니다.

![](/image/intellij_remote_add.png)

<br>
아래 이미지와 같이 원격 IP와 위에서 톰캣에 작성했었던 디버깅 포트 번호를 입력해 줍니다.

![](/image/intellij_remote_setting.png)

<br>

#### 사용

디버깅 적용은 로컬에 저장되어 있는 소스코드에 디비깅을 사용합니다. 그렇기 때문에 원격 서버의 소스와 로컬의 소스코드가 일치해야 합니다.