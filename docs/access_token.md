###请求 access_token

#####请求地址 
/oauth/token
#####请求方式
POST
#####参数说明
参数|是否必须|说明
---|---|---
client_id|是|分配给应用的id
client_secret|是|分配给应用的密钥
grant_type|是|必须为 `authorization_code`
code|是|请求request_token中返回的code   
redirect_uri|否|注册应用时有填写的话可以不传
#####返回值
返回值|说明
---|---
access_token|接口获取授权后的 access token
token_type|bearer 暂时无用
refresh_token|用来获取新的 access token
expires_in|access_token的生命周期，单位是秒数
scope|暂时无用
uid|当前授权用户的 uid
refresh_token_expiration|refresh_token 过期时间
#####例子
> http://open-test.bong.cn/oauth/token?client_id=id&grant_type=authorization_code&client_secret=secret&redirect_uri=uri&code=code
