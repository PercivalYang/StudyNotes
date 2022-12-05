[toc]
## 头部格式
- TCP的头部格式：  
![head](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzYuanBn?x-oss-process=image/format,png =400x300) 
- 标志位：
  - ACK：该位为 1 时，「确认应答」的字段变为有效，TCP 规定除了最初建立连接时的 SYN 包之外该位必须设置为 1 。
  - RST：该位为 1 时，表示 TCP 连接中出现异常必须强制断开连接。
  - SYN：该位为 1 时，表示希望建立连接，并在其「序列号」的字段进行序列号初始值的设定。
  - FIN：该位为 1 时，表示今后不会再有数据发送，希望断开连接。当通信结束希望断开连接时，通信双方的主机之间就可以相互交换 FIN 位为 1 的 TCP 段。
- **窗口大小**(流量控制) 通信双方各声明一个窗口(缓存)大小，代表自己当前的处理能力
- 拥塞控制 拥塞是指通信双方路程上的，TCP可控制自身发送速度
- 相比之下UDP的头部格式:  
![udp](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzEyLmpwZw?x-oss-process=image/format,png =400x300) 

## 与UDP的区别
- 连接  
  - TCP传输数据需要前三次握手建立连接，UDP不需要
- 对象数  
  - TCP是一对一，UDP可以一对多
- 可靠性  
  - TCP可保证无差错；UDP不保证可靠，但可以改进如[QUIC](https://xiaolincoding.com/network/3_tcp/quic.html)
- 拥塞/流量控制  
  - TCP有，UDP没有所以传输很快
  - 什么是拥塞/流量控制？  
- 头部开销  
  - TCP头部较长，没有使用时为`20`个字节；UDP头部仅`8`个字节  
- 传输方式  
  - TCP->字节流；UDP->按包发送
- 分片方式  
  - TCP：超过`MSS`进行分片，在**传输层**完成分片/组装
  - UDP：超过`MTU`进行分片，在**网络层**完成分片/组装

## TCP三次握手详细流程
- 刚开始服务器和客户端都处于`CLOSE`状态，服务端会主动监听某个端口，处于`LISTEN`状态
- 三次握手的报文：
  1. 随机初始化`client_isn`(isn: Initial sequence number)，将标志位`SYN`置1，然后发送给服务端，之后客户端处于`SYN-SENT`状态  
  ![TCP1th](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzE1LmpwZw?x-oss-process=image/format,png =400x300) 
  2. 服务端收到后随机初始化`server_isn`，同时在确认应答号中填入`client_isn+1`，并将标志位`SYN`和`ACK`置1，发送给客户端后此时服务端处于`SYN-RCVD`状态
  ![TCP2th](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzE2LmpwZw?x-oss-process=image/format,png =400x300) 
  3. 客户端收到后还要回复一个**应答报文**，在确认应答号中写入`server_isn+1`，并将`ACK`置1发送给服务端，此时发送的报文中可以携带数据，之后客户端处于`ESTABLISHED`状态。服务器收到应答报文后也处于`ESTABLISHED`状态  
  ![TCP3th](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzE3LmpwZw?x-oss-process=image/format,png =400x300) 
- 三次握手**前两次不能携带数据，最后一次可以携带数据部分**
### 为什么要三次握手？
- 三次握手才可以阻止重复历史连接的初始化（主要原因）
- 三次握手才可以同步双方的初始序列号
- 三次握手才可以避免资源浪费


### 三次握手的数据包丢失会怎么样？

#### 第一次握手丢失
- 客户端发送`SYN`报文后处于`SYN-SENT`状态，若迟迟未收到服务端的`SYN-ACK`报文，会触发**超时重传**，**重传的`SYN`报文的序列号都是一样的**
- 客户端`SYN`报文最大的重传次数由`tcp_syn_retries`内核参数控制，默认一般是5
- 通常第一次超时重传是1秒，之后每次超时的时间是上次的两倍
#### 第二次握手丢失
- 客户端会认为自己的`SYN`包丢失了，触发超时重传，同时服务端也会触发超时重传，服务端的最大重传次数由`tcp_synack_retries`内核参数决定  
![2thLost](https://cdn.xiaolincoding.com/gh/xiaolincoder/network/tcp/%E7%AC%AC2%E6%AC%A1%E6%8F%A1%E6%89%8B%E4%B8%A2%E5%A4%B1.png) 
#### 第三次握手丢失
- 即客户端发送`ACK`报文，并处于`ESTABLISHED`状态，但**`ACK`报文不会有重传**，因此需要服务端重传第二次握手的`SYN+ACK`报文
![3thLost](https://cdn.xiaolincoding.com/gh/xiaolincoder/network/tcp/%E7%AC%AC%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E4%B8%A2%E5%A4%B1.drawio.png) 

## TCP连接断开
### 四次挥手
![TCPCutOff](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3hpYW9saW5jb2Rlci9JbWFnZUhvc3QyLyVFOCVBRSVBMSVFNyVBRSU5NyVFNiU5QyVCQSVFNyVCRCU5MSVFNyVCQiU5Qy9UQ1AtJUU0JUI4JTg5JUU2JUFDJUExJUU2JThGJUExJUU2JTg5JThCJUU1JTkyJThDJUU1JTlCJTlCJUU2JUFDJUExJUU2JThDJUE1JUU2JTg5JThCLzMwLmpwZw?x-oss-process=image/format,png =400x400)
- 客户端和服务端都可以主动断开连接，上图以客户端主动断开连接为例，Q: 为什么要四次挥手?    
    - 客户端首先发送`FIN`报文给服务端，处于`FIN_WAIT_1`状态，表示客户端此时不再发送数据，**但可以接受服务端还没发送完的数据**
    - 服务端首先回复`ACK`报文给客户端(避免超时重传)，客户端进行如`FIN_WAIT_2`状态，服务端将剩余数据发送完后进行第三次握手即发送`FIN`报文
    - 客户端回复`ACK`报文后服务端立即关闭，客户端处于`TIME_WAIT`状态一段时间后关闭
- **主动关闭连接的，才有`TIME_WAIT`状态**
### 两种关闭连接的函数`close()` & `shutdown()`
- 关闭连接的函数分别有`close()`和`shutdown()`两种，不同在于第二次握手的处理，即客户端发送`FIN`报文后收到服务端`ACK`报文，处于`FIN_WAIT_2`状态
    - 对于`close()`函数  
        - 关闭的连接无法在接收和发送数据
        - `tcp_fin_timeout`控制`FIN_WAIT_2`状态的持续时间，默认是60秒，如果客户端在这个状态下超时没有收到服务端发来的第三次握手(`FIN`报文)，客户端的连接会直接关闭
    - 对于`shutdown()`函数
        - 关闭的连接无法发送数据，可以接收数据
        - `tcp_fin_timeout`无法控制`FIN_WAIT_2`状态，如果没收到服务端的`FIN`报文，客户端会一直处于`FIN_WAIT_2`状态

### 握手丢失
#### 第一次握手丢失
- 客户端超时重传`FIN`报文，重传次数由`tcp_orphan_retries`控制，超出重传次数会再等待一段时间(上次超时的2倍)，若还未收到服务端的`ACK`报文，**则直接进入`CLOSE`状态
#### 第二次握手丢失
- 因为`ACK`报文不会重传，丢失后等于客户端认为第一次握手的`FIN`报文丢失，触发超时重传
#### 第三次握手丢失
![3thCutLost](https://cdn.xiaolincoding.com/gh/xiaolincoder/network/tcp/%E7%AC%AC%E4%B8%89%E6%AC%A1%E6%8C%A5%E6%89%8B%E4%B8%A2%E5%A4%B1.drawio.png =400x400) 
#### 第四次握手丢失
![4thCutLost](https://cdn.xiaolincoding.com/gh/xiaolincoder/network/tcp/%E7%AC%AC%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B%E4%B8%A2%E5%A4%B1drawio.drawio.png =400x400)
### TIME_WAIT
#### 为什么要TIME_WAIT
- 防止历史连接中的数据，被后面相同四元组的连接错误的接收；(主要原因)
- 保证「被动关闭连接」的一方，能被正确的关闭；
##### 防止历史连接数据错收
- 初始化序列号可被视为一个 32 位的计数器，该计数器的数值每 4 微秒加 1，循环一次需要 4.55 小时。可以知道它不是无限递增的，会发生**绕回初始值**的情况，代表无法根据序列号判断新老数据；
- 因此可能发生如下情况：  
![PreventHistoryData](https://img-blog.csdnimg.cn/img_convert/6385cc99500b01ba2ef288c27523c1e7.png =400x400)   
    - 即第一个连接持续到计数器一次循环结束时开始关闭，然后又立刻建立新的连接，若没有`TIME_WAIT`，网络中还存在上次连接的数据包可能会在新的连接创立时到达，造成数据混乱
##### 保证被动关闭方正确关闭
![SureOpCut](https://img-blog.csdnimg.cn/img_convert/3a81c23ce57c27cf63fc2b77e34de0ab.png =400x400) 
- 避免被动方因收到`RST`报文，导致异常终止的关闭
#### 为什么`TIME_WAIT`是2MSL？
- `MSL`：Maximum Segment Lifetime,报文最大生存时间
- `MSL`和`TTL`的区别：   
    - `TTL`是经过路由跳数，`MSL`是时间，因此MSL 应该要大于等于 TTL 消耗为 0 的时间
- `TTL`一般为64，Linux将MSL设置为30秒，意味着 Linux 认为数据报文经过 64 个路由器的时间不会超过 30 秒，如果超过了，就认为报文已经消失在网络中了。
- **A**:   网络中可能存在来自发送方的数据包，当这些发送方的数据包被接收方处理后又会向对方发送响应，所以一来一回需要等待 2 倍的时间。
#### [TIME_WAIT过多的危害](https://xiaolincoding.com/network/3_tcp/tcp_interview.html#time-wait-%E8%BF%87%E5%A4%9A%E6%9C%89%E4%BB%80%E4%B9%88%E5%8D%B1%E5%AE%B3) 
#### [如何优化TIME_WAIT](https://xiaolincoding.com/network/3_tcp/tcp_interview.html#%E5%A6%82%E4%BD%95%E4%BC%98%E5%8C%96-time-wait) 


## TCP分割数据
- 概念：
    - `MTU` 一个网络包最大长度，以太网中一般为`1500`字节
    - `IP`和`TCP`包头一般为20字节
    - `MSS` 除去`IP`和`TCP`，一个TCP数据包能包含的最大长度，一般为`1460=1500-20-20`
![segmessage](./imgs/segmessage.jpg)
- 若HTTP请求消息较长，超出`MSS`范围，TCP需要将其拆分为一个个数据包，而不是一次性发送
![segprocess](./imgs/segprocess.jpg)
### 粘包
- 粘包发生在TCP报文中，原因是一个报文中可能存在多个用户的消息，那么如何区分呢？
    - HTTP 协议通过设置回车符、换行符作为 HTTP header 的边界，通过 Content-Length 字段作为 HTTP body 的边界，这两个方式都是为了解决“粘包”的问题

## 字节流和报文流
- **UDP是面向报文流**，**TCP是面向字节流**，如何理解呢？
    - 操作系统不会拆分UDP数据报，一个UDP报文就是一个用户的完整消息；同时操作系统接收到UDP报文后会将其推入队列中，通过`recvfrom()`获取，如下图：
    ![UDP](https://img-blog.csdnimg.cn/img_convert/a9116c5b375d356048df033dcb53582e.png)
    - 而操作系统可能会将TCP拆分成多个报文(超出`MSS`)，导致一个报文里可能会有多个用户的消息，因此不能认为一个用户对应一个TCP报文

## TCP如何处理历史数据包

### 初始序列号要求不一样
- TCP避免历史报文的方法：每次建立连接时随即初始化ISN
![ISN](https://cdn.xiaolincoding.com/gh/xiaolincoder/network/tcp/isn%E4%B8%8D%E7%9B%B8%E5%90%8C.png =400x400)

#### ISN如何随即产生
- RFC793 提到初始化序列号 ISN 随机生成算法：ISN = M + F(localhost, localport, remotehost, remoteport)  
    - M 是一个计时器，这个计时器每隔 4 微秒加 1。
    - F 是一个 Hash 算法，根据源 IP、目的 IP、源端口、目的端口生成一个随机数值。要保证 Hash 算法不能被外部轻易推算得出，用 MD5 算法是一个比较好的选择。

