##部署到Clound9(C9)
1. 在[Cloud9](https://c9.io/?redirect=0)的控制面板上创建一个rails环境的工作空间，进入工作空间后，依次执行以下命令：
```
git clone git://github.com/cantino/huginn.git
cd huginn/
cp .env.example .env
bundle
mysql-ctl start
bundle exec rake db:create
bundle exec rake db:migrate
bundle exec rake db:seed 
```
1. 在.env文件中更改端口地址和IP地址：

```
`DOMAIN=locahost:3000` ==> `DOMAIN=$IP:$PORT`
`PORT=3000`==>`PORT=$PORT`
```
1. 最后，启动Huginn网站：
```
bundle exec foreman start
```
打开Cloud9提供的网址就可以看到Huginn网站的首页，你可以注册该网站，注册邀请码为`try-huginn`，但是，Cloud9建立的网站在关闭工作空间之后无法长时间维持，因此，在正常使用时不推荐使用它来运行Huginn。