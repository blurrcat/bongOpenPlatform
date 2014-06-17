###说明
这个接口获取用户每日的睡眠时长，质量评分，适合用做睡眠类应用。
###调用方法
#####正常调用:
调用地址： /1/sleep/blocks/yyyymmdd?uid=uid&access_token=token_get_from_auth

参数|说明
---|---
yyyymmdd|查询的当日，例子 20140101
uid|认证返回的 uid
access_token|认证返回的 token

#####jsonp: 
调用地址： /1/sleep/blocks/yyyymmdd/jsonp?uid=uid&callback=func_name&access_token=token_get_from_auth

参数|说明
---|---
yyyymmdd|同上
uid|同上
callback|回调函数名
access_token|同上
###返回数据：

```json
{
    "code": "200", 
    "message": "成功标示!", 
    "value": [
        {
            "startTime": "2014-04-27 01:01:00", 
            "endTime": "2014-04-27 09:17:00", 
            "type": 1, 
            "dsNum": 113, 
            "lsNum": 384, 
            "wakeNum": 0, 
            "wakeTimes": 0, 
            "score": 3.00
        }
    ]
}
```

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


### 获取多日数据

#####正常调用
地址 1/bongday/dailysum/yyyymmdd/n?uid=uid&access_token=access_token
#####jsonp 调用
地址 1/bongday/dailysum/yyyymmdd/n/jsonp?uid=uid&access_token=access_token

#####说明
yyyymmmdd: 起始日期。例子：20140313
n: 从起始日期开始的天书。例子：7
