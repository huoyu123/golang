# forwardPort
端口转发/映射工具 forward for port data

全新带web控制台的端口转发工具，已迁移至 http://git.oschina.net/tavenli/port-forward

##编译：
```
配置好你的GO开发环境，推荐GO 1.7以上；

执行build.dat，会自动编译出linux和windows的执行程序，文件名分别为：“forwardPort”、“forwardPort.exe”

```

##不想编译：
```
为了方便大家使用，我将已编译好的二进制文件也提供给大家：
Windows-64位：
https://github.com/tavenli/forwardPort/releases/download/1.0/forwardPort-win-64.zip

Linux-64位：
https://github.com/tavenli/forwardPort/releases/download/1.0/forwardPort-linux-64.zip

```

##场景1：
工作中，有时候会碰到A服务器可以访问B服务器，但是你只能访问到A服务器，B服务器限制了只有A服务器能访问它；如果你需要访问B服务器，必须通过A服务器跳一次。

forwardPort工具就是可以让你在A服务器上开启一个端口，当你访问A服务器上的端口时，实际访问的是B服务器的某个端口。

##操作步骤：
```
  A服务器IP：10.10.1.100
  B服务器IP：10.11.2.20

  1、在A服务器上执行:
  forwardPort.exe

  （注：linux系统执行 ./forwardPort）

  2、执行成功后，默认监听8000端口，这时打开浏览器，访问：http://10.10.1.100:8000/ServerSummary

  会返回当前程序的统计信息，返回内容为JSON数据。

  如果访问不了，请检查A服务器上防火墙是否开启8000端口的访问。


  3、开启转发，将A机器上的8010端口转发到B服务器上的3389端口：

    http://10.10.1.100:8000/ForwardWork?auth=taven123&status=1&fromAddr=:8010&toAddr=10.11.2.20:3389

  4、开启后，您就可以通过 10.10.1.100:8010 端口连接了，此时你实际连接到的是 10.11.2.20 上的 3389


  上面只是个例子，你可以随时启用任意端口与任意机器之间的端口映射。

  当你使用完毕后，可以立即关闭端口转发，只需要执行如下请求即可：
  http://10.10.1.100:8000/ForwardWork?auth=taven123&status=0

  执行后，端口转发关闭，端口被释放。

```
##其它说明
```
windows-64下：
> forwardPort.exe
或
> forwardPort.exe -auth 123 -restApi 10.10.1.100:9999

linux-64下：
$ ./forwardPort
或
$ ./forwardPort -auth 123 -restApi 10.10.1.100:9999

```
#参数说明：
```
auth：rest接口调用时的密码，启动时不带该参数，则默认为：taven123
restApi：rest接口监听的地址，启动时不带该参数，则默认为：0.0.0.0:8000

#REST接口参数说明：
/ForwardWork?auth=taven123&status=1&fromAddr=:8010&toAddr=10.11.2.20:3389
auth：密码
status：如果是开启转发，则为1，如果是关闭转发，则为0
fromAddr：要用来在A机器上监听的一个端口，用来给客户端连接
toAddr：把fromAddr端口的数据转发到哪个IP的端口上
```

#如果是本机内转发，可以这样：
```
/ForwardWork?auth=taven123&status=1&fromAddr=:8010&toAddr=:22

表示把机器上的8010端口映射到本机的22端口.

```
##最后想说的话
#我是一名Java程序员，当我遇上Go语言后，我想用一句话来赞美它：“Go是世界上最好的语言”，希望您不要生气。











