这个Vagrantfile和chef 手册，现在存在一个单独的库里面：https://github.com/elijah/cookbook-huginn


@elijah 正分别运行在vagrant and chef 基于huginn 的repository 库。 这将有可能取代huginn 在huginn/deployment的这部分，以及@elijah/vagrant-huginn -在the refactored / tests-to-be-added chef cookbook at @elijah/cookbook-huginn这部分内容。


这样做的目的是为了更便于管理，以及更好地进行实施部署，使得huginn的社区发展迭代少一些麻烦。 从几个社区的提出的改进意见都是希望重构源码相关的，或是至少改进一下一些有严重问题的源码架构 - 所以我们正在尝试这样去做。
