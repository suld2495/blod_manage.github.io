---
title: 오라클 테이블 전체 삭제
date: 2019-12-19 09:13:35
tags: [oracle,drop]
categories:
- oracle
---

오라클에서는 유저가 생성한 테이블을 한번에 모두 삭제하는 query문이 없습니다. 하나씩 일일이 삭제하는 방법은 매우 귀찮은 작업이기 때문에 조금이지만 더 편한 방법이 있습니다. 유저가 생성한 테이블을 조회해서 DROP TABLE QUERY 문자열로 조합하는 방법입니다.

```sql
SELECT  'DROP TABLE ' || object_name || ' CASCADE CONSTRAINTS;'
  FROM    user_objects
WHERE   object_type = 'TABLE';
```