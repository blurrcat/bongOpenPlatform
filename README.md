###前言
***
欢迎你加入 bong 开放平台的大部落，对于每一位开发者我们都倍感珍贵。不需要掌握硬件知识和拥有专业设备，你就可以学习和运用通过传感器全时监测所获得前所未有的海量身体数据，开发出真正把虚拟带入现实的各种有趣应用，并且通过这些应用去改善你所关心的人们的生活。第三方应用将内嵌到 bong iOS 和 Android 软件中，每一位 bong 用户均可以访问到这些应用。

###加入开发者计划
任何对 bong 有兴趣的开发者都可以申请完全免费的开发者计划。加入的办法是发邮件到 <oliver@bong.cn>，主题写「加入开发者计划」并附上你的 portfolio。经过认证后，即可成为 bong 开放平台的开发者，获得授权开发者头衔，提前享受最新最火热的内测消息。我们更提供创意支持（用户投票的 [bong 原型菜园](http://openbong.lofter.com)），测试用户募集，用户调研，运营支持等全方位免费配套的服务，帮助你在快速成长的 bong 平台上获得成功。


###关于bong
bong 是世界上第一个能够全自动识别你的运动和睡眠状态的手环，只要佩戴在手腕上，无需手动切换识别状态。更重要的是，续航能力长达四周，省去频繁充电的烦恼，而且佩戴无感，不会有卡手的感觉。现在 bong 更进一步，通过开放接口与第三方应用开发者实现合作共赢，帮助你在可穿戴设备的第一波浪潮中抢得先机。


###OAuth 2 认证
####授权原理图示
![](https://raw.githubusercontent.com/Ginshell/bongOpenPlatform/master/images/auth.png)

###授权过程
1. [需要向我们提交的资料](signup.md)
2. [测试和正式环境的地址](address.md)
3. [请求 request_token](request_token.md)
4. [请求 access_token](access_token.md)
5. [用 access_token 请求 request_token](request_token.md)
5.
* 恶
	* 先提供应用名，重定向地址，app首页，67px * 67px 图标，应用描述。
	* 我们将分配给你接入 client_id 和 secret。
	* 测试平台 http://open-test.bong.cn，正式平台 http://open.bong.cn（以下同）
		* 请求方式： POST
		* 请求授权： oauth/authorize
			* 参数：client_id 应用对应的 id
			* 参数：rediret_uri 应用的回调地址
			* 例子：请求 https://open.bong.cn/oauth/authorize?client_id=hello_bong&redirect_uri=http://my.awesomeapp.com/bong
			* 返回值
				* code：返回码，用来接下来获取 access_token
		* 获取授权： oauth/token
			* 参数：client_id 同上
			* 参数：redirect_uri 同上
			* 参数：grant_type 必须为 "authorization_code"
			* 参数：请求授权中获得的 code
			* 例子：请求 https://open.bong.cn/oauth/token?client_id=hello_bong&grant_type=authorization_code&client_secret=dont_touch_it&redirect_uri=http://my.awesomeapp.com/bong&code=idontknow
			* 返回值
				* access_token: "i_got_the_token",
				* token_type: "bearer",
				* refresh_token: "you_need_to_store_this",
				* expire_in: seconds,
				* scope: "read",
				* uid: "my_alternate_ego",
				* refresh_token_expiration: "time_since_unix_begins"
			* access_token 过期时间为 12 小时
			* 使用 refresh_token 请求 access_token
				* 参数：client_id
				* 参数：client_secret
				* 参数：grant_type
				* 参数：refresh_token
				* 返回值与获取授权的返回值相同

###API说明
1. 名词说明
	* bongday: bongday 不是自然天(0:00 到 24:00)。bongday 是指当日入睡点到第二天入睡点之间的时段，含有用户的睡眠和运动的数据。比如昨晚 11:30 分入睡，今天晚上 10:30 分入睡，那么 bongday 数据的开始就是从昨天晚上 11:30 到今天晚上 10:30，而不是自然天的昨天晚上 0:00 到今天晚上的 0:00。
2. 运动数据接口
	* 说明：该接口返回某一个 bongday 的运动数据。每个人每天的运动数据是持续变化的，我们认为分为两大类，即bong（燃烧脂肪态）和非bong（相对静止态）。有了归类后，你就可以方便的对不同类型的运动进行归纳操作，从而制作出有趣的应用。因为 bong 不止识别运动静止态，还会进一步对不同的运动类型做出判断，以下会做详细说明。
	* 调用方法
		* 线上地址 https://open.bong.cn 测试环境 http://open-test.bong.cn
		* 正常调用 /1/bongday/blocks/YYYYMMDD?uid=user_id_get_from_oauth&access_token=token_get_from_oauth
			* YYYYMMDD: 查询的那一日，例子 20140101
			* uid: 认证中返回的 user_id
			* access_token: 认证中返回的 token
		* jsonp /1/bongday/blocks/20140419/jsonp?uid=user_id_get_from_oauth&callback=func_name&access_token=token_get_from_oauth
			* YYYYMMDD: 同上
			* uid: 同上
			* callback: 回调函数名
			* access_token: 同上
	* 返回数据
		* startTime: 该区块的开始时间，例如，用户早起跑步的跑步区块从 8:31 开始。
		* endTime: 该区块的结束时间，例如这个用户跑了 20 分钟，那么区块结束时间是 8:51 分。通过块的划分，可以使用户和开发者均能方便对一天的活动状态做出归纳。
		* type: 该区块的类型，用这个来区分bong和非bong状态。如果这个区段是bong态，则返回值是「2」，如果是比较平静的非bong态，则返回值是「3」。
		* distance: 用户在该区段间走过的距离，单位是米。
		* speed: 用户在该区段的平均运动速度，单位是千米每小时。
		* calories: 用户在这段时间内消耗了多少热量，单位是千焦耳。
		* steps: 该区段的步数，单位是步。
		* subType: 该区段的子运动类型，bong态（当上文中提到的 type 为「2」时）下有4个子类型，具体描述见下，非bong态（type 为「3」时）下有2个子类型，具体描述见下。
		* actTime: 该区段用户的活动时间，单位是秒。
		* nonActTime: 该区段用户的非运动时间，单位是秒。
	* bong（燃烧脂肪态）
		* 说明：在 bong 态下，也就是手环震动，手环 LED 指示灯开始点亮的状态下，用户进入了燃烧脂肪的运动状态。该状态下总共有4个子状态，type为2，subType返回值为1-4：
			* 1、热身运动：和字面意思相同，运动强度最轻的一类运动。
			* 2、健走：强度稍高。
			* 3、运动：球类等运动。
			* 4、跑步：有氧跑步运动。
	* 非bong（相对静止态），type为3，subType返回值为1-2：
		* 说明：非bong下共有2个子状态：
			* 1、静坐：例如坐在椅子上办公。
			* 2、散步：速度相当于走路。
3. 天综合数据接口
	* 说明：这个接口用于返回经过聚合了的bongday数据，只有一条，不分块。适合用来做统计类的应用。
	* 调用方法：
		* 调用地址 /1/bongday/dailysum/YYYYMMDD?uid=user_id_get_from_auth&access_token=token_get_from_auth
			* YYYYMMDD: 查询的当日，例子 20140101
			* uid: 认证返回的 user_id
			* access_token: 认证返回的 token
	* 返回数据：
		* Calories: 该日运动消耗的热量总和，单位是千焦耳。
		* Steps：该日的移动的步数综合，单位是步。 
		* Distance：该日移动的总距离，单位是米。
		* stillTime：该日的静坐时间总和，单位是秒。
4. 睡眠数据接口
	* 说明：这个接口获取用户每日的睡眠时长，质量评分，适合用做睡眠类应用。
	* 调用方法
		* 正常调用: /1/sleep/blocks/YYYYMMDD?uid=user_id_get_from_auth&access_token=token_get_from_auth
			* YYYYMMDD: 查询的当日，例子 20140101
			* uid: 认证返回的 user_id
			* access_token: 认证返回的 token
		* jsonp: /1/sleep/blocks/YYYYMMDD/jsonp?uid=user_id_get_from_auth&callback=func_name&access_token=token_get_from_auth
			* YYYYMMDD: 同上
			* uid: 同上
			* callback: 回调函数名
			* access_token: 同上
	* 返回数据：
		* startTime：睡眠开始时间
		* endTime：睡眠结束时间
		* type：该区段的用户状态（睡眠的type为1）
		* dsNum：深睡眠时长，单位：分钟
		* lsNum：浅睡眠时长，单位：分钟
		* wakeNum：清醒时长，单位：分钟
		* wakeTimes：清醒次数，单位：次
		* score：睡眠质量评分
				
###接入流程
1. 提交应用。认证开发者需要提交的内容有：应用名称，重定向 URI，APP 主页，图标，应用说明（OAuth 2）。
2. 测试开发。提交审核通过后，我们提供线上测试环境和必要的参数。
3. 联调上线。第三方应用和 bong 开放平台的测试环境联调成功后，可以申请接入正式环境，通过后正式发布到 bong app 内置的「我的应用」栏目中去。
4. 后期运营。我们目前会通过官方宣传渠道和应用内的「编辑推荐」的办法为有创意的应用提供持续的用户导入。

###什么样的应用将被拒绝
1. 暴露用户隐私 
2. 不符合国家有关法律法规
3. 广告或含有大量广告材料
4. 与宣传的功能不符或含有隐藏、虚假的功能
5. 界面粗糙，逻辑含有错误以及过于消耗手机电能
6. 载入速度过慢，导致界面崩溃
7. 推广不健康的生活方式
8. 内含错误的诊断信息
9. 与已上线的应用相似的应用

###什么样的应用将被下架
1. 滥用 API
2. 上线后的运营过程中出现了被拒绝应用中的任何一条的
3. 恶意记录、传播用户授权的信息

###运营支持
上线后，我们会持续对有创意有趣味的 app 进行运营上的支持和推广。
