###说明
这个接口获取用户的头像.
####返回图片的URL地址
图片文件URL的形式返回。
###调用方法
调用地址： 1/userInfo/avatar/img/uid?access_token=access_token

参数|说明
---|---
uid|用户id
access_token|access token
###返回数据
```json
{
    "code": "200",
    "message": "成功标示!",
    "value": "http://bongapptest.qiniudn.com/165.png?e=1418898591&token=t1eeOI6ZlbovsEOAHRnX9glBB79UOKvME5W3XO-a:D3FfvkLHlTIcB-Z7cVsKQECp1MM="
}
```

参数|说明
---|---
value|图片的URL地址

####返回图片的base64编码
这个接口获取用户的头像，图片文件以 base64 的方式编码。
**该接口将不再维护，推荐使用返回图片URL地址的接口。**
###调用方法
调用地址： 1/userInfo/avatar/uid?access_token=access_token

参数|说明
---|---
uid|用户id
access_token|access token
###返回数据
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
