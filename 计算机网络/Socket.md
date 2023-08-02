# socket

## 一、socket基本概念
什么是socket？提到socket我们就不得不提到网络模型，就拿如下图的四层网络模型来说，TCP/IP协议涉及了传输层和网络层，**socket是在应用层和传输层中间的一个接口**，在用户进程和TCP/IP协议之间充当一个**中间人**。  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/socket/assets/102791379/89961f84-32fa-4423-92c3-db97b5b41dde" alt="HTTP层级结构">
</div> 
<div align="center">
    <img src="https://github.com/xuehao-in-studing/socket/assets/102791379/850654b2-dee2-4d92-821e-ab0c1f5bc10f" alt="Socket作用图">
</div>   

那么如何在网络通信中唯一确定服务器或者客户端的进程？通过(IP,协议，端口号)这个三元组就可以唯一标定一个网络进程了，网络层的“ip地址”可以唯一标识网络中的主机，而传输层的“协议+端口”可以唯一标识主机中的应用程序（进程）。  

## 二、socket基本通信流程
对于服务器端和客户端来说，使用socket的过程大致相同，仅仅在一些细微的操作上有不同。如下图所示：  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/socket/assets/102791379/94ba3c3b-99f9-488d-90f8-75b27e655833" alt="Socket通信流程">
</div> 

以政府部门(服务端)申请一项服务，然后企业(客户端)订阅这个服务举例：  

政府申请服务首先要把自己的ID(IP)、地址(address)、以及服务所属部门(port)交给上级去申请(socket对象)，然后政府得到这项服务实施权，之后就监听(listen)企业是否有这个服务的需求。  

对于企业来说，只需要跟政府部门建立连接(connect)，然后提出自己的需求(write)，然后政府提供需求，企业接受需求(read)，接受完成、结束沟通(close)。这就是一次socket通讯的直观理解。  

代码实现：  
单客户端与单服务器的连接  
**服务器端**：
- 创建socket对象，指定协议族，socket类型，传输协议。
- bind，将特定的地址赋值给socket对象
- 创建监听listen，指定监听数量，并监听客户端
- 客户端调用connect方法时，服务器端通过accept方法接受客户端的socket对象并返回接收到的socket对象。
- 服务端设置完成

**客户端**：
- 创建socket对象，指定协议族，socket类型，传输协议。
- 客户端connect连接服务器端，
- 连接成功

客户端与服务器通信：
- 服务器通过send方法发送，在客户端通过receive方法进行接受，要指定接受字节。
- 接受完成可以选择继续传输或者断开连接
- 断开连接：close方法，将客户端和服务器创建的socket对象都close


## 三、socketAPI
### 3.1 socket(domain,type,protocol)
创建socket对象的实例化函数，domain为协议族(网络层)，type为传输方式、指定socket类型，protocol为对应协议(传输层)  
domain：
- AF_INET
- AF_INET6
- AF_LOCAL（AF_UNIX，本地通信用）
- AF_ROUTE

type：
- SOCK_STREAM
- SOCK_DGRAM
- SOCK_RAW
- SOCK_PACKET
- SOCK_SEQPACKET

protocol:
- IPPROTO_TCP TCP传输协议
- IPPROTO_UDP UDP传输协议
- IPPROTO_SCTP STCP传输协议
- IPPROTO_TIPCTIPC传输协议
- 0 自动选择对应的协议

注：但是这三个参数不能随意匹配，如SOCK_STREAM不可以跟IPPROTO_UDP组合


## 四、客户端与服务器的连接
### 4.1 建立连接
客户端与服务器建立连接通常需要**三次握手**：  
1. 客户端向服务器发送一个SYN J  
2. 服务器向客户端响应一个SYN K，并对SYN J进行确认ACK J+1  
3. 客户端再想服务器发一个确认ACK K+1  

<div align="center">
    <img src="https://github.com/xuehao-in-studing/socket/assets/102791379/77c002a4-ef8a-4612-a19e-950d897b9b55" alt="Socket握手过程">
</div> 
上述也基本上是一次TCP通信的流程。

### 4.2 结束连接
客户端与服务器断开连接通常需要**四次挥手**的过程：  
1. 某个应用进程首先调用close主动关闭连接，这时TCP发送一个FIN M；  
2. 另一端接收到FIN M之后，执行被动关闭，对这个FIN进行确认。它的接收也作为文件结束符传递给应用进程，因为FIN的接收意味着应用进程在相应的连接上再也接收不到额外数据；  
3. 一段时间之后，接收到文件结束符的应用进程调用close关闭它的socket。这导致它的TCP也发送一个FIN N；  
4. 接收到这个FIN的源发送端TCP对它进行确认。这样每个方向上都有一个FIN和ACK。  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/socket/assets/102791379/b51bb445-e378-4543-a43d-240ce69c0fad" alt="Socket结束">
</div> 


## 五、socket同步与异步、阻塞与非阻塞
### 5.1同步与异步
同步与异步指向的是client端，也就是客户端。  
- 同步：clietn端发送请求后，在没有得到结果返回时，client一直等待。
- 异步：异步与同步相对，就是客户端发出调用请求，c端不能立即得到结果，去做其他事情，等待结果到达后通知c端。

可以简单理解同步就是阻塞了client的进程，异步就是不会阻塞client进程。

### 5.2 阻塞与非阻塞
阻塞与非阻塞主要针对server端。
- 阻塞：指当前线程得到调用结果之前，线程会被挂起，CPU不会给线程分配时间片，只等到调用结果返回。
- 非阻塞：指不能得到结果之前，函数不会阻挡当前线程，而是会立即返回，拿到结果后，其他事件会通知函数取得返回结果。


## 六、I/O模型
I/O模型有如下几种：
1. 阻塞I/O（blocking I/O）
2. 非阻塞I/O （nonblocking I/O）
3. I/O复用(select 和poll) （I/O multiplexing）
4. 信号驱动I/O （signal driven I/O (SIGIO)）
5. 异步I/O （asynchronous I/O (the POSIX aio_functions)）

### 6.4阻塞I/O模型
阻塞I/O模型指的是线程会一直阻塞，直到数据拷贝完成。




