###用 refresh_token 重新请求 access_token

#####说明
当前线上环境access_token的时效默认是12小时，当access_token失效后，HTTP请求会返回401，error=invalid_token，开发者的应用需要具备遇到该情况时用 refresh_token 来重新获取 access_token 的功能。
#####请求地址 
/oauth/token
#####请求方式
POST
#####参数说明
参数|是否必须|说明
---|---|---
client_id|是|分配给应用的id
client_secret|是|分配给应用的密钥
grant_type|是|必须为 `refresh_token`
refresh_token|是|refresh token
#####返回值
返回值|说明
---|---
access_token|接口获取授权后的 access token
token_type|bearer
refresh_token|用来获取新的 access token
expires_in|access_token的生命周期，单位是秒数
scope|暂时无用
uid|当前授权用户的 uid
refresh_token_expiration|refresh_token 过期时间
#####例子
> http://open-test.bong.cn/oauth/token?client_id=id&grant_type=refresh_token&client_secret=secret&refresh_token=token

