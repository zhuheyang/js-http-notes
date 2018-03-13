# Get与Post

## GET

GET方式传送数据量小，处理效率高，且可会被缓存从而提到利用效率, 但也因此可根据历史记录访问到用户名, 密码, 消息等敏感信息, 因而安全性低.  

特点: Get方式具有幂等性, 相同的url会返回相同的结果, 则为防止幂等性访问常在url后加上?new Date()随机参数保持更新.  
方式: Get方式大小限制在1KB以下, 可传送简单数据, 数据追加到header中进行发送.  

注意:

1. 对于get请求, 涉及到url参数, 发送前都需经过encodeURIComponent方法处理

## POST

特点: 而Post的则会被认为是变动性访问(浏览器默认Post的提交必定有改变.)
方式: 会将表单字段元素及其数据作为HTTP消息的实体内容(content)发送给Web服务器, 因而传递的数据量也大很多.

注意: 

1. 需设置请求的hearder中的Content-Type为application/x-www-from-urlencode, 确保服务器知道请求实体中带有参数变量. 通常使用`XMLHttpRequest`对象的`setRequestHearder('Content-Type', 'application/x-www-from-urlencode')`进行设置
2. Post请求参数在send(var)方法中进行发送, xmlHttp.send(name), 如果为get方式, 则xmlHttp.send(null)
3. 参数是key-value对, 使用'&'进行隔开