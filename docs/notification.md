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
calories|该日运动消耗的热量总和，单位是千焦耳。
steps|该日的移动的步数综合，单位是步。 
distance|该日移动的总距离，单位是米。
stillTime|该日的静坐时间总和，单位是秒。
sleepNum|总睡眠时长,单位是分钟
dsNum|深睡眠时长,单位是分钟
sleepTimes|睡眠次数
complete|该日数据是否完整。  0:不完整    1:完整
date|yyyymmdd,日期。
