---
title: 유저 생성 및 DB 권한 부여까지
date: 2020-02-18 11:07:31
tags: user_create
categories:
- postgre
---

### 유저 생성

```sql
CREATE USER 유저명 PASSWORD '유저비밀번호'
```

<br>

### 데이터베이스 생성

```sql
create database 데이터베이스명
```

<br>

### 유저에게 데이터베이스 권한 부여

```sql
GRANT ALL ON database 데이터베이스명 TO 유저명
```