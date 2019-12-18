---
title: json 파일 입출력
date: 2019-12-19 01:01:32
tags: [java, input/output]
categories:
- java
- input / output
---

## JSON파일 출력(파일 작성)

### 소스코드

```java
public void writeJSON() {
    String jsonString = "json파일에 저장할 내용",
           path = "C:/json/",
           fileName = "jsonTest.json";

    FileWriter fileWriter = null;

    File dirFile = new File(path),
         fullFile = new File(path + fileName);

    if (!dirFile.exists()) {
        dirFile.mkdirs();
    }

    try {
        fileWriter = new FileWriter(fullFile);
        fileWriter.write(jsonString);
        fileWriter.flush();
    } catch(Exception e) {

    } finally {
        fileWriter.close();
    }    
}
```

### 설명

json파일을 작성하는 것은 기타 다른 파일을 작성하는 것과 별반 차이가 없다. 예제에서는 일반 string문자열을 저장하고 있지만 실제 JSON 데이터를 저장할 때도 JSONData를 JSONString으로 변환후에 저장하면 된다.

## JSON파일 호출

```java
import com.google.gson.Gson;

public void readJSON() {
    String path = "C:/json/",
           fileName = "jsonTest.json";

    JsonParser jsonParser = new JsonParser();

    FileReader fileReader = null;

    File file = new File(path + fileName);

    if (file.isFile()) {
        throw new Exception("존재하지 않는 파일입니다.");
    }

    try {
        fileReader = new FileReader(file);  // 1. FileReader 인스턴스를 생성합니다.

        Object resultObj = gson.fromJson(fileReader, Object.class);    // gson의 fromJson메서드의 첫번째 파라미터로는 Reader를 두번째 파라미터로는 Json 데이터 타입을 넘겨주면 된다.

        // Map map = (Map) gson.fromJson(fileReader, Object.class);     // 타입을 Object로 하고 casting 가능
        // Map map = gson.fromJson(fileReader, Map.class);  // 타입을 바로 Map으로 해서 casting 없이 사용 가능
    } catch(Exception e) {

    } finally {
        fileReader.close();
    }   
}
```


### 설명

해당 소스코드는 구글에서 제공하는 Gson을 이용해서 parse를 한다. Gson의 fromJson은 첫번째 파라미터로 Reader와 두번째 파라미터로 데이터 타입을 받는다. 두번째 파라미터로 Object.class를 넘겨주니 모든 데이터 타입(원시 타입 포함)의 값을 오류없이 가져올수 있었다.

> gson.fromJson(Reader, Type) : JSON 파일로 부터 데이터 가져오기
> gson.fromJson(String, type) : JSONString 으로부터 각 데이터 타입으로 변환