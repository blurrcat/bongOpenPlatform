
##无线开放 

###[首页 http://bong.cn/sdk](http://bong.cn/sdk)

###开放轨迹

####1. 2014-10-11 
第一版ndroid SDK 即1.0.0版
- 获取 Yes! 键 短触、长触事件
- 获取 传感器三轴数据
- bong点亮
- bong震动

###快速调试

- 1. 登记：注册新应用，获取appid（现阶段暂用“common”可略过此步）
- 2. [下载](http://bong.cn/sdk/bong-sdk-1.0.0.zip)：SDK开发包，可运行Demo测试。
- 3. 安装开发包里的 SDK专用APK，登录，点击设置里查看“Yes! 键”选项是否开启，确认开启后可退出app，运行Demo，可查看事件记录。

###快速集成

- 1. 集成： 引lib：将开发包里libs文件夹里jar包拷入你项目的libs并引入项目。
- 2. 使用：
    - 初始化：BongManager.initialize(this);
    - 事件监听：BongManager.turnOnEventListen(dataEventListener, touchEventListener);
    - 停止监听：BongManager.turnOffEventListen();
    - 震动：BongManager.bongVibrate();
    - 点灯：BongManager.bongLight();
    - 动态开启传感器监听：BongManager.bongStartSensorOutput();
    - 动态关闭传感器监听：BongManager.bongStopSensorOutput();
- 3. 其他参见 demo 项目。

###一些特性

- 1. 直接触摸 Yes! 键：1秒触发短触,3秒以上激活数据模式，可获取三轴原始数（持续200秒）。
- 2. 因硬件特性 bongII 需在命令发出前、后20秒内短触 Yes!键 才能建立连接，连接后20秒无指令即断开。