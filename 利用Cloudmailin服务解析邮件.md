## 利用Cloudmailin服务解析邮件
[Cloudmailin](https://www.cloudmailin.com/)服务可以将邮件转化成HTTP POST，这与Webhook agent结合使用的话，可以实现很多有趣的功能，具体的设置步骤如下：
* 生成一个新的UUID（通用唯一识别码），可以使用在线服务生成，或者在终端中执行`uuidgen`命令生成；
* 新建一个Webhook agent，将生成的UUID作为secret，将payload设为`.`，如下所示：
```
{
  "secret": "supersecretstring", # 填入前面生成的UUID，其实，你也可以使用其他任意的字符，为安全起见，最好使用生成的UUID
  "expected_receive_period_in_days": 1,
  "payload_path": "."
}
```
* 在agent的summary中，你将会看到类似这样的链接（URL）：`https://YOUR_DOMAIN/users/1/web_requests/15/YOUR_SECRET`；
* 在[Cloudmailin](https://www.cloudmailin.com/)官网注册一个新的账号；
* 注册时，在第二步`Where shall we send your email?`中，在`Enter the URL of your server (HTTP Endpoint)`中填入上面的URL，在`POST Format`中选择`JSON Format`；

直此，你已经完成了基本的设置，当你发送邮件给Cloudmailin提供的邮箱时，将会触发Webhook agent，产生事件（event）。如果你将Webhook agent作为触发其他agent的条件时，也将会触发其他agent，从而实现自动化功能。