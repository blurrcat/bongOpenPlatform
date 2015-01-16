
##无线开放 

###[首页 http://bong.cn/share](http://bong.cn/share)

###开放轨迹

####2014-10-11 
第一版android SDK 即1.0.0版
- 获取 Yes! 键 短触、长触事件
- 获取 传感器三轴数据
- 获取用户、绑定bong信息

android SDK 1.1.0版新增：
- 授权打通，调用SDK方法可以快速完成授权获取API访问令牌。
- 环境切换，可以切换线上、预发、测试三个环境
- 开闭调试模式，日志一键开关
- 可以云端配置[长、短触]事件啦，借助此项结合长短触，甚至可以不接入SDK来极速启动你的App：可配置是否点亮屏幕、解锁，支持broadcast、activity、service三种intent拉起方式。

触摸Yes!键大约1秒为短触后松开(亮灯一次)，3秒触摸为长触（亮灯两次），[手机]也会对应有短、长震动反馈。

###快速调试

- 1. [下载](http://bong.cn/share/bong-sdk-android.zip)：SDK开发包，可运行Demo测试。
- 2. 安装开发包里的SDK测试用APK，登录bong app并进入[设置]里查看“Yes! 键”选项确认开启状态。
- 3. 至此可以退出app了，运行Demo，触摸Yes!键，可查看事件记录

###调测、账号注意事项
1. [测试环境]和[线上环境]的账号、数据完全独立，交叉登录则会报密码错误或未注册；开发者可到[开放平台](http://www.bong.cn/share/mobile.html)
下载测试环境的安装包，来自助注册测试账号并在测试环境使用、调测。[点击直接下载](http://bongads.b0.upaiyun.com/bong-sdk-android.zip)
2. 默认最初分配的AppID、AppKey等信息仅对测试环境生效，意味着只能在测试环境授权通过并调测，否则可能报授权失败。
3. 测试完成将要上线时请到[开放平台](http://www.bong.cn/share/mobile.html) 或者联系 share@bong.cn申请上线，通过后线上环境即生效。

###开发环境
1. 测试环境：发布前测试专用，线上用户看不到。
2. 预发环境：预发环境（GM）和线上环境账号、数据体系一致，预发环境测试通过后将发布线上。
3. 正式环境：发布给线上用户使用。

###快速集成


- 1. 注册:  点击[申请创建应用](http://bong.cn/share/mobile.html)，注册并获取AppID、AppKey、AppSecret等信息。
- 2. 集成： 将开发包里libs文件夹里jar包拷入你项目的libs文件夹并引入项目。
- 3. 注册： 将下面权限等信息注册到你项目的manifest文件。

permission
```xml
    <uses-permission android:name="android.permission.INTERNET"/>
```
application
```xml
    <activity
            android:name="cn.bong.android.sdk.api.AuthActivity"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"/>
    <receiver android:name="cn.bong.android.sdk.BongDataReceiver">
        <intent-filter>
            <action android:name="cn.bong.android.action.common"/>
        </intent-filter>
    </receiver>
```
- 4. 使用简介：

####初始化
必须要到开放平台[申请]申请专属的获取AppID、AppKey、AppSecret等信息。
```java
        // 初始化（只接收触摸事件仅需appid即可）
        BongManager.initialize(this, "appid");
        // 或者(接入api时需要key和secret，且请注意appsecret的保密工作，防止被盗用)
        BongManager.initialize(this, "appid", "appkey", "appsecret"); 
        // 开启 调试模式，打印日志（默认关闭）
        BongManager.setDebuged(true);
        // 设置 环境（默认线上）：Daily（测试）,  PreDeploy（预发，线上数据）, Product（线上）;
        BongManager.setEnvironment(Environment.Daily);
```

####释放资源

```java
        // 会释放所有监听者（如果有的话），清空各种数据，释放一切资源，恢复到调用initialize方法前的状态。
        BongManager.releaseAll();
```

####开启触摸监听
```java
        // 开启 bong 触摸监听 实例 
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
        // At last. 关闭 bong 触摸监听（和开启是一对，请注意在合适的时候注销监听防止内存泄露）
        BongManager.turnOffTouchEventListen();
```

####开启授权（注意一次授权有效期为3个月，所以注意不要频繁调用，仅当没有授权和授权过期时才调用此方法）
注意SDK初始化时输入你的AppID以及AppKey，测试环境注册一个测试账号，调用下面方法，就能在demo中通过登录来完成授权了。
```java
      // 开启 bong 触摸监听 实例 
      BongManager.bongLogin(this, "demo", new UiListener() {
           @Override
           public void onError(AuthError error) {
           }

           @Override
           public void onSucess(AuthInfo result) {
           }

           @Override
           public void onCancel() {
           }
       });
```

####授权相关方法
```java
      // 清除AccessToken和UserID等信息并登出
      BongManager.bongLogout();
      // 判断是否有AccessToken和UserID
      BongManager.isSessionValid();
      // 获取AccessToken
      BongManager.getAccessToken();
      // 获取UserID
      BongManager.getLoginUid();
```

####获取bong app 当前登录用户信息示例 
```java
        // 1. 将会刷新获取最新的用户信息（此监听在得到一次反馈后会自动释放，不需要解显式注销监听）
        BongManager.refreshUserInfo(new UserInfoListener() {
            @Override
            public void onReceive(BongUser bongUser) {
            
            }
        });
        
        // 2. 另外一种获取user的方法：有可能不是最新
        BongUser bongUser = BongManager.getBongUser();
```
###注意：下面方法只在接收到触摸事件后的短时间内发出命令方可被bong2接受（bong2仅在触摸时发广播包）
####开启传感器示例 
```java
        BongManager.bongStartSensorOutput(new DataEventListener() {
            @Override
            public void onReceive(DataEvent event) {
            }
        });
```
####关闭传感器示例 
```java
        和开启传感器是一对，请注意在合适的时候注销监听防止内存泄露
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

