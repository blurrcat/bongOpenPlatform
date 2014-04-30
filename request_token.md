###请求 request_token

#####请求地址 
/oauth/authorize
#####请求方式
GET/POST
#####参数说明
参数|是否必须|说明
---|---|---
client_id|是|分配给应用的id
redirect_uri|否|授权回调地址,注册应用时有填写的话可以不传
response_type|是|code
state|否|
#####返回值
返回值|说明
---|---
code|用来请求 access_token
state|传递这个参数，会回传相同的参数
#####例子
> http://open-test.bong.cn/oauth/authorize?client_id=id&redirect_uri=http://yourapp.com&response_type=code
