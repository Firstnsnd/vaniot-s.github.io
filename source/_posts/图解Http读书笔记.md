---
title: 图解Http读书笔记
date: 2018-01-21 20:34:07
tags: [http,计算机网络]
categories: basic
---
### 1. 网络基础
TCP/IP协议族:

- 1.IEEE 802.3
- 2.FDDI
- 3.ICMP
- 4.IP
- 5.TCP
- 6.HTTP
- 7.PPPoE
- 8.DNS
- 9.FTP
- 10.UDP
-  11.SNMP

#### TCP/IP分层管理
 4层:应用层、传输层、网络层、数据链路层

应用层：决定向用户提供应用服务时通信的活动。(FTP:文件传输协议,DNS:域名系统,HTTP)

传输层：为应用层提供网络连接中的两台计算机之间的数据传输,(TCP:传输控制协议,UDP:用户数据报协议)

网络层:处理网络上流动的数据包(数据包是网络传输的最小的数据单位),规定传输路线。

链路层:链接网络的硬件部分(NIC:网卡)

TCP/IP通信传输流

完整的http请求：
发送端(封装：把数据信息包装起来)：传输层(TCP协议)从把应用层(http协议)收到的数据(HTTP请求报文)分割,在各个报文上打上标记序号及端口号后发给网络层.在网络层(IP协议),增加通信目的地的MAC地址后转发给链路层。

接收端：
将发送端每一层加上的头部信息去掉
image

IP(网络层）
IP协议:把各种数据包传送给对方(由IP地址、MAC地址确定),

IP地址:节点被分配的地址(可变换)。

MAC地址:网卡所属的固定位置(不可变)。
IP间的通信。

与HTTP关系密切的协议:IP、TCP、DNS
网络层的IP协议(负责传输):
对于不在同一局域网(LAN)的通信会利用下一站中转设备的MAC地址搜索下一个中转目标(ARP协议),

传输层的TCP协议:提供字节流服务[将大块的数据分割为报文段(segment)为单位的数据包进行管理,更容易传送大数据],

TCP三次握手：

1、发送端先发送一个带有SYN(synchorize)标志的数据包给接收端,

2、接收端接收回传带有SYN/ACK标志的数据包以示传达确认信息

3、发送端回传ACK标志数据包
image

域名解析DNS
DNS:应用层的协议，域名到IP地址间的解析服务。
image
HTTP协议、TCP协议、IP协议、DNS服务的关系：
image

URI和URL
URI(Uniform Resource Identifier)：统一资源标识符,由某个协议方案表示的资源的定位标识符。
Uniform:规定的统一格式,协议方案
Resource:资源,可标识的
Identifier:可标识的对象
URI的格式：
image
登录信息：指定用户和密码作为从服务器端获取资源时必要的登录信息(可选)
服务器地址：使用绝对URI必须指定待访问的服务器地址(DNS可解析的名称、IPV4地址、IPV6地址)
服务端端口号:指定服务器连接的网络端口号(可选,省略使用默认的端口号)
带层次的文件路径:指定服务器上的文件路径来指定特定的资源
查询字符串:针对已指定的文件路径内的资源,可使用查询字符串闯入任意的参数(可选)
片段标识符：使用片段标识符(可选)可表记出已经获取资源中的子资源(如网页中的某个位置)，
URL(Uniform Resource Locator):统一资源定位符,
image
2.简单的HTTP协议
1.客户端和服务端的通信
客户端：请求访问文本或图像资源的一端，通信由客户端建立，发出请求(请求报文)
请求报文：image
http/1.1 特点：

不保存状态的协议，为了验证身份引入了cookie
使用URI定位资源
http方法：

1.GET:获取资源，访问已被URI识别的资源，返回服务器解析后的结果(一般用于获取响应的主体)，未变化返回304 not modified。

2.POST：传输实体的主体,与GET类似,响应返回接受数据的处理结果。

3.PUT:传输文件，在请求报文的主体包含文件内容，不会对文件的内容做验证，若无其他的验证方式(web验证、rest架构)不采用，响应返回204 no content等。

4.HEAD：获取报文的首部

5.DELETE:删除文件，按请求的URI指定的资源，不会对内容作出验证

6.OPTIONS:查询针对URI指定的资源支持的方法

7.TRACE:追踪路径，让web服务器将之前的请求通信环回给客户端

8.CONNECT:用隧道协议链接代理，与代理服务器通信时建立隧道，实现用

方法命令： 在向URI指定的资源发送请求报文时，指定方法，资源按期望产生某种行为。
持久连接：keep-alive 多个请求在一次连接中处理，在任意一端未提出断开连接，保持tcp连接状态
管线化：在持久连接后，可多次发出请求而不需要响应后再发出请求。
服务端：提供资源响应的一端，服务端接受到请求返回消息(响应报文)
响应报文：image

3.http报文
http报文：http协议交互的信息,(种类：请求报文、响应报文)报文内容分为：
报文首部(服务端或客户端处理的请求或响应的内容及信息)报文主体(数据，非必需的)
image

报文主体：HTTP通信的基本单位，由8位组字节流组成，通过过HTTP通信传输

实体主体：作为请求或响应的有效载荷数据(补充项)被传输，其内容由实体首部和实体主体组成

http报文的主体用于传输请求或响应实体主体，通常报文主体与实体主体相同,只有当传输中进行编码操作时，实体的主体内容发生变化。

请求报文：
image
请求行：请求的方法，请求的URI和HTTP版本

响应报文：
image

响应行：响应结果的状态码,原因短语和http版本

头部字段(通用首部、请求首部、响应首部、实体首部)：请求和响应的各种条件和属性的各类首部

其他：未定义的首部

编码：

压缩内容编码：
gzip(GNU zip)
compress(UNIX系统的标准压缩)
deflate(zlib)
identity(不进行编码)
分割发送的分块传输编码
将实体分成多块每一块都会用十六进制来标记块的大小，最后一块会使用”0(CR+LF)”
发送多种数据的(MIME拓展 Multipart)多部分对象集合 RFC2046

multipart/form-data web表单文件上传时使用

1
2
3
4
5
6
7
8
9
Content-Type:multipart/form-data;boundary=AaB03x//boundary 标识符
--AaB03x//部分开始
Content-Disposition:form-data;name="field1"
Joe Blow
--AaB03x
Content-Disposition:form-data;name="pics";filename="file1.txt"
Content-type:text/plain
...(file.txt的数据)...
--AaB03x--//对象结束标识符
multipart/byteranges 状态码206(partial Content,部分内容)响应报文包含了多个范围内容时使用

1
2
3
4
5
6
7
8
9
10
Content-Type:multipart/form-data;boundary=THIS_STRING_SEPARATES//boundary 标识符
--THIS_STRING_SEPARATES//部分开始
Content-Type:application/pdf
Content-Range:bytes 500-999/8000
...(范围指定的数据)...
--THIS_STRING_SEPARATES
Content-type:text/pdf
Content-Range:bytes 7000-7999/8000
...(范围指定的数据)...
--THIS_STRING_SEPARATES--//对象结束标识符
获取部分内容的范围
1
2
3
4
5
6
5001-10000字节
 Range:byte=5001-10000
5001后的全部字节
 Range:byte=5001-
多重范围
Range:byte=-3000,5001-7000 //开始到3000，5001到7000
内容协商：客户端与服务端就响应的资源内容进行交涉，然后客户端提供给客户端最为合适的资源。

请求报文的首部进行

Accept
Accept-Charset
Accept-Encoding
Accept-Language
Content-Language
内容协商技术的分类：

服务端驱动协商：服务器以请求的首部字段为参考，在服务端进行处理
客户端驱动协商：用户从浏览器显示的可选项列表中手动选择
透明协商：服务器和客户端各自进行内容协商的一种方法
4.HTTP状态码
状态码	|类别	|原因短语
--|--|--
1XX	|information(信息状态码)	|接受请求正在处理
2XX	|success(成功状态码)	|请求处理完毕
3XX	|redirection(重定向状态码)	|需要附加操作以完成请求
4XX	|Client Error(客户端错误状态码)	|服务器无法处理的请求
5XX	|Server Error(服务端状态码)	|服务器处理请求出错
常用的14种状态码
2XX：客户端发来的请求被正常的处理
1.   204(No Content):返回的响应报文中不含(不允许)主体部分，浏览器的页面不会发生任何变化。(一般用于浏览器往服务端发送数据,而不需要返回数据)
2.   206(Partial Content):客户端进行了范围请求，而服务端成功执行了这部分GET请求，响应报文中包含Content-Range指定范围的实体内容
3XX:重定向
1.   301(Moved Permanently)永久重定向，请求的资源已被分配了新的URI，以后都使用
2.   302(Found)临时性重定向,请求的资源已经被分配了新的URI，本次使用
3.   303(See Other)请求对应的资源存在另一个URI，应使用GET方法定向获取请求的资源。
4.   304(Not Modified)客户端发送请求附带条件的请求，服务器端允许请求访问资源，但因为发生请求未满足条件，返回304
5.   307(Temporay Redirect)临时重定向
4XX客户端错误
1.   400(Bad Request)客户端请求的报文中存在语法错误,
2.   401(Unauthorized)发送的请求需要通过HTTP认证(BASIC认证、DIGEST认证)的认证信息，若之前已经请求一次，则表示用户认证失败，第一次会弹出认证的对话框。
3.   403(Forbidden)请求的资源被服务器拒绝。
4.   404(Not Found)服务器上不能找到请求的资源
5XX服务器错误
1.  500(Internet Server Error)服务器端在执行请求时发生错误
2.  503(Service Unavailable)服务器暂时处于超载或正在进行维护。
5.http协作与web服务器
数据转发程序(程序和服务器将请求转发给通信线路上的下一站服务器，并将其返回的响应转发给客户端)
代理：有转发功能的应用程序,扮演服务端与客户端的”中间”,转发两者的请求。
image
代理服务器：接受客户端发送的请求后转发给其他代理服务器，代理不会改变请求的URI，直接转发给持有资源的服务器。资源服务器将会把响应返回给代理服务器在返回给客户端
image
多级联台代理在转发时需要添加Via首部字段标记经过的主机信息
代理服务器的优势：

利用缓存技术减少网络带宽的流量
组织内部针对特定网站访问的控制
以获取访问日志为主的目的
代理分类：

1.是否使用缓存：代理转发响应时，缓存代理会预先将资源的副本保存在代理服务器上
2.是否修改报文：透明代理不会对报文做任何加工，非透明代理度对报文进行加工

网关：网关转发其他服务器通信数据的服务器，网关可以使通信线路上的服务器提供非http协议服务。

image

网关的优势：

提高通信的安全性，可在客户端与网关的通信线路上加密
2.连接非http非http服务器
隧道：在相隔甚远的客户端和服务器端两者间进行中转，使用SSL等手段进行加密，并保持双方通信连接的应用程序。

![image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSaCYhgpzMfEypGQJ_H_NnqHgk2y2ETuMk0IzPhDuKmqx1iDRWAwA

缓存
缓存：代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可减少对资源服务器的访问，节省了通信流量和通信时间。

缓存服务器：是代理服务器的一种，归于缓存代理中，代理服务器转发服务器的响应时，代理服务器会保存一份资源的副本，下次请求从就近的缓存服务器上获取资源。缓存过了有效期会重新请求。

客户端缓存：将缓存存在客户端浏览器，在有效期内直接访问本地的缓存文件。

6.http首部
http协议的请求报文和响应报文中必定包含HTTP首部，首部内容为客户端和服务器分别处理请求和响应提供了所需的信息。

1. 4种HTTP首部字段类型
通用首部字段：请求和响应均会使用的首部
image
Via:可用于追踪报文的转发，可避免请求回环的发生，在经过代理时附加该首部字段的内容。

1
Via:1.0 gw.hacker.jp(squid/3.1),1.1 a1.wxample.com(squid/2.7)
Warning:告知用户与缓存相关的问题的警告

1
Warning:[警告码][警告的主机:端口号] "[警告的内容]" ([日期时间])
image

Cache-Control:操作缓存工作机制
image
image

请求首部字段：从客户端发送请求报文时使用的首部，补充了请求的附加内容、客户端信息、响应内容相关优先等级等信息
image

Accept:通知服务器用户代理可以处理的媒体类型及媒体类型的相对优先级，
1
2
3
4
5
6
7
8
9
10
Accept:text/html.application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 //q优先等级
文本文件
    text/html,text/plain,text/css...
    application/xhtml+xml,application/xml...
图片文件
    image/jpeg,imge/gif,image/png...
视频文件
  video/mpeg,video/quicktime...
应用程序使用的二进制文件
   application/octet-stream,application/zip...
Accept-Charset:通知服务器用户代理支持的字符集及字符集的相对优先顺序

1
Accept-Charset:iso-8859-5,unicode-1-1;q=0.8
Accept-Encoding:告知服务器用户代理支持的内容编码及内容编码的优先级顺序，可一次指定多种内容编码

1
Accept-Encoding:gzip,deflate
gzip:gzip程序生成的编码格式

compress:unix中文件压缩程序compress生成的编码格式

deflate:组合使用zlib格式及deflate压缩算法生成编码格式

identity:不压缩或不会变化的默认编码格式

Accept-Language:告知服务器用户代理能够处理的自然语言及自然语言集的相对优先级

1
Accept-Language:zh-cn,zh;q=0.7,en-us,en;q=0.3
Authorization:告知服务器，用户代理认证信息
image

Expect:客户端告知服务器期望出现的某种行为
1
Except：100-continue //等待状态100响应的客户端发生请求时需指定
From:告知服务器使用用户代理的电子邮件地址

1
From:vaniot@yeah.net
Host:告知服务器请求资源所处在的互联网的主机名和端口号(必须被包含在请求的首部字段)

1
Host:www.vaniot.com
If-XXX:条件请求，服务器接收到附带条件的请求后，只有判断指定条件为真时才会执行请求

If-Modified-Since:告知服务器若If-Modified-Since字段值(缓存页面的修改时间)，若早于服务器上的修改时间更新，否者返回304
If-None-Match:当字段值的实体标记(ETag)值与请求资源的ETag不同时,服务器处理该环境
IF-Match:告知服务器匹配资源所用的实体标记(ETag)值,服务器会匹配资源的ETag值，当两者一致时会执行请求。

1
2
GET /index.html
If-Match:"123456"
If-Range：告知服务器若指定的If-Range字段值(ETag值或时间)与请求的值一致则返回范围内的值否则返回全体资源。

If-Unmodified-Since:在指定的日期内为发生改变就进行处理。
Max-Forwards:通过TRACE 方法或OPTIONS方法，发送包含首部字段Max-Forwards的请求时,该字段以十进制整数的形式可经过服务器的最大的数目。经过一个服务器值减一。当值为0时不再转发而是直接响应返回
Proxy-Authorization:接收到代理服务器发来的认证质询时客户端会发送包含首部字段Proxy-Authorization的请求，以告知服务器认证所需的信息。
Range:秩序获取部分资源的范围请求，
Referer:请求资源的原始URI
TE:客户端能够处理响应的传输编码方式(还可指定伴随trailer字段的分块传输的方式，应用时把trilers赋值给该字段)
User-Agent:将创建请求的浏览器和用户代理名称等信息传达给服务器
响应首部字段：从服务器端返回响应报文时使用的首部，补充响应的附加内容，也会要求客户端附加额外的内容信息
image

Accept-Ranges:告知客户端服务器可以处理的范围请求，指定服务器端某个部分的资源，其值有两种：1.在可处理范围内请求时指定为bytes，2.不在范围内指定为none
Age:告知客户端源务器在多久前创建了响应,字段值的单位为秒。若创建该响应的服务器的是缓存服务器，age值是指缓存后响应再次发起认证到认证完成的时间值，代理创建响应时必须加上首部字段Age。
ETag:告知客户端实体标识，将资源以字符串形式做唯一性标识的方式。服务器会为每一份资源分配对应的ETag值。资源更新时会将ETag更新，下载等情况都会根据ETag指定资源。

1
2
3
4
强ETag(不论实体发生多么细微的变化都会改变其值)
ETag:"usagi-1234"
弱ETag(用于提示资源是否相同，只有资源发生根本改变产生差异时才会改变ETag值)
ETag:W/"usagi-1234"
Location：将响应接收方引导至某个与请求URI位置不同的资源，重定向操作

1
Location:http://www.vaniot.cn
Proxy-Authenticate:把由代理服务器所要求的认证信息发送给客户端

1
Proxy-Authenticate: Basic realm="Usagidesign Auth"
Retry-After:告知客户端应该在多久之后再次发送请求。字段值为多少秒后或具体的日期

1
Retry-After:120
Server: 告知客户端当前服务器上安装的HTTP服务器应用程序的信息，

1
Server:Apache/2.2.6(Unix) php/5.2.5
Vary:对缓存进行控制,源服务器会向代理服务器传达关于本地缓存使用方法的命令，会对请求中含有相同的首部字段的 请求缓存

1
Vary:Accept-language
WWW-Authenticate：告知客户端使用于访问请求URI所指定资源的认证方案(Basic或是Diggest)和带参数提示的质询(challenge),状态码401Unauthorized响应中，带有 WWW-Authenticate

1
WWW-Authenticate:Basic realm="Usagidesign Auth"//realm字段辨别请求URI指定资源所受到的保护策略
实体首部字段：针对请求报文和响应报文的实体部分使用的首部，补充了资源内容更新时间等于实体相关的信息。
image
Allow:通知客户端能够支持Request-URI指定资源的所有http方法，当服务器接收到不支持的HTTP方法时，会以405 Method Not Allowed 作为响应返回，并将所支持的HTTP方法写入首部字段Allow后返回。
Content-Encoding:告知客户端服务器对实体的主体部分选用的内容编码方式，内容编码指在不丢失实体信息的前提下进行的压缩,编码方式：1.gzip 2.compress 3.deflate 3.identity

1
Content-Encoding:gzip
Content-Language:告知客户端,实体主体使用的自然语言

1
Content-Language:zh-CN
Content-Length:表明实体主体部分的大小(单位是字节),对实体主体进行内容编码传输,不可再使用Content-Length首部字段。

Content-Location:给出与报文主体相对应的URI(报文主体返回资源对应的URI)

1
Content-Location:http://www.vaniot.cn/index.html
Content-MD5:检测报文主体传输过程中是否保持完整

Content-Range:告知客户端作为响应返回实体的那个部分符合范围请求，字段以字节为单位，表示当前发送部分及整个实体大小。

1
Content-Range:bytes 5001-10000/10000
Content-Type:实体主体内对象的媒体类型与首部字段Accept一样，字段值用type/subtype形式赋值

1
Content-Type:text/html;charset=UTF-8
Expires:将资源失效的日期告知客户端，缓存服务器接收到含有首部字段Expires的响应后,会缓存应答请求，在Expires字段值指定的时间之前，响应的副本会一直被保存，当超过指定的时间后，缓存服务器在请求发送过来时，会转向源服务器请求资源。

Last-Modified:指明资源最终修改的时间
2. 非HTTP/1.1首部字段
Cookie:用户识别及状态管理
1
Cookie:status=enable//告诉服务器客户端想获得HTTP状态管理支持
首部字段名	说明	首部类型
Set-Cookie	开始管理所使用的Cookie信息	响应首部字段
Cookie	服务器接收到的Cookie信息	请求首部字段
Set-Cookie：当服务器准备开始管理客户端的状态时，会事先告知各种信息。

1
Set-Cookie:status=enable;expires=Tue, 09 Jul 2017 07:09:43 GMT;=>path=/;domain=.vaniot.cn;
属性	说明
NAME=VALUE	赋予Cookie的名称和其值(必须项)
expires=Date	Cookie的有效期(若不明确指定则默认为浏览器关闭为止)
path=PATH	将服务器上的文件目录作为Cookie的适用对象(若不指定则默认为文档所在的文件目录)
domain=域名	作为Cookie适用对象的域名(若不指定则默认为创建Cookie的服务器的域名)
Secure	仅在HTTPS安全通信时才会发送Cookie
HttpOnly	加以限制,使Cookie不能被Javascript脚本访问
expires:指定浏览器可发送Cookie的有效期(ps:Cookie从服务器端发送到客户端，服务器端就不存在可显式删除Cookie的方法，只可通过覆盖的方法)
path:限制指定Cookie的发送范围的文件目录(有办法绕过不可作为安全机制)
domain:通过Cookie的domain属性指定域名可做到结尾匹配一致，可指定多个域名
secure:Cookie的secure属性用于限制Web页面仅在HTTPS安全连接，才可以发送Cookie

1
Set-Cookie:name=value;secure//当是HTTPS时会进行Cookie的连接
HttpOnly:拓展Cookie使Javascript不能获取Cookie，可防止跨站脚本攻击(XSS)对Cookie的信息窃取

1
Set-Cookie:name=value;HttpOnly
Content-Disposition:

X-Frame-Options:属于HTTP响应首部,用于控制网站内容在其他web网站的Frame标签内的显示,防止点击劫持攻击,(IE8 FIREFOX3.6.9+ CHROME4.1 SAFARI4+ OPERA 10.5+)可指定一下两个值：

DENY：拒绝
SAMEORIGIN:仅在同源域名下的页面匹配时许可。
在web服务器端预先设定好X-Frame-Options字段值,
apache.conf的配置:
1
2
3
<IfModule mod_headers.c>
    Header append X-Frame-Options "SAMEORIGIN"
</IfModule>
X-XSS-Options：HTTP响应首部,针对跨站脚本攻击的一种对策,用于控制浏览器XSS防护机制的开关。可指定的字段值:

0：将XSS过滤设置成无效状态
1:将XSS过滤设置成有效的状态
DNT(Do Not Track)：HTTP请求首部,拒绝个人信息被收集，拒绝被精准广告追踪的一种方法,字段值如下：

0:同意被追踪
1:拒绝被追踪
P3P(The Platform for Privacy Preferences):HTTP响应首部,让web网站上的个人隐私变成一种仅供程序可理解的形式，以达到保护用户隐私的目地
1
P3P：CP=“CAO DSP LAW CURa ADMa  DEVa TALa PsAa PSDa => IVAa IVDa OUR BUS IND UNI COM NAV INT"
3.首部定义代理缓存与非缓存代理的行为 ???
1.端到端首部(end-to-end)
此类别中的首部会转发给请求/响应对应的最终接受目标，且必须保存在由缓存生成的响应中，必须被转发
逐跳首部(Hop-by-hop)
首部只对单次转发有效，会因通过缓存或代理而不再转发。如果要使用hop-by-hop首部，需要提供Connection首部字段。只有以下8个首部字段属于逐跳首部字段：

Connection
控制不再转发给代理的首部字段

1
Connection:Upgrade//首部字段Upgrade被删除后再转发
管理持久连接(HTTP/1.1的连接默认都是持久的，之前均是非持久的)

1
2
Connection:Keep-Alive//打开持久连接
Connection:close//关闭连接
Keep-Alive
Proxy-Authenticate
Proxy-Authorization
Trailer(指定的首部字段会在报文主体后出现)

1
2
3
4
Trailer:Expries
...(报文主体)...
0
Expries:Tue ,28,Sep 2004 23:59:59  GMT
TE

Transfer-Encoding(传输报文的主体时采用的编码方式)

1
Transfer-Encoding:chunked
Upgrade(检测HTTP协议及其他协议是否可以使用更高的版本进行通信,参数值可以指定一个完全不同的通信协议，使用Upgrade时必须需使用Connection:UPgrade)

1
2
Upgrade:TLS/1.0
Connection:Upgrade
7.HTTPS
HTTP缺点
通信使用明文，内容可能会被窃听
HTTP本身不具备加密的功能，无法做到对通信整体进行加密，即HTTP报文使用明文方式发送
TCP/IP是可能被窃听的网络
image
不验证通信方的身份，可能会遭遇伪装
无法确定发送目标的web服务器是否按真实意图返回响应的服务器
无法确定接受响应的客户端是否是真实的客户端
无法确定正在通信的对方是否具备访问的权限
无法确定请求来自何方，
即使无意义的请求也会照单全收，无法阻止海量请求下的Dos攻击
无法验证报文的完整性，所以可能遭篡改
HTTP协议无法证明通信的报文完整性，即使遭到攻击者拦截并篡改内容(中间人攻击)，也无法获悉
HTTPS=HTTP+加密+认证+完整性保护
HTTPS
HTTPS是HTTP通信接口部分用SSL和TLS协议代替，HTTP不会先和TCP通信，当使用SSL时，先和SSL通信,HTTP就拥有加密、证书和完整性保护。

加密
1.通信加密：通过SSL(Secure Socket Layer,安全套接层)或TLS(Transport Layer Security,安全传输层协议)的组合使用，加密HTTP的通信内容
image

共享密钥加密：加密时需将密钥发送
公开密钥加密(SSL采用)：公开密钥加密使用一对非对称的密钥，
私有密钥:接收方使用私有密钥解密
公开密钥:发送方使用共有密钥加密
2.内容加密：将参与通信的内容本身加密，把HTTP报文里所含的内容进行加密处理,客户端会对HTTP报文进行加密处理后再发送请求(客户端和服务器同时具有加密和解密的机制，内容仍有被篡改的风险)
image

认证
使用HTTP协议无法确定通行的双方，SSL提供加密处理，使用证书的手段，用于确定通信的双方,

数据完整性
SSL提供认证和加密处理及摘要功能

HTTPS采用混合加密的方式加密：在交换密钥时使用公开密钥，通信阶段使用共享密钥(共享密钥速度高于公开密钥)
交换密钥时为了验证公开密钥的正确性，使用第三方数字认证书认证机构的公开密钥证书
image

HTTP安全通信机制
image
1.客户端通过发送client hello报文开始SSL通信.报文中包含客户端支持的SSL指定版本,加密组件列表(所使用的加密算法 密钥长度等)

2.服务器可进行SSL通信时,会议Server Hello报文作文回应.报文中含SSL版本 加密组件.服务器的加密组件内容是从接受客户端加密组件内筛选出来的.

3.之后服务器发送certificate报文.报文中包好公开密钥证书.

4.服务器发送server hello done报文通知客户端.最初阶段握手协商部分结束.

5.SSL第一次握手结束后,客户端以client key exchange报文作为回应.报文包含通信加密中使用的随机密码串.该报文已用步骤3中的公钥加密;

6.客户端继续发送change cipher spec报文.该报文会提示想服务器,在此报文之后的通讯会采用pre-master secret密钥加密

7.客户端发送finished报文 该报文包含连接至今前部报文的整体校验值.这次握手协商是否成功,要以服务器是否能够正确解密该报文作文判定标准.

8.服务器同样发送change cipher spec报文

9.服务器同样发送finished报文

10.C&S finishe报文交换完毕之后,ssl连接就算建立完成.通信会受到SSL的保护.从此开始进行应用层的协议通信,即发送HTTP响应

11.应用层协议通信,即发送HTTP响应

12.最后客户端断开连接.断开连接时,发送close_notity报文.上图做了一些省略

在以上流程中,应用层发送数据时会附加一种叫做MAC(message authentication code)的报文摘要.MAC能够查知报文是佛遭到篡改.从而保护报文的完整性.
image

8.用户身份认证
HTTP认证的方式
BASIC(基本认证)
是从 HTTP/1.0 就定义的认证方式。即便是现在仍有一部分的网站会使用这种认证方式。是 Web 服务器与通信客户端之间进行的认证方式。
image
步骤 1： 当请求的资源需要 BASIC 认证时，服务器会随状态码 401 Authorization Required，返回带 WWW-Authenticate 首部字段的响应。该字段内包含认证的方式（BASIC） 及 Request-URI 安全域字符串（realm）。
步骤 2： 接收到状态码 401 的客户端为了通过 BASIC 认证，需要将用户 ID 及密码发送给服务器。发送的字符串内容是由用户 ID 和密码构成，两者中间以冒号（:）连接后，再经过 Base64 编码处理。

步骤 3： 接收到包含首部字段 Authorization 请求的服务器，会对认证信息的正确性进行验证。如验证通过，则返回一条包含 Request-URI 资源的响应。

BASIC 认证使用上不够便捷灵活，且达不到多数 Web 网站期望的安全性等级(不是加密处理)，因此它并不常用。

DIGEST(摘要认证)
同样使用质询 / 响应的方式（challenge/response），但不会像 BASIC 认证那样直接发送明文密码；所谓质询响应方式是指，一开始一方会先发送认证要求给另一方，接着使用从另一方那接收到的质询码计算生成响应码。最后将响应码返回给对方进行认证的方式。
image
步骤 1： 请求需认证的资源时，服务器会随着状态码 401 Authorization Required，返 回带 WWW-Authenticate 首部字段的响应。该字段内包含质问响应方式认证所需的临时质询码（随机数，nonce）。
步骤 2： 接收到 401 状态码的客户端，返回的响应中包含 DIGEST 认证必须的首部字段 Authorization 信息。

步骤 3： 接收到包含首部字段 Authorization 请求的服务器，会确认认证信息的正确性。认证通过后则返回包含 Request-URI 资源的响应。

SSL客户端认证
SSL 客户端认证是借由 HTTPS 的客户端证书完成认证的方式。凭借客户端证书认证，服务器可确认访问是否来自已登录的客户端。
SSL 客户端认证的认证步骤
步骤 1： 接收到需要认证资源的请求，服务器会发送 Certificate Request 报文，要求客户端提供客户端证书。

步骤 2： 用户选择将发送的客户端证书后，客户端会把客户端证书信息以 Client Certificate 报文方式发送给服务器。

步骤 3： 服务器验证客户端证书验证通过后方可领取证书内客户端的公开密钥，然后开始 HTTPS 加密通信。

SSL 客户端认证采用双因素认证

所谓双因素认证就是指，认证过程中不仅需要密码这一个因素，还需要申请认证者提供其他持有信息，从而作为另一个因素，与其组合使用的认证方式。
SSL 客户端认证必要的费用

FormBase(基于表单认证)
认证多半为基于表单认证
由于使用上的便利性及安全性问题，HTTP 协议标准提供的 BASIC 认证和 DIGEST 认证几乎不怎么使用。另外，SSL 客户端认证虽然具有高度的安全等级，但因为导入及维持费用等问题，还尚未普及。
Session 管理及 Cookie 应用
基于表单认证的标准规范尚未有定论，一般会使用 Cookie 来管理 Session（会话）。
image

步骤 1： 客户端把用户 ID 和密码等登录信息放入报文的实体部分，通常是以 POST 方法把请求发送给服务器。而这时，会使用 HTTPS 通信来进行 HTML 表单画面的显示和用户输入数据的发送。

步骤 2： 服务器会发放用以识别用户的 Session ID。通过验证从客户端发送过来的登录信息进行身份认证，然后把用户的认证状态与 Session ID 绑定后记录在服务器端。

向客户端返回响应时，会在首部字段 Set-Cookie 内写入 Session ID（如 PHPSESSID=028a8c…）。可以把 Session ID 想象成一种用以区分不同用户的等位号。然而，如果 Session ID 被第三方盗走，对方就可以伪装成你的身份进行恶意操作了。因此必须防止 Session ID 被盗，或被猜出。为了做到这点，Session ID 应使用难以推测的字符串，且服务器端也需要进行有效期的管理，保证其安全性。另外，为减轻跨站脚本攻击（XSS）造成的损失，建议事先在 Cookie 内加上 httponly 属性。

步骤 3： 客户端接收到从服务器端发来的 Session ID 后，会将其作为 Cookie 保存在本地。下次向服务器发送请求时，浏览器会自动发送 Cookie，所以 Session ID 也随之发送到服务器。服务器端可通过验证接收到的 Session ID 识别用户和其认证状态。
另外，不仅基于表单认证的登录信息及认证过程都无标准化的方法，服务器端应如何保存用户提交的密码等登录信息等也没有标准化。通常，一种安全的保存方法是，先利用给密码加盐（salt）1 的方式增加额外信息，再使用散列（hash）函数计算出散列值后保存。但是我们也经常看到直接保存明文密码的做法，而这样的做法具有导致密码泄露的风险；salt 其实就是由服务器随机生成的一个字符串，但是要保证长度足够长，并且是真正随机生成的。然后把它和密码字符串相连接（前后都可以）生成散列值。当两个用户使用了同一个密码时，由于随机生成的 salt 值不同，对应的散列值也将是不同的。这样一来，很大程度上减少了密码特征，攻击者也就很难利用自己手中的密码特征库进行破解。

9.HTTP协议的拓展
HTTP的瓶颈
一条连接上只可以发送一个请求
请求只可以从客户端发起，客户端不能接受除了响应以外的指令
请求/响应首部未经压缩就发送,首部信息越多延迟越大
发送冗长的首部，每次互相发送相同的首部造成的浪费较多
可任意选择数据压缩格式，非强制压缩发送
image
http的优化手段：
1.Ajax:通过脚本语言Javascript调用XMLHttpRequest和HTTP通信，从已经加载完毕的web页面上发起请求,只更新局部页面，
2.Comet(反向AJAX):实现服务端向客户端推送,服务端接收到请求后，Comet会将响应挂起(服务器和客户端建立了一个永久的连接),当服务器内有内容更新时，再返回该更新。实现由两种方式：1.AJAX长轮询 2.Iframe流
image
SPDY
SPDY在TCP/IP应用层与运输层之间通过新加会话层的形式控制数据的流动，且规定使用SSL通信，采用HTTP建立通信连接，http中的方法Cookie、HTTP报文仍然适用
HTTP 应用层
SPDY 会话层
SSL 表示层
TCP 传输层
利用SPDY后增加的功能：
多路复用流：通过单一的TCP连接，可以无限制处理多个HTTP请求,所有请求的处理都在一条TCP连接上完成
赋予请求优先级：SPDY不仅可以无限制的处理并发请求，还可以给请求逐个的分配优先级顺序，解决发送多个请求，因带宽低而响应变慢
压缩HTTP首部:压缩HTTP请求和响应首部
推送功能:支持服务器向客户端推送数据
服务器提示功能：服务器可以主动提示客户端请求所需的资源，不必客户端主动请求资源
SPDY将单域名的通信多路复用，当使用多个域名下的资源就会受到限制。
Websocket
websocket:web浏览器与web服务器之间全双工通信，websocket连接的发起方式客户端。
websocket主要特点：
推送功能：服务器向客户端推送数据功能，服务端直接发送数据不需要客户端的请求
减少通信量：建立Websocket连接后，就一直保持连接状态，websocket的首部信息量很小，通信量相对较少
握手：websocket通信须在HTTP建立连接后完成一次握手

请求：

1
2
3
4
5
6
7
Request Method:GET
Host:server.example.com
Upgrade:websocket
Connection:Upgrade
Sec-WebSocket-Key:fajdsfjewruewuiorueqworreioq==
Sec-WebSocket-Protocol:chat,superchat
Sec-WebSocket-Version:13
Sec-Websocket-Key字段记录握手过程不可少的键值，Sec-Websocket-Protocol记录使用的子协议。

响应：
1
2
3
4
5
Status Code:101 Switching Protocols
Upgrade:websocket
Connection:Upgrade
Sec-WebSocket-Accept:fsfjlsdjfjds42fasfs3fsdfj==
Sec-WebSocket-Protocol:chat
对于之前的请求，返回状态码101 Switching Protocols的响应,Sec-WebSocket-Accept字段值是由握手请求中的Sec-WebSocket-Key字段值生成的。如果成功握手确立WebSocket连接之后，通信时不再使用HTTP的数据帧，而采用WebSocket独立的数据帧。
image
WebSocket是全双工通信，因此服务器端不必等待请求，可直接发送数据，从而实现服务器端推送功能。(当HTTP握手动作结束时，浏览器和服务器之间就形成了一个快速通道，当然从WebSocket名字也可以理解这个快速通道类似于Socket连接，客户端和服务器端就可以通过TCP连接直接交换数据了，快速通道连接一直持续到客户端或服务器端的某一方主动的关闭连接。所以WebSocket本质上是一个基于TCP的协议。)

WebSocket API
JavaScript可调用WebSocket API内提供的WebSocket程序接口，以实现WebSocket协议下的全双工通信。

1
var webSocket = new WebSocket(url,[subProtocol]); //创建一个WebSocket对象
提示：url是指定连接的URL，subProtocol是可选的子协议。

WebSocket事件
事件	事件处理程序	说明
open	webSocket.onopen	连接建立时触发
message	webSocket.onmessage	客户端接收服务端数据时触发
error	webSocket.onerror	通信发生错误时触发
message	webSocket.onclose	连接关闭时触发
WebSocket方法
方法	说明
send()	使用连接发送数据
close()	关闭连接
WebSocket属性
属性	说明
Socket.readyState	表示连接状态
Socket.bufferedAmount	只读属性，表明已经被send()放入发送等待列表但是还没有发出的UTF-8文本字节数
客户端使用示例：

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
function WebSocketExample(data){
    if("WebSocket" in window){            //检验浏览器是否支持WebSocket()接口
        var ws = new WebSocket(ws://localhost:7070/echo)     //创建一个WebSocket对象
        /*连接建立*/
        ws.onopen = function(){
            if(ws.bufferedAmount === 0){  //如果发送等待列表是空，则可以继续发送数据
                ws.send(data);
            }
        }
        /*接收数据*/
        ws.onmessage = function(event){
            var receiveData = event.data;
        }
        /*关闭连接*/
        ws.onclose = function(){
            alert("连接关闭!")；
        }
        /*连接错误*/
        ws.onerror = function(err){
        }
    }else {
        alert("该浏览器不支持WebSocket连接!");
    }
}
#####HTTP/2.0

回顾一下HTTP的发展历史：

HTTP/0.9
已经过时，只接受GET一种请求方法，没有在通讯中指定版本号，且不支持请求头，由于该版本不支持POST方法，所以客户端无法向服务器传递太多信息。
HTTP/1.0
在通讯中指定版本号的HTTP协议，至今仍被广泛使用，特别是在代理服务器中。
HTTP/1.1
与HTTP/1.0相比，主要区别在于
缓存处理
带宽优化及网络连接的使用
错误通知的管理
消息在网络中发送
互联网地址维护
安全性及完整性
目前主流的HTTP/1.1标准，在SPDY和WebSocket出现以后，很难断言仍是适用于当下的Web的协议。于是负责互联网技术标准的IETF（Internet Engineering Task Force，互联网工程任务组）创立了httpbis工作组，目标是推进下一代HTTP（HTTP/2.0）。

HTTP/2.0特点

SPDY
HTTP Speed + Mobility
Network-Friendly HTTP Upgrade
HTTP Speed + Mobility由微软公司起草，用于改善提高移动端通信时通信速度和性能的标准。建立与Google公司提出的SPDY与WebSocket的基础之上。

HTTP/2.0 的7项技术及讨论

cxc	czc
压缩	SPDY、Friendly
多路复用	SPDY
TLS义务化	Speed + Mobility
协商	Speed + Mobility、 Friendly
客户端拉拽和服务器推送	Speed + Mobility
流量控制	SPDY
WebSocket	Speed + Mobility
Web服务器管理文件的WebDAV
WebDAV（Web-based Distributed Authoring and Versioning，基于万维网的分布式创作和版本控制）是一个可对Web服务器上的内容直接进行文件复制、编辑等操作的分布式文件系统。除了创建、删除文件等基本功能，它还具备文件创建者管理、文件编辑过程中禁止其他用户内容覆盖的加锁功能，以及对文件内容修改的版本控制功能。
使用HTTP/1.1的PUT方法和DELETE方法就可以对Web服务器上的文件进行创建和删除操作。但是出于安全性考虑一般是不使用的。

WebDAV新增的方法及状态码

方法

FROPFIND：获取属性
PROPPATCHL：修改属性
MKCOL：创建集合
…
状体码

102 Processing：可正常处理请求，但目前是处理中状态
207 Multi-Status：存在多种状态
423 Locked：资源已被加锁

10.构建 web内容的技术
html、css、js、xml、RSS、CGI
11.Web攻击技术
攻击模式
主动攻击
攻击者通过直接访问的模式(针对服务器上的资源进行攻击)，把攻击代码传入的攻击模式，典型：sql注入、OS命令注入
被动攻击
利用圈套策略执行攻击代码的攻击模式，在被动攻击的过程中，攻击者不直接对目标web应用访问发起攻击,有代表性的攻击是跨站脚本攻击和跨站点请求伪造
被动攻击通常的攻击模式如下所示
　　步骤1：攻击者诱使用户触发已设置好的陷阱，而陷阱会启动发送已嵌入攻击代码的HTTP请求

　　步骤2：当用户不知不觉中招之后，用户的浏览器或邮件客户端就会触发这个陷阱

　　步骤3：中招后的用户浏览器会把含有攻击代码的HTTP请求发送给作为攻击目标的Web应用，运行攻击代码

　　步骤4：执行完攻击代码，存在安全漏洞的Web应用会成为攻击者的跳板，可能导致用户所持的Cookie等个人信息被窃取，登录状态中的用户权限遭恶意滥用等后果
image

输出值转义不完全
　　实施Web应用的安全对策可大致分为以下两部分：客户端的验证和Web应用端(服务器端)的验证
　　而Web应用端(服务器端)的验证又包括输入值验证和输出值转义

跨站脚本攻击(Cross-Site Scripting，XSS)
指通过存在安全漏洞的Web网站注册用户的浏览器内运行非法的HTML标签或JavaScript进行的一种攻击

　　动态创建的HTML部分有可能隐藏着安全漏洞。就这样，攻击者编写脚本设下陷阱，用户在自己的浏览器上运行时，一不小心就会受到被动攻击

　　跨站脚本攻击有可能造成以下影响：利用虚假输入表单骗取用户个人信息；利用脚本窃取用户的Cookie值，被害者在不知情的情况下，帮助攻击者发送恶意请求；显示伪造的文章或图片

XSS是攻击者利用预先设置的陷阱触发的被动攻击
　　跨站脚本攻击属于被动攻击模式，因此攻击者会事先布置好用于攻击的陷阱

SQL注入
SQL注入(SQL Injection)是指针对Web应用使用的数据库，通过运行非法的SQL而产生的攻击。SQL注入攻击有可能会造成以下影响：非法查看或篡改数据库内的数据；规避认证；执行和数据库服务器业务关联的程序等

OS命令注入攻击
　OS命令注入攻击(OS Command Injection)是指通过Web应用，执行非法的操作系统命令达到攻击的目的。在能调用Shell函数的地方就有存在被攻击的风险。可以从Web应用中通过Shell来调用操作系统命令。倘若调用Shell时存在疏漏，就可以执行插入的非法OS命令。OS命令注入攻击可以向Shell发送命令，让Windows或Linux操作系统的命令行启动程序。也就是说，通过OS注入攻击可执行OS上安装着的各种程序

HTTP首部注入攻击
　HTTP首部注入攻击(HTTPHeader Injection)是指攻击者通过在响应首部字段内插入换行，添加任意响应首部或主体的一种攻击。属于被动攻击模式。
　　向首部主体内添加内容的攻击称为HTTP响应截断攻击(HTTPResponse Splitting Attack)。如下所示，Web应用有时会把从外部接收到的数值，赋给响应首部字段Location和Set-Cookie

1
2
Location: http://www.example.com/a.cgi?q=12345
Set-Cookie: UID=12345
　　HTTP首部注入可能像这样，通过在某些响应首部字段需要处理输出值的地方，插入换行发动攻击。
　　HTTP首部注入攻击有可能会造成以下一些影响：设置任何Cookie信息；重定向至任意URL；显示任意的主体(HTTP响应截断攻击)

HTTP响应截断攻击
　　HTTP响应截断攻击是用在HTTP首部注入的一种攻击。攻击顺序相同，但是要将两个并排插入字符串后发送。利用这两个连续的换行就可作出HTTP首部与主体分隔所需的空行了，这样就能显示伪造的主体，达到攻击目的。这样的攻击叫做HTTP响应截断攻击

1
<HTML><HEAD><TITLE>之后，想要显示的网页内容<!--
　　在可能进行HTTP首部注入的环节，通过发送上面的字符串，返回结果得到以下这种响应

1
2
3
Set-Cookie: UID=(：换行符)
(：换行符)
<HTML><HEAD><TITLE>之后，想要显示的网页内容<!--(原来页面对应的首部字段和主体部分全视为注释)
　　利用这个攻击，已触发陷阱的用户浏览器会显示伪造的Web页面，再让用户输入自己的个人信息等，可达到和跨站脚本攻击相同的效果

　　另外，滥用HTTP/1.1中汇集多响应返回功能，会导致缓存服务器对任意内容进行缓存操作。这种攻击称为缓存污染。使用该缓存服务器的用户，在浏览遭受攻击的网站时，会不断地浏览被替换掉的Web网页

邮件首部注入攻击
　　邮件首部注入(Mail Header Injection)是指Web应用中的邮件发送功能，攻击者通过向邮件首部To或Subject内任意添加非法内容发起的攻击。利用存在安全漏洞的Web网站，可对任意邮件地址发送广告邮件或病毒邮件

邮件首部注入攻击案例
　　下面以Web页面中的咨询表单为例讲解邮件首部注入攻击。该功能可在表单内填入咨询者的邮件地址及咨询内容后，以邮件的形式发送给网站管理员

　　攻击者将以下数据作为邮件地址发起请求

1
bob@hackr.jp%0D%0ABcc:user@example.com
　　%0D%0A在邮件报文中代表换行符。一旦咨询表单所在的Web应用接收了这个换行符，就可能实现对Bcc邮件地址的追加发送，而这原本是无法指定的

　　另外像下面一样，使用两个连续的换行符就有可能篡改邮件文本内容并发送

1
bob@hackr.jp%0D%0A%0D%0ATest Message
　　再以相同的方法，就有可能改写To和Subject等任意邮件首部，或向文本添加附件等动作

目录遍历攻击
　　目录遍历(Directory Traversal)攻击是指对本无意公开的文件目录，通过非法截断其目录路径后，达成访问目的的一种攻击。这种攻击有时也称为路径遍历(Path Traversal)攻击

　　通过Web应用对文件处理操作时，在由外部指定文件名的处理存在疏漏的情况下，用户可使用…/等相对路径定位到etcpassed等绝对路径上，因此服务器上任意的文件或文件目录皆有可能被访问到。这样一来，就有可能非法浏览、篡改或删除Web服务器上的文件

　　固然存在输出值转义的问题，但更应该关闭指定对任意文件名的访问权限

远程文件包含漏洞
　　远程文件包含漏洞(Remote File Inclusion)是指当部分脚本内容需要从其他文件读入时，攻击者利用指定外部服务器的URL充当依赖文件，让脚本读取之后，就可运行任意脚本的一种攻击

　　这主要是PHP存在的安全漏洞，对PHP的include或require来说，这是一种可通过设定，指定外部服务器的URL作为文件名的功能。但是，该功能太危险，PHP5.2.0之后默认设定此功能无效

　　固然存在输出值转义的问题，但更应控制对任意文件名的指定