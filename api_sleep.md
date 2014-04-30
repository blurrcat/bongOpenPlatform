###说明
这个接口获取用户每日的睡眠时长，质量评分，适合用做睡眠类应用。
###调用方法
#####正常调用:
调用地址： /1/sleep/blocks/YYYYMMDD?uid=uid&access_token=token_get_from_auth

参数|说明
---|---
YYYYMMDD|查询的当日，例子 20140101
uid|认证返回的 uid
access_token|认证返回的 token

#####jsonp: 
调用地址： /1/sleep/blocks/YYYYMMDD/jsonp?uid=uid&callback=func_name&access_token=token_get_from_auth

参数|说明
---|---
YYYYMMDD|同上
uid|同上
callback|回调函数名
access_token|同上
###返回数据：

参数|说明
---|---
startTime|睡眠开始时间
endTime|睡眠结束时间
type|该区段的用户状态（睡眠的type为1）
dsNum|深睡眠时长，单位：分钟
lsNum|浅睡眠时长，单位：分钟
wakeNum|清醒时长，单位：分钟
wakeTimes|清醒次数，单位：次
score|睡眠质量评分
