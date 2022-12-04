# ICMP
- ping是基于`ICMP`协议工作的
## 包头格式
![HeadFormat](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/5.jpg =400x400) 
## ICMP类型
![ICMPTypes](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/6.jpg =400x300) 
### 查询报文类型
![QueryM](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/8.jpg) 
- 选项数据中ping还会存放发送请求的时间值，来计算往返时间，说明路程的长短。
### 差错报文类型
#### 目标不可达消息
- 常见的6种DUM的**代码段**：  
![DUM](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/9.jpg =400x280) 
- 网络不可达：路由器匹配不到IP的网络号
- 主机不可达：路由表匹配不到IP的主机号
- 协议不可达：例如防火墙禁止TCP协议
- 端口不可达：目标主机端口没有应用程序监听
- Fragmentation needed but no frag：如果将**IP首部的分片禁止标志位**设置为`1`。路由器遇到超出`MTU`的包会直接丢弃

#### 原点抑制消息
- 当路由器向低速线路发送数据时，其发送缓存队列已经满时，会向源IP地址发送一个`ICMP`的原点抑制消息，从而让源主机增大该IP包的传输间隔

#### 重定向消息
- 路由器发现发送端使用的不是最优路线，会发送个ICMP重定向消息给源主机
- Q: 如何发现不是最优路线？

#### 超时消息
- IP包有一个字段`TTL`，没经过一个路由器减1直到0时被丢弃，此时路由器会向源主机发送一个ICMP超时消息；
- 目的是避免IP包在网络上无休止的转发

