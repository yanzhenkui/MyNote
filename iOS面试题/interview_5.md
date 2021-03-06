### 常用的设计模式
- 单例模式
- 组合模式
- 观察者模式
- 代理模式
- 享元模式
- 工厂方法模式
- 抽象工厂模式

### MVC的理解
- 数据管理者(M)、数据展示者(V)、数据加工者(C)
- M应该做的事：
    - 给ViewController提供数据
    - 给ViewController存储数据提供接口
    - 提供经过抽象的业务基本组件，供Controller调度
- C应该做的事：
    - 管理View Container的生命周期
    - 负责生成所有的View实例，并放入View Container
    - 监听来自View与业务有关的事件，通过与Model的合作，来完成对应事件的业务。
- V应该做的事：
    - 响应与业务无关的事件，并因此引发动画效果，点击反馈（如果合适的话，尽量还是放在View去做）等。
    - 界面元素表达

### MVC 和 MVVM 的区别
- MVVM是对胖模型进行的拆分，其本质是给控制器减负，将一些弱业务逻辑放到VM中处理
- MVC是一切设计的基础，所有新的设计模式都是基于MVC进行的改进
- 补充：常见的设计模式有：MVC、MVCS、MVVM、viper

### TCP和UDP有什么区别？
- TCP是`面向连接`的，建立连接需要经历三次握手，保证数据正确性和数据顺序
- UDP是`非连接`的协议，传送数据受生成速度，传输带宽等限制，可能造成`丢包`
- UDP一台服务端可以同时向`多个`客户端传输信息
- TCP报头体积更大，对系统资源要求更多

### TCP的三次握手
- 第一次握手：客户端发送syn包到服务器，并进入syn_send状态，等待服务器进行确认；
- 第二次握手：服务器收到客户端的syn包，必须确认客户的SYN，同时自己也发送一个SYN包,即SYN + ACK包，此时服务器进入SYN_RECV状态;
- 第三次握手：客户收到服务器发送的SYN+ACK包之后，向服务器发送确认包,此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成第三次握手。

### 如何制作一个静态库/动态库？他们的区别是什么？
- Xcode6支持制作静态库/动态库 framework
- 无论是动态库还是静态库都是区分真机和模拟器的
- 静态库编译静态库文件装入程序空间，动态库是文件动态装入内存
- 动态库执行到相关函数才会被调用，节省空间
- 苹果一般不允许第三方动态库，APP容易被拒

### - 一个lib包含了很多的架构，会打到最后的包里么？
- 不会，如果lib中有armv7, armv7s, arm64, i386,x86_64架构，而target architecture选择了armv7s,arm64，那么只会从lib中link指定的这两个架构的二进制代码，其他架构下的代码不会link到最终可执行文件中；反过来，一个lib需要在模拟器环境中正常link，也得包含i386架构的指令

```objc
每一个设备都有属于自己的CPU架构
每一个静态支持的架构是固定
模拟器
4s-->5 : i386
5s-->6plus : x86_64

真机
3gs-->4s : armv7
5/5c : armv7s,静态库只要支持了armv7,就可以跑在armv7s的架构上
5s-->6plus : arm64

常用命令总结：
// 使用lipo -info命令，查看指定库支持的架构，比如UIKit框架
lipo -info UIKit.framework/UIKit

// 想看的更详细的信息可以使用lipo -detailed_info
lipo -detailed_info UIKit.framework/UIKit

// 还可以使用file命令
file UIKit.framework/UIKit

// 合并MyLib-32.a和MyLib-64.a，可以使用lipo -create命令合并
lipo -create MyLib-32.a MyLib-64.a -output MyLib.a
```

### 支持64-bit后程序包会变大么？
- 会，支持64-bit后，多了一个arm64架构，理论上每个架构一套指令，但相比原来会大多少还不好说

### 用过Core Data 或者 SQLite吗？读写是分线程的吗？遇到过死锁没？如何解决的？
- 用过SQLite，使用FMDB框架
- 丢给FMDatabaseQueue 或者 添加互斥锁（NSLock/@synchronized(锁对象)）

### 请简单的介绍下APNS发送系统消息的机制
- APNS优势：杜绝了类似安卓那种为了接受通知不停在后台唤醒程序保持长连接的行为，由iOS系统和APNS进行长连接替代
- APNS的原理：
    - 应用在通知中心注册，由iOS系统向APNS请求返回设备令牌(device Token)
    - 应用程序接收到设备令牌并发送给自己的后台服务器
    - 服务器把要推送的内容和设备发送给APNS
    - APNS根据设备令牌找到设备，再由iOS根据APPID把推送内容展示

### 不用中间变量,用两种方法交换A和B的值
- 方法1：
```objc
A = A + B
B = A - B
A = A - B
```
- 方法2：异或
```objc
A = A^B;
B = A^B;
A = A^B;
```

### 开发常用的工具有哪些？

### 你一般是怎么用 Instruments 的？

### 你一般是如何调试 Bug 的？

### 如何实现单例，单例会有什么弊端？
- 节省内存资源，一个应用就一个对象

### APP上架后如何搜集错误信息？

### 简答描述下你所理解的敏捷开发
