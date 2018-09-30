---
layout:     post
title:      Http&Https
date:       2017-07-14
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - http
    - https
    
---
刚工作的时候，一个大牛说他面试了一个哥们连http, https都说不清楚还好意思做开发...    
然后我默默的回家恶补http,https....

---

## Http(报文由Http Header 和 Http Body组成)
- 基于tcp协议    
- 短连接    
- 无状态    

### Http基于TCP的通讯机制
1. Web浏览器首先要通过网络与Web服务器建立TCP连接，三次握手  
2. Web浏览器向Web服务器发送请求命令，例如：GET/sample/hello.jsp HTTP/1.1  
3. Web服务器确认。  
4. Web服务器向浏览器发送数据。  
5. 浏览器确认。  
6. 浏览器发送请求。  
7. Web服务器向浏览器发送数据，协议的版本号和应答状态码。例如：HTTP/1.1 200 OK。它会发送一个空白行来表示头信息的发送到此为结束，接着以Content-Type应答头信息所描述的格式发送用户所请求的实际数据  
8. Web服务器关闭TCP连接。一般情况下，一旦Web服务器向浏览器发送了请求数据就要关闭TCP连接，如果浏览器或者服务器在其头信息加入了这行代码Connection:keep-alive，TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求，节省为每个请求建立新连接所需的时间，节约网络带宽。  


### Http Header    
#### Http通用头域    
* Cache-control 头域  
指定请求和响应遵循的缓存机制,请求时的缓存指令包括no-cache、no-store、max-age、max-stale、min-fresh、only-if-cached，响应消息中的指令包括public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age。各个消息中的指令含义如下：    

```
Public指示响应可被任何缓存区缓存。  
Private指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。这允许服务器仅仅描述当前用户的部分响应消息，此响应消息对于其他用户的请求无效。  
no-cache指示请求或响应消息不能缓存  
no-store用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。   
max-age指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应。  
min-fresh指示客户机可以接收响应时间小于当前时间加上指定时间的响应。  
max-stale指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。  
```

* Connection头域  
Keep-Alive（持久连接）、close。在http1.1中，client和server都是默认对方支持长链接，如果服务端接受持久连接，则会在响应header中同样携带Connection: Keep-Alive，这样客户端便会继续使用同一个TCP连接发送接下来的若干请求。（Keep-Alive的默认参数是[timout=5, max=100]，即一个TCP连接可以服务至多5秒内的100次请求）  
不论request还是response的header中包含了值为close的connection，都表明当前正在使用的tcp链接在当天请求处理完毕后会被断掉  
* Data头域
消息发送的时间，时间的描述格式由rfc822定义。例如，Date:Mon,31Dec200104:25:57GMT。Date描述的时间表示世界标准时，换算成本地时间，需要知道用户所在的时区。  
* Pragma头域
Pragma头域用来包含实现特定的指令，最常用的是Pragma:no-cache。在HTTP/1.1协议中，它的含义和Cache-Control:no-cache相同。  

#### Http请求消息头域
![img](https://dongjx.github.io/img/posts/http-request-header.jpg)    
* MethodSPRequest-URISPHTTP-VersionCRLF  
Method表示对于Request-URI完成的方法，这个字段是大小写敏感的，包括 `OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE`  
* host头域  
指定请求资源的Intenet主机和端口号，必须表示请求url的原始服务器或网关的位置。HTTP/1.1请求必须包含主机头域，否则系统会以400状态码返回  
* Referer头域  
Referer头域允许客户端指定请求uri的源资源地址，这可以允许服务器生成回退链表，可用来登陆、优化cache等。他也允许废除的或错误的连接由于维护的目的被追踪。如果请求的uri没有自己的uri地址，Referer不能被发送。如果指定的是部分uri地址，则此地址应该是一个相对地址。  
* Range头域  
Range头域可以请求实体的一个或者多个子范围。例如，  

```
表示头500个字节：bytes=0-499  
表示最后500个字节：bytes=-500  
表示500字节以后的范围：bytes=500-  
第一个和最后一个字节：bytes=0-0,-1  
同时指定几个范围：bytes=500-600,601-999  
```

但是服务器可以忽略此请求头，如果无条件GET包含Range请求头，响应会以状态码206（PartialContent）返回而不是以200（OK）  
* User-Agent头域  
User-Agent头域的内容包含发出请求的用户信息  

#### Http响应消息头域
![img](https://dongjx.github.io/img/posts/http-response-header.jpg)    
* HTTP-VersionSPStatus-CodeSPReason-PhraseCRLF  
HTTP-Version表示支持的HTTP版本，例如为HTTP/1.1。Status-Code是一个三个数字的结果代码。  
Reason-Phrase给Status-Code提供一个简单的文本描述。Status-Code主要用于机器自动识别，Reason-Phrase主要用于帮助用户理解。Status-Code的第一个数字定义响应的类别，后两个数字没有分类的作用。第一个数字可能取5个不同的值：  

```
1xx:信息响应类，表示接收到请求并且继续处理
2xx:处理成功响应类，表示动作被成功接收、理解和接受
3xx:重定向响应类，为了完成指定的动作，必须接受进一步处理
4xx:客户端错误，客户请求包含语法错误或者是不能正确执行
5xx:服务端错误，服务器不能正确执行一个正确的请求
```

* Location响应头  
Location响应头用于重定向接收者到一个新URI地址。  
* Server响应头  
Server响应头包含处理请求的原始服务器的软件信息。此域能包含多个产品标识和注释，产品标识一般按照重要性排序。  

#### 实体信息头域: Http请求&响应都可以包含
实体头域包含关于实体的原信息Allow、Content-Base、Content-Encoding、Content-Language、Content-Length、Content-Location、Content-MD5、Content-Range、Content-Type、Etag、Expires、Last-Modified、extension-header。extension-header允许客户端定义新的实体头，但是这些域可能无法被接受方识别。实体可以是一个经过编码的字节流，它的编码方式由Content-Encoding或Content-Type定义，它的长度由Content-Length或Content-Range定义。  
* Content-Type实体头  
Content-Type实体头用于向接收方指示实体的介质类型，指定HEAD方法送到接收方的实体介质类型，或GET方法发送的请求介质类型  
* Content-Range实体头  
Content-Range实体头用于指定整个实体中的一部分的插入位置，他也指示了整个实体的长度。在服务器向客户返回一个部分响应，它必须描述响应覆盖的范围和整个实体长度。一般格式：  
Content-Range:bytes-unitSPfirst-byte-pos-last-byte-pos/entity-length  
例如，传送头500个字节次字段的形式：Content-Range:bytes0-499/1234如果一个http消息包含此节（例如，对范围请求的响应或对一系列范围的重叠请求），Content-Range表示传送的范围，Content-Length表示实际传送的字节数。    
* Last-modified实体头  
Last-modified实体头指定服务器上保存内容的最后修订时间。   

## Https（SSL/TLS+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比HTTP协议安全，默认端口443）
### 不对称加密算法
对称密钥加密算法：编 / 解码使用相同密钥的算法  
不对称密钥加密算法：编 / 解码使用不同密钥的算法 — RSA算法    
### SSL/TSL协议
- SSL：Secure Socket Layer，安全套接字层，SSL协议位于TCP/IP协议与各种应用层协议之间，利用数据加密(Encryption)技术，确保数据在网络上之传输过程中不会被截取。SSL协议可分为两层： SSL记录协议（SSL Record Protocol）：它建立在可靠的传输协议（如TCP）之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。 SSL握手协议（SSL Handshake Protocol）：它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份认证、协商加密算法、交换加密密钥等。
- TLS：Transport Layer Security，传输层安全协议，用于两个应用程序之间提供保密性和数据完整性。TLS 1.0建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本，可以理解为SSL 3.1。该协议由两层组成： TLS 记录协议（TLS Record）和 TLS 握手协议（TLS Handshake）。  

### SSL/TSL 协议通讯机制
1. Jack(client)给出协议版本号、一个客户端生成的随机数（Client random），以及客户端支持的加密方法。    
2. Tom(server)确认双方使用的加密方法，并给出数字证书、以及一个服务器生成的随机数（Server random）。    
3. Jack确认数字证书有效，然后生成一个新的随机数（Premaster secret），并使用数字证书中的公钥，加密这个随机数（非对称算法），发给Tom。    
4. Tom使用自己的私钥，获取Jack发来的随机数（即Premaster secret）。  
5. Tom和Jack根据约定的加密方法(对称算法)，使用前面的三个随机数，生成对话密钥（session key），用来加密接下来的整个对话过程。  
`SSL协议既使用了非对称算法，也使用了对称算法， 第五步以后，通讯也是使用对称算法加密的报文，没有使用非对称算法是性能的考虑哦`

### 数字证书
![img](https://dongjx.github.io/img/posts/ca.png)   
#### 数字证书的主要组成
1. 企业信息   
3. 有效日期  
4. 公钥信息  
5. 签发者信息
6. 证书签名

### 根证书
![img](https://dongjx.github.io/img/posts/roots-ca.png)    
打开mac系统的Keychain Access里面的`system roots&certificates`看到系统所有受信任的根证书:      
- 这些根证书是系统内置的，都是由一些权威的ca机构颁发的     
- 这些根证书都是自签证书，意思就是他们证书的签名可以通过他们自己的公钥信息解析的    

### 数字证书有效性
1. 当client访问一个https网站的时候, 会拿到对应网站的数字证书     
2. client会先校验证书中的有效日期    
3. client会校验该证书的签发者证书是否受信任     

```
这里是一个链式校验:
如果签发者证书已经是受信任的，校验通过  
如果签发者证书未受信任，则对签发者证书进行第1步~第5步的校验
直到根证书
```

4. 签发者证书受信任，用签发者证书的公钥解开当前网站的证书签名，与企业信息，有效日期，公钥信息等比对是否都一致    
5. 验证当前的网站证书的host和client访问的host是否一致    
