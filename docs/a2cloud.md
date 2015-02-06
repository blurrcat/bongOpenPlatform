# 第三方手环接入指南

本⽂文档内容中提到的接口仅供已经签署了合作协议的公司使用，如果你的手环也想接入全球第一的全自动运动手环算法，请联系Tracy@bong.cn

## 一、活跃点生态接入
需要在您的产品APP中增加活跃点生态，具体流程如下图
UI部署要求，主要是会有账号绑定（只有在用户使用活跃点时才会要求激活，不影响原生体验）以及活跃点页面（需要在APP中接入活跃点使用界面，并引导到活跃点商品页面）。

### 1 引导账号绑定

第三方需要有自己的注册账号体系，这个体系的规则bong不会介入，但是需要在注册结束时，引导用户进行一次账号绑定。这里的目的是为了防止用户如果已经是bong的老用户，在激活活跃点时会导致活跃点丢失。

为了不影响第三方自身的用户体验，此处用户有两个选择
- 已有bong账户：用户点击此处，会拉起一个绑定bong账号的授权页面。具体部署请参考：

  - 如果是android可以使用已经开发完成的SDK：https://github.com/Ginshell/bongOpenPlatform/blob/master/docs/android_sdk.md  。 AuthInfo里会提供uid
  - 如果是iOS，本周才能完成SDK的开发。所以，可以先使用我们的服务端API接入，文档地址：https://github.com/Ginshell/bongOpenPlatform。

- 没有bong账号，或者不愿意马上绑定，可直接跳过，注意警告用户活跃点可能会丢失。用户可以选择#我没有bong账号#，用户直接跳过此步，立即开始使用第三方的APP。在bong的服务器上，我们会帮用户自动生成一个处于#冻结状态#的bong账号，用户在使用第三方手环期间，所获得的活跃点，将存储在此账户中。



### 2 活跃点接入
第三方APP中需要在自己的APP里展示活跃点相关的信息，包含:
活跃点总数*
今日获得活跃点（可选）
立即使用活跃点*（如果账号处于正常状态）——指向www.bong.cn/products
立即激活活跃点*（如果账号处于正常状态）——对于Weloop来说，使用的方案是，Weloop直接将账号和密码传给bong。如果，bong这边检测到这个用户的手机号码没有注册过bong账户，那么直接帮该用户激活。如果用户的手机号码注册过，引导用户去绑定账号。绑定账号后两个账号的活跃点会合并，取活跃点值最大的账户，账号绑定参考上一步。（见下图）
什么是活跃点*——指向http://www.bong.cn/ap
















## 二. 算法云API接口（初稿）

说明：本文档仅限API用户内部阅读，请勿外传。
第一部分为接入方法。
第二部分是标准api文档。
第三部分是sandbox,无验证信息接口，帮助调通数据流。

### 1.接入方法：
签署协议后，可向Wenfeng@bong.cn提交申请，申请通过后将获得api访问token。具体包含如下：
client:字符串,申请时请提交信息，需惟一，建议采用产品名。如bongll
accessToken:字符串，访问token。
secretKey:字符串，AES密钥。

### 2.api接口：
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

#### 数据上传
url:{domain}/device/{client}/data/upload/
参数map:
“mac”: {mac},//硬件mac地址，示例:B62BC687C3E3 
"rawData”:{rawData}.//16进制原始数据字符串，按硬件接口文档格式给出，每条数据中间由,分隔.数据条数不超过500条，超过请分批上传。示例:bab880e0e8000000c140000001410141,bab820e0d0c00000a1c4000001490155
“clientUserId”:{clientUserId}//用户在三方设备商惟一标识符，数据类型

结果map:
空

#### 结构数据下载
url:{domain}/device/{client}/data/getBlocks/
参数map:
“mac”: {mac},//硬件mac地址
“date”: {date}.//yyyy-mm-dd格式日期
“clientUserId”:{clientUserId}//用户在三方设备商惟一标识符，数据类型

结果map:
"sum”:{sum结构json串},
"blocks”{list{block结构son串}}.
具体结构请参阅：https://github.com/Ginshell/bongOpenPlatform

#### 请求该用户设备最后一次同步的数据时间
url:{domain}/device/{client}/data/uploadTime/
参数map:
“mac”: {mac}.//硬件mac地址
“clientUserId”:{clientUserId}//用户在三方设备商惟一标识符，数据类型

结果map:
“time”:{time}.//unix时间戳,单位毫秒。

#### 用户在算法云端绑定设备，用户没有bong帐号
url:{domain}/device/{client}/bind/
参数map:
“mac”: {mac}.//硬件mac地址
“clientUserId”: {clientUserId}.//用户在第三方设备商惟一标识符，数字类型
“gender”:{gender}.//1男，2女
“height“:{height}.//数字类型，单位厘米

结果map:
空

#### 用户在算法云端绑定设备，用户拥有bong帐号。通过bong Oauth开放平台接口获取用户uid
url:{domain}/device/{client}/bindWithAccount/
参数map:
“mac”: {mac}.//硬件mac地址
“clientUserId”: {clientUserId}.//用户在第三方设备商惟一标识符，数字类型
“uid”:{uid}//用户在bong的惟一标识符

结果map:
空

#### 用户在算法云端解绑设备
url:{domain}/device/{client}/unbind/
参数map:
“mac”: {mac}.//硬件mac地址
“clientUserId”: {clientUserId}.//用户在第三方设备商惟一标识符，数字类型

结果map:
空

#### 获取bong用户活跃点，总活跃点和指定日期获得的活跃点
url:{domain}/device/{client}/ap/
参数map:
“mac”: {mac}.//硬件mac地址
“clientUserId”: {clientUserId}.//用户在第三方设备商惟一标识符，数字类型
“date”:{yyyy-mm-dd}.//可选参数，不填则使用当前日期
结果map:
"total”:{数字类型},//用户总活跃点
"date”{数字类型}.//选定日期内获取的活跃点

### 3.Sandbox环境：
为方便第三方尽快打通数据流，我们在测试环境部署sandbox接口。sandbox接口全部采用无验证get请求。具体接口如下：
#### 数据上传，用于只验证接口连接性，bong将不接受实际数据。
http://open-test.bong.cn/device/data/upload?mac=E69EB2618485&deviceId=1&data=bab880e0e8000000c140000001410141,bab820e0d0c00000a1c4000001490155
参数：
mac:硬件mac地址
deviceId:合作方设备Id号，由bong分配
data:16进制原始数据字符串，按硬件接口文档格式给出，每条数据中间由,分隔.数据条数不超过500条，超过请分批上传。
结果示例：
{
code: "0",
message: "上传成功",
data: null
}

#### 结构数据下载
http://open-test.bong.cn/device/data/getBlocks?mac=E69EB2618485&deviceId=1&date=2014-11-21
参数：
date:yyyy-mm-dd格式日期
结果示例：
{
code: "0",
message: "请求成功",
data: "{"sum":{"deviceId":1,"userId":2,"date":1416499200000,"calories":0.0,"step":0,"distance":0.0,"stillTime":120,"sleepNum":0,"dsNum":0,"sleepTimes":0,"finishFlag":1,"modifyTime":1420441906000},"blocks":[{"deviceId":1,"userId":2,"date":1416499200000,"startTime":1416550020000,"endTime":1416550080000,"type":3,"subType":1,"calories":0.0,"step":0,"distance":0.0,"actTime":0,"nonActTime":120,"values":[0,0,0,0],"isGps":0,"speed":0.0}]}"
}
具体含义请参考：https://github.com/Ginshell/bongOpenPlatform

#### 请求该用户设备最后一次同步的数据时间
http://open-test.bong.cn//device/data/uploadTime?mac=E69EB2618485&deviceId=1
参数如上：
结果示例：
{
code: "0",
message: "请求成功",
data: "{"time":1416550080000}"
}
## 固件SDK植入（初稿）：

第三⽅方⼿手环需要根据硬件⽂文档的指导,将算法嵌⼊入⾃自⼰己的固件,需要签署协议后获得。
- 注:此部分为bong已申请发明级专利,需要签署保密协议和专利使⽤用协议。 
1、执⾏行代码以函数库 lib ⽅方式提供。
2、所有函数为线性执⾏行,不会存在死循环,造成系统卡死。
3、所有涉及到算法的数据、函数以“bong_”开头,以作区分。
4、采样范围 4g。 
5、加速计采样率 25Hz,采样后的数据为 8bits 有符号整型。 
6、每秒定时传⼊入 1 次采⽤用数据,即每秒 25 个数据。 7、算法计算结果为 16 个 8bits 数据。 8、计算结果存储在 Flash,通过 app 上传服务器,服务器返回运动类型。 

具体参考附件中硬件资料:  bong_open_v1.01.zip 
