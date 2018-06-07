myokhttp
=======

## gradle
```
implementation 'com.jingyu.java:myokhttp:0.3.1'
```

## maven
```
<dependency>
  <groupId>com.jingyu.java</groupId>
  <artifactId>myokhttp</artifactId>
  <version>0.3.1</version>
  <type>pom</type>
</dependency>
```

### example
```
MyReqInfo.Builder builder = new MyReqInfo.Builder()
        .get()
        .headersMap(headerMap)
        .url("http://")
        .queryMap(queryMap);

MyReqInfo.Builder builder = new MyReqInfo.Builder()
        .post()
        .headersMap(headerMap)
        .url("http://")
        .queryMap(queryMap)
        .bodyContent(json)
        .contentType(HttpConstants.JSON);
        
MyReqInfo.Builder builder = new MyReqInfo.Builder()
        .post()
        .headersMap(headerMap)
        .url("http://")
        .bodyMap(bodyMap);  

new MyHttpClient().httpAsync(builder.builder(), new MyStringHttpHandler() {
    @Override
    public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, String resultBean) {
        super.onSuccess(myReqInfo, myRespInfo, resultBean);
        System.out.println(resultBean);
    }
});

```

### MyHttpUtil(简化使用)
**同步/异步请求**
```
 // 同步请求
 MyHttpUtil.Async.get(url, queryMap, null);
 // 异步请求
 MyHttpUtil.Sync.post(url, bodyMap, null); 
 
```

**json传参**
```
MyHttpUtil.Sync.post(url, "{\"key\":\"value\"}", HttpConstants.JSON, new MyStringHttpHandler() {
      @Override
      public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, String resultBean) {
          super.onSuccess(myReqInfo, myRespInfo, resultBean);
          System.out.println(resultBean);
      }
});
```

**表单传参**
```
MyHttpUtil.Async.post(url, "key=value&key2=value2", HttpConstants.FORM, new MyStringHttpHandler() {
      @Override
      public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, String resultBean) {
          super.onSuccess(myReqInfo, myRespInfo, resultBean);
          System.out.println(resultBean);
      }
});

MyHttpUtil.Async.post(url, bodyMap, new MyStringHttpHandler() {
     @Override
     public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, String str) {
         super.onSuccess(myReqInfo, myRespInfo, str);
         System.out.println(str);
     }
});
```

**自动解析**
```
MyHttpUtil.Async.post(url, bodyMap, new MyGsonHttpHandler<User>(User.class) {
     @Override
     public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, User user) {
         super.onSuccess(myReqInfo, myRespInfo, user);
         System.out.println(resultBean);
     }
  });
```

**自定义解析**
```
MyHttpUtil.Async.post(url, bodyMap, new MyBaseHttpHandler<User>() {
      @Override
      public User onParse(MyReqInfo myReqInfo, MyRespInfo myRespInfo, InputStream inputStream, long totalSize) {
          // todo, inputStream
          return new User();
      }

      @Override
      public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, User user) {
          super.onSuccess(myReqInfo, myRespInfo, user);
      }
});
```


**下载文件**
```
MyHttpUtil.Async.get(url, queryMap, new MyFileHttpHandler(saveFile) {
    @Override
    public void onSuccess(MyReqInfo myReqInfo, MyRespInfo myRespInfo, File saveFile) {
        super.onSuccess(myReqInfo, myRespInfo, saveFile);
        System.out.println(saveFile);
    }
});
```

**文件上传,进度监听**
```
  HashMap<String, Object> bodyMap = new HashMap<>();
  map.put("key1", "value1");
  map.put("file", file1);
  map.put("file2", file2);
  map.put("file3", null); //空的file会被过滤
  MyHttpUtil.Async.post(url, bodyMap, new MyStringHttpHandler() {
      @Override
      public void onUploadProgress(long bytesWritten, long totalSize) {
          super.onUploadProgress(bytesWritten, totalSize);
          Logger.i(bytesWritten + "--" + totalSize);
      }
  });
```
