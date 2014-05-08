###说明
这个接口获取用户的头像，图片文件以 base64 的方式编码。
###调用方法
调用地址： 1/userInfo/avatar/uid?access_token=access_token
参数|说明
---|---
uid|用户id
access_token|access token

#返回数据
```json
{
 "code":"200",
 "message":"成功标示!",
 "value":""
}
```
参数|说明
---|---
value|base64位编码的图片
