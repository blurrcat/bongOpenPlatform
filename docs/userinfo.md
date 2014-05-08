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
{
  "code":"200",
  "message":"成功标示!",
  "value":{
    "name":"能能",
    "gender":1,
    "birthday":1985,
    "weight":71.00,
    "height":174.00,
    "targetSleepTime":480,
    "targetCalorie":2147
  }
}
```

参数|说明
---|---
name|用户姓名
gender|性别：1 为男，2 为女
birthday|生日年份
weight|体重，单位为kg
height|身高，单位为cm
targetSleeptTime|目标睡眠时间，单位分钟
targetCalorie|目标卡路里消耗，单位千焦
