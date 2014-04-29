###说明
这个接口用于返回经过聚合了的bongday数据，只有一条，不分块。适合用来做统计类的应用。
###调用方法：
调用地址： /1/bongday/dailysum/YYYYMMDD?uid=user_id_get_from_auth&access_token=token_get_from_auth

参数|说明
---|---
YYYYMMDD|查询的当日，例子 20140101
uid|认证返回的 user_id
access_token|认证返回的 token
	
### 返回数据：

参数|说明
---|---
Calories|该日运动消耗的热量总和，单位是千焦耳。
Steps|该日的移动的步数综合，单位是步。 
Distance|该日移动的总距离，单位是米。
stillTime|该日的静坐时间总和，单位是秒。