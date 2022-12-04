# 基本概念
- **两点之间**的超文本传输协议(**双向协议**)
    - 两点之间：不限于<服务器--主机>，也可以<服务器--服务器>
    - 超文本：HTML就是典型的超文本，本身是文字文件，内部通过标签定义了图片、视频等的链接，经过浏览器的解释，呈现出有文字和画面的网页

# 头部
![httpmessage](./imgs/request.jpg)

# [常见的状态码](https://xiaolincoding.com/network/2_http/http_interview.html#http-%E5%B8%B8%E8%A7%81%E7%9A%84%E7%8A%B6%E6%80%81%E7%A0%81%E6%9C%89%E5%93%AA%E4%BA%9B)

# [常见的字段](https://xiaolincoding.com/network/2_http/http_interview.html#http-%E5%B8%B8%E8%A7%81%E5%AD%97%E6%AE%B5%E6%9C%89%E5%93%AA%E4%BA%9B)
- Host字段：指定服务器域名  
    ```markdown
    HostName: www.A.com
    ```
- Content-length: 指明本次回应的数据长度
    - HTTP用TCP协议，该字段可防止**粘包**
- Connection: 常用于要求服务器使用**HTTP长连接**  
    `Connection: Keep-Alive`
- Content-Type: 指明数据的格式  
    - `Content-Type: text/html; charset=utf-8` 服务器说明数据文本为`html`，编码为`utf-8`
    - `Accept: */*` 客户端声明自己可以接受任何格式
- Content-Encoding: 指明数据压缩方法  
    `Content-Encoding: gzip`

# 缓存
## 强制缓存
- 强制缓存指浏览器只要判断缓存**没有过期**，就使用缓存，**决定权在浏览器这边**
- 判断依据：
    - `Cache-Control` 是一个相对时间；
    - `Expires` 是一个绝对时间；
- `Cache-Control`优先级更高
- 服务器在第一次返回资源时，在Response头部中加入`Cache-Control`，之后浏览器会先检查是否过期，若过期则再次向服务器请求，并更新`Cache-Control`

## 协商缓存
- 若本地缓存过期，浏览器发送标识符给服务器，若标识符一致服务器返回响应码`304`，指示浏览器使用本地缓存；若不一致返回响应码`200`，发送请求的内容并在Response头部加上最新的标识符。
- 两种标识符：
    - 请求头部的`If-Modified-Since`和响应头部的`Last-Modified`
    - 请求头部的`If-None-Match`和响应头部的`ETag`
- `Etag`优先级更高，原因如下：
    - 打开但没有修改文件的情况，也可能更改文件的修改时间；
    - 有些文件修改小于1秒，`If-Modified-Since`的粒度是秒级，难以检测到；

# [HTTP 1.1的性能](https://xiaolincoding.com/network/2_http/http_interview.html#http-1-1-%E7%9A%84%E4%BC%98%E7%82%B9%E6%9C%89%E5%93%AA%E4%BA%9B)
## [优点](https://xiaolincoding.com/network/2_http/http_interview.html#http-1-1-%E7%9A%84%E4%BC%98%E7%82%B9%E6%9C%89%E5%93%AA%E4%BA%9B)
## 缺点
### 两个双刃剑
- 无状态
    - 优点：服务器不用耗费额外资源记录信息，减小负担
    - 缺点：服务器没有记忆能力，无法完成关联性的操作
- 明文传输
    - 优点：方便调试
    - 缺点：信息泄漏风险
    - 可以通过HTTPS解决，即引入SSL/TLS层
## 性能
- 长连接
    - 省去TCP传输三次握手的开销，但仍是<请求-响应>模式
- 队头阻塞
    - <请求-响应>模式导致，第一次请求长时间未得到响应，导致之后的请求一直等待而产生阻塞。  
    ![zuse](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTP/18-%E9%98%9F%E5%A4%B4%E9%98%BB%E5%A1%9E.png)

# HTTPS
## 区别
- HTTP在TCP三次握手后，还要进行SSL/TLS握手
- HTTP默认`80`端口，HTTPS默认`443`端口
- HTTPS 协议需要向 CA（证书权威机构）申请数字证书，来保证服务器的身份是可信的。
## [HTTPS如何连接](https://xiaolincoding.com/network/2_http/http_interview.html#https-%E6%98%AF%E5%A6%82%E4%BD%95%E5%BB%BA%E7%AB%8B%E8%BF%9E%E6%8E%A5%E7%9A%84-%E5%85%B6%E9%97%B4%E4%BA%A4%E4%BA%92%E4%BA%86%E4%BB%80%E4%B9%88)
##
