---
title: DataSourceUtils
date: 2019-12-20 16:23:25
tags: [java,transaction,datasource]
categories:
- java
- api
---


## DataSource

DataSource는 ConnectionPool을 관리하기 위한 목적으로 사용되는 객체로 DataSource를 통해서 Connection을 얻어오고 반납하는 등의 작업을 구현합니다. ConnectionPool에는 여러개의 Connection 객체가 생성되어 운용되는데, 이를 직접 웹 어플리케이션에서 다루기 힘들기 때문에 DataSource라는 개념이 도입되었습니다.

## DataSourceUtils

DataSourceUtils 클래스는 JNDI에서 연결을 얻고 필요한 경우 연결을 닫는 메소드를 제공하는 기능을 제공 합니다. 그리고 DataSourceTransactionManager로 쓰레드에 기반한 연결을 지원합니다.

## DataSourceUtils에서 제공하는 메소드

##### getConnection(DataSource dataSource)

getConnection메소드는 제공 된 DataSource를 기반으로 하여 Connection을 생성해줍니다. Connection 생성은 트랜잭션 동기화 여부에 따라 다르게 작동합니다. 만약 트랜잭션 동기화가 활성화 되어 있다면 Connection을 생성하고 트랜잭션 저장소에 바인딩 합니다. 이후 트랜잭션 동기화 작업을 종료 할때까지 getConnection메소드를 호출하면 동일한 Connection을 반환하는 것을 확인할수 있습니다.

```java
public class DataSourceUtilsTest {
    @Autowired
    DataSource dataSource;

    public void testGetConnection() {
        TransactionSynchronizationManager.initSynchronization();

        Connection conn1 = DataSourceUtils.getConnection(dataSource);
        Connection conn2 = DataSourceUtils.getConnection(dataSource);

        System.out.println("동일한 인스턴스 인가요? " + conn1.equals(conn2));
    }
}
```

![](/image/java-datasourceutils1.png)

트랜잭션이 동기화되어 있지 않을 경우에는 `dataSource.getConnection()`과 동일하게 작동합니다. 그래서 항상 새로운 Connection을 반환합니다.

```java
public class DataSourceUtilsTest {
    @Autowired
    DataSource dataSource;

    public void testGetConnection() {
        Connection conn1 = DataSourceUtils.getConnection(dataSource);
        Connection conn2 = DataSourceUtils.getConnection(dataSource);

        System.out.println("동일한 인스턴스 인가요? " + conn1.equals(conn2));
    }
}
```

![](/image/java-datasourceutils2.PNG)


##### releaseConnection(Connection conn, DataSource dataSource)

releaseConnection메소드 또한 getConnection와 비슷하게 동작 합니다. 만약 트랜잭션 동기화가 활성화 되어 있다면 트랜잭션 저장소에서 Connection을 초기화 합니다. 이때 Connection의 연결은 종료하지 않습니다. 그리고 release 후 다시 getConnection으로 Connection을 받아왔을때 동일한 값이라는 것을 확인 할수 있습니다.

```java
public class DataSourceUtilsTest {
    @Autowired
    DataSource dataSource;

    public void testGetConnection() {
        TransactionSynchronizationManager.initSynchronization();

        Connection conn = DataSourceUtils.getConnection(dataSource);

        DataSourceUtils.releaseConnection(conn, dataSource);
        System.out.println("Connection이 종료 되었니? " + conn.isClosed());

        Connection conn2 = DataSourceUtils.getConnection(dataSource);

        System.out.println("동일한 Connection 인가요? " + conn.equals(conn2));
    }
}
```

![](/image/java-datasourceutils3.PNG)


위와 같이 releaseConnection 메소드를 호출 하더라도 Connection은 종료되지 않는다. 단지 트랜잭션에서만 제거되었을 뿐이다. 이 때문에 사용시 주의할 필요가 있다.

```java
public class DataSourceUtilsTest {
    @Autowired
    DataSource dataSource;

    public void testGetConnection() {
        TransactionSynchronizationManager.initSynchronization();

        Connection conn = DataSourceUtils.getConnection(dataSource);

        System.out.println("트랜잭션 저장소에 저장 중인가? " + DataSourceUtils.isConnectionTransactional(conn, dataSource));

        DataSourceUtils.releaseConnection(conn, dataSource);

        TransactionSynchronizationManager.unbindResource(dataSource);
        TransactionSynchronizationManager.clearSynchronization();

        System.out.println("Connection이 종료 되었니? " + conn.isClosed());
        System.out.println("트랜잭션 저장소에 저장 중인가? " + DataSourceUtils.isConnectionTransactional(conn, dataSource));
    }
}
```

![](/image/java-datasourceutils4.PNG)

## 결론

DataSourceUtils를 사용하면 동기화된 트랜잭션의 Connection을 가져와서 사용하기에 좋다. 하지만 releaseConnection은 Connection을 종료해주는 기능이 아니므로 사용시에 주의할 필요가 있다.

{% alert info %}
트랜잭션 동기화 상태가 아닌 Connection이라면 releaseConnection메소드를 호출하면 close 해준다.
{% endalert %}