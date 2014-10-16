
##无线开放 

###[首页 http://bong.cn/sdk](http://bong.cn/sdk)

###开放轨迹

####1. 2014-10-11 
第一版android SDK 即1.0.0版
- 获取 Yes! 键 短触、长触事件
- 获取 传感器三轴数据

直接触摸 Yes! 键：1秒触发短触,3秒以上激活数据模式，可获取三轴原始数（持续200秒）。

###快速调试

- 1. 登记：注册新应用，获取appid（现阶段暂用“common”可略过此步）
- 2. [下载](http://bong.cn/sdk/bong-sdk-1.0.0.zip)：SDK开发包，可运行Demo测试。
- 3. 安装开发包里的 SDK专用APK，登录，点击设置里查看“Yes! 键”选项，如果没启用，设置开启。
- 4. 至此可以退出app了，运行Demo，触摸Yes!键，可查看事件记录

###快速集成

- 1. 集成： 引lib：将开发包里libs文件夹里jar包拷入你项目的libs并引入项目。
- 2. 使用：
    - 初始化：BongManager.initialize(this,appId,appKey);正确输入bong.cn分配给你的appid和appkey，否则可能导致异常。
    - 事件监听：BongManager.turnOnEventListen(dataEventListener, touchEventListener);传入一个或两个监听器来获取数据。
    - 停止监听：BongManager.turnOffEventListen();和turnOnEventListen对应，开启后不要忘记在合适时间调用关闭。
    - 动态开启传感器监听：BongManager.bongStartSensorOutput();开启读取传感器数据
    - 动态关闭传感器监听：BongManager.bongStopSensorOutput();关闭传感器数据读取，开启读取后请在合适时间调用关闭。
- 3. 其他参见 demo 项目。

