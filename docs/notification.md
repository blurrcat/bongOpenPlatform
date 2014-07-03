###说明
这个接口用于向用户发送通知，目前只适用于 iOS。

###调用方法：
调用地址：1/notification/uid?message=message&access_token=token

参数|说明
---|---
uid|认证返回的 uid
message|推送给用户的消息
access_token|认证返回的 token
	
### 返回数据：

```json
{
"code":"200",
"message":"成功标示!",
"value":null
}
```

参数|说明
---|---
code|200表示成功，其他为失败
