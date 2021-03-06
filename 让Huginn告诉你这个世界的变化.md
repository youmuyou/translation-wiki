# 让Huginn告诉你这个世界的变化
这是我关于Huginn的第二篇文章，Huginn是一款被开发者广泛支持的一款工具。Huginn是建立数据采集和数据反应任务日常生活重量轻的平台。把它看成是一个开源 的 Yahoo! Pipes, IFTTT, 或 Zapier。

在这篇文章中，我将告诉你如何设置关于世界的长期提醒。基本上，你的Huginn将能够回答诸如“下次超级碗的日期公布时告诉我”，“当我们发现引力波告诉我”，或“有向旧金山海啸时告诉我”的任意请求。

问题：我想知道一件事的进展，但常常会错过它们的最新消息。
答案：让Huginn来替你追踪这件事吧（通过Twitter），当你关心的话题有热点发生，huginn会提醒你
![01](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_01.jpg "01")


￼

在￼设置你的Huginn服务￼后, 你需要创建一些twitter证书。 接着按照￼Huginn维基说明￼注册一个Twitter应用程序，编辑.env文件以便在Huginn实例上安装它。

现在，您已经完成了建立一个Twitter的应用程序；重新启动您的Huginn实例；访问服务页，并点击“Authenticate with Twitter”。Twitter将请求你登录并授权给twitter应用。当你这样做，你应该看到Huginn新的服务与您的Twitter用户名。

最后，你准备制作一个TwitterStreamAgent。该TwitterStreamAgent根据提供的关键字或者过滤器实时的监控Twitter流。我们将使用一个新的TwitterStreamAgent监控感兴趣的关键字，并且每30分钟通知我们关键字的次数。
这项技术对普通的关键字支持良好，对冷门的有待提高。如果你想追踪冷门的关键字，像独特的产品名称，你可以做一个第二TwitterStreamAgent，设置他的生成events 代替 counts，当有关键字出现的时候就发邮件给你。

好了，设置新TwitterStreamAgent每30分钟运行一次，保持活动7天，并产生计数。filters部分，你可以进入superbowl date announced，gravity waves detected和huginn open source。你的代理将保持追踪这些术语独立。

您的屏幕现在看起来应该是这样的：

![02](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_02.png "02")￼

接下来，让我们建立一个新的PeakDetectorAgent。PeakDetectorAgent被用于检测数据流的趋势。在我们的例子中，我们要寻找在Twitter的事件计数尖峰。他只从这里更改默认是把“stdmultiple”5代替3，这都由你决定，越高的数字代表越高的敏感度，如果你得到的提醒太多或太少，您可以自定义该值。（这里STD表示标准偏差。您的数据可能是不实际的Gaussian，但std使得一个很好的调整因素）
￼![03](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_03.png "03")

如果你让你的Huginn运行了一段时间，等待发生的一些科学革命，然后单击“显示”，你可能会看到这样的事情：

![04](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_04.png "04")￼

如果你愿意，你可以在这里停止。你需要这个功能！PeakDetectorAgent的输出发送到电子邮件或电子邮件消化剂，您会收到警报时的Twitter感兴趣的尖峰。但是，为了提高可读性，我建议增加一个代理流程：一个 EventFormattingAgent.

￼![05](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_05.png "05")

此代理将从峰值检测器输出JSON格式成为一个更可读的格式以链接到搜索Twitter，看到了骚动是关于什么的。这个连接到邮件代理，就大功告成了！

![06](http://7oxfwx.com1.z0.glb.clouddn.com/2016_08_17_06.png "06")￼

这个例子的Huginn代理流程具有中等响应时间，感兴趣的峰值在Twitter上开始后，响应30-60分钟。有些题目要求更快的响应时间，像像“旧金山海啸预警”，“闪售票”或“股市崩溃”。处理警报喜欢这些，我运行不同TwitterStreamAgent和PeakDetectorAgent与TwitterStreamAgent检查每2分钟，PeakDetectorAgent设置为做即时传播。结果峰值是直接发送到电子邮件或短信，而不是电子邮件摘要，让我得到警报非常及时。

Huginn的更多想法和更新，请参与并关注我的Twitter！