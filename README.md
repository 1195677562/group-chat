# ChatServer-project
基于moduo ，运行在nginx、tcp负载均衡环境中的集群聊天项目（redis, mysql）

----------------------------------------------------------------------
----------------------------------------------------------------------
/bin 保存可执行文件
/build CMake 
/include 保存 .hpp文件
/src  .cpp文件
/test  与该项目无关，属于json.hpp和muduo库测试文件
/thirdparty  保存 json.hpp文件

autobuild.sh 直接编译脚本文件，chmod u + x  直接直接执行文件，进行cmake操作下


----------------------------------------------------------------------
----------------------------------------------------------------------
对于moduo库的使用
muduo网络库给用户提供了两个主要的类
TcpServer : 用于编写服务器程序
TcpClient : 用于编写客户端程序

epoll + 线程池 
好处：能够把网络I/O的代码和业务代码区区分开
                        | 用户的连接和断开 用户的可读写事件

基于muduo网络库开发的服务器程序开发流程:
1. 组合TcpServer对象
2. 创建EventLoop事件循环对象的指针
3. 明确TcpServer构造函数需要什么函数，输出ChatServer的构造函数
4. 当前服务器类的构造函数当中,注册处理连接的回调函数和注册用户处理读写的回调函数
5. 设置合适的服务端线程数量, muduo库会自己划分i/O线程线程和worker线程

----------------------------------------------------------------------
----------------------------------------------------------------------

ChatServer集群后引入负载均衡器
- 把client的请求按照负载算法分发到具体的业务服务器ChatServer上面
- 能够和ChatServer保持心跳机制，检测ChatServer故障
- 能够发现新添加的ChatServer设备，方便扩展服务器数量

----------------------------------------------------------------------
----------------------------------------------------------------------

为解决跨服务器通信问题
- 引入中间件消息队列，解耦各个服务器，是整个系统松耦合， 提高服务器的响应能力，节省服务器的带宽资源
- 基于发布-订阅的redis消息队列

