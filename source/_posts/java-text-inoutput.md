---
title: 텍스트파일 입출력
date: 2019-12-19 00:54:42
tags: [java, input/output]
categories:
- java
- input / output
---

## 1. 텍스트 파일 출력

```java
String path = "./test.txt",
       text = "안녕하세요\n"
            + "어서오세요\n"
            + "인사합니다";

BufferedWriter writer = new BufferedWriter(
    new OutputStreamWriter(new FileOutputStream(path), "UTF-8"));

writer.write(text);
writer.close();
```

## 2. 텍스트 파일 입력

```java
String path = "./test.txt",
       str  = "";

StringBuilder sb = new StringBuilder();

BufferedReader reader = new BufferedReader(
    new InputStreamReader(new FileInputStream(path), "UTF-8"));

while((str = reader.readLine()) != null) {
    sb.append(str);
}
```

FileReader/FileWriter는 인코딩 설정이 불가능 하므로 FileInputStream/FileOuputStream을 쓰자
많은 양의 데이터를 입출력 할시에 Buffer를 쓰는것과 안 쓰는것의 차이가 크다 => 버퍼를 쓰자
InputStream/OutputStream은 단위가 바이트단위라서 한글이 깨지므로 InputStreamReader/OutputStreamReader를 쓰자