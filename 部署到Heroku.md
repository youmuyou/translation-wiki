##部署到Heroku
如果要将Huginn部署到Heroku平台，我们**推荐你使用最便宜的付费方案**，但是，如果你要使用免费方案的话，你需要注意以下几点：
* 使用Heroku免费方案的用户，其每个网站在30分钟内无人访问后便会自动关闭，再有人访问时才会自动重新打开，因此为了使Huginn网站能长时间运行，可以使用类似[uptimerobot](https://uptimerobot.com/)的网站监控服务，不停地ping你的Huginn网站地址；
* Heroku免费方案只允许用户所有的app每个月运行的总时长不超过550个小时（绑定信用卡的用户可额外再获得450个小时），这意味着你无法保证部署好的Huginn网站每个月每天都在运行，因此，你可选择绑定信用卡，或者让你的Huginn网站每天只允许18个小时，这样的话，你的网站每天都可以在运行；
* Heroku免费方案提供给用户的Postgres数据库存储的数据不能超过10000行，因此你需要控制Agent生成的事件的保留周期，同时限制数据库中Agent日志文件的长度，比如`heroku config:set AGENT_LOG_LENGTH=20`。

###初次部署到Heroku的操作步骤：
1.  注册[Heroku](https://www.heroku.com/)平台账号，然后下载安装[Heroku Toolbelt](https://toolbelt.heroku.com/)；
2.  远程登录Heroku： `heroku login`
3.  创建app：`heroku create xxx`（xxx为app的名字，下同）；
4.  将app下载到本地：`heroku git:clone --app xxx`
5.  将[huginn](https://github.com/cantino/huginn)源代码全部下载拷贝到本地app的文件夹内；
6.  进入app文件目录，依次执行命令：`cp .env.example .env` 和`bundle`；
7.  提交代码变更：`git add .`、`git commit -am 'commit code'`；
8.  最后，执行脚本：`bin/setup_heroku`，完成部署。
###更新已经部署好的Huginn
Huginn的功能还在继续开发过程中，在将Huginn部署到Heroku平台之后，可以通过以下命令来更新Huginn的代码：
```
git fetch origin
git merge origin/master
git push -f heroku master # 更新Heroku平台上的Huginn代码
heroku run rake db:migrate # 迁移数据库到最新状态的Huginn（尽管不需要每次更新时都运行该命令，但是安全起见，最好每次更新代码时，都运行该命令）
```
###使用自己的邮箱服务器
在安装过程中默认使用的是SendGrid的邮箱服务器（安装后需要自行配置），你也可以使用其他邮箱服务器，下面以谷歌邮箱服务器为例，需要进行以下配置：
```
heroku config:set SMTP_DOMAIN=google.com
heroku config:set SMTP_USER_NAME=you@gmail.com
heroku config:set SMTP_PASSWORD=somepassword
heroku config:set SMTP_SERVER=smtp.gmail.com
heroku config:set EMAIL_FROM_ADDRESS=you@gmail.com # 指定显示的发件人邮箱地址
```
###备份你的数据
参见[Heroku的官方文档](https://devcenter.heroku.com/articles/heroku-postgres-import-export)