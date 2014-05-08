###说明
这个接口用户返回用户的头像，图形文件以 base64 方式编码。
###调用方法
调用地址： /1/userInfo/uid?access_token=access_token

参数|说明
---|---
uid|用户id
access_token| access token

###返回数据

```json
{"code":"200","message":"成功标示!","value":{"name":"能能","gender":1,"birthday":1985,"weight":71.00,"height":174.00,"targetSleepTime":480,"targetCalorie":2147}}
```
