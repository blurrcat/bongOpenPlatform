###说明
该接口返回某一个 bongday 的运动数据。每个人每天的运动数据是持续变化的，我们认为分为两大类，即bong（燃烧脂肪态）和非bong（相对静止态）。有了归类后，你就可以方便的对不同类型的运动进行归纳操作，从而制作出有趣的应用。因为 bong 不止识别运动静止态，还会进一步对不同的运动类型做出判断，以下会做详细说明。

###调用方法

#####正常调用 
地址：/1/bongday/blocks/YYYYMMDD?uid=user_id_get_from_oauth&access_token=token_get_from_oauth

参数|说明
---|---
YYYYMMDD|查询的那一日，例子 20140101
uid|认证中返回的 user_id
access_token|认证中返回的 token

#####jsonp 调用 
地址： /1/bongday/blocks/20140419/jsonp?uid=user_id_get_from_oauth&callback=func_name&access_token=token_get_from_oauth

参数|说明
---|---
YYYYMMDD|同上
uid|同上
callback|回调函数名
access_token|同上

###返回数据
参数|说明
---|---
startTime| 该区块的开始时间，例如，用户早起跑步的跑步区块从 8:31 开始。
endTime|该区块的结束时间，例如这个用户跑了 20 分钟，那么区块结束时间是 8:51 分。通过块的划分，可以使用户和开发者均能方便对一天的活动状态做出归纳。
type|该区块的类型，用这个来区分bong和非bong状态。如果这个区段是bong态，则返回值是「2」，如果是比较平静的非bong态，则返回值是「3」。
distance|用户在该区段间走过的距离，单位是米。
speed| 用户在该区段的平均运动速度，单位是千米每小时。
calories| 用户在这段时间内消耗了多少热量，单位是千焦耳。
steps|该区段的步数，单位是步。
subType| 该区段的子运动类型，bong态（当上文中提到的 type 为「2」时）下有4个子类型，具体描述见下，非bong态（type 为「3」时）下有2个子类型，具体描述见下。
actTime|该区段用户的活动时间，单位是秒。
nonActTime|该区段用户的非运动时间，单位是秒。
###bong（燃烧脂肪态）
#####说明
在 bong 态下，也就是手环震动，手环 LED 指示灯开始点亮的状态下，用户进入了燃烧脂肪的运动状态。该状态下总共有4个子状态，type为2，subType返回值为1-4：
subType返回值|对应运动|说明
---|---|---
`1`|热身运动|和字面意思相同，运动强度最轻的一类运动。
`2`|健走|强度稍高。
`3`|运动|球类等运动。
`4`|跑步|有氧跑步运动。
###非bong（相对静止态），type为3，subType返回值为1-2：
#####说明
非bong下共有2个子状态：
subType返回值|对应运动|说明
---|---|---
`1`|静坐|例如坐在椅子上办公。
`2`|散步|速度相当于走路。