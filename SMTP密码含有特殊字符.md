## SMTP密码含有特殊字符
假如你的SMTP（邮件传输协议）密码中包含特殊字符（比如#、&、$等），你需要将密码包含在引号当中，像这样：
SMTP密码 = “My$Password#With&SpecialChars”
假如你没有这么做的话，你会在logs文件中看到类似如下的错误信息：
> Exception during check. end of file reached: /usr/lib/ruby/2.2.0/net/protocol.rb:153:in read_nonblock' /usr/lib/ruby/2.2.0/net/protocol.rb:153:inrbuf_fill' /usr/lib/ruby/2.2.0/net/protocol.rb:13...

