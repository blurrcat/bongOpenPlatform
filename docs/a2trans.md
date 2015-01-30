# 数据管道API接口（初稿）

说明：本文档仅限API用户内部阅读，请勿外传。
第一部分为接入方法。
第二部分是标准api文档。

## 一 接入方法：
向Wenfeng@bong.cn提交申请，申请通过后将获得api访问token。具体包含如下：
client:字符串,申请时请提交信息，需惟一，建议采用产品名。如bongll
accessToken:字符串，访问token。
secretKey:字符串，AES密钥。

## 二 api接口：
测试环境domain:http://open-test.bong.cn/
线上环境domain:http://open.bong.cn/

所有请求均为http post方式，application/json格式，参数统一如下格式：
String data,
String sign,
String token。

说明：
data:为Map<String,String>的json字符串。具体map内容详见各接口。该字段为AES算法对json字符串加密结果，密钥secretKey为256字节。
sign:数据签名。具体算法为md5({client} + '.'.join(params.keys()) + {client})。params.keys()按自然顺序顺序。32位md5。
token:访问token的AES加密结果。密钥secretKey。

bong采用jncryptor-1.2.0 AES加密库。地址如下：
https://github.com/RNCryptor
Maven依赖：
<dependency>
	<groupId>org.cryptonode.jncryptor</groupId>
	<artifactId>jncryptor</artifactId>
	<version>1.2.0</version>
</dependency>

返回json说明：
String code:请求结果代码
String message:处理结果文本
String data:请求结果内容,Map<String, String>的AES加密结果，可能为空。
以下接口均只说明参数data加密前map的内容和返回data加密前map的内容。

### 1.数据上传
url:{domain}/device/{client}/bongll/data/upload/
参数map:
“mac”: {mac},//硬件mac地址
"rawData”:{rawData}.//16进制原始数据字符串，按硬件接口文档格式给出，每条数据中间由,分隔.数据条数不超过500条，超过请分批上传。
“pipemac”:{pipemac}//管道设备MAC

结果map:
空

### 2.结构数据下载
url:{domain}/device/{client}/bongll/data/getBlocks/
参数map:
“mac”: {mac},//硬件mac地址
“date”: {date}.//yyyy-mm-dd格式日期
“pipemac”:{pipemac}//管道设备MAC

结果map:
"sum”:{sum结构json串},
"blocks”{list{block结构son串}}.
具体结构请参阅：https://github.com/Ginshell/bongOpenPlatform

### 3.请求该用户设备最后一次同步的数据时间
url:{domain}/device/{client}/bongll/data/uploadTime/
参数map:
“mac”: {mac}.//硬件mac地址
“pipemac”:{pipemac}//管道设备MAC

结果map:
“time”:{time}.//unix时间戳,单位毫秒。

### 4.获取bong用户活跃点，总活跃点和指定日期获得的活跃点
url:{domain}/device/{client}/bongll/ap/
参数map:
“mac”: {mac}.//硬件mac地址
“pipemac”:{pipemac}//管道设备MAC
“date”:{yyyy-mm-dd}.//可选参数，不填则使用当前日期
结果map:
"total”:{数字类型},//用户总活跃点
"date”{数字类型}.//选定日期内获取的活跃点

