###说明
这个接口用于返回经过聚合了的bongday数据，只有一条，不分块。适合用来做统计类的应用。
###调用方法：
调用地址： /1/bongday/dailysum/yyyymmdd?uid=uid&access_token=token

参数|说明
---|---
yyyymmdd|查询的当日，例子 20140101
uid|认证返回的 uid
access_token|认证返回的 token
	
### 返回数据：

```json
{
    "code": "200", 
    "message": "成功标示!", 
    "value": {
        "calories": "1440.33", 
        "steps": 6762, 
        "distance": "4613.75", 
        "stillTime": 43766, 
        "date": "20140427"
    }
}
```

参数|说明
---|---
calories|该日运动消耗的热量总和，单位是千焦耳。
cteps|该日的移动的步数综合，单位是步。 
distance|该日移动的总距离，单位是米。
stillTime|该日的静坐时间总和，单位是秒。
date|yyyymmdd,日期
