根据此安装说明：

https://github.com/cantino/huginn/blob/master/doc/manual/installation.md



在VirtualBox下安装Huginn，最低配置要求如下。


首先准备一个安装好了的VirtualBox还有Vagrant （从vagrantup.com）。
测试


这个仓库是我每天在上面进行编译测试的，所以我希望能收集到任何源于Huginn项目本身变更引起的问题。 如果我哪里有问题或者你发现任何其他问题，发邮件到burns.sj@gmail.com。
安装手册


如果你知道安装Huginn需要的各种设置（比如SMTP主机，MySQL的设置等），您可以：
克隆这个存储库： git clone https://github.com/m0nty/huginn-vagrant.git
跳转到这个文件执行./setup.sh，并按照提示操作。 你操作的时候不需要做其他的验证，除了设置必要的用户名，密码等。
执行vagrant up，Huginn实例准备就绪。
手动编辑配置文件


如果你根据上面运行./setup.sh的话，就不需要做这个， 但是，如果您希望自己编辑的配置文件，或者你需要更改设置：
克隆这个库： git clone https://github.com/m0nty/huginn-vagrant.git
编辑这个env文件，该文件将被复制到/home/huginn/huginn/.env （当你修改设置的时候查找文件中的FIXME相关建议）。
编辑provision.sh，更改MySQL的root用户和Huginn DB用户（再次搜索FIXME）MySQL的密码。
编辑Vagrantfile如果要更改虚拟机的设置。 默认设置为vb.memory = "1024"如果你觉得你的huginn负载比较大的话，可以根据需要进行变更增加
如果您的电脑不支持64位的话。 您可能需要修改这一行
config.vm.box = "ubuntu/trusty64"
如果你Huginn实例想修改监听地址或端口，修改huginn文件。
Procfile这个文件不需要修改，你看看也行。
做了上面的变更后，运行vagrant up置备Huginn实例。


如果我哪里有问题请告知我。 我的Huginn实例已经在正常工作了，如果大家安装的时候遇到什么问题我很乐意提供帮助的。
需要做的


我之后可能会做一个Dockerfile，然后你就可以自己创建一个docker 实例，而不用拖Huginn的docker镜像。
