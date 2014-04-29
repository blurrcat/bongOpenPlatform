###用 refresh_token 重新请求 access_token

#####说明
access_token的时效是12小时，当用户初次授权完成后，为了避免再次授权，开发者可以通过捕获异常响应（401），用 refresh_token 来重新请求 access_token。
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
access_token|获取授权后的 access token
token_type|bearer
refresh_token|accesstoken 失效后用来重新获取 新的 access token
expires_in|有效时间，单位是秒
scope|
uid|当前授权用户的 user id
refresh_token_expiration|refresh_token 过期时间
#####例子
> http://open-test.bong.cn/oauth/token?client_id=id&grant_type=refresh_token&client_secret=secret&refresh_token=token

