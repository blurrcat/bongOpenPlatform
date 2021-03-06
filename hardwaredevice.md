### 为什么要合作
1. 对智能硬件来说，和bong连接的最大好处是：第一，用户不需要再掏出手机去控制自己的设备，随时随地可以控制自己的硬件，场景更自然；第二，用户还可以使用自动模式去触发一些更智能的场景。
 - bong X/XX 提供人体行为自动感应反馈机制。现在包括了，未来还会不断增加：    
     - 主动触摸行为
          - 上
          - 下
          - 左
          - 右
          - 长按
          - 顺时针
          - 逆时针
     - 自动模式识别
          - 睡
          - 醒
          - 起身
          - 坐下
          - 开始跑步
          - 结束跑步
          - 开始运动
          - 结束运动
          - 靠近
          - 离开
2. 品牌曝光提高销量。bong APP内可以提供直接购买的渠道，可以让你的硬件被所有的bong用户看到并购买。
3. 更多的腕部设备与你的设备互联。bong凭着自己的算法优势，还会接入其他品牌腕部设备，所有支持bong协议的腕部设备都可以和你的硬件进行互动。

### 如何合作
#### 什么是Trigger
- bong提供一些包装好的用户行为和用户操作，以广播包的形式向外发送，我们定义这些行为和操作为Trigger。我们会不断地增加更多的腕部设备和Trigger，让腕部设备与硬件之间的互动更为智能和有趣。

#### 什么Event
- 你的硬件可能有一些功能，可以被人们操作，这些行为我们称之为Event。
- 在接入前，你需要定义自己的硬件行为。比如对于一款灯来说，它可以有开和关的行为，它的Event可以定义为开和关。

#### 如何使用Trigger-Event来控制你的硬件
- 当Trigger发生时，手环会发出广播包，里面含有Trigger发生的标志和Mac地址等信息。第三方硬件扫描到广播包和Mac地址后和自己硬件里存储的Mac地址、Trigger-Event配置来匹配应该触发的Event。如果同时有多个Trigger被触发，按收到的广播包顺序处理。



#### 三种合作方式
方式一是我们推荐的模式，此模式下用户可以脱离手机直接控制你的硬件，对网络没有要求，不会产生时滞。
方式二需要植入我们的SDK，有少量的开发量，如果你的设备本身有自己的APP，可以快速实现连接，此模式对硬件的通讯方式没有要求，如果你不符合硬件直连的条件，也可以考虑此种方式。bongX/XX的SDK预计会在4月下旬发布。
方式三提供API，bong团队接入。如果你的设备有自己的API，可以考虑和bong团队进行深入的合作。

##### 方式一：硬件直连
- 基本要求
     - 适合需要灵活控制的蓝牙4.0硬件，无需通过手机中转，无需依赖网络，直接使用手环控制。
        - 支持蓝牙4.0
        - 蓝牙主设备
        - 需要更新固件支持《bong通讯与控制协议》（签署协议后可获得）
- 申请流程
    - [提交申请](http://www.mikecrm.com/f.php?t=5FJFxc)
    - 等待审核结果，我们会在1-3个工作日内完成审核
    - 审核通过，我们会联系你，签署协议，并提供更为详细的接入方案

##### 方式二：手机APP植入SDK中转
- 基本要求
     - 对硬件的通讯方式没有要求，需要自己调通手机到设备的连接部分。
        - 拥有一个iOS或者Android的应用可以用于控制自己的硬件
        - 花一些时间将[bong SDK](https://github.com/Ginshell/bongOpenPlatform#bong%E6%97%A0%E7%BA%BF%E5%BC%80%E6%94%BE%E5%B9%B3%E5%8F%B0)接入你的APP
- 申请流程
    - [提交申请](http://www.mikecrm.com/f.php?t=5FJFxc)
    - 等待审核结果，我们会在1-3个工作日内完成审核
    - 审核通过，我们会联系你，签署协议，并提供更为详细的接入方案

##### 方式三：通过硬件开放的API控制
- 基本要求
     - 需要bong团队介入开发。
        - 硬件需要本身已经拥有开放的Open API，可以用于控制自己的硬件。
- 申请流程
    - [提交申请](http://www.mikecrm.com/f.php?t=5FJFxc)
    - 等待审核结果，我们会在1-3个工作日内完成审核
    - 审核通过，我们会联系你，签署协议，并提供更为详细的接入方案

### 案例
#### Emie精灵灯
####
