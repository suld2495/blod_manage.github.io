---
title: 오라클 LOCK 걸렸을때
date: 2019-12-19 09:08:46
tags: [oracle,lock]
categories:
- oracle
---

## LOCK걸린 테이블 조회

```SQL
SELECT A.SID
     , A.SERIAL#
     , object_name
     , A.SID || ', ' || A.SERIAL# AS KILL_TASK
  FROM V$SESSION A
 INNER JOIN V$LOCK B
    ON A.SID = B.SID
 INNER JOIN DBA_OBJECTS C
    ON B.ID1 = C.OBJECT_ID
 WHERE B.TYPE  = 'TM'
   AND OBJECT_NAME = '테이블 네임';
```

## LOCK 해제

LOCK 조회한 결과에서 SID와 SERIAL# 번호를 입력 하면 해제가 가능합니다.

```SQL
ALTER SYSTEM KILL SESSION 'SID, SERIAL#';
```