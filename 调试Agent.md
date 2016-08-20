# 调试Agent

假如我们写了一个彗星撞地球会触发的Agent.但是上一次的彗星撞地球事件,Agent遇到了错误,不能触发event.如果你不想在触发错误,最好要去调试.

如果你认为你的Agent有bug, log()或errors.add()是有用的.然后使用web页面运行你的Agent(Actions -> Run), 检查Log信息(Actions -> Show -> Logs).

你也能用一个通用的Agent去手动触发事件运行.

最后,所有日志消息和错误信息可以在 huginn/log/development.log 查看, 但是这个文件很难筛选出单独Agent的log信息.

TODO:

Explain the difference between log() and errors.add()