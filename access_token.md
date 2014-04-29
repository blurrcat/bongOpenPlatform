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
redirect_uri|否|
#####返回值
返回值|说明
---|---
access_token|获取授权后的 access token
token_type|bearer
refresh_token|accesstoken 失效后用来重新获取 新的 access token
expires_in|有效时间，单位是秒
scope|
uid|当前授权用户的 user id
refresh_token_expiration|refresh_token 过期时间
#####例子
> http://open-test.bong.cn/oauth/token?client_id=id&grant_type=authorization_code&client_secret=secret&redirect_uri=uri&code=code