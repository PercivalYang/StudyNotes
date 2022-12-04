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

