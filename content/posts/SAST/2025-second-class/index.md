---
title: "SAST授课-从HTTP到RPC & 服务发现与注册" 
date: 2025-03-05T11:30:03+00:00 
# weight: 1
# aliasesSAST"] 
author: "Whitea"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "2025 SAST.Web第二次授课" 
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/Whitea029/myblog/blob/main/content"
    Text: "Source Code" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 我想先说说 TCP

大家如果在开发时候遇到希望不同进程可以通讯，常用到的就是 Socket 编程，这是一种面向传输层的网络编程方式，无非就是 TCP 或者 UDP。
大部分人无脑选择 TCP 就好了

![](images/2025-second-class/001.png)

这是一种基于字节流的传输方式，就这样一个裸的TCP就可以解决我们收发数据的全部问题了么？

## 裸TCP的问题

八股文常背，TCP 是有三个特点，**面向连接、可靠、基于字节流**
我们今天重点关注的是基于字节流这个特点
字节流可以理解成一个双向通道里面流淌的数据，也就是我们常说的二进制数据，说白了也就是一串`01`串，纯裸 TCP 在收发这一堆 `01` 串的时候是没有任何边界的，
它根本不知道从哪到哪才是一条完整的消息。

![](images/2025-second-class/002.png)

上面这张图就是粘包问题
因此很显然，纯裸的 TCP 是不适合直接拿过来用的，需要制定一些规则用于区分**消息的边界**
比如我们可以把消息封装一下，分为 Header 和 Body, Header 用于存放一些元数据，比如这个消息有多长， Body 用于存放真正的消息

![](images/2025-second-class/003.png)

这样封装方式，只要收发信息的上下游约定好，这就是所谓的协议
因此基于 TCP 就衍生出很多的协议，比如 HTTP 和 RPC

## HTTP 和 RPC

![](images/2025-second-class/004.png)

- **HTTP协议**：超文本传输协议 （HTTP） 是一种用于传输超媒体文档（如 HTML）的应用层协议。它专为 Web 浏览器和 Web 服务器之间的通信而设计，但也可用于其他目的，例如机器对机器通信、对 API 的编程访问等

- **RPC**：RPC协议是一种通过网络调用远程服务的方法，使得不同系统间可以像调用本地方法一样进行通信

这两个都是应用层协议，了解微服务的同学应该知道，常常在服务内部一般都会使用 RPC 协议进行服务间通讯，而不是使用 HTTP。要弄懂这个需要从这两个协议的设计原理出发了。

## HTTP 协议

HTTP的请求报文由四部分组成：请求行(request line)、请求头部(header)、空行和请求数据(request data)

![](images/2025-second-class/005.png)

不过 HTTP 协议不是我们今天的重点
更多内容可以看 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

## RPC 协议

### 本地函数调用

```go
func main() {
	a := 1
	b := 2
	res := add(a, b)
	fmt.Println(res)
	return
}

func add(x, y int) int {
	z := x + y
    return z
}
```
执行步骤：
1. 将a, b的值压栈
2. 通过函数指针找到add函数，取出栈中的1 2并将其赋值给x, y
3. 计算 x + y 并将得到的值赋值给z, 然后压栈返回
4. 从栈中取出z的值，赋值给res

这是一个简单的例子，那我们看一下生产项目中出现的例子

![](images/2025-second-class/006.png)

像这种跨服务的调用需要解决一些问题
- 函数映射
- 数据转化成字节流
- 网络传输

### RPC 概念模型

![](images/2025-second-class/007.png)

1984 年 Nelson 发布了论文《Implementing Remote Procedure Calls》，其中提出了 RPC 的过程由 5 个模型组成: User、User-Stub、RPC-Runtime、Server-Stub、Server

### 一次完整的 RPC 请求过程

![](images/2025-second-class/008.png)

- IDL(Interface Description Language)文件：通过一种中立的方式来描述接口，使得在不同平台上运行的对象和用不同语言编写的程序可以相互通信。
- 生成代码：通过编译器把IDL文件转换成语言对应的静态库。
- 编解码：从内存中表示到字节序列的转换称作编码，反之叫解码。
- 通信协议：规范了数据在网络中的传输内容和格式，除必须的请求/响应数据外，还会包含额外的元数据。
- 网络传输：通常基于成熟网络库走TCP/UDP传输。

### RPC的好处和问题
这样当我们使用了 RPC 协议之后，调用远程服务就像调用本地方法一样简单，在微服务的架构中也有很多好处
- 服务单一职责，更加有利于分工和协作
- 扩展性更强，资源使用率更优秀
- 故障隔离，服务整体有着更高的可靠性

当然也有一些问题
- 服务宕机了怎么办？
- 网络异常怎么办？
- 请求量激增导致服务无法及时处理，怎么办？

这些问题的处理常常需要 RPC 框架来帮我们解决

## RPC框架

### 分层设计

RPC框架常被分为三层：编解码层，协议层，网络通信层
这里的协议我们以 Apache 的 Thrift 协议为例

![](images/2025-second-class/009.png)

**编解码层**

编解码层需要通过IDL文件生成各种语言的代码

![](images/2025-second-class/010.png)

而它的数据格式有多种
- 语言特定格式：许多的编程语言都有内建的将内存对象编码成字节序列的支持，比如 Java 的 java.io.Serializable
- 文本格式：JSON, XML, CSV等文本格式，人类易于阅读
- 二进制编码：具备跨语言和高性能的优点，常见有 Thirft 的 BinaryProtocol, Google 的 Protobuf

我们重点说一下二进制编码，其中 Thirft 是基于 TLV 编码， Protobuf 是基于 Varint 编码

**TLV编码**

```thirft
struct Person {
    1: require string        userName,
    2: optional i64          favoriteNumber,
    3: optional list<string> interests
}
```
![](images/2025-second-class/011.png)

因此在 RPC 框架的编解码层的选型上往往考虑的因素是
- 兼容性
- 通用性
- 性能

**协议层**

我们从数据划分上常常有这么两种

- 特殊的结束符号：一个特殊字符作为每个协议单元结束的标示

![](images/2025-second-class/012.png)

- 变长协议：由定长部分和不定长部分组成，其中定长部分要描述不定长部分内容的长度

![](images/2025-second-class/013.png)

**协议构造**

```
  0 1 2 3 4 5 6 7 8 9 a b c d e f 0 1 2 3 4 5 6 7 8 9 a b c d e f
+----------------------------------------------------------------+
| 0|                          LENGTH                             |
+----------------------------------------------------------------+
| 0|       HEADER MAGIC          |            FLAGS              |
+----------------------------------------------------------------+
|                         SEQUENCE NUMBER                        |
+----------------------------------------------------------------+
| 0|     Header Size(/32)        | ...
+---------------------------------

                  Header is of variable size:
                   (and starts at offset 14)

+----------------------------------------------------------------+
|         PROTOCOL ID  (varint)  |   NUM TRANSFORMS (varint)     |
+----------------------------------------------------------------+
|      TRANSFORM 0 ID (varint)   |        TRANSFORM 0 DATA ...
+----------------------------------------------------------------+
|         ...                              ...                   |
+----------------------------------------------------------------+
|        INFO 0 ID (varint)      |       INFO 0  DATA ...
+----------------------------------------------------------------+
|         ...                              ...                   |
+----------------------------------------------------------------+
|                                                                |
|                              PAYLOAD                           |
|                                                                |
+----------------------------------------------------------------+
```

- LENGTH: 数据包大小，不包含自身
- HEADER MAGIC: 标识版本信息，协议解析时快速校验
- SEQUENCE NUMBER: 表示数据包的 seqID，可用于多路复用，单链接内递增
- HEADER SIZE: 头部长度，从第14个字符开始计算一直到 PAYLOAD前
- PROTOCOL ID: 编解码方式，有 Binary 和 Compact 两种
- TRANSFORM ID: 压缩方式，如 zlib 和 snappy
- INFO ID: 传递一些定制的 meta 信息
- PAYLOAD: 消息体

那么框架里面怎么对我们的协议进行解析呢
![](images/2025-second-class/014.png)

**网络通信层**

常用的便是 Socket API

![](images/2025-second-class/015.png)

![](images/2025-second-class/016.png)

框架选择合适的网路库需要考虑的因素：
- 易用的API：封装底层 Socket API, 连接管理和事件分发
- 功能：协议支持（TCP，UDP，UDS等），优雅退出，错误异常等
- 性能：应用层的 buffer 减少 copy，高性能定时器，对象池等

**框架**

- Alibaba 开源的 Java RPC 框架 Dubbo https://github.com/apache/dubbo
- Facebook 开源的 Apache Thirft https://github.com/apache/thrift
- Google 开源的 gPRC https://github.com/grpc/grpc
- ByteDance 开源的 Kitex https://github.com/cloudwego/kitex
- Didi 开源的微服务框架 Go-zero https://github.com/zeromicro/go-zero
- Bilibili 开源的微服务框架 Kratos https://github.com/go-kratos/kratos
- Spring 下的微服务框架 Spring Cloud https://github.com/spring-cloud 
- 常与 Spring Cloud 结合的 OpenFeign https://github.com/OpenFeign/feign

## 注册中心

刚刚我们了解到 RPC 以及一些框架，使用 RPC 框架进行开发的服务往往都是微服务，微服务则是服务于分布式系统的，因此在企业实际的生产中，服务的部署往往是跨主机，网段，机房甚至省市自治区的。
那么各个服务如果想通过 RPC 调用就需要知道这个服务的 IP 和端口号，总不至于说我把 IP 和端口号在代码里面写死吧，这绝对不是一种好的办法。因此在架构上最重要的一环就是**服务发现**，对应重要的就是**服务注册**

### 服务注册与服务发现

![](images/2025-second-class/017.png)


- Service B 把自己注册到 Service Registry 叫做 服务注册
- Service A 从 Service Registry 发现 Service B 的节点信息叫做 服务发现

**服务注册**

服务注册是针对服务端的，服务启动后需要注册，分为几个部分：

- 启动注册：当一个服务节点起来之后，需要把自己注册到 Service Registry 上，便于其它节点来发现自己。注册需要在服务启动完成并可以接受请求时才会去注册自己，并且会设置有效期，防止进程异常退出后依然被访问。
定时续期
- 定时续期：定时续期相当于 keep alive，定期告诉 Service Registry 自己还在，能够继续服务
- 退出撤销：当进程退出时，我们应该主动去撤销注册信息，便于调用方及时将请求分发到别的节点。同时，go-zero 通过自适应的负载均衡来保证即使节点退出没有主动注销，也能及时摘除该节点

**服务发现**

服务发现是针对调用端的，一般分为两类问题：

- 存量获取：当 Service A 启动时，需要从 Service Registry 获取 Service B 的已有节点列表：Service B1, Service B2, Service B3，然后根据自己的负载均衡算法来选择合适的节点发送请求

![](images/2025-second-class/017.png)

- 增量侦听：上图已经有了 Service B1, Service B2, Service B3，如果此时又启动了 Service B4，那么我们就需要通知 Service A 有个新增的节点

![](images/2025-second-class/018.png)

- 应对服务发现故障： 当服务发现系统出现故障时，通常会使用故障转移策略来确保服务的可用性。例如，如果主服务发现系统出现问题，系统可以切换到备份的服务发现实例，确保依然可以发现服务

![](images/2025-second-class/019.png)

### 注册中心

- Consul https://github.com/hashicorp/consul
- Nacos https://github.com/alibaba/nacos
- etcd https://github.com/etcd-io/etcd
- zookeeper https://github.com/apache/zookeeper

> 作为一名后端开发,文章存在有任何问题都可以联系我，当然也欢迎与我交流技术相关的问题，感谢你的阅读🤗