
##无线开放 

###[首页 http://bong.cn/share](http://bong.cn/share)

###开放轨迹

####1. 2014-10-11 
第一版android SDK 即1.0.0版
- 获取 Yes! 键 短触、长触事件
- 获取 传感器三轴数据
- 获取用户、绑定bong信息

触摸Yes!键大约1秒为短触，3秒为长触，手机也会对应有短、长震动

###快速调试

- 1. [登记](http://bong.cn/share/mobile.html)：注册新应用，获取appid（现阶段暂用“common”可略过此步）
- 2. [下载](http://bong.cn/share/bong-sdk-android.zip)：SDK开发包，可运行Demo测试。
- 3. 安装开发包里的 SDK专用APK，登录并进入设置里查看“Yes! 键”选项确认开启状态。
- 4. 至此可以退出app了，运行Demo，触摸Yes!键，可查看事件记录

###快速集成

- 1. 集成： 将开发包里libs文件夹里jar包拷入你项目的libs文件夹并引入项目。
- 2. 注册： 将下面receiver注册到你项目的manifest文件
```xml
        <receiver android:name="cn.bong.android.sdk.BongDataReceiver">
            <intent-filter>
                <action android:name="cn.bong.android.action.common"/>
            </intent-filter>
        </receiver>
```
- 3. 使用：

####初始化
```java
        // 初始化
        BongManager.initialize(this, "appid");
        // 开启 调试模式，打印日志
        BongManager.setDebuged(true);
```
####开启触摸监听
```java
        // 1. 开启 bong 触摸监听 实例 
        BongManager.turnOnTouchEventListen(new TouchEventListener() {
            @Override
            public void onTouch(TouchEvent event) {
            }

            @Override
            public void onLongTouch(TouchEvent event) {
            }
        });
```

####关闭触摸监听
```java
        // At last. 关闭 bong 触摸监听
        BongManager.turnOffTouchEventListen();
```
####获取用户信息示例 
```java
        // 1. 将会刷新获取最新的用户信息
        BongManager.refreshUserInfo(new UserInfoListener() {
            @Override
            public void onReceive(BongUser bongUser) {
            
            }
        });
        
        // 2. 另外一种获取user的方法：有可能不是最新
        BongUser bongUser = BongManager.getBongUser();
```
###注意：下面方法只在接收到触摸事件后10秒内发出命令方可被bong接受
####开启传感器示例 
```java
        BongManager.bongStartSensorOutput(new DataEventListener() {
            @Override
            public void onReceive(DataEvent event) {
                events.add(();
            }
        });
```
####关闭传感器示例 
```java
        BongManager.bongStopSensorOutput();
```
####震动示例  
```java
         BongManager.bongVibrate();
```
####点亮示例  
```java
        BongManager.bongLight();
```

- 3. 其他参见 demo 项目。

