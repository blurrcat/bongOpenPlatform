# 数据管道API接口（初稿）

说明：本文档仅限API用户内部阅读，请勿外传。

第一部分为接入方法。

第二部分是标准api文档。

## 一 接入方法：
向Wenfeng@bong.cn提交申请，申请通过后将获得api访问token。具体包含如下：
- client:字符串,申请时请提交信息，需惟一，建议采用产品名。如bongll
- accessToken:字符串，访问token。
- secretKey:字符串，AES密钥。

## 二 api接口：
测试环境domain:http://open-test.bong.cn/
线上环境domain:http://open.bong.cn/

### http请求说明：
所有请求均为http post方式，参数以http body形式发送。
application/json格式，也就是注意 ：http header中"Content-Type"的值为"application/json; charset=UTF-8" 。

### 参数说明：
参数统一如下格式：
```json
{
    "token":"加密串",
    "data":"加密串",
    "sign":"加密串"
}
```
- token:访问token的AES加密结果。密钥secretKey。
- data:为Map<String,String>转化为json字符串。具体map内容详见**【各接口参数说明】**，该字段为AES算法对json字符串加密结果，密钥secretKey为256字节。
- sign:数据签名。具体算法为md5({client} + '.'.join(params.keys()) + {client})。params.keys()按**字母升序**排列。32位md5。

###注意：
- 将JNCryptor 的迭代次数设置为100：JNCryptor cryptor = new AES256JNCryptor(100);
- sign中params的key是指data参数中的key。例如假设 client="here"; 以上传数据接口为例，data参数的mapJson为：
```json
{
    "mac":"",
    "pipemac":"",
    "rawData":""
}
```
那么：
```java
    String token = AES.encode(accessToken);
    String sign = MD5.encode("heremac.pipemac.rawDatahere");
    String data = AES.encode(mapJson);
}
```

### 加密说明：
bong采用jncryptor-1.2.0 AES加密库。地址如下：
https://github.com/RNCryptor
Maven依赖：
```xml
<dependency>
	<groupId>org.cryptonode.jncryptor</groupId>
	<artifactId>jncryptor</artifactId>
	<version>1.2.0</version>
</dependency>
```

### 返回结果说明：
String code:请求结果代码
String message:处理结果文本
String data:请求结果内容,Map<String, String>的AES加密结果，可能为空。

### 各接口参数说明：
以下接口均只说明参数data加密前map的内容和返回data加密前map的内容。
#### 1.数据上传
url:{domain}/device/{client}/bongll/data/upload/

data参数为：对指定map的json串进行AES加密，map json示例:
```json
{
    "mac":"B62BC687C3E3",
    "pipemac":"B62BC687C3E4",
    "rawData":"bab880e0e8000000c140000001410141,bab820e0d0c00000a1c4000001490155 "
}
```

参数map：

- "mac": {mac},//硬件mac地址，示例:B62BC687C3E3
- "pipemac":{pipemac}//管道设备MAC,示例:B62BC687C3E4
- "rawData":{rawData}.//16进制原始数据字符串，按硬件接口文档格式给出，每条数据中间由,分隔。
数据条数不超过500条，超过请分批上传。示例:bab880e0e8000000c140000001410141,bab820e0d0c00000a1c4000001490155

结果map:空

#### 2.结构数据下载
url:{domain}/device/{client}/bongll/data/getBlocks/

参数map:
- "mac": {mac},//硬件mac地址
- "date": {date}.//yyyy-mm-dd格式日期,示例:2015-01-01
- "pipemac":{pipemac}//管道设备MAC

结果map:
- "sum":{sum结构json串},
- "blocks"{list{block结构son串}}.
具体结构请参阅：https://github.com/Ginshell/bongOpenPlatform

#### 3.请求该用户设备最后一次同步的数据时间
url:{domain}/device/{client}/bongll/data/uploadTime/

参数map:
- "mac": {mac}.//硬件mac地址
- "pipemac":{pipemac}//管道设备MAC

结果map:
- "time":{time}.//unix时间戳,单位毫秒。

#### 4.获取bong用户活跃点，总活跃点和指定日期获得的活跃点
url:{domain}/device/{client}/bongll/ap/

data参数map:
- "mac": {mac}.//硬件mac地址
- "pipemac":{pipemac}//管道设备MAC
- "date":{yyyy-mm-dd}.//可选参数，不填则使用当前日期

结果map:
- "total":{数字类型},//用户总活跃点
- "date"{数字类型}.//选定日期内获取的活跃点

