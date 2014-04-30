###签名密钥
#####说明
开发者申请应用时需要提交一个 首页url，该url作为用户在bong的手机app中点击开发者应用时跳转到开发者应用的地址，同时会在url后面跟上代表当前用户的 uid，为了帮助开发者验证请求是否来自bong，以及是否请求指定 uid 的用户数据，增加了 签名和时间戳两个参数。
#####参数
参数|说明
---|---
sign|当前请求url的验证签名
uid|uid
timestamp|时间戳，用于增强安全性，防止过时的连接
#####签名验证方法
获取请求中除了 sign 外所有参数的 key、value 键值对，除去值为空的键值对
把所有key排序，并按照“参数=参数值”的模式用“&”字符拼接成字符串
如：source=bong&timestamp=1398594572050uid=22088882811149902693
在生成的字符串后加上签名秘钥 xxx（申请应用后会获得该秘钥） 得到 source=bong&timestamp=1398594572050uid=22088882811149902693xxx
再对字符串进行 md5 加密，得到 sign，跟请求参数中的 sign做对比
示例：开发者提交的 首页url 为：http://developer.com/dev?source=bong
最后请求开发者应用的请求为：http://developer.com/dev?sign=e5abb5a1337f70f5a52e052c93e01c0e&uid=22088882811149902693&timestamp=1398594572050&source=bong



