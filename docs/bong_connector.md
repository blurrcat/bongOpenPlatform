
##无线开放 

###[首页 http://bong.cn/share](http://bong.cn/share)

###bong connector文档

更详细文档可向share@bong.cn索取。

### 一、概述
Connector是一个同时具有主从能力的蓝牙4.1适配器。可以实现与手环等从设备连接，读取手环数据或者广播报文控制信息；也可以作为从设备与手机app连接配置，并支持iBeacon。

####主模式
主模式分为两个通道，即数据通道和控制通道。数据通道需要连接从设备实现数据上下行传输；控制通道仅需要抓取从设备的广播报文信息从而进行相应的控制命令处理。
Connector实时获取周围的蓝牙mac等信息(手环需要触摸YES键，才会广播数据5秒钟，没有连接请求则进入休眠模式)，并跟通过与在flash中的mac地址做鉴权匹配(flash中的mac由用户授权后写入)，鉴权成功之后往串口送mac信息，否则舍弃。（具体是否需要连接手环由服务端处理，提供连接手环等串口命令）
Connector实现命令：1.写时间 2.读取数据 3.读取系统信息 4.测试命令

####从模式
从模式可以通过手机APP实现与Connector的连接，并通过手机读取数据和下发控制信息。

###二、硬件接口

####提供usb接口，使用的CH341芯片
波特率：9 6 00  格式：N 8 1

####提供uart接口
波特率：9 6 00  格式：N 8 1

###三、通信协议

####读数据通信流程
####通信报文定义
- 3.2通信报文定义
报文格式
整个报文的最大长度不能超过32个byte。
整个报文包括上图中所有的部分。
前导序列
每个报文必须已一个前导序列开始，否则忽略该报文。前导序列为两个byte 0xFF, 0xFF
报文长度
报文长度为一个byte，指示包含本byte在内后续报文的总长度（除checksum）。
报文长度的内容=1 + 报文内容的长度。 其中1指的就是报文长度本身占一个字节
报文内容
包括了命令和参数，不同的命令可能会有不同的参数
Checksum
Checksum共有两个byte。两个byte的值=报文长度和报文内容所有byte值相加。用来验证报文是否合法
超时
一个报文中每个byte的发送间隔不能超过250ms，否则会被当成一个新报文来处理。
举例
发送一个长度为1，内容为5的报文。总体报文应该如下：
0xFF 0xFF 0x02 0x05 0x00 0x07
0xFF 0xFF 为前导序列
0x02 为报文长度。 长度包括1byte内容，和本身的1byte。结果为2
0x05 是报文内容
0x00 0x07是checksum。等于报文长度加报文内容各个byte相加。 这里等于2+5=7
- 3.3发送报文
 
发送报文指的是PC发给connector的报文。报文内容主要由两部分组成：命令字和参数。
命令字：用来指示connector动作
参数：有些命令字需要一些参数来完成动作。不需要参数的命令字可忽略此字段。具体参数在每个命令字中有定义。
- 3.4接收报文
 
接收报文指的是由connector发出由PC接收的报文。
接收报文和发送报文相比在命令字和参数中间多了一个结果字段。主要是显示送主机发送过来的命令的处理结果。
- 3.5查询设备列表
命令字：0x02
参数：N/A
报文举例：FF FF 02 02 00 04
f1 0b e0 FF FF 02 02 00 04 e2 fa
F0 0B E0 FF FF 03 02 00 00 05 E3 FA 
报文时序
 
1.	由PC发起查询设备列表的命令
2.	Connector返回扫描到的设备，每个设备信息作为单独报文返回。上图中有两个设备，所以发送了两条设备信息结果。注意：返回设备报文的操作结果为8，8表示过程数据。
3.	在所有的设备都发送完毕之后，Connector会发送操作成功给PC表示本次操作完成
- 3.6连接设备
命令字：0x03
参数：mac地址
返回值：FF FF 03 03 00 00 06
在获取到设备列表之后，由PC决定是否连接以及连接哪个设备。在请求连接时PC需要把mac地址作为参数和连接请求一起发送过来。下图假设PC请求连接mac地址为11 22 33 44 55 66的设备。
 
- 3.7读取手环数据
命令字：0x01
参数: N/A 
返回值：FF FF 03 01 00 00 04
在连接到设备之后，PC可以发送请求来读取手环中的数据。
 
这个通信逻辑和读取设备列表的基本一样。手环数据会作为过程数据（返回结果为8），在每一条记录里返回，最后会返回操作结果。
- 3.8添加蓝牙MAC地址
命令字：0x04
参数：mac地址
命令示例：FF FF 08 04 EC 11 4F 8E 11 F6 02 ED
//f1 0b e0 FF FF 08 04 EC 11 4F 8E 11 F6 02 ED b6 fa
返回值：FF FF 03 04 00 00 07 
//F0 0B E0 FF FF 03 04 00 00 07 E7 FA 
- 3.9保存蓝牙接口列表
命令字：0x07
参数：N/A
命令示例：FF FF 02 07 00 09
f1 0b e0 FF FF 02 07 00 09 ec fa
返回值：FF FF 03 07 00 00 0A 
F0 0B E0 FF FF 03 07 00 00 0A ED FA 
- 3.A查询蓝牙Mac列表
命令字：0x06
参数：N/A
命令示例: FF FF 02 06 00 08
f1 0b e0 FF FF 02 06 00 08 ea fa
返回参考：FF FF 09 06 08 EC 11 4F 8E 11 F6 02 F8 FF FF 03 06 00 00 09
F0 0B E0 FF FF 09 06 08 EC 11 4F 8E 11 F6 02 F8 CB FA F0 0B E0 FF FF 03 06 00 00 09 EB FA
- 3.B删除蓝牙MAC地址
命令字：0x05
参数：mac地址
命令示例：FF FF 08 05 EC 11 4F 8E 11 F6 02 EE
f1 0b e0 FF FF 08 05 EC 11 4F 8E 11 F6 02 Ee b8 fa
返回值：FF FF 03 05 00 00 08  
F0 0B E0 FF FF 03 05 00 00 08 E9 FA 
- 3.C使能开门
命令字：0x08
参数：flag=0关闭门。Else 开门
示例命令：FF FF 03 08 01 00 0C
f1 0b e0 FF FF 03 08 01 00 0C f2 fa

返回值：FF FF 03 08 00 00 0B
F0 0B E0 FF FF 03 08 00 00 0B EF FA 
- 3.D主动上报功能
命令字：0x09
参数：控制状态参数
push值：FF FF 09 09 00 EC 11 4F 8E 11 F6 02 F3
F0 0B E3 FF FF 09 09 00 EC 11 4F 8E 11 F6 02 F3 C4 FA
F0 0B E3 FF FF 09 09 03 EC 11 4F 8E 11 F6 02 F6 CA FA
FF FF 是前导序列、09是长度、09是命令字、红色的03是控制状态参数(现支持的状态有00无按键按下，01有按键按下，03长按，05短按)、EC开头6字节是MAC地址,最后2字节是CheckSum
- 3.E查询本机MAC地址
命令字：0x0A
参数：N/A
返回值示例：FF FF 09 0A 00 27 77 9F CD C3 EE 03 CE

- 3.10错误码定义
 
如下记录了目前所有Connector有可能返回的结果。
```
#define COMM_ERR_OK                     0       /* Operation success */
#define COMM_ERR_INVALID_LEN            1       /* Length invalid. maybe too long, beyonded the limitation */
#define COMM_ERR_CHECKSUM_FAIL         2       /* Checksum can't match the packet */
#define COMM_ERR_UNKNOW_CMD          3       /* Unknow CMD */
#define COMM_ERR_SYS_BUSY              4       /* System busy, Connector is processing some command. Couldn't process another */
#define COMM_ERR_CONNECTION_NOT_READY   5       /* Connector havn't connected to a peripherals */
#define COMM_ERR_EN_NOTIFICATION_FAIL   6       /* Enable bong notification fail */
#define COMM_ERR_OPERATION_TIMEOUT      7       /* Processing timeout */
#define COMM_ERR_PROCESSING_DATA        8       /* Indicate that this packet contain processing data */
#define COMM_ERR_CANT_FIND_DEVICE       9       /* Device in connect request packet is not existed */
#endif
```


###蓝牙命令
和串口命令十分的类似，只有如下区别：
- 1. 蓝牙在发送命令的时候不需要发两个0xFF的前导序列。 拿读设备列表来说，串口命令是FF FF 02 02 00 04，蓝牙命令是02 02 00 04
- 2. 在读手环采集数据的时候，采集上报的数据没有checksum字段
注意：除了上面提到的采集数据没有checksum字段之外，所有的相应流程均和串口一致

####
####
####
####
####
####

