#  基础篇
面试经验谈--来自知乎芋头Live 

##  一、HTML、HTTP、web综合问题
# 1 前端需要注意哪些SEO
合理的title、description、keywords：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
重要内容不要用js输出：爬虫不会执行js获取内容
少用iframe：搜索引擎不会抓取iframe中的内容
非装饰性图片必须加alt
提高网站速度：网站速度是搜索引擎排序的一个重要指标
# 2 <img>的title和alt有什么区别
通常当鼠标滑动到元素上的时候显示
alt是<img>的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。
# 3 HTTP的几种请求方法用途
GET方法

发送一个请求来取得服务器上的某一资源
POST方法

向URL指定的资源提交数据或附加新的数据
PUT方法

跟POST方法很像，也是想服务器提交数据。但是，它们之间有不同。PUT指定了资源在服务器上的位置，而POST没有
HEAD方法

只请求页面的首部
DELETE方法

删除服务器上的某资源
OPTIONS方法

它用于获取当前URL所支持的方法。如果请求成功，会有一个Allow的头包含类似“GET,POST”这样的信息
TRACE方法

TRACE方法被用于激发一个远程的，应用层的请求消息回路
CONNECT方法

把请求连接转换到透明的TCP/IP通道
# 4 从浏览器地址栏输入url到显示页面的步骤
基础版本

浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
载入解析到的资源文件，渲染页面，完成。
详细版

在浏览器地址栏输入URL
浏览器查看缓存，如果请求资源在缓存中并且新鲜，跳转到转码步骤
如果资源未缓存，发起新请求
如果已缓存，检验是否足够新鲜，足够新鲜直接提供给客户端，否则与服务器进行验证。
检验新鲜通常有两个HTTP头进行控制Expires和Cache-Control：
HTTP1.0提供Expires，值为一个绝对时间表示缓存新鲜日期
HTTP1.1增加了Cache-Control: max-age=,值为以秒为单位的最大新鲜时间
浏览器解析URL获取协议，主机，端口，path
浏览器组装一个HTTP（GET）请求报文
浏览器获取主机ip地址，过程如下：
浏览器缓存
本机缓存
hosts文件
路由器缓存
ISP DNS缓存
DNS递归查询（可能存在负载均衡导致每次IP不一样）
打开一个socket与目标IP地址，端口建立TCP链接，三次握手如下：
客户端发送一个TCP的SYN=1，Seq=X的包到服务器端口
服务器发回SYN=1， ACK=X+1， Seq=Y的响应包
客户端发送ACK=Y+1， Seq=Z
TCP链接建立后发送HTTP请求
服务器接受请求并解析，将请求转发到服务程序，如虚拟主机使用HTTP Host头部判断请求的服务程序
服务器检查HTTP请求头是否包含缓存验证信息如果验证缓存新鲜，返回304等对应状态码
处理程序读取完整请求并准备HTTP响应，可能需要查询数据库等操作
服务器将响应报文通过TCP连接发送回浏览器
浏览器接收HTTP响应，然后根据情况选择关闭TCP连接或者保留重用，关闭TCP连接的四次握手如下：
主动方发送Fin=1， Ack=Z， Seq= X报文
被动方发送ACK=X+1， Seq=Z报文
被动方发送Fin=1， ACK=X， Seq=Y报文
主动方发送ACK=Y， Seq=X报文
浏览器检查响应状态吗：是否为1XX，3XX， 4XX， 5XX，这些情况处理与2XX不同
如果资源可缓存，进行缓存
对响应进行解码（例如gzip压缩）
根据资源类型决定如何处理（假设资源为HTML文档）
解析HTML文档，构件DOM树，下载资源，构造CSSOM树，执行js脚本，这些操作没有严格的先后顺序，以下分别解释
构建DOM树：
Tokenizing：根据HTML规范将字符流解析为标记
Lexing：词法分析将标记转换为对象并定义属性和规则
DOM construction：根据HTML标记关系将对象组成DOM树
解析过程中遇到图片、样式表、js文件，启动下载
构建CSSOM树：
Tokenizing：字符流转换为标记流
Node：根据标记创建节点
CSSOM：节点创建CSSOM树
根据DOM树和CSSOM树构建渲染树 (opens new window):
从DOM树的根节点遍历所有可见节点，不可见节点包括：1）script,meta这样本身不可见的标签。2)被css隐藏的节点，如display: none
对每一个可见节点，找到恰当的CSSOM规则并应用
发布可视节点的内容和计算样式
js解析如下：
浏览器创建Document对象并解析HTML，将解析到的元素和文本节点添加到文档中，此时document.readystate为loading
HTML解析器遇到没有async和defer的script时，将他们添加到文档中，然后执行行内或外部脚本。这些脚本会同步执行，并且在脚本下载和执行时解析器会暂停。这样就可以用document.write()把文本插入到输入流中。同步脚本经常简单定义函数和注册事件处理程序，他们可以遍历和操作script和他们之前的文档内容
当解析器遇到设置了async属性的script时，开始下载脚本并继续解析文档。脚本会在它下载完成后尽快执行，但是解析器不会停下来等它下载。异步脚本禁止使用document.write()，它们可以访问自己script和之前的文档元素
当文档完成解析，document.readState变成interactive
所有defer脚本会按照在文档出现的顺序执行，延迟脚本能访问完整文档树，禁止使用document.write()
浏览器在Document对象上触发DOMContentLoaded事件
此时文档完全解析完成，浏览器可能还在等待如图片等内容加载，等这些内容完成载入并且所有异步脚本完成载入和执行，document.readState变为complete，window触发load事件
显示页面（HTML解析过程中会逐步显示页面）
详细简版

从浏览器接收url到开启网络请求线程（这一部分可以展开浏览器的机制以及进程与线程之间的关系）

开启网络线程到发出一个完整的HTTP请求（这一部分涉及到dns查询，TCP/IP请求，五层因特网协议栈等知识）

从服务器接收到请求到对应后台接收到请求（这一部分可能涉及到负载均衡，安全拦截以及后台内部的处理等等）

后台和前台的HTTP交互（这一部分包括HTTP头部、响应码、报文结构、cookie等知识，可以提下静态资源的cookie优化，以及编码解码，如gzip压缩等）

单独拎出来的缓存问题，HTTP的缓存（这部分包括http缓存头部，ETag，catch-control等）

浏览器接收到HTTP数据包后的解析流程（解析html-词法分析然后解析成dom树、解析css生成css规则树、合并成render树，然后layout、painting渲染、复合图层的合成、GPU绘制、外链资源的处理、loaded和DOMContentLoaded等）

CSS的可视化格式模型（元素的渲染规则，如包含块，控制框，BFC，IFC等概念）

JS引擎解析过程（JS的解释阶段，预处理阶段，执行阶段生成执行上下文，VO，作用域链、回收机制等等）

其它（可以拓展不同的知识模块，如跨域，web安全，hybrid模式等等内容）

# 5 如何进行网站性能优化
content方面

减少HTTP请求：合并文件、CSS精灵、inline Image
减少DNS查询：DNS缓存、将资源分布到恰当数量的主机名
减少DOM元素数量
Server方面

使用CDN
配置ETag
对组件使用Gzip压缩
Cookie方面

减小cookie大小
css方面

将样式表放到页面顶部
不使用CSS表达式
使用<link>不使用@import
Javascript方面

将脚本放到页面底部
将javascript和css从外部引入
压缩javascript和css
删除不需要的脚本
减少DOM访问
图片方面

优化图片：根据实际颜色需要选择色深、压缩
优化css精灵
不要在HTML中拉伸图片
你有用过哪些前端性能优化的方法？

减少http请求次数：CSS Sprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，CDN托管，data缓存 ，图片服务器。
前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。
当需要设置的样式很多时设置className而不是直接操作style
少用全局变量、缓存DOM节点查找的结果。减少IO读取操作
避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)
图片预加载，将样式表放在顶部，将脚本放在底部 加上时间戳
避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢
谈谈性能优化问题

代码层面：避免使用css表达式，避免使用高级选择器，通配选择器
缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等
请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载
请求带宽：压缩文件，开启GZIP
前端性能优化最佳实践？

性能评级工具（PageSpeed 或 YSlow）
合理设置 HTTP 缓存：Expires 与 Cache-control
静态资源打包，开启 Gzip 压缩（节省响应流量）
CSS3 模拟图像，图标base64（降低请求数）
模块延迟(defer)加载/异步(async)加载
Cookie 隔离（节省请求流量）
localStorage（本地存储）
使用 CDN 加速（访问最近服务器）
启用 HTTP/2（多路复用，并行加载）
前端自动化（gulp/webpack）
# 6 HTTP状态码及其含义
1XX：信息状态码
100 Continue 继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
2XX：成功状态码
200 OK 正常返回信息
201 Created 请求成功并且服务器创建了新的资源
202 Accepted 服务器已接受请求，但尚未处理
3XX：重定向
301 Moved Permanently 请求的网页已永久移动到新位置。
302 Found 临时性重定向。
303 See Other 临时性重定向，且总是使用 GET 请求新的 URI。
304 Not Modified 自从上次请求后，请求的网页未修改过。
4XX：客户端错误
400 Bad Request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
401 Unauthorized 请求未授权。
403 Forbidden 禁止访问。
404 Not Found 找不到如何与 URI 相匹配的资源。
5XX: 服务器错误
500 Internal Server Error 最常见的服务器端错误。
503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
# 7 语义化的理解
用正确的标签做正确的事情！
HTML语义化就是让页面的内容结构化，便于对浏览器、搜索引擎解析；
在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的。
搜索引擎的爬虫依赖于标记来确定上下文和各个关键字的权重，利于 SEO。
使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解
# 8 介绍一下你对浏览器内核的理解？
主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎

渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核

JS引擎则：解析和执行javascript来实现网页的动态效果

最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎

常见的浏览器内核有哪些

Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
Presto内核：Opera7及以上。 [Opera内核原为：Presto，现为：Blink;]
Webkit内核：Safari,Chrome等。 [ Chrome的Blink（WebKit的分支）]
# 9 html5有哪些新特性、移除了那些元素？
HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加

新增选择器 document.querySelector、document.querySelectorAll
拖拽释放(Drag and drop) API
媒体播放的 video 和 audio
本地存储 localStorage 和 sessionStorage
离线应用 manifest
桌面通知 Notifications
语意化标签 article、footer、header、nav、section
增强表单控件 calendar、date、time、email、url、search
地理位置 Geolocation
多任务 webworker
全双工通信协议 websocket
历史管理 history
跨域资源共享(CORS) Access-Control-Allow-Origin
页面可见性改变事件 visibilitychange
跨窗口通信 PostMessage
Form Data 对象
绘画 canvas
移除的元素：

纯表现的元素：basefont、big、center、font、 s、strike、tt、u
对可用性产生负面影响的元素：frame、frameset、noframes
支持HTML5新标签：

IE8/IE7/IE6支持通过document.createElement方法产生的标签
可以利用这一特性让这些浏览器支持HTML5新标签
浏览器支持新标签后，还需要添加标签默认的样式
当然也可以直接使用成熟的框架、比如html5shim

如何区分 HTML 和 HTML5

DOCTYPE声明、新增的结构元素、功能元素
# 10 HTML5的离线储存怎么使用，工作原理能不能解释一下？
在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件

原理：HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示

如何使用：

页面头部像下面一样加入一个manifest的属性；
在cache.manifest文件的编写离线存储的资源
在离线状态时，操作window.applicationCache进行需求实现
CACHE MANIFEST
# v0.11
CACHE:
js/app.js
css/style.css
NETWORK:
resourse/logo.png
FALLBACK:
/offline.html
# 11 浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢
在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件，如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件与旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。

离线的情况下，浏览器就直接使用离线存储的资源。

# 12 请描述一下 cookies，sessionStorage 和 localStorage 的区别？
cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）

cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递

sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存

存储大小：

cookie数据大小不能超过4k
sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大
有期时间：

localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据
sessionStorage 数据在当前浏览器窗口关闭后自动删除
cookie 设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
# 13 iframe有那些缺点？
iframe会阻塞主页面的Onload事件
搜索引擎的检索程序无法解读这种页面，不利于SEO
iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载
使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript动态给iframe添加src属性值，这样可以绕开以上两个问题
# 14 WEB标准以及W3C标准是什么?
标签闭合、标签小写、不乱嵌套、使用外链css和js、结构行为表现的分离
# 15 xhtml和html有什么区别?
一个是功能上的差别

主要是XHTML可兼容各大浏览器、手机以及PDA，并且浏览器也能快速正确地编译网页
另外是书写习惯的差别

XHTML 元素必须被正确地嵌套，闭合，区分大小写，文档必须拥有根元素
# 16 Doctype作用? 严格模式与混杂模式如何区分？它们有何意义?
页面被加载的时，link会同时被加载，而@imort页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载 import只在IE5以上才能识别，而link是XHTML标签，无兼容问题 link方式的样式的权重 高于@import的权重
<!DOCTYPE> 声明位于文档中的最前面，处于 <html> 标签之前。告知浏览器的解析器， 用什么文档类型 规范来解析这个文档
严格模式的排版和 JS 运作模式是 以该浏览器支持的最高标准运行
在混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作。 DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现
# 17 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？行内元素和块级元素有什么区别？
行内元素有：a b span img input select strong
块级元素有：div ul ol li dl dt dd h1 h2 h3 h4… p
空元素：<br> <hr> <img> <input> <link> <meta>
行内元素不可以设置宽高，不独占一行
块级元素可以设置宽高，独占一行
# 18 HTML全局属性(global attribute)有哪些
class:为元素设置类标识
data-*: 为元素增加自定义属性
draggable: 设置元素是否可拖拽
id: 元素id，文档内唯一
lang: 元素内容的的语言
style: 行内css样式
title: 元素相关的建议信息
# 19 Canvas和SVG有什么区别？
svg绘制出来的每一个图形的元素都是独立的DOM节点，能够方便的绑定事件或用来修改。canvas输出的是一整幅画布
svg输出的图形是矢量图形，后期可以修改参数来自由放大缩小，不会失真和锯齿。而canvas输出标量画布，就像一张图片一样，放大会失真或者锯齿
# 20 HTML5 为什么只需要写 <!DOCTYPE HTML>
HTML5 不基于 SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为
而HTML4.01基于SGML,所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型
# 21 如何在页面上实现一个圆形的可点击区域？
svg
border-radius
纯js实现 需要求一个点在不在圆上简单算法、获取鼠标坐标等等
# 22 网页验证码是干嘛的，是为了解决什么安全问题
区分用户是计算机还是人的公共全自动程序。可以防止恶意破解密码、刷票、论坛灌水
有效防止黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试
# 23 viewport
 <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
    // width    设置viewport宽度，为一个正整数，或字符串‘device-width’
    // device-width  设备宽度
    // height   设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
    // initial-scale    默认缩放比例（初始缩放比例），为一个数字，可以带小数
    // minimum-scale    允许用户最小缩放比例，为一个数字，可以带小数
    // maximum-scale    允许用户最大缩放比例，为一个数字，可以带小数
    // user-scalable    是否允许手动缩放
延伸提问
怎样处理 移动端 1px 被 渲染成 2px问题
局部处理

meta标签中的 viewport属性 ，initial-scale 设置为 1
rem按照设计稿标准走，外加利用transfrome 的scale(0.5) 缩小一倍即可；
全局处理

mate标签中的 viewport属性 ，initial-scale 设置为 0.5
rem 按照设计稿标准走即可
# 24 渲染优化
禁止使用iframe（阻塞父文档onload事件）

iframe会阻塞主页面的Onload事件
搜索引擎的检索程序无法解读这种页面，不利于SEO
iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载
使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript
动态给iframe添加src属性值，这样可以绕开以上两个问题
禁止使用gif图片实现loading效果（降低CPU消耗，提升渲染性能）

使用CSS3代码代替JS动画（尽可能避免重绘重排以及回流）

对于一些小图标，可以使用base64位编码，以减少网络请求。但不建议大图使用，比较耗费CPU

小图标优势在于
减少HTTP请求
避免文件跨域
修改及时生效
页面头部的<style></style> <script></script> 会阻塞页面；（因为 Renderer进程中 JS线程和渲染线程是互斥的）

页面中空的 href 和 src 会阻塞页面其他资源的加载 (阻塞下载进程)

网页gzip，CDN托管，data缓存 ，图片服务器

前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数

用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能

当需要设置的样式很多时设置className而不是直接操作style

少用全局变量、缓存DOM节点查找的结果。减少IO读取操作

图片预加载，将样式表放在顶部，将脚本放在底部 加上时间戳

对普通的网站有一个统一的思路，就是尽量向前端优化、减少数据库操作、减少磁盘IO

# 25 meta viewport相关
```js
<!DOCTYPE html>  <!--H5标准声明，使用 HTML5 doctype，不区分大小写-->
<head lang=”en”> <!--标准的 lang 属性写法-->
<meta charset=’utf-8′>    <!--声明文档使用的字符编码-->
<meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>   <!--优先使用 IE 最新版本和 Chrome-->
<meta name=”description” content=”不超过150个字符”/>       <!--页面描述-->
<meta name=”keywords” content=””/>     <!-- 页面关键词-->
<meta name=”author” content=”name, email@gmail.com”/>    <!--网页作者-->
<meta name=”robots” content=”index,follow”/>      <!--搜索引擎抓取-->
<meta name=”viewport” content=”initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no”> <!--为移动设备添加 viewport-->
<meta name=”apple-mobile-web-app-title” content=”标题”> <!--iOS 设备 begin-->
<meta name=”apple-mobile-web-app-capable” content=”yes”/>  <!--添加到主屏后的标题（iOS 6 新增）
是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏-->
<meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”>
<!--添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）-->
<meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
<meta name=”format-detection” content=”telphone=no, email=no”/>  <!--设置苹果工具栏颜色-->
<meta name=”renderer” content=”webkit”> <!-- 启用360浏览器的极速模式(webkit)-->
<meta http-equiv=”X-UA-Compatible” content=”IE=edge”>     <!--避免IE使用兼容模式-->
<meta http-equiv=”Cache-Control” content=”no-siteapp” />    <!--不让百度转码-->
<meta name=”HandheldFriendly” content=”true”>     <!--针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓-->
<meta name=”MobileOptimized” content=”320″>   <!--微软的老式浏览器-->
<meta name=”screen-orientation” content=”portrait”>   <!--uc强制竖屏-->
<meta name=”x5-orientation” content=”portrait”>    <!--QQ强制竖屏-->
<meta name=”full-screen” content=”yes”>              <!--UC强制全屏-->
<meta name=”x5-fullscreen” content=”true”>       <!--QQ强制全屏-->
<meta name=”browsermode” content=”application”>   <!--UC应用模式-->
<meta name=”x5-page-mode” content=”app”>   <!-- QQ应用模式-->
<meta name=”msapplication-tap-highlight” content=”no”>    <!--windows phone 点击无高亮
设置页面不缓存-->
<meta http-equiv=”pragma” content=”no-cache”>
<meta http-equiv=”cache-control” content=”no-cache”>
<meta http-equiv=”expires” content=”0″>
```
# 26 你做的页面在哪些流览器测试过？这些浏览器的内核分别是什么?
IE: trident内核
Firefox：gecko内核
Safari:webkit内核
Opera:以前是presto内核，Opera现已改用Google - Chrome的Blink内核
Chrome:Blink(基于webkit，Google与Opera Software共同开发)
# 27 div+css的布局较table布局有什么优点？
改版的时候更方便 只要改css文件。
页面加载速度更快、结构化清晰、页面显示简洁。
表现与结构相分离。
易于优化（seo）搜索引擎更友好，排名更容易靠前。
# 28 a：img的alt与title有何异同？b：strong与em的异同？
alt(alt text):为不能显示图像、窗体或applets的用户代理（UA），alt属性用来指定替换文字。替换文字的语言由lang属性指定。(在IE浏览器下会在没有title时把alt当成 tool tip显示)

title(tool tip):该属性为设置该属性的元素提供建议性的信息

strong:粗体强调标签，强调，表示内容的重要性

em:斜体强调标签，更强烈强调，表示内容的强调点

# 29 你能描述一下渐进增强和优雅降级之间的不同吗
渐进增强：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
优雅降级：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
区别：优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带

# 30 为什么利用多个域名来存储网站资源会更有效？
CDN缓存更方便
突破浏览器并发限制
节约cookie带宽
节约主域名的连接数，优化页面响应速度
防止不必要的安全问题
# 31 简述一下src与href的区别
src用于替换当前元素，href用于在当前文档和引用资源之间确立联系。
src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素
<script src ="js.js"></script> 当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部

href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加
<link href="common.css" rel="stylesheet"/>那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用link方式来加载css，而不是使用@import方式
# 32 知道的网页制作会用到的图片格式有哪些？
png-8、png-24、jpeg、gif、svg
但是上面的那些都不是面试官想要的最后答案。面试官希望听到是Webp,Apng。（是否有关注新技术，新鲜事物）

Webp：WebP格式，谷歌（google）开发的一种旨在加快图片加载速度的图片格式。图片压缩体积大约只有JPEG的2/3，并能节省大量的服务器带宽资源和数据空间。Facebook Ebay等知名网站已经开始测试并使用WebP格式。
在质量相同的情况下，WebP格式图像的体积要比JPEG格式图像小40%。
Apng：全称是“Animated Portable Network Graphics”, 是PNG的位图动画扩展，可以实现png格式的动态图片效果。04年诞生，但一直得不到各大浏览器厂商的支持，直到日前得到 iOS safari 8的支持，有望代替GIF成为下一代动态图标准
# 33 在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求一般情况下有哪些地方会有缓存处理？
dns缓存，cdn缓存，浏览器缓存，服务器缓存

# 33 一个页面上有大量的图片（大型电商网站），加载很慢，你有哪些方法优化这些图片的加载，给用户更好的体验。
图片懒加载，在页面上的未可视区域可以添加一个滚动事件，判断图片位置与浏览器顶端的距离与页面的距离，如果前者小于后者，优先加载。
如果为幻灯片、相册等，可以使用图片预加载技术，将当前展示图片的前一张和后一张优先下载。
如果图片为css图片，可以使用CSSsprite，SVGsprite，Iconfont、Base64等技术。
如果图片过大，可以使用特殊编码的图片，加载时会先加载一张压缩的特别厉害的缩略图，以提高用户体验。
如果图片展示区域小于图片的真实大小，则因在服务器端根据业务需要先行进行图片压缩，图片压缩后大小与展示一致。
# 34 常见排序算法的时间复杂度,空间复杂度


# 35 web开发中会话跟踪的方法有哪些
cookie
session
url重写
隐藏input
ip地址
# 36 HTTP request报文结构是怎样的
首行是Request-Line包括：请求方法，请求URI，协议版本，CRLF
首行之后是若干行请求头，包括general-header，request-header或者entity-header，每个一行以CRLF结束
请求头和消息实体之间有一个CRLF分隔
根据实际请求需要可能包含一个消息实体 一个请求报文例子如下：
```js
GET /Protocols/rfc2616/rfc2616-sec5.html HTTP/1.1
Host: www.w3.org
Connection: keep-alive
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
Referer: https://www.google.com.hk/
Accept-Encoding: gzip,deflate,sdch
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: authorstyle=yes
If-None-Match: "2cc8-3e3073913b100"
If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT

name=qiu&age=25
```
# 37 HTTP response报文结构是怎样的
首行是状态行包括：HTTP版本，状态码，状态描述，后面跟一个CRLF
首行之后是若干行响应头，包括：通用头部，响应头部，实体头部
响应头部和响应实体之间用一个CRLF空行分隔
最后是一个可能的消息实体 响应报文例子如下：
```js
HTTP/1.1 200 OK
Date: Tue, 08 Jul 2014 05:28:43 GMT
Server: Apache/2
Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
ETag: "40d7-3e3073913b100"
Accept-Ranges: bytes
Content-Length: 16599
Cache-Control: max-age=21600
Expires: Tue, 08 Jul 2014 11:28:43 GMT
P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
Content-Type: text/html; charset=iso-8859-1

{"name": "qiu", "age": 25}
```
# 38 title与h1的区别、b与strong的区别、i与em的区别
title属性没有明确意义只表示是个标题，H1则表示层次明确的标题，对页面信息的抓取也有很大的影响
strong是标明重点内容，有语气加强的含义，使用阅读设备阅读网络时：<strong>会重读，而<B>是展示强调内容
i内容展示为斜体，em表示强调的文本
# 39 请你谈谈Cookie的弊端
cookie虽然在持久保存客户端数据提供了方便，分担了服务器存储的负担，但还是有很多局限性的

每个特定的域名下最多生成20个cookie
IE6或更低版本最多20个cookie
IE7和之后的版本最后可以有50个cookie
Firefox最多50个cookie
chrome和Safari没有做硬性限制
IE 和 Opera 会清理近期最少使用的 cookie，Firefox 会随机清理 cookie
cookie 的最大大约为 4096 字节，为了兼容性，一般设置不超过 4095 字节
如果 cookie 被人拦截了，就可以取得所有的 session 信息
# git fetch和git pull的区别
git pull：相当于是从远程获取最新版本并merge到本地
git fetch：相当于是从远程获取最新版本到本地，不会自动merge
# 
二、CSS部分
# 1 css sprite是什么,有什么优缺点
概念：将多个小图片拼接到一个图片中。通过background-position和元素尺寸调节需要显示的背景图案。

优点：

减少HTTP请求数，极大地提高页面加载速度
增加图片信息重复度，提高压缩比，减少图片大小
更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现
缺点：

图片合并麻烦
维护麻烦，修改一个图片可能需要从新布局整个图片，样式
# 2 display: none;与visibility: hidden;的区别
联系：它们都能让元素不可见

区别：

display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式
修改常规流中元素的display通常会造成文档重排。修改visibility属性只会造成本元素的重绘。
读屏器不会读取display: none;元素内容；会读取visibility: hidden;元素内容
# 3 link与@import的区别
link是HTML方式， @import是CSS方式
link最大限度支持并行下载，@import过多嵌套导致串行下载，出现FOUC(文档样式短暂失效)
link可以通过rel="alternate stylesheet"指定候选样式
浏览器对link支持早于@import，可以使用@import对老浏览器隐藏样式
@import必须在样式规则之前，可以在css文件中引用其他文件
总体来说：link优于@import
# 4 什么是FOUC?如何避免
Flash Of Unstyled Content：用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再从新显示文档，造成页面闪烁。
解决方法：把样式表放到文档的<head>
# 5 如何创建块级格式化上下文(block formatting context),BFC有什么用
BFC(Block Formatting Context)，块级格式化上下文，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响

触发条件 (以下任意一条)

float的值不为none
overflow的值不为visible
display的值为table-cell、tabble-caption和inline-block之一
position的值不为static或则releative中的任何一个
在IE下, Layout,可通过zoom:1 触发

.BFC布局与普通文档流布局区别 普通文档流布局:

浮动的元素是不会被父级计算高度
非浮动元素会覆盖浮动元素的位置
margin会传递给父级元素
两个相邻元素上下的margin会重叠
BFC布局规则:

浮动的元素会被父级计算高度(父级元素触发了BFC)
非浮动元素不会覆盖浮动元素的位置(非浮动元素触发了BFC)
margin不会传递给父级(父级触发BFC)
属于同一个BFC的两个相邻元素上下margin会重叠
开发中的应用

阻止margin重叠
可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个 div都位于同一个 BFC 区域之中)
自适应两栏布局
可以阻止元素被浮动元素覆盖
# 6 display、float、position的关系
如果display取值为none，那么position和float都不起作用，这种情况下元素不产生框
否则，如果position取值为absolute或者fixed，框就是绝对定位的，float的计算值为none，display根据下面的表格进行调整。


否则，如果float不是none，框是浮动的，display根据下表进行调整
否则，如果元素是根元素，display根据下表进行调整
其他情况下display的值为指定值
总结起来：绝对定位、浮动、根元素都需要调整display
# 7 清除浮动的几种方式，各自的优缺点
父级div定义height
结尾处加空div标签clear:both
父级div定义伪类:after和zoom
父级div定义overflow:hidden
父级div也浮动，需要定义宽度
结尾处加br标签clear:both
比较好的是第3种方式，好多网站都这么用
# 8 为什么要初始化CSS样式?
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化
# 9 css3有哪些新特性
新增选择器 p:nth-child(n){color: rgba(255, 0, 0, 0.75)}
弹性盒模型 display: flex;
多列布局 column-count: 5;
媒体查询 @media (max-width: 480px) {.box: {column-count: 1;}}
个性化字体 @font-face{font-family: BorderWeb; src:url(BORDERW0.eot);}
颜色透明度 color: rgba(255, 0, 0, 0.75);
圆角 border-radius: 5px;
渐变 background:linear-gradient(red, green, blue);
阴影 box-shadow:3px 3px 3px rgba(0, 64, 128, 0.3);
倒影 box-reflect: below 2px;
文字装饰 text-stroke-color: red;
文字溢出 text-overflow:ellipsis;
背景效果 background-size: 100px 100px;
边框效果 border-image:url(bt_blue.png) 0 10;
转换
旋转 transform: rotate(20deg);
倾斜 transform: skew(150deg, -10deg);
位移 transform: translate(20px, 20px);
缩放 transform: scale(.5);
平滑过渡 transition: all .3s ease-in .1s;
动画 @keyframes anim-1 {50% {border-radius: 50%;}} animation: anim-1 1s;
CSS3新增伪类有那些？

p:first-of-type 选择属于其父元素的首个<p>元素的每个<p> 元素。
p:last-of-type 选择属于其父元素的最后 <p> 元素的每个<p> 元素。
p:only-of-type 选择属于其父元素唯一的 <p>元素的每个 <p> 元素。
p:only-child 选择属于其父元素的唯一子元素的每个 <p> 元素。
p:nth-child(2) 选择属于其父元素的第二个子元素的每个 <p> 元素。
:after 在元素之前添加内容,也可以用来做清除浮动。
:before 在元素之后添加内容。
:enabled 已启用的表单元素。
:disabled 已禁用的表单元素。
:checked 单选框或复选框被选中。
# 10 display有哪些值？说明他们的作用
block 转换成块状元素。
inline 转换成行内元素。
none 设置元素不可见。
inline-block 象行内元素一样显示，但其内容象块类型元素一样显示。
list-item 象块类型元素一样显示，并添加样式列表标记。
table 此元素会作为块级表格来显示
inherit 规定应该从父元素继承 display 属性的值
# 11 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？
有两种， IE盒子模型、W3C盒子模型；
盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)；
区 别： IE的content部分把 border 和 padding计算了进去;
盒子模型构成：内容(content)、内填充(padding)、 边框(border)、外边距(margin)
IE8及其以下版本浏览器，未声明 DOCTYPE，内容宽高会包含内填充和边框，称为怪异盒模型(IE盒模型)
标准(W3C)盒模型：元素宽度 = width + padding + border + margin
怪异(IE)盒模型：元素宽度 = width + margin
标准浏览器通过设置 css3 的 box-sizing: border-box 属性，触发“怪异模式”解析计算宽高
box-sizing 常用的属性有哪些？分别有什么作用

box-sizing: content-box; 默认的标准(W3C)盒模型元素效果
box-sizing: border-box; 触发怪异(IE)盒模型元素的效果
box-sizing: inherit; 继承父元素 box-sizing 属性的值
# 12 CSS优先级算法如何计算？
优先级就近原则，同权重情况下样式定义最近者为准
载入样式以最后载入的定位为准
优先级为: !important > id > class > tag; !important 比 内联优先级高
# 13 对BFC规范的理解？
一个页面是由很多个 Box 组成的,元素的类型和 display` 属性,决定了这个 Box 的类型
不同类型的 Box,会参与不同的 Formatting Context（决定如何渲染文档的容器）,因此Box内的元素会以不同的方式渲染,也就是说BFC内部的元素和外部的元素不会互相影响
# 14 谈谈浮动和清除浮动
浮动的框可以向左或向右移动，直到他的外边缘碰到包含框或另一个浮动框的边框为止。由于浮动框不在文档的普通流中，所以文档的普通流的块框表现得就像浮动框不存在一样。浮动的块框会漂浮在文档普通流的块框上
# 15 position的值， relative和absolute定位原点是
absolute：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位
fixed：生成绝对定位的元素，相对于浏览器窗口进行定位
relative：生成相对定位的元素，相对于其正常位置进行定位
static 默认值。没有定位，元素出现在正常的流中
inherit 规定从父元素继承 position 属性的值
# 16 display:inline-block 什么时候不会显示间隙？(携程)
移除空格
使用margin负值
使用font-size:0
letter-spacing
word-spacing
# 17 PNG\GIF\JPG的区别及如何选
GIF

8位像素，256色
无损压缩
支持简单动画
支持boolean透明
适合简单动画
JPEG

颜色限于256
有损压缩
可控制压缩质量
不支持透明
适合照片
PNG

有PNG8和truecolor PNG
PNG8类似GIF颜色上限为256，文件小，支持alpha透明度，无动画
适合图标、背景、按钮
# 18 行内元素float:left后是否变为块级元素？
行内元素设置成浮动之后变得更加像是inline-block（行内块级元素，设置成这个属性的元素会同时拥有行内和块级的特性，最明显的不同是它的默认宽度不是100%），这时候给行内元素设置padding-top和padding-bottom或者width、height都是有效果的

# 19 在网页中的应该使用奇数还是偶数的字体？为什么呢？
偶数字号相对更容易和 web 设计的其他部分构成比例关系
# 20 ::before 和 :after中双冒号和单冒号 有什么区别？解释一下这2个伪元素的作用
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素
用于区分伪类和伪元素
# 21 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）
多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60*1000ms ＝ 16.7ms
# 22 CSS合并方法
避免使用@import引入多个css文件，可以使用CSS工具将CSS合并为一个CSS文件，例如使用Sass\Compass等
# 23 CSS不同选择器的权重(CSS层叠的规则)
！important规则最重要，大于其它规则
行内样式规则，加1000
对于选择器中给定的各个ID属性值，加100
对于选择器中给定的各个类属性、属性选择器或者伪类选择器，加10
对于选择其中给定的各个元素标签选择器，加1
如果权值一样，则按照样式规则的先后顺序来应用，顺序靠后的覆盖靠前的规则
以下是权重的规则：标签的权重为1，class的权重为10，id的权重为100，以下/// 例子是演示各种定义的权重值：

/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
# id1{
}
/*权重为100+1=101*/
# id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
}

如果权重相同，则最后定义的样式会起作用，但是应该避免这种情况出现

# 24 列出你所知道可以改变页面布局的属性
position、display、float、width、height、margin、padding、top、left、right、`
# 25 CSS在性能优化方面的实践
css压缩与合并、Gzip压缩
css文件放在head里、不要用@import
尽量用缩写、避免用滤镜、合理使用选择器
# 26 CSS3动画（简单动画的实现，如旋转等）
依靠CSS3中提出的三个属性：transition、transform、animation
transition：定义了元素在变化过程中是怎么样的，包含transition-property、transition-duration、transition-timing-function、transition-delay。
transform：定义元素的变化结果，包含rotate、scale、skew、translate。
animation：动画定义了动作的每一帧（@keyframes）有什么效果，包括animation-name，animation-duration、animation-timing-function、animation-delay、animation-iteration-count、animation-direction
# 27 base64的原理及优缺点
优点可以加密，减少了HTTTP请求
缺点是需要消耗CPU进行编解码
# 28 几种常见的CSS布局
流体布局
```js
	.left {
		float: left;
		width: 100px;
		height: 200px;
		background: red;
	}
	.right {
		float: right;
		width: 200px;
		height: 200px;
		background: blue;
	}
	.main {
		margin-left: 120px;
		margin-right: 220px;
		height: 200px;
		background: green;
	}
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="main"></div>
</div>
```
# 圣杯布局
```js
要求：三列布局；中间主体内容前置，且宽度自适应；两边内容定宽
好处：重要的内容放在文档流前面可以优先渲染
原理：利用相对定位、浮动、负边距布局，而不添加额外标签
  .container {
      padding-left: 150px;
      padding-right: 190px;
  }
  .main {
      float: left;
      width: 100%;
  }
  .left {
      float: left;
      width: 190px;
      margin-left: -100%;
      position: relative;
      left: -150px;
  }
  .right {
      float: left;
      width: 190px;
      margin-left: -190px;
      position: relative;
      right: -190px;
  }
<div class="container">
	<div class="main"></div>
	<div class="left"></div>
	<div class="right"></div>
</div>
```
# 双飞翼布局
```js
双飞翼布局：对圣杯布局（使用相对定位，对以后布局有局限性）的改进，消除相对定位布局
原理：主体元素上设置左右边距，预留两翼位置。左右两栏使用浮动和负边距归位，消除相对定位。
.container {
    /*padding-left:150px;*/
    /*padding-right:190px;*/
}
.main-wrap {
    width: 100%;
    float: left;
}
.main {
    margin-left: 150px;
    margin-right: 190px;
}
.left {
    float: left;
    width: 150px;
    margin-left: -100%;
    /*position: relative;*/
    /*left:-150px;*/
}
.right {
    float: left;
    width: 190px;
    margin-left: -190px;
    /*position:relative;*/
    /*right:-190px;*/
}
<div class="content">
    <div class="main"></div>
</div>
<div class="left"></div>
<div class="right"></div>
```
# 29 stylus/sass/less区别
均具有“变量”、“混合”、“嵌套”、“继承”、“颜色混合”五大基本特性
Scss和LESS语法较为严谨，LESS要求一定要使用大括号“{}”，Scss和Stylus可以通过缩进表示层次与嵌套关系
Scss无全局变量的概念，LESS和Stylus有类似于其它语言的作用域概念
Sass是基于Ruby语言的，而LESS和Stylus可以基于NodeJS NPM下载相应库后进行编译；
# 30 postcss的作用
可以直观的理解为：它就是一个平台。为什么说它是一个平台呢？因为我们直接用它，感觉不能干什么事情，但是如果让一些插件在它上面跑，那么将会很强大
PostCSS 提供了一个解析器，它能够将 CSS 解析成抽象语法树
通过在 PostCSS 这个平台上，我们能够开发一些插件，来处理我们的CSS，比如热门的：autoprefixer
postcss可以对sass处理过后的css再处理 最常见的就是autoprefixer
# 31 css样式（选择器）的优先级
计算权重确定
!important
内联样式
后写的优先级高
# 32 自定义字体的使用场景
宣传/品牌/banner等固定文案
字体图标
# 33 如何美化CheckBox
<label> 属性 for 和 id
隐藏原生的 <input>
:checked + <label>
# 34 伪类和伪元素的区别
伪类表状态
伪元素是真的有元素
前者单冒号，后者双冒号
# 35 base64的使用
用于减少 HTTP 请求
适用于小图片
base64的体积约为原图的4/3
# 36 自适应布局
思路：

左侧浮动或者绝对定位，然后右侧margin撑开
使用<div>包含，然后靠负margin形成bfc
使用flex
# 37 请用CSS写一个简单的幻灯片效果页面
知道是要用CSS3。使用animation动画实现一个简单的幻灯片效果

/**css**/
.ani{
  width:480px;
  height:320px;
  margin:50px auto;
  overflow: hidden;
  box-shadow:0 0 5px rgba(0,0,0,1);
  background-size: cover;
  background-position: center;
  -webkit-animation-name: "loops";
  -webkit-animation-duration: 20s;
  -webkit-animation-iteration-count: infinite;
}
@-webkit-keyframes "loops" {
    0% {
        background:url(http://d.hiphotos.baidu.com/image/w%3D400/sign=c01e6adca964034f0fcdc3069fc27980/e824b899a9014c08e5e38ca4087b02087af4f4d3.jpg) no-repeat;             
    }
    25% {
        background:url(http://b.hiphotos.baidu.com/image/w%3D400/sign=edee1572e9f81a4c2632edc9e72b6029/30adcbef76094b364d72bceba1cc7cd98c109dd0.jpg) no-repeat;
    }
    50% {
        background:url(http://b.hiphotos.baidu.com/image/w%3D400/sign=937dace2552c11dfded1be2353266255/d8f9d72a6059252d258e7605369b033b5bb5b912.jpg) no-repeat;
    }
    75% {
        background:url(http://g.hiphotos.baidu.com/image/w%3D400/sign=7d37500b8544ebf86d71653fe9f9d736/0df431adcbef76095d61f0972cdda3cc7cd99e4b.jpg) no-repeat;
    }
    100% {
        background:url(http://c.hiphotos.baidu.com/image/w%3D400/sign=cfb239ceb0fb43161a1f7b7a10a54642/3b87e950352ac65ce2e73f76f9f2b21192138ad1.jpg) no-repeat;
    }
}
# 38 什么是外边距重叠？重叠的结果是什么？
外边距重叠就是margin-collapse

在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。
折叠结果遵循下列计算规则：

两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
两个外边距一正一负时，折叠结果是两者的相加的和。
# 39 rgba()和opacity的透明效果有什么不同？
rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，
而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）
# 40 css中可以让文字在垂直和水平方向上重叠的两个属性是什么？
垂直方向：line-height
水平方向：letter-spacing
# 41 如何垂直居中一个浮动元素？
/**方法一：已知元素的高宽**/

# div1{
  background-color:# 6699FF;
  width:200px;
  height:200px;
  position: absolute;        //父元素需要相对定位
  top: 50%;
  left: 50%;
  margin-top:-100px ;   //二分之一的height，width
  margin-left: -100px;
}

/**方法二:**/

# div1{
  width: 200px;
  height: 200px;
  background-color: # 6699FF;
  margin:auto;
  position: absolute;        //父元素需要相对定位
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
}
如何垂直居中一个<img>?（用更简便的方法。）

# container     /**<img>的容器设置如下**/
{
    display:table-cell;
    text-align:center;
    vertical-align:middle;
}
# 42 px和em的区别
px和em都是长度单位，区别是，px的值是固定的，指定是多少就是多少，计算比较容易。em得值不是固定的，并且em会继承父级元素的字体大小。
浏览器的默认字体高都是16px。所以未经调整的浏览器都符合: 1em=16px。那么12px=0.75em, 10px=0.625em。
px 相对于显示器屏幕分辨率，无法用浏览器字体放大功能
em 值并不是固定的，会继承父级的字体大小： em = 像素值 / 父级font-size
# 43 Sass、LESS是什么？大家为什么要使用他们？
他们是CSS预处理器。他是CSS上的一种抽象层。他们是一种特殊的语法/语言编译成CSS。
例如Less是一种动态样式语言. 将CSS赋予了动态语言的特性，如变量，继承，运算， 函数. LESS 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可一在服务端运行 (借助 Node.js)
为什么要使用它们？

结构清晰，便于扩展。
可以方便地屏蔽浏览器私有语法差异。这个不用多说，封装对- 浏览器语法差异的重复处理，减少无意义的机械劳动。
可以轻松实现多重继承。
完全兼容 CSS 代码，可以方便地应用到老项目中。LESS 只- 是在 CSS 语法上做了扩展，所以老的 CSS 代码也可以与 LESS 代码一同编译
# 44 知道css有个content属性吗？有什么作用？有什么应用？
css的content属性专门应用在 before/after伪元素上，用于来插入生成内容。最常见的应用是利用伪类清除浮动。

/**一种常见利用伪类清除浮动的代码**/
.clearfix:after {
    content:".";       //这里利用到了content属性
    display:block;
    height:0;
    visibility:hidden;
    clear:both; 
 }
.clearfix {
    *zoom:1;
}
# 45 水平居中的方法
元素为行内元素，设置父元素text-align:center
如果元素宽度固定，可以设置左右margin为auto;
绝对定位和移动: absolute + transform
使用flex-box布局，指定justify-content属性为center
display设置为tabel-ceil
# 46 垂直居中的方法
将显示方式设置为表格，display:table-cell,同时设置vertial-align：middle
使用flex布局，设置为align-item：center
绝对定位中设置bottom:0,top:0,并设置margin:auto
绝对定位中固定高度时设置top:50%，margin-top值为高度一半的负值
文本垂直居中设置line-height为height值
如果是单行文本, line-height 设置成和 height 值
.vertical {
    height: 100px;
    line-height: 100px;
  }
已知高度的块级子元素，采用绝对定位和负边距
.container {
  position: relative;
}
.vertical {
  height: 300px;  /*子元素高度*/
  position: absolute;
  top:50%;  /*父元素高度50%*/
  margin-top: -150px; /*自身高度一半*/
}
未知高度的块级父子元素居中，模拟表格布局
缺点：IE67不兼容，父级 overflow：hidden 失效
.container {
    display: table;
  }
  .content {
    display: table-cell;
    vertical-align: middle;
  }

新增 inline-block 兄弟元素，设置 vertical-align
缺点：需要增加额外标签，IE67不兼容
.container {
  height: 100%;/*定义父级高度，作为参考*/
}
.extra .vertical{
  display: inline-block;  /*行内块显示*/
  vertical-align: middle; /*垂直居中*/
}
.extra {
  height: 100%; /*设置新增元素高度为100%*/
}
绝对定位配合 CSS3 位移
.vertical {
  position: absolute;
  top:50%;  /*父元素高度50%*/
  transform:translateY(-50%, -50%);
}
CSS3弹性盒模型
.container {
  display:flex;
  justify-content: center; /*子元素水平居中*/
  align-items: center; /*子元素垂直居中*/
}
# 47 如何使用CSS实现硬件加速？
硬件加速是指通过创建独立的复合图层，让GPU来渲染这个图层，从而提高性能，

一般触发硬件加速的CSS属性有transform、opacity、filter，为了避免2D动画在 开始和结束的时候的repaint操作，一般使用tranform:translateZ(0)
# 48 重绘和回流（重排）是什么，如何避免？
重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
注意：JS获取Layout属性值（如：offsetLeft、scrollTop、getComputedStyle等）也会引起回流。因为浏览器需要通过回流计算最新值
回流必将引起重绘，而重绘不一定会引起回流
如何最小化重绘(repaint)和回流(reflow)：

需要要对元素进行复杂的操作时，可以先隐藏(display:"none")，操作完成后再显示
需要创建多个DOM节点时，使用DocumentFragment创建完后一次性的加入document
缓存Layout属性值，如：var left = elem.offsetLeft; 这样，多次使用 left 只产生一次回流
尽量避免用table布局（table元素一旦触发回流就会导致table里所有的其它元素回流）
避免使用css表达式(expression)，因为每次调用都会重新计算值（包括加载页面）
尽量使用 css 属性简写，如：用 border 代替 border-width, border-style, border-color
批量修改元素样式：elem.className 和 elem.style.cssText 代替 elem.style.xxx
# 49 说一说css3的animation
css3的animation是css3新增的动画属性，这个css3动画的每一帧是通过@keyframes来声明的，keyframes声明了动画的名称，通过from、to或者是百分比来定义
每一帧动画元素的状态，通过animation-name来引用这个动画，同时css3动画也可以定义动画运行的时长、动画开始时间、动画播放方向、动画循环次数、动画播放的方式，
这些相关的动画子属性有：animation-name定义动画名、animation-duration定义动画播放的时长、animation-delay定义动画延迟播放的时间、animation-direction定义 动画的播放方向、animation-iteration-count定义播放次数、animation-fill-mode定义动画播放之后的状态、animation-play-state定义播放状态，如暂停运行等、animation-timing-function
定义播放的方式，如恒速播放、艰涩播放等。
# 50 左边宽度固定，右边自适应
左侧固定宽度，右侧自适应宽度的两列布局实现

html结构

<div class="outer">
    <div class="left">固定宽度</div>
    <div class="right">自适应宽度</div>
</div>
在外层div（类名为outer）的div中，有两个子div，类名分别为left和right，其中left为固定宽度，而right为自适应宽度

方法1：左侧div设置成浮动：float: left，右侧div宽度会自拉升适应

.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
    float: left;
}
.right {
    height: 200px;
    background-color: blue;
}
方法2：对右侧:div进行绝对定位，然后再设置right=0，即可以实现宽度自适应

绝对定位元素的第一个高级特性就是其具有自动伸缩的功能，当我们将 width设置为 auto 的时候（或者不设置，默认为 auto ），绝对定位元素会根据其 left 和 right 自动伸缩其大小

.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    position: absolute;
    left: 200px;
    top:0;          
    right: 0;
}
方法3：将左侧div进行绝对定位，然后右侧div设置margin-left: 200px

.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
    position: absolute;
}
.right {
    height: 200px;
    background-color: blue;
    margin-left: 200px;
}
方法4：使用flex布局

.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    display: flex;
    flex-direction: row;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    flex: 1;
}
# 51 两种以上方式实现已知或者未知宽度的垂直水平居中
/** 1 **/
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100px;
    height: 100px;
    margin: -50px 0 0 -50px;
  }
}

/** 2 **/
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}

/** 3 **/
.wraper {
  .box {
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100px;
  }
}

/** 4 **/
.wraper {
  display: table;
  .box {
    display: table-cell;
    vertical-align: middle;
  }
}
# 52 如何实现小于12px的字体效果
transform:scale()这个属性只可以缩放可以定义宽高的元素，而行内元素是没有宽高的，我们可以加上一个display:inline-block;

transform: scale(0.7);
css的属性，可以缩放大小

# 53 css hack原理及常用hack
原理：利用不同浏览器对CSS的支持和解析结果不一样编写针对特定浏览器样式。
常见的hack有
属性hack
选择器hack
IE条件注释
# 54 CSS有哪些继承属性
关于文字排版的属性如：
font
word-break
letter-spacing
text-align
text-rendering
word-spacing
white-space
text-indent
text-transform
text-shadow
line-height
color
visibility
cursor
# 55 外边距折叠(collapsing margins)
毗邻的两个或多个 margin 会合并成一个margin，叫做外边距折叠。规则如下：
两个或多个毗邻的普通流中的块元素垂直方向上的margin会折叠
浮动元素或inline-block元素或绝对定位元素的margin不会和垂直方向上的其他元素的margin折叠
创建了块级格式化上下文的元素，不会和它的子元素发生margin折叠
元素自身的margin-bottom和margin-top相邻时也会折
# 56 CSS选择符有哪些？哪些属性可以继承
id选择器（ #  myid）
类选择器（.myclassname）
标签选择器（div, h1, p）
相邻选择器（h1 + p）
子选择器（ul > li）
后代选择器（li a）
通配符选择器（ * ）
属性选择器（a[rel = "external"]）
伪类选择器（a:hover, li:nth-child）
CSS哪些属性可以继承？哪些属性不可以继承

可继承的样式： font-size font-family color, UL LI DL DD DT
不可继承的样式：border padding margin width height
# 57 CSS3新增伪类有那些
:root 选择文档的根元素，等同于 html 元素
:empty 选择没有子元素的元素
:target 选取当前活动的目标元素
:not(selector) 选择除 selector 元素意外的元素
:enabled 选择可用的表单元素
:disabled 选择禁用的表单元素
:checked 选择被选中的表单元素
:after 在元素内部最前添加内容
:before 在元素内部最后添加内容
:nth-child(n) 匹配父元素下指定子元素，在所有子元素中排序第n
:nth-last-child(n) 匹配父元素下指定子元素，在所有子元素中排序第n，从后向前数
:nth-child(odd)
:nth-child(even)
:nth-child(3n+1)
:first-child
:last-child
:only-child
:nth-of-type(n) 匹配父元素下指定子元素，在同类子元素中排序第n
:nth-last-of-type(n) 匹配父元素下指定子元素，在同类子元素中排序第n，从后向前数
:nth-of-type(odd)
:nth-of-type(even)
:nth-of-type(3n+1)
:first-of-type
:last-of-type
:only-of-type
::selection 选择被用户选取的元素部分
:first-line 选择元素中的第一行
:first-letter 选择元素中的第一个字符
# 58 如何居中div？如何居中一个浮动元素？如何让绝对定位的div居中
给div设置一个宽度，然后添加margin:0 auto属性
div{
  width:200px;
  margin:0 auto;
 
居中一个浮动元素
/* 确定容器的宽高 宽500 高 300 的层
设置层的外边距 */

.div {
  width:500px ; height:300px;//高度可以不设
  margin: -150px 0 0 -250px;
  position:relative;         //相对定位
  background-color:pink;     //方便看效果
  left:50%;
  top:50%;
}
让绝对定位的div居中

position: absolute;
width: 1200px;
background: none;
margin: 0 auto;
top: 0;
left: 0;
bottom: 0;
right: 0;
# 59 用纯CSS创建一个三角形的原理是什么
/* 把上、左、右三条边隐藏掉（颜色设为 transparent） */
# demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
# 60 一个满屏 品 字布局 如何设计?
简单的方式：
上面的div宽100%，
下面的两个div分别宽50%，
然后用float或者inline使其不换行即可
# 61 li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法
行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了

# 62 为什么要初始化CSS样式
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异

# 63 请列举几种隐藏元素的方法
visibility: hidden; 这个属性只是简单的隐藏某个元素，但是元素占用的空间任然存在
opacity: 0; CSS3属性，设置0可以使一个元素完全透明
position: absolute; 设置一个很大的 left 负值定位，使元素定位在可见区域之外
display: none; 元素会变得不可见，并且不会再占用文档的空间。
transform: scale(0); 将一个元素设置为缩放无限小，元素将不可见，元素原来所在的位置将被保留
<div hidden="hidden"> HTML5属性,效果和display:none;相同，但这个属性用于记录一个元素的状态
height: 0; 将元素高度设为 0 ，并消除边框
filter: blur(0); CSS3属性，将一个元素的模糊度设置为0，从而使这个元素“消失”在页面中
# 64 rgba() 和 opacity 的透明效果有什么不同
opacity 作用于元素以及元素内的所有内容（包括文字）的透明度
rgba() 只作用于元素自身的颜色或其背景色，子元素不会继承透明效果
# 65 css 属性 content 有什么作用
content 属性专门应用在 before/after 伪元素上，用于插入额外内容或样式
# 66 请解释一下 CSS3 的 Flexbox（弹性盒布局模型）以及适用场景
1Flexbox1 用于不同尺寸屏幕中创建可自动扩展和收缩布局

# 67 经常遇到的浏览器的JS兼容性有哪些？解决方法是什么
当前样式：getComputedStyle(el, null) VS el.currentStyle
事件对象：e VS window.event
鼠标坐标：e.pageX, e.pageY VS window.event.x, window.event.y
按键码：e.which VS event.keyCode
文本节点：el.textContent VS el.innerText
# 68 请写出多种等高布局
在列的父元素上使用这个背景图进行Y轴的铺放，从而实现一种等高列的假像
模仿表格布局等高列效果：兼容性不好，在ie6-7无法正常运行
css3 flexbox 布局： .container{display: flex; align-items: stretch;}
# 69 浮动元素引起的问题
父元素的高度无法被撑开，影响与父元素同级的元素
与浮动元素同级的非浮动元素会跟随其后
# 70 CSS优化、提高性能的方法有哪些
多个css合并，尽量减少HTTP请求
将css文件放在页面最上面
移除空的css规则
避免使用CSS表达式
选择器优化嵌套，尽量避免层级过深
充分利用css继承属性，减少代码量
抽象提取公共样式，减少代码量
属性值为0时，不加单位
属性值为小于1的小数时，省略小数点前面的0
css雪碧图
# 71 浏览器是怎样解析CSS选择器的
浏览器解析 CSS 选择器的方式是从右到左
# 72 在网页中的应该使用奇数还是偶数的字体
在网页中的应该使用“偶数”字体：
偶数字号相对更容易和 web 设计的其他部分构成比例关系
使用奇数号字体时文本段落无法对齐
宋体的中文网页排布中使用最多的就是 12 和 14
# 73 margin和padding分别适合什么场景使用
需要在border外侧添加空白，且空白处不需要背景（色）时，使用 margin
需要在border内测添加空白，且空白处需要背景（色）时，使用 padding
# 74 抽离样式模块怎么写，说出思路
CSS可以拆分成2部分：公共CSS 和 业务CSS：
网站的配色，字体，交互提取出为公共CSS。这部分CSS命名不应涉及具体的业务
对于业务CSS，需要有统一的命名，使用公用的前缀。可以参考面向对象的CSS
# 75 元素竖向的百分比设定是相对于容器的高度吗
元素竖向的百分比设定是相对于容器的宽度，而不是高度

# 76 全屏滚动的原理是什么？ 用到了CSS的那些属性
原理类似图片轮播原理，超出隐藏部分，滚动时显示
可能用到的CSS属性：overflow:hidden; transform:translate(100%, 100%); display:none;
# 77 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE
响应式设计就是网站能够兼容多个终端，而不是为每个终端做一个特定的版本
基本原理是利用CSS3媒体查询，为不同尺寸的设备适配不同样式
对于低版本的IE，可采用JS获取屏幕宽度，然后通过resize方法来实现兼容：
$(window).resize(function () {
  screenRespond();
});
screenRespond();
function screenRespond(){
var screenWidth = $(window).width();
if(screenWidth <= 1800){
  $("body").attr("class", "w1800");
}
if(screenWidth <= 1400){
  $("body").attr("class", "w1400");
}
if(screenWidth > 1800){
  $("body").attr("class", "");
}
}
# 78 什么是视差滚动效果，如何给每页做不同的动画
视差滚动是指多层背景以不同的速度移动，形成立体的运动效果，具有非常出色的视觉体验

一般把网页解剖为：背景层、内容层和悬浮层。当滚动鼠标滚轮时，各图层以不同速度移动，形成视差的

实现原理

以 “页面滚动条” 作为 “视差动画进度条”
以 “滚轮刻度” 当作 “动画帧度” 去播放动画的
监听 mousewheel 事件，事件被触发即播放动画，实现“翻页”效果
# 79 a标签上四个伪类的执行顺序是怎么样的
link > visited > hover > active

L-V-H-A love hate 用喜欢和讨厌两个词来方便记忆
# 80 伪元素和伪类的区别和作用
伪元素 -- 在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。
它们只在外部显示可见，但不会在文档的源代码中找到它们，因此，称为“伪”元素。例如：
p::before {content:"第一章：";}
p::after {content:"Hot!";}
p::first-line {background:red;}
p::first-letter {font-size:30px;}

伪类 -- 将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。例如：
a:hover {color: # FF00FF}
p:first-child {color: red}
# 81 ::before 和 :after 中双冒号和单冒号有什么区别
在 CSS 中伪类一直用 : 表示，如 :hover, :active 等
伪元素在CSS1中已存在，当时语法是用 : 表示，如 :before 和 :after
后来在CSS3中修订，伪元素用 :: 表示，如 ::before 和 ::after，以此区分伪元素和伪类
由于低版本IE对双冒号不兼容，开发者为了兼容性各浏览器，继续使使用 :after 这种老语法表示伪元素
综上所述：::before 是 CSS3 中写伪元素的新语法； :after 是 CSS1 中存在的、兼容IE的老语法
# 82 如何修改Chrome记住密码后自动填充表单的黄色背景
产生原因：由于Chrome默认会给自动填充的input表单加上 input:-webkit-autofill 私有属性造成的
解决方案1：在form标签上直接关闭了表单的自动填充：autocomplete="off"
解决方案2：input:-webkit-autofill { background-color: transparent; }
input [type=search] 搜索框右侧小图标如何美化？

input[type="search"]::-webkit-search-cancel-button{
  -webkit-appearance: none;
  height: 15px;
  width: 15px;
  border-radius: 8px;
  background:url("images/searchicon.png") no-repeat 0 0;
  background-size: 15px 15px;
}
# 83 网站图片文件，如何点击下载？而非点击预览
<a href="logo.jpg" download>下载</a> <a href="logo.jpg" download="网站LOGO" >下载</a>

# 63 iOS safari 如何阻止“橡皮筋效果”
  $(document).ready(function(){
      var stopScrolling = function(event) {
          event.preventDefault();
      }
      document.addEventListener('touchstart', stopScrolling, false);
      document.addEventListener('touchmove', stopScrolling, false);
  });
# 84 你对 line-height 是如何理解的
line-height 指一行字的高度，包含了字间距，实际上是下一行基线到上一行基线距离
如果一个标签没有定义 height 属性，那么其最终表现的高度是由 line-height 决定的
一个容器没有设置高度，那么撑开容器高度的是 line-height 而不是容器内的文字内容
把 line-height 值设置为 height 一样大小的值可以实现单行文字的垂直居中
line-height 和 height 都能撑开一个高度，height 会触发 haslayout，而 line-height 不会
# 85 line-height 三种赋值方式有何区别？（带单位、纯数字、百分比）
带单位：px 是固定值，而 em 会参考父元素 font-size 值计算自身的行高
纯数字：会把比例传递给后代。例如，父级行高为 1.5，子元素字体为 18px，则子元素行高为 1.5 * 18 = 27px
百分比：将计算后的值传递给后代
# 86 设置元素浮动后，该元素的 display 值会如何变化
设置元素浮动后，该元素的 display 值自动变成 block

# 87 让页面里的字体变清晰，变细用CSS怎么做？（IOS手机浏览器字体齿轮设置）
  -webkit-font-smoothing: antialiased;
# 88 font-style 属性 oblique 是什么意思
font-style: oblique; 使没有 italic 属性的文字实现倾斜

# 89 display:inline-block 什么时候会显示间隙
相邻的 inline-block 元素之间有换行或空格分隔的情况下会产生间距
非 inline-block 水平元素设置为 inline-block 也会有水平间距
可以借助 vertical-align:top; 消除垂直间隙
可以在父级加 font-size：0; 在子元素里设置需要的字体大小，消除垂直间隙
把 li 标签写到同一行可以消除垂直间隙，但代码可读性差
# 90 一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度
方案1：
.sub { height: calc(100%-100px); }
方案2：
.container { position:relative; }
.sub { position: absolute; top: 100px; bottom: 0; }
方案3：
.container { display:flex; flex-direction:column; }
.sub { flex:1; }
# 
三、JavaScript
# 1 闭包
闭包就是能够读取其他函数内部变量的函数

闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域

闭包的特性：

函数内再嵌套函数
内部函数可以引用外层的参数和变量
参数和变量不会被垃圾回收机制回收
说说你对闭包的理解

使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念

闭包 的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中

闭包的另一个用处，是封装对象的私有属性和私有方法

好处：能够实现封装和缓存等；

坏处：就是消耗内存、不正当使用会造成内存溢出的问题

使用闭包的注意点

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露
解决方法是，在退出函数之前，将不使用的局部变量全部删除
# 2 说说你对作用域链的理解
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的
简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期
# 3 JavaScript原型，原型链 ? 有什么特点？
每个对象都会在其内部初始化一个属性，就是__proto__，当我们访问一个对象的属性时
如果这个对象内部不存在这个属性，那么他就会去__proto__里找这个属性，这个__proto__又会有自己的__proto__，于是就这样一直找下去，也就是我们平时所说的原型链的概念。按照标准，__proto__ 是不对外公开的，也就是说是个私有属性
关系：instance.constructor.prototype == instance.__proto__
// eg.
var a = {}

a.constructor.prototype == a.__proto__
特点：

JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变
当我们需要一个属性的时，Javascript引擎会先看当前对象中是否有这个属性， 如果没有的

就会查找他的Prototype对象是否有这个属性，如此递推下去，一直检索到 Object 内建对象

原型：

JavaScript的所有对象中都包含了一个 [__proto__] 内部属性，这个属性所对应的就是该对象的原型
JavaScript的函数对象，除了原型 [__proto__] 之外，还预置了 prototype 属性
当函数对象作为构造函数创建实例时，该 prototype 属性值将被作为实例对象的原型 [__proto__]。
原型链：

当一个对象调用的属性/方法自身不存在时，就会去自己 [__proto__] 关联的前辈 prototype 对象上去找
如果没找到，就会去该 prototype 原型 [__proto__] 关联的前辈 prototype 去找。依次类推，直到找到属性/方法或 undefined 为止。从而形成了所谓的“原型链”
原型特点：

JavaScript对象是通过引用来传递的，当修改原型时，与之相关的对象也会继承这一改变
# 4 请解释什么是事件代理
事件代理（Event Delegation），又称之为事件委托。是 JavaScript 中常用绑定事件的常用技巧。顾名思义，“事件代理”即是把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务。事件代理的原理是DOM元素的事件冒泡。使用事件代理的好处是可以提高性能
可以大量节省内存占用，减少事件注册，比如在table上代理所有td的click事件就非常棒
可以实现当新增子对象时无需再次对其绑定
# 5 Javascript如何实现继承？
构造继承

原型继承

实例继承

拷贝继承

原型prototype机制或apply和call方法去实现较简单，建议使用构造函数与原型混合方式

function Parent(){
	this.name = 'wang';
}

function Child(){
        this.age = 28;
}
    
Child.prototype = new Parent();//继承了Parent，通过原型

var demo = new Child();
alert(demo.age);
alert(demo.name);//得到被继承的属性
# 6 谈谈This对象的理解
this总是指向函数的直接调用者（而非间接调用者）
如果有new关键字，this指向new出来的那个对象
在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象Window
# 7 事件模型
W3C中定义事件的发生经历三个阶段：捕获阶段（capturing）、目标阶段（targetin）、冒泡阶段（bubbling）

冒泡型事件：当你使用事件冒泡时，子级元素先触发，父级元素后触发
捕获型事件：当你使用事件捕获时，父级元素先触发，子级元素后触发
DOM事件流：同时支持两种事件模型：捕获型事件和冒泡型事件
阻止冒泡：在W3c中，使用stopPropagation()方法；在IE下设置cancelBubble = true
阻止捕获：阻止事件的默认行为，例如click - <a>后的跳转。在W3c中，使用preventDefault()方法，在IE下设置window.event.returnValue = false
# 8 new操作符具体干了什么呢?
创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型
属性和方法被加入到 this 引用的对象中
新创建的对象由 this 所引用，并且最后隐式的返回 this
# 9 Ajax原理
Ajax的原理简单来说是在用户和服务器之间加了—个中间层(AJAX引擎)，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据
Ajax的过程只涉及JavaScript、XMLHttpRequest和DOM。XMLHttpRequest是ajax的核心机制
/** 1. 创建连接 **/
var xhr = null;
xhr = new XMLHttpRequest()
/** 2. 连接服务器 **/
xhr.open('get', url, true)
/** 3. 发送请求 **/
xhr.send(null);
/** 4. 接受请求 **/
xhr.onreadystatechange = function(){
	if(xhr.readyState == 4){
		if(xhr.status == 200){
			success(xhr.responseText);
		} else { 
			/** false **/
			fail && fail(xhr.status);
		}
	}
}
ajax 有那些优缺点?

优点：
通过异步模式，提升了用户体验.
优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用.
Ajax在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载。
Ajax可以实现动态不刷新（局部刷新）
缺点：
安全问题 AJAX暴露了与服务器交互的细节。
对搜索引擎的支持比较弱。
不容易调试。
# 10 如何解决跨域问题?
首先了解下浏览器的同源策略 同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源

那么怎样解决跨域问题的呢？

通过jsonp跨域
var script = document.createElement('script');
script.type = 'text/javascript';

// 传参并指定回调执行函数为onBack
script.src = 'http://www.....:8080/login?user=admin&callback=onBack';
document.head.appendChild(script);

// 回调执行函数
function onBack(res) {
    alert(JSON.stringify(res));
}

document.domain + iframe跨域
此方案仅限主域相同，子域不同的跨域应用场景

1.）父窗口：(http://www.domain.com/a.html)

<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>
2.）子窗口：(http://child.domain.com/b.html)

document.domain = 'domain.com';
// 获取父窗口中变量
alert('get js data from parent ---> ' + window.parent.user);
nginx代理跨域
nodejs中间件代理跨域
后端在头部信息里面设置安全域名
# 11 模块化开发怎么做？
立即执行函数,不暴露私有成员
var module1 = (function(){
　　　　var _count = 0;
　　　　var m1 = function(){
　　　　　　//...
　　　　};
　　　　var m2 = function(){
　　　　　　//...
　　　　};
　　　　return {
　　　　　　m1 : m1,
　　　　　　m2 : m2
　　　　};
})();
# 12 异步加载JS的方式有哪些？
设置<script>属性 async="async" （一旦脚本可用，则会异步执行）
动态创建 script DOM：document.createElement('script');
XmlHttpRequest 脚本注入
异步加载库 LABjs
模块加载器 Sea.js
# 13 那些操作会造成内存泄漏？
JavaScript 内存泄露指对象在不需要使用它时仍然存在，导致占用的内存不能使用或回收

未使用 var 声明的全局变量
闭包函数(Closures)
循环引用(两个对象相互引用)
控制台日志(console.log)
移除存在绑定事件的DOM元素(IE)
setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏
垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收
# 14 XML和JSON的区别？
数据体积方面

JSON相对于XML来讲，数据的体积小，传递的速度更快些。
数据交互方面

JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互
数据描述方面

JSON对数据的描述性比XML较差
传输速度方面

JSON的速度要远远快于XML
# 15 谈谈你对webpack的看法
WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、Javascript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源
# 16 说说你对AMD和Commonjs的理解
CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数
AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的
# 17 常见web安全及防护原理
sql注入原理

就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令
总的来说有以下几点

永远不要信任用户的输入，要对用户的输入进行校验，可以通过正则表达式，或限制长度，对单引号和双"-"进行转换等
永远不要使用动态拼装SQL，可以使用参数化的SQL或者直接使用存储过程进行数据查询存取
永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接
不要把机密信息明文存放，请加密或者hash掉密码和敏感的信息
XSS原理及防范

Xss(cross-site scripting)攻击指的是攻击者往Web页面里插入恶意html标签或者javascript代码。比如：攻击者在论坛中放一个看似安全的链接，骗取用户点击后，窃取cookie中的用户私密信息；或者攻击者在论坛中加一个恶意表单，当用户提交表单的时候，却把信息传送到攻击者的服务器中，而不是用户原本以为的信任站点
XSS防范方法

首先代码里对用户输入的地方和变量都需要仔细检查长度和对”<”,”>”,”;”,”’”等字符做过滤；其次任何内容写到页面之前都必须加以encode，避免不小心把html tag 弄出来。这一个层面做好，至少可以堵住超过一半的XSS 攻击
XSS与CSRF有什么区别吗？

XSS是获取信息，不需要提前知道其他用户页面的代码和数据包。CSRF是代替用户完成指定的动作，需要知道其他用户页面的代码和数据包。要完成一次CSRF攻击，受害者必须依次完成两个步骤

登录受信任网站A，并在本地生成Cookie

在不登出A的情况下，访问危险网站B

CSRF的防御

服务端的CSRF方式方法很多样，但总的思想都是一致的，就是在客户端页面增加伪随机数
通过验证码的方法
# 18 用过哪些设计模式？
工厂模式：

工厂模式解决了重复实例化的问题，但还有一个问题,那就是识别问题，因为根本无法
主要好处就是可以消除对象间的耦合，通过使用工程方法而不是new关键字
构造函数模式

使用构造函数的方法，即解决了重复实例化的问题，又解决了对象识别的问题，该模式与工厂模式的不同之处在于
直接将属性和方法赋值给 this对象;
# 19 为什么要有同源限制？
同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议
举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。
# 20 offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别
offsetWidth/offsetHeight返回值包含content + padding + border，效果与e.getBoundingClientRect()相同
clientWidth/clientHeight返回值只包含content + padding，如果有滚动条，也不包含滚动条
scrollWidth/scrollHeight返回值包含content + padding + 溢出内容的尺寸
# 21 javascript有哪些方法定义对象
对象字面量： var obj = {};
构造函数： var obj = new Object();
Object.create(): var obj = Object.create(Object.prototype);
# 22 常见兼容性问题？
png24位的图片在iE6浏览器上出现背景，解决方案是做成PNG8
浏览器默认的margin和padding不同。解决方案是加一个全局的*{margin:0;padding:0;}来统一,，但是全局效率很低，一般是如下这样解决：
body,ul,li,ol,dl,dt,dd,form,input,h1,h2,h3,h4,h5,h6,p{
margin:0;
padding:0;
}
IE下,event对象有x,y属性,但是没有pageX,pageY属性
Firefox下,event对象有pageX,pageY属性,但是没有x,y属性.
# 23 说说你对promise的了解
依照 Promise/A+ 的定义，Promise 有四种状态：

pending: 初始状态, 非 fulfilled 或 rejected.

fulfilled: 成功的操作.

rejected: 失败的操作.

settled: Promise已被fulfilled或rejected，且不是pending

另外， fulfilled与 rejected一起合称 settled

Promise 对象用来进行延迟(deferred) 和异步(asynchronous) 计算

Promise 的构造函数

构造一个 Promise，最基本的用法如下：
var promise = new Promise(function(resolve, reject) {

        if (...) {  // succeed

            resolve(result);

        } else {   // fails

            reject(Error(errMessage));

        }
    });
Promise 实例拥有 then 方法（具有 then 方法的对象，通常被称为thenable）。它的使用方法如下：
promise.then(onFulfilled, onRejected)
接收两个函数作为参数，一个在 fulfilled 的时候被调用，一个在rejected的时候被调用，接收参数就是 future，onFulfilled 对应resolve, onRejected对应 reject
# 24 你觉得jQuery源码有哪些写的好的地方
jquery源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当jquery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找undefined时的作用域链
jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法
有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度
jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率
# 25 vue、react、angular
Vue.js 一个用于创建 web 交互界面的库，是一个精简的 MVVM。它通过双向数据绑定把 View 层和 Model 层连接了起来。实际的 DOM 封装和输出格式都被抽象为了Directives 和 Filters

AngularJS 是一个比较完善的前端MVVM框架，包含模板，数据双向绑定，路由，模块化，服务，依赖注入等所有功能，模板功能强大丰富，自带了丰富的 Angular指令

react React 仅仅是 VIEW 层是facebook公司。推出的一个用于构建UI的一个库，能够实现服务器端的渲染。用了virtual dom，所以性能很好。

# 26 Node的应用场景
特点：

1、它是一个Javascript运行环境
2、依赖于Chrome V8引擎进行代码解释
3、事件驱动
4、非阻塞I/O
5、单进程，单线程
优点：

高并发（最重要的优点）
缺点：

1、只支持单核CPU，不能充分利用CPU
2、可靠性低，一旦代码某个环节崩溃，整个系统都崩溃
# 27 谈谈你对AMD、CMD的理解
CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数

AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的

es6模块 CommonJS、AMD、CMD

CommonJS 的规范中，每个 JavaScript 文件就是一个独立的模块上下文（module context），在这个上下文中默认创建的属性都是私有的。也就是说，在一个文件定义的变量（还包括函数和类），都是私有的，对其他文件是不可见的。
CommonJS是同步加载模块,在浏览器中会出现堵塞情况，所以不适用
AMD 异步，需要定义回调define方式
es6 一个模块就是一个独立的文件，该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量 es6还可以导出类、方法，自动适用严格模式
# 28 那些操作会造成内存泄漏
内存泄漏指任何对象在您不再拥有或需要它之后仍然存在
setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏
闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
# 29 web开发中会话跟踪的方法有哪些
cookie
session
url重写
隐藏input
ip地址
# 30 JS的基本数据类型和引用数据类型
基本数据类型：undefined、null、boolean、number、string、symbol
引用数据类型：object、array、function
# 31 介绍js有哪些内置对象
Object 是 JavaScript 中所有对象的父对象
数据封装类对象：Object、Array、Boolean、Number 和 String
其他对象：Function、Arguments、Math、Date、RegExp、Error
# 32 说几条写JavaScript的基本规范
不要在同一行声明多个变量
请使用===/!==来比较true/false或者数值
使用对象字面量替代new Array这种形式
不要使用全局函数
Switch语句必须带有default分支
If语句必须使用大括号
for-in循环中的变量 应该使用var关键字明确限定作用域，从而避免作用域污
# 33 JavaScript有几种类型的值
栈：原始数据类型（Undefined，Null，Boolean，Number、String）
堆：引用数据类型（对象、数组和函数）
两种类型的区别是：存储位置不同；
原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其
在栈中的地址，取得地址后从堆中获得实体
# 34 javascript创建对象的几种方式
javascript创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用JSON；但写法有很多种，也能混合使用

对象字面量的方式
person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
用function来模拟无参的构造函数
function Person(){}
	var person=new Person();//定义一个function，如果使用new"实例化",该function可以看作是一个Class
        person.name="Mark";
        person.age="25";
        person.work=function(){
        alert(person.name+" hello...");
}
person.work();
用function来模拟参构造函数来实现（用this关键字定义构造的上下文属性）
function Pet(name,age,hobby){
       this.name=name;//this作用域：当前对象
       this.age=age;
       this.hobby=hobby;
       this.eat=function(){
           alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
       }
}
var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
maidou.eat();//调用eat方法
用工厂方式来创建（内置对象）
var wcDog =new Object();
     wcDog.name="旺财";
     wcDog.age=3;
     wcDog.work=function(){
       alert("我是"+wcDog.name+",汪汪汪......");
     }
     wcDog.work();
用原型方式来创建
function Dog(){}
Dog.prototype.name="旺财";
Dog.prototype.eat=function(){
	alert(this.name+"是个吃货");
}
var wangcai =new Dog();
wangcai.eat();

用混合方式来创建
 function Car(name,price){
	this.name=name;
	this.price=price;
}
Car.prototype.sell=function(){
	alert("我是"+this.name+"，我现在卖"+this.price+"万元");
}
var camry =new Car("凯美瑞",27);
camry.sell();
# 35 eval是做什么的
它的功能是把对应的字符串解析成JS代码并运行
应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）
由JSON字符串转换为JSON对象的时候可以用eval，var obj =eval('('+ str +')')
# 36 null，undefined 的区别
undefined 表示不存在这个值。

undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined

例如变量被声明了，但没有赋值时，就等于undefined

null 表示一个对象被定义了，值为“空值”

null : 是一个对象(空对象, 没有任何属性和方法)

例如作为函数的参数，表示该函数的参数不是对象；

在验证null时，一定要使用　=== ，因为 ==无法分别null 和　undefined

# 37 ["1", "2", "3"].map(parseInt) 答案是多少
[1, NaN, NaN]因为 parseInt 需要两个参数 (val, radix)，其中radix 表示解析时用的基数。
map传了 3个(element, index, array)，对应的 radix 不合法导致解析失败。
# 38 javascript 代码中的"use strict";是什么意思
use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,使JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为
# 39 JSON 的了解
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式

它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小

JSON字符串转换为JSON对象:

var obj =eval('('+ str +')');
var obj = str.parseJSON();
var obj = JSON.parse(str);
JSON对象转换为JSON字符串：
var last=obj.toJSONString();
var last=JSON.stringify(obj);
# 40 js延迟加载的方式有哪些
设置<script>属性 defer="defer" （脚本将在页面完成解析时执行）
动态创建 script DOM：document.createElement('script');
XmlHttpRequest 脚本注入
延迟加载工具 LazyLoad
# 41 同步和异步的区别
同步：浏览器访问服务器请求，用户看得到页面刷新，重新发请求,等请求完，页面刷新，新内容出现，用户看到新内容,进行下一步操作
异步：浏览器访问服务器请求，用户正常操作，浏览器后端进行请求。等请求完，页面不刷新，新内容也会出现，用户看到新内容
# 42 渐进增强和优雅降级
渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容
# 43 defer和async
defer并行加载js文件，会按照页面上script标签的顺序执行
async并行加载js文件，下载完成立即执行，不会按照页面上script标签的顺序执行
# 44 说说严格模式的限制
变量必须声明后再使用
函数的参数不能有同名属性，否则报错
不能使用with语句
不能对只读属性赋值，否则报错
不能使用前缀0表示八进制数，否则报错
不能删除不可删除的属性，否则报错
不能删除变量delete prop，会报错，只能删除属性delete global[prop]
eval不会在它的外层作用域引入变量
eval和arguments不能被重新赋值
arguments不会自动反映函数参数的变化
不能使用arguments.callee
不能使用arguments.caller
禁止this指向全局对象
不能使用fn.caller和fn.arguments获取函数调用的堆栈
增加了保留字（比如protected、static和interface）
# 45 attribute和property的区别是什么
attribute是dom元素在文档中作为html标签拥有的属性；
property就是dom元素在js中作为对象拥有的属性。
对于html的标准属性来说，attribute和property是同步的，是会自动更新的
但是对于自定义的属性来说，他们是不同步的
# 46 谈谈你对ES6的理解
新增模板字符串（为JavaScript提供了简单的字符串插值功能）
箭头函数
for-of（用来遍历数据—例如数组中的值。）
arguments对象可被不定参数和默认参数完美代替。
ES6将promise对象纳入规范，提供了原生的Promise对象。
增加了let和const命令，用来声明变量。
增加了块级作用域。
let命令实际上就增加了块级作用域。
还有就是引入module模块的概念
# 47 ECMAScript6 怎么写class么
这个语法糖可以让有OOP基础的人更快上手js，至少是一个官方的实现了
但对熟悉js的人来说，这个东西没啥大影响；一个Object.creat()搞定继承，比class简洁清晰的多
# 48 什么是面向对象编程及面向过程编程，它们的异同和优缺点
面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了
面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为
面向对象是以功能来划分问题，而不是步骤
# 49 面向对象编程思想
基本思想是使用对象，类，继承，封装等基本概念来进行程序设计
优点
易维护
采用面向对象思想设计的结构，可读性高，由于继承的存在，即使改变需求，那么维护也只是在局部模块，所以维护起来是非常方便和较低成本的
易扩展
开发工作的重用性、继承性高，降低重复工作量。
缩短了开发周期
# 50 对web标准、可用性、可访问性的理解
可用性（Usability）：产品是否容易上手，用户能否完成任务，效率如何，以及这过程中用户的主观感受可好，是从用户的角度来看产品的质量。可用性好意味着产品质量高，是企业的核心竞争力
可访问性（Accessibility）：Web内容对于残障用户的可阅读和可理解性
可维护性（Maintainability）：一般包含两个层次，一是当系统出现问题时，快速定位并解决问题的成本，成本低则可维护性好。二是代码是否容易被人理解，是否容易修改和增强功能。
# 51 如何通过JS判断一个数组
instanceof方法
instanceof 运算符是用来测试一个对象是否在其原型链原型构造函数的属性
var arr = [];
arr instanceof Array; // true
constructor方法
constructor属性返回对创建此对象的数组函数的引用，就是返回对象相对应的构造函数
var arr = [];
arr.constructor == Array; //true
最简单的方法
这种写法，是 jQuery 正在使用的
Object.prototype.toString.call(value) == '[object Array]'
// 利用这个方法，可以写一个返回数据类型的方法
var isType = function (obj) {
     return Object.prototype.toString.call(obj).slice(8,-1);
}
ES5新增方法isArray()
var a = new Array(123);
var b = new Date();
console.log(Array.isArray(a)); //true
console.log(Array.isArray(b)); //false
# 52 谈一谈let与var的区别
let命令不存在变量提升，如果在let前使用，会导致报错
如果块区中存在let和const命令，就会形成封闭作用域
不允许重复声明，因此，不能在函数内部重新声明参数
# 53 map与forEach的区别
forEach方法，是最基本的方法，就是遍历与循环，默认有3个传参：分别是遍历的数组内容item、数组索引index、和当前遍历数组Array
map方法，基本用法与forEach一致，但是不同的，它会返回一个新的数组，所以在callback需要有return值，如果没有，会返回undefined
# 54 谈一谈你理解的函数式编程
简单说，"函数式编程"是一种"编程范式"（programming paradigm），也就是如何编写程序的方法论
它具有以下特性：闭包和高阶函数、惰性计算、递归、函数是"第一等公民"、只用"表达式"
# 55 谈一谈箭头函数与普通函数的区别？
函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象
不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误
不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替
不可以使用yield命令，因此箭头函数不能用作Generator函数
# 56 谈一谈函数中this的指向
this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象

《javascript语言精髓》中大概概括了4种调用方式：

方法调用模式

函数调用模式

构造器调用模式

graph LR
A-->B
apply/call调用模式
# 57 异步编程的实现方式
回调函数

优点：简单、容易理解
缺点：不利于维护，代码耦合高
事件监听(采用时间驱动模式，取决于某个事件是否发生)：

优点：容易理解，可以绑定多个事件，每个事件可以指定多个回调函数
缺点：事件驱动型，流程不够清晰
发布/订阅(观察者模式)

类似于事件监听，但是可以通过‘消息中心’，了解现在有多少发布者，多少订阅者
Promise对象

优点：可以利用then方法，进行链式写法；可以书写错误时的回调函数；
缺点：编写和理解，相对比较难
Generator函数

优点：函数体内外的数据交换、错误处理机制
缺点：流程管理不方便
async函数

优点：内置执行器、更好的语义、更广的适用性、返回的是Promise、结构清晰。
缺点：错误处理机制
# 58 对原生Javascript了解程度
数据类型、运算、对象、Function、继承、闭包、作用域、原型链、事件、RegExp、JSON、Ajax、DOM、BOM、内存泄漏、跨域、异步装载、模板引擎、前端MVC、路由、模块化、Canvas、ECMAScript
# 59 Js动画与CSS动画区别及相应实现
CSS3的动画的优点
在性能上会稍微好一些，浏览器会对CSS3的动画做一些优化
代码相对简单
缺点
在动画控制上不够灵活
兼容性不好
JavaScript的动画正好弥补了这两个缺点，控制能力很强，可以单帧的控制、变换，同时写得好完全可以兼容IE6，并且功能强大。对于一些复杂控制的动画，使用javascript会比较靠谱。而在实现一些小的交互动效的时候，就多考虑考虑CSS吧
# 60 JS 数组和对象的遍历方式，以及几种方式的比较
通常我们会用循环的方式来遍历数组。但是循环是 导致js 性能问题的原因之一。一般我们会采用下几种方式来进行数组的遍历

for in循环

for循环

forEach

这里的 forEach回调中两个参数分别为 value，index
forEach 无法遍历对象
IE不支持该方法；Firefox 和 chrome 支持
forEach 无法使用 break，continue 跳出循环，且使用 return 是跳过本次循环
这两种方法应该非常常见且使用很频繁。但实际上，这两种方法都存在性能问题

在方式一中，for-in需要分析出array的每个属性，这个操作性能开销很大。用在 key 已知的数组上是非常不划算的。所以尽量不要用for-in，除非你不清楚要处理哪些属性，例如 JSON对象这样的情况

在方式2中，循环每进行一次，就要检查一下数组长度。读取属性（数组长度）要比读局部变量慢，尤其是当 array 里存放的都是 DOM 元素，因为每次读取都会扫描一遍页面上的选择器相关元素，速度会大大降低

# 61 gulp是什么
gulp是前端开发过程中一种基于流的代码构建工具，是自动化项目的构建利器；它不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成
Gulp的核心概念：流
流，简单来说就是建立在面向对象基础上的一种抽象的处理数据的工具。在流中，定义了一些处理数据的基本操作，如读取数据，写入数据等，程序员是对流进行所有操作的，而不用关心流的另一头数据的真正流向
gulp正是通过流和代码优于配置的策略来尽量简化任务编写的工作
Gulp的特点：
易于使用：通过代码优于配置的策略，gulp 让简单的任务简单，复杂的任务可管理
构建快速 利用 Node.js 流的威力，你可以快速构建项目并减少频繁的 IO 操作
易于学习 通过最少的 API，掌握 gulp 毫不费力，构建工作尽在掌握：如同一系列流管道
# 62 说一下Vue的双向绑定数据的原理
vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调
# 63 事件的各个阶段
1：捕获阶段 ---> 2：目标阶段 ---> 3：冒泡阶段
document ---> target目标 ----> document
由此，addEventListener的第三个参数设置为true和false的区别已经非常清晰了
true表示该元素在事件的“捕获阶段”（由外往内传递时）响应事件
false表示该元素在事件的“冒泡阶段”（由内向外传递时）响应事件
# 64 let var const
let

允许你声明一个作用域被限制在块级中的变量、语句或者表达式
let绑定不受变量提升的约束，这意味着let声明不会被提升到当前
该变量处于从块开始到初始化处理的“暂存死区”
var

声明变量的作用域限制在其声明位置的上下文中，而非声明变量总是全局的
由于变量声明（以及其他声明）总是在任意代码执行之前处理的，所以在代码中的任意位置声明变量总是等效于在代码开头声明
const

声明创建一个值的只读引用 (即指针)
基本数据当值发生改变时，那么其对应的指针也将发生改变，故造成 const申明基本数据类型时
再将其值改变时，将会造成报错， 例如 const a = 3 ; a = 5时 将会报错
但是如果是复合类型时，如果只改变复合类型的其中某个Value项时， 将还是正常使用
# 65 快速的让一个数组乱序
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(function(){
    return Math.random() - 0.5;
})
console.log(arr);
# 66 如何渲染几万条数据并不卡住界面
这道题考察了如何在不卡住页面的情况下渲染数据，也就是说不能一次性将几万条都渲染出来，而应该一次渲染部分 DOM，那么就可以通过 requestAnimationFrame 来每 16 ms 刷新一次

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <ul>控件</ul>
  <script>
    setTimeout(() => {
      // 插入十万条数据
      const total = 100000
      // 一次插入 20 条，如果觉得性能不好就减少
      const once = 20
      // 渲染数据总共需要几次
      const loopCount = total / once
      let countOfRender = 0
      let ul = document.querySelector("ul");
      function add() {
        // 优化性能，插入不会造成回流
        const fragment = document.createDocumentFragment();
        for (let i = 0; i < once; i++) {
          const li = document.createElement("li");
          li.innerText = Math.floor(Math.random() * total);
          fragment.appendChild(li);
        }
        ul.appendChild(fragment);
        countOfRender += 1;
        loop();
      }
      function loop() {
        if (countOfRender < loopCount) {
          window.requestAnimationFrame(add);
        }
      }
      loop();
    }, 0);
  </script>
</body>
</html>
# 67 希望获取到页面中所有的checkbox怎么做？
不使用第三方框架

 var domList = document.getElementsByTagName(‘input’)
 var checkBoxList = [];
 var len = domList.length;　　//缓存到局部变量
 while (len--) {　　//使用while的效率会比for循环更高
 　　if (domList[len].type == ‘checkbox’) {
     　　checkBoxList.push(domList[len]);
 　　}
 }

# 68 怎样添加、移除、移动、复制、创建和查找节点
创建新节点

createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点
添加、移除、替换、插入

appendChild()      //添加
removeChild()      //移除
replaceChild()      //替换
insertBefore()      //插入
查找

getElementsByTagName()    //通过标签名称
getElementsByName()     //通过元素的Name属性的值
getElementById()        //通过元素Id，唯一性
# 69 正则表达式
正则表达式构造函数var reg=new RegExp(“xxx”)与正则表达字面量var reg=//有什么不同？匹配邮箱的正则表达式？

当使用RegExp()构造函数的时候，不仅需要转义引号（即\”表示”），并且还需要双反斜杠（即\\表示一个\）。使用正则表达字面量的效率更高
邮箱的正则匹配：

var regMail = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((.[a-zA-Z0-9_-]{2,3}){1,2})$/;
# 70 Javascript中callee和caller的作用？
caller是返回一个对函数的引用，该函数调用了当前函数；
callee是返回正在被执行的function函数，也就是所指定的function对象的正文
那么问题来了？如果一对兔子每月生一对兔子；一对新生兔，从第二个月起就开始生兔子；假定每对兔子都是一雌一雄，试问一对兔子，第n个月能繁殖成多少对兔子？（使用callee完成）

var result=[];
  function fn(n){  //典型的斐波那契数列
     if(n==1){
          return 1;
     }else if(n==2){
             return 1;
     }else{
          if(result[n]){
                  return result[n];
         }else{
                 //argument.callee()表示fn()
                 result[n]=arguments.callee(n-1)+arguments.callee(n-2);
                 return result[n];
         }
    }
 }
# 71 window.onload和$(document).ready
原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？如何用原生JS实现Jq的ready方法？

window.onload()方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。
$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕
function ready(fn){
      if(document.addEventListener) {        //标准浏览器
          document.addEventListener('DOMContentLoaded', function() {
              //注销事件, 避免反复触发
              document.removeEventListener('DOMContentLoaded',arguments.callee, false);
              fn();            //执行函数
          }, false);
      }else if(document.attachEvent) {        //IE
          document.attachEvent('onreadystatechange', function() {
             if(document.readyState == 'complete') {
                 document.detachEvent('onreadystatechange', arguments.callee);
                 fn();        //函数执行
             }
         });
     }
 };
# 72 addEventListener()和attachEvent()的区别
addEventListener()是符合W3C规范的标准方法; attachEvent()是IE低版本的非标准方法
addEventListener()支持事件冒泡和事件捕获; - 而attachEvent()只支持事件冒泡
addEventListener()的第一个参数中,事件类型不需要添加on; attachEvent()需要添加'on'
如果为同一个元素绑定多个事件, addEventListener()会按照事件绑定的顺序依次执行, attachEvent()会按照事件绑定的顺序倒序执行
# 73 获取页面所有的checkbox
var resultArr= [];
var input = document.querySelectorAll('input');
for( var i = 0; i < input.length; i++ ) {
    if( input[i].type == 'checkbox' ) {
        resultArr.push( input[i] );
    }
}
//resultArr即中获取到了页面中的所有checkbox
# 74 数组去重方法总结
方法一、利用ES6 Set去重（ES6中最常用）

function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
方法二、利用for嵌套for，然后splice去重（ES5中最常用）

function unique(arr){            
        for(var i=0; i<arr.length; i++){
            for(var j=i+1; j<arr.length; j++){
                if(arr[i]==arr[j]){         //第一个等同于第二个，splice方法删除第二个
                    arr.splice(j,1);
                    j--;
                }
            }
        }
	return arr;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
    //[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]     //NaN和{}没有去重，两个null直接消失了
双层循环，外层循环元素，内层循环时比较值。值相同时，则删去这个值。
想快速学习更多常用的ES6语法
方法三、利用indexOf去重

function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array .indexOf(arr[i]) === -1) {
            array .push(arr[i])
        }
    }
    return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
   // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]  //NaN、{}没有去重
新建一个空的结果数组，for 循环原数组，判断结果数组是否存在当前元素，如果有相同的值则跳过，不相同则push进数组

方法四、利用sort()

function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return;
    }
    arr = arr.sort()
    var arrry= [arr[0]];
    for (var i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i-1]) {
            arrry.push(arr[i]);
        }
    }
    return arrry;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
// [0, 1, 15, "NaN", NaN, NaN, {…}, {…}, "a", false, null, true, "true", undefined]      //NaN、{}没有去重
利用sort()排序方法，然后根据排序后的结果进行遍历及相邻元素比对

方法五、利用对象的属性不能相同的特点进行去重

function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var arrry= [];
     var  obj = {};
    for (var i = 0; i < arr.length; i++) {
        if (!obj[arr[i]]) {
            arrry.push(arr[i])
            obj[arr[i]] = 1
        } else {
            obj[arr[i]]++
        }
    }
    return arrry;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "true", 15, false, undefined, null, NaN, 0, "a", {…}]    //两个true直接去掉了，NaN和{}去重
方法六、利用includes

function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array =[];
    for(var i = 0; i < arr.length; i++) {
            if( !array.includes( arr[i]) ) {//includes 检测数组是否有某个值
                    array.push(arr[i]);
              }
    }
    return array
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
    //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]     //{}没有去重
方法七、利用hasOwnProperty

function unique(arr) {
    var obj = {};
    return arr.filter(function(item, index, arr){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]   //所有的都去重了
利用hasOwnProperty 判断是否存在对象属性

方法八、利用filter

function unique(arr) {
  return arr.filter(function(item, index, arr) {
    //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
    return arr.indexOf(item, 0) === index;
  });
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, "NaN", 0, "a", {…}, {…}]
方法九、利用递归去重

function unique(arr) {
    var array= arr;
    var len = array.length;

	array.sort(function(a,b){   //排序后更加方便去重
		return a - b;
	})

	function loop(index){
        if(index >= 1){
            if(array[index] === array[index-1]){
            array.splice(index,1);
            }
            loop(index - 1);    //递归loop，然后数组去重
        }
	}
	loop(len-1);
	return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
方法十、利用Map数据结构去重

function arrayNonRepeatfy(arr) {
	let map = new Map();
		let array = new Array();  // 数组用于返回结果
		for (let i = 0; i < arr.length; i++) {
			if(map .has(arr[i])) {  // 如果有该key值
			map .set(arr[i], true);
		} else {
			map .set(arr[i], false);   // 如果没有该key值
			array .push(arr[i]);
		}
	}
	return array ;
}
 var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
创建一个空Map数据结构，遍历需要去重的数组，把数组的每一个元素作为key存到Map中。由于Map中不会出现相同的key值，所以最终得到的就是去重后的结果

方法十一、利用reduce+includes

function unique(arr){
    return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[]);
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr));
// [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
方法十二、[...new Set(arr)]

[...new Set(arr)]
//代码就是这么少----（其实，严格来说并不算是一种，相对于第一种方法来说只是简化了代码）
# 75 （设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）
给需要拖拽的节点绑定mousedown, mousemove, mouseup事件
mousedown事件触发后，开始拖拽
mousemove时，需要通过event.clientX和clientY获取拖拽位置，并实时更新位置
mouseup时，拖拽结束
需要注意浏览器边界的情况
# 76 Javascript全局函数和全局变量
全局变量

Infinity 代表正的无穷大的数值。
NaN 指示某个值是不是数字值。
undefined 指示未定义的值。
全局函数

decodeURI() 解码某个编码的 URI。
decodeURIComponent() 解码一个编码的 URI 组件。
encodeURI() 把字符串编码为 URI。
encodeURIComponent() 把字符串编码为 URI 组件。
escape() 对字符串进行编码。
eval() 计算 JavaScript 字符串，并把它作为脚本代码来执行。
isFinite() 检查某个值是否为有穷大的数。
isNaN() 检查某个值是否是数字。
Number() 把对象的值转换为数字。
parseFloat() 解析一个字符串并返回一个浮点数。
parseInt() 解析一个字符串并返回一个整数。
String() 把对象的值转换为字符串。
unescape() 对由escape() 编码的字符串进行解码
# 77 使用js实现一个持续的动画效果
定时器思路

var e = document.getElementById('e')
var flag = true;
var left = 0;
setInterval(() => {
    left == 0 ? flag = true : left == 100 ? flag = false : ''
    flag ? e.style.left = ` ${left++}px` : e.style.left = ` ${left--}px`
}, 1000 / 60)

requestAnimationFrame

//兼容性处理
window.requestAnimFrame = (function(){
    return window.requestAnimationFrame       ||
           window.webkitRequestAnimationFrame ||
           window.mozRequestAnimationFrame    ||
           function(callback){
                window.setTimeout(callback, 1000 / 60);
           };
})();

var e = document.getElementById("e");
var flag = true;
var left = 0;

function render() {
    left == 0 ? flag = true : left == 100 ? flag = false : '';
    flag ? e.style.left = ` ${left++}px` :
        e.style.left = ` ${left--}px`;
}

(function animloop() {
    render();
    requestAnimFrame(animloop);
})();

使用css实现一个持续的动画效果

animation:mymove 5s infinite;

@keyframes mymove {
    from {top:0px;}
    to {top:200px;}
}
animation-name 规定需要绑定到选择器的 keyframe名称。
animation-duration 规定完成动画所花费的时间，以秒或毫秒计。
animation-timing-function 规定动画的速度曲线。
animation-delay 规定在动画开始之前的延迟。
animation-iteration-count 规定动画应该播放的次数。
animation-direction 规定是否应该轮流反向播放动画
# 78 封装一个函数，参数是定时器的时间，.then执行回调函数
function sleep (time) {
    return new Promise((resolve) => setTimeout(resolve, time));
}
# 79 怎么判断两个对象相等？
obj={
    a:1,
    b:2
}
obj2={
    a:1,
    b:2
}
obj3={
    a:1,
    b:'2'
}
可以转换为字符串来判断

JSON.stringify(obj)==JSON.stringify(obj2);//true
JSON.stringify(obj)==JSON.stringify(obj3);//false
# 80 项目做过哪些性能优化？
减少 HTTP 请求数
减少 DNS 查询
使用 CDN
避免重定向
图片懒加载
减少 DOM 元素数量
减少DOM 操作
使用外部 JavaScript 和 CSS
压缩 JavaScript 、 CSS 、字体、图片等
优化 CSS Sprite
使用 iconfont
字体裁剪
多域名分发划分内容到不同域名
尽量减少 iframe 使用
避免图片 src 为空
把样式表放在link 中
把JavaScript放在页面底部
# 81 浏览器缓存
浏览器缓存分为强缓存和协商缓存。当客户端请求某个资源时，获取缓存的流程如下

先根据这个资源的一些 http header 判断它是否命中强缓存，如果命中，则直接从本地获取缓存资源，不会发请求到服务器；
当强缓存没有命中时，客户端会发送请求到服务器，服务器通过另一些request header验证这个资源是否命中协商缓存，称为http再验证，如果命中，服务器将请求返回，但不返回资源，而是告诉客户端直接从缓存中获取，客户端收到返回后就会从缓存中获取资源；
强缓存和协商缓存共同之处在于，如果命中缓存，服务器都不会返回资源； 区别是，强缓存不对发送请求到服务器，但协商缓存会。
当协商缓存也没命中时，服务器就会将资源发送回客户端。
当 ctrl+f5 强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存；
当 f5刷新网页时，跳过强缓存，但是会检查协商缓存；
强缓存

Expires（该字段是 http1.0 时的规范，值为一个绝对时间的 GMT 格式的时间字符串，代表缓存资源的过期时间）
Cache-Control:max-age（该字段是 http1.1的规范，强缓存利用其 max-age 值来判断缓存资源的最大生命周期，它的值单位为秒）
协商缓存

Last-Modified（值为资源最后更新时间，随服务器response返回）
If-Modified-Since（通过比较两个时间来判断资源在两次请求期间是否有过修改，如果没有修改，则命中协商缓存）
ETag（表示资源内容的唯一标识，随服务器response返回）
If-None-Match（服务器通过比较请求头部的If-None-Match与当前资源的ETag是否一致来判断资源是否在两次请求之间有过修改，如果没有修改，则命中协商缓存）
# 82 WebSocket
由于 http 存在一个明显的弊端（消息只能有客户端推送到服务器端，而服务器端不能主动推送到客户端），导致如果服务器如果有连续的变化，这时只能使用轮询，而轮询效率过低，并不适合。于是 WebSocket被发明出来

相比与 http 具有以下有点

支持双向通信，实时性更强；
可以发送文本，也可以二进制文件；
协议标识符是 ws，加密后是 wss ；
较少的控制开销。连接创建后，ws客户端、服务端进行数据交换时，协议控制的数据包头部较小。在不包含头部的情况下，服务端到客户端的包头只有2~10字节（取决于数据包长度），客户端到服务端的的话，需要加上额外的4字节的掩码。而HTTP协议每次通信都需要携带完整的头部；
支持扩展。ws协议定义了扩展，用户可以扩展协议，或者实现自定义的子协议。（比如支持自定义压缩算法等）
无跨域问题。
实现比较简单，服务端库如 socket.io、ws，可以很好的帮助我们入门。而客户端也只需要参照 api 实现即可

# 83 尽可能多的说出你对 Electron 的理解
最最重要的一点，electron 实际上是一个套了 Chrome 的 nodeJS程序

所以应该是从两个方面说开来

Chrome （无各种兼容性问题）；
NodeJS（NodeJS 能做的它也能做）
# 84 深浅拷贝
浅拷贝

Object.assign
或者展开运算符
深拷贝

可以通过 JSON.parse(JSON.stringify(object)) 来解决
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
该方法也是有局限性的

会忽略 undefined
不能序列化函数
不能解决循环引用的对象
# 85 防抖/节流
防抖

在滚动事件中需要做个复杂计算或者实现一个按钮的防二次点击操作。可以通过函数防抖动来实现

// 使用 underscore 的源码来解释防抖动

/**
 * underscore 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
_.debounce = function(func, wait, immediate) {
    var timeout, args, context, timestamp, result;

    var later = function() {
      // 现在和上一次时间戳比较
      var last = _.now() - timestamp;
      // 如果当前间隔时间少于设定时间且大于0就重新设置定时器
      if (last < wait && last >= 0) {
        timeout = setTimeout(later, wait - last);
      } else {
        // 否则的话就是时间到了执行回调函数
        timeout = null;
        if (!immediate) {
          result = func.apply(context, args);
          if (!timeout) context = args = null;
        }
      }
    };

    return function() {
      context = this;
      args = arguments;
      // 获得时间戳
      timestamp = _.now();
      // 如果定时器不存在且立即执行函数
      var callNow = immediate && !timeout;
      // 如果定时器不存在就创建一个
      if (!timeout) timeout = setTimeout(later, wait);
      if (callNow) {
        // 如果需要立即执行函数的话 通过 apply 执行
        result = func.apply(context, args);
        context = args = null;
      }

      return result;
    };
  };
整体函数实现

对于按钮防点击来说的实现

开始一个定时器，只要我定时器还在，不管你怎么点击都不会执行回调函数。一旦定时器结束并设置为 null，就可以再次点击了
对于延时执行函数来说的实现：每次调用防抖动函数都会判断本次调用和之前的时间间隔，如果小于需要的时间间隔，就会重新创建一个定时器，并且定时器的延时为设定时间减去之前的时间间隔。一旦时间到了，就会执行相应的回调函数
节流

防抖动和节流本质是不一样的。防抖动是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行

/**
 * underscore 节流函数，返回函数连续调用时，func 执行频率限定为 次 / wait
 *
 * @param  {function}   func      回调函数
 * @param  {number}     wait      表示时间窗口的间隔
 * @param  {object}     options   如果想忽略开始函数的的调用，传入{leading: false}。
 *                                如果想忽略结尾函数的调用，传入{trailing: false}
 *                                两者不能共存，否则函数不能执行
 * @return {function}             返回客户调用函数   
 */
_.throttle = function(func, wait, options) {
    var context, args, result;
    var timeout = null;
    // 之前的时间戳
    var previous = 0;
    // 如果 options 没传则设为空对象
    if (!options) options = {};
    // 定时器回调函数
    var later = function() {
      // 如果设置了 leading，就将 previous 设为 0
      // 用于下面函数的第一个 if 判断
      previous = options.leading === false ? 0 : _.now();
      // 置空一是为了防止内存泄漏，二是为了下面的定时器判断
      timeout = null;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    };
    return function() {
      // 获得当前时间戳
      var now = _.now();
      // 首次进入前者肯定为 true
	  // 如果需要第一次不执行函数
	  // 就将上次时间戳设为当前的
      // 这样在接下来计算 remaining 的值时会大于0
      if (!previous && options.leading === false) previous = now;
      // 计算剩余时间
      var remaining = wait - (now - previous);
      context = this;
      args = arguments;
      // 如果当前调用已经大于上次调用时间 + wait
      // 或者用户手动调了时间
 	  // 如果设置了 trailing，只会进入这个条件
	  // 如果没有设置 leading，那么第一次会进入这个条件
	  // 还有一点，你可能会觉得开启了定时器那么应该不会进入这个 if 条件了
	  // 其实还是会进入的，因为定时器的延时
	  // 并不是准确的时间，很可能你设置了2秒
	  // 但是他需要2.2秒才触发，这时候就会进入这个条件
      if (remaining <= 0 || remaining > wait) {
        // 如果存在定时器就清理掉否则会调用二次回调
        if (timeout) {
          clearTimeout(timeout);
          timeout = null;
        }
        previous = now;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      } else if (!timeout && options.trailing !== false) {
        // 判断是否设置了定时器和 trailing
	    // 没有的话就开启一个定时器
        // 并且不能不能同时设置 leading 和 trailing
        timeout = setTimeout(later, remaining);
      }
      return result;
    };
  };
# 86 谈谈变量提升？
当执行 JS 代码时，会生成执行环境，只要代码不是写在函数中的，就是在全局执行环境中，函数中的代码会产生函数执行环境，只此两种执行环境

接下来让我们看一个老生常谈的例子，var
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
    console.log('call b')
}
变量提升

这是因为函数和变量提升的原因。通常提升的解释是说将声明的代码移动到了顶部，这其实没有什么错误，便于大家理解。但是更准确的解释应该是：在生成执行环境时，会有两个阶段。第一个阶段是创建的阶段，JS 解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为 undefined，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用

在提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升

b() // call b second

function b() {
    console.log('call b fist')
}
function b() {
    console.log('call b second')
}
var b = 'Hello world'
复制代码var 会产生很多错误，所以在 ES6中引入了 let。let 不能在声明前使用，但是这并不是常说的 let 不会提升，let 提升了，在第一阶段内存也已经为他开辟好了空间，但是因为这个声明的特性导致了并不能在声明前使用

# 87 什么是单线程，和异步的关系
单线程 - 只有一个线程，只能做一件事
原因 - 避免 DOM 渲染的冲突
浏览器需要渲染 DOM
JS 可以修改 DOM 结构
JS 执行的时候，浏览器 DOM 渲染会暂停
两段 JS 也不能同时执行（都修改 DOM 就冲突了）
webworker 支持多线程，但是不能访问 DOM
解决方案 - 异步
# 88 是否用过 jQuery 的 Deferred
         

# 89 前端面试之hybrid
http://blog.poetries.top/2018/10/20/fe-interview-hybrid/(opens new window)

# 90 前端面试之组件化
http://blog.poetries.top/2018/10/20/fe-interview-component/(opens new window)

# 91 前端面试之MVVM浅析
http://blog.poetries.top/2018/10/20/fe-interview-mvvm/(opens new window)

# 92 实现效果，点击容器内的图标，图标边框变成border 1px solid red，点击空白处重置
const box = document.getElementById('box');
function isIcon(target) {
  return target.className.includes('icon');
}

box.onClick = function(e) {
  e.stopPropagation();
  const target = e.target;
  if (isIcon(target)) {
    target.style.border = '1px solid red';
  }
}
const doc = document;
doc.onclick = function(e) {
  const children = box.children;
  for(let i; i < children.length; i++) {
    if (isIcon(children[i])) {
      children[i].style.border = 'none';
    }
  }
}
# 93 请简单实现双向数据绑定mvvm
<input id="input"/>
const data = {};
const input = document.getElementById('input');
Object.defineProperty(data, 'text', {
  set(value) {
    input.value = value;
    this.value = value;
  }
});
input.onChange = function(e) {
  data.text = e.target.value;
}
# 94 实现Storage，使得该对象为单例，并对localStorage进行封装设置值setItem(key,value)和getItem(key)
var instance = null;
class Storage {
  static getInstance() {
    if (!instance) {
      instance = new Storage();
    }
    return instance;
  }
  setItem = (key, value) => localStorage.setItem(key, value),
  getItem = key => localStorage.getItem(key)
}
# 95 说说event loop
首先，js是单线程的，主要的任务是处理用户的交互，而用户的交互无非就是响应DOM的增删改，使用事件队列的形式，一次事件循环只处理一个事件响应，使得脚本执行相对连续，所以有了事件队列，用来储存待执行的事件，那么事件队列的事件从哪里被push进来的呢。那就是另外一个线程叫事件触发线程做的事情了，他的作用主要是在定时触发器线程、异步HTTP请求线程满足特定条件下的回调函数push到事件队列中，等待js引擎空闲的时候去执行，当然js引擎执行过程中有优先级之分，首先js引擎在一次事件循环中，会先执行js线程的主任务，然后会去查找是否有微任务microtask（promise），如果有那就优先执行微任务，如果没有，在去查找宏任务macrotask（setTimeout、setInterval）进行执行

众所周知 JS 是门非阻塞单线程语言，因为在最初 JS 就是为了和浏览器交互而诞生的。如果 JS 是门多线程的语言话，我们在多个线程中处理 DOM 就可能会发生问题（一个线程中新加节点，另一个线程中删除节点）

JS 在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到 Task（有多种 task） 队列中。一旦执行栈为空，Event Loop 就会从 Task 队列中拿出需要执行的代码并放入执行栈中执行，所以本质上来说 JS 中的异步还是同步行为


console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

console.log('script end');
不同的任务源会被分配到不同的 Task 队列中，任务源可以分为 微任务（microtask） 和 宏任务（macrotask）。在 ES6 规范中，microtask 称为 jobs，macrotask 称为 task

console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

new Promise((resolve) => {
    console.log('Promise')
    resolve()
}).then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
// script start => Promise => script end => promise1 => promise2 => setTimeout
以上代码虽然 setTimeout 写在 Promise 之前，但是因为 Promise 属于微任务而 setTimeout 属于宏任务

微任务

process.nextTick
promise
Object.observe
MutationObserver
宏任务

script
setTimeout
setInterval
setImmediate
I/O
UI rendering
宏任务中包括了 script ，浏览器会先执行一个宏任务，接下来有异步代码的话就先执行微任务

所以正确的一次 Event loop 顺序是这样的

执行同步代码，这属于宏任务
执行栈为空，查询是否有微任务需要执行
执行所有微任务
必要的话渲染 UI
然后开始下一轮 Event loop，执行宏任务中的异步代码
通过上述的 Event loop 顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作 DOM 的话，为了更快的响应界面响应，我们可以把操作 DOM 放入微任务中

# 96 说说事件流
事件流分为两种，捕获事件流和冒泡事件流

捕获事件流从根节点开始执行，一直往子节点查找执行，直到查找执行到目标节点
冒泡事件流从目标节点开始执行，一直往父节点冒泡查找执行，直到查到到根节点
事件流分为三个阶段，一个是捕获节点，一个是处于目标节点阶段，一个是冒泡阶段

# 97 JavaScript 对象生命周期的理解
当创建一个对象时，JavaScript 会自动为该对象分配适当的内存
垃圾回收器定期扫描对象，并计算引用了该对象的其他对象的数量
如果被引用数量为 0，或惟一引用是循环的，那么该对象的内存即可回收
# 98 我现在有一个canvas，上面随机布着一些黑块，请实现方法，计算canvas上有多少个黑块
https://www.jianshu.com/p/f54d265f7aa4

# 99 请手写实现一个promise
https://segmentfault.com/a/1190000013396601

# 100 说说从输入URL到看到页面发生的全过程，越详细越好
首先浏览器主进程接管，开了一个下载线程。
然后进行HTTP请求（DNS查询、IP寻址等等），中间会有三次捂手，等待响应，开始下载响应报文。
将下载完的内容转交给Renderer进程管理。
Renderer进程开始解析css rule tree和dom tree，这两个过程是并行的，所以一般我会把link标签放在页面顶部。
解析绘制过程中，当浏览器遇到link标签或者script、img等标签，浏览器会去下载这些内容，遇到时候缓存的使用缓存，不适用缓存的重新下载资源。
css rule tree和dom tree生成完了之后，开始合成render tree，这个时候浏览器会进行layout，开始计算每一个节点的位置，然后进行绘制。
绘制结束后，关闭TCP连接，过程有四次挥手
# 101 描述一下this
this，函数执行的上下文，可以通过apply，call，bind改变this的指向。对于匿名函数或者直接调用的函数来说，this指向全局上下文（浏览器为window，NodeJS为global），剩下的函数调用，那就是谁调用它，this就指向谁。当然还有es6的箭头函数，箭头函数的指向取决于该箭头函数声明的位置，在哪里声明，this就指向哪里

# 102 说一下浏览器的缓存机制
浏览器缓存机制有两种，一种为强缓存，一种为协商缓存

对于强缓存，浏览器在第一次请求的时候，会直接下载资源，然后缓存在本地，第二次请求的时候，直接使用缓存。
对于协商缓存，第一次请求缓存且保存缓存标识与时间，重复请求向服务器发送缓存标识和最后缓存时间，服务端进行校验，如果失效则使用缓存
协商缓存相关设置

Exprires：服务端的响应头，第一次请求的时候，告诉客户端，该资源什么时候会过期。Exprires的缺陷是必须保证服务端时间和客户端时间严格同步。
Cache-control：max-age：表示该资源多少时间后过期，解决了客户端和服务端时间必须同步的问题，
If-None-Match/ETag：缓存标识，对比缓存时使用它来标识一个缓存，第一次请求的时候，服务端会返回该标识给客户端，客户端在第二次请求的时候会带上该标识与服务端进行对比并返回If-None-Match标识是否表示匹配。
Last-modified/If-Modified-Since：第一次请求的时候服务端返回Last-modified表明请求的资源上次的修改时间，第二次请求的时候客户端带上请求头If-Modified-Since，表示资源上次的修改时间，服务端拿到这两个字段进行对比
# 103 现在要你完成一个Dialog组件，说说你设计的思路？它应该有什么功能？
该组件需要提供hook指定渲染位置，默认渲染在body下面。
然后改组件可以指定外层样式，如宽度等
组件外层还需要一层mask来遮住底层内容，点击mask可以执行传进来的onCancel函数关闭Dialog。
另外组件是可控的，需要外层传入visible表示是否可见。
然后Dialog可能需要自定义头head和底部footer，默认有头部和底部，底部有一个确认按钮和取消按钮，确认按钮会执行外部传进来的onOk事件，然后取消按钮会执行外部传进来的onCancel事件。
当组件的visible为true时候，设置body的overflow为hidden，隐藏body的滚动条，反之显示滚动条。
组件高度可能大于页面高度，组件内部需要滚动条。
只有组件的visible有变化且为ture时候，才重渲染组件内的所有内容
# 104 caller和callee的区别
callee

caller返回一个函数的引用，这个函数调用了当前的函数。

使用这个属性要注意

这个属性只有当函数在执行时才有用
如果在javascript程序中，函数是由顶层调用的，则返回null
functionName.caller: functionName是当前正在执行的函数。

function a() {
  console.log(a.caller)
}
callee

callee放回正在执行的函数本身的引用，它是arguments的一个属性

使用callee时要注意:

这个属性只有在函数执行时才有效
它有一个length属性，可以用来获得形参的个数，因此可以用来比较形参和实参个数是否一致，即比较arguments.length是否等于arguments.callee.length
它可以用来递归匿名函数。
function a() {
  console.log(arguments.callee)
}
# 105 ajax、axios、fetch区别
jQuery ajax

$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
});
优缺点：

本身是针对MVC的编程,不符合现在前端MVVM的浪潮
基于原生的XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案
JQuery整个项目太大，单纯使用ajax却要引入整个JQuery非常的不合理（采取个性化打包的方案又不能享受CDN服务）
axios

axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
优缺点：

从浏览器中创建 XMLHttpRequest
从 node.js 发出 http 请求
支持 Promise API
拦截请求和响应
转换请求和响应数据
取消请求
自动转换JSON数据
客户端支持防止CSRF/XSRF
fetch

try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);

}
优缺点：

fetcht只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
fetch默认不会带cookie，需要添加配置项
fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费
fetch没有办法原生监测请求的进度，而XHR可以
# 106 JavaScript的组成
JavaScript 由以下三部分组成：
ECMAScript（核心）：JavaScript` 语言基础
DOM（文档对象模型）：规定了访问HTML和XML的接口
BOM（浏览器对象模型）：提供了浏览器窗口之间进行交互的对象和方法
# 107 检测浏览器版本版本有哪些方式？
根据 navigator.userAgent UA.toLowerCase().indexOf('chrome')
根据 window 对象的成员 'ActiveXObject' in window
# 108 介绍JS有哪些内置对象
数据封装类对象：Object、Array、Boolean、Number、String
其他对象：Function、Arguments、Math、Date、RegExp、Error
ES6新增对象：Symbol、Map、Set、Promises、Proxy、Reflect
# 109 说几条写JavaScript的基本规范
代码缩进，建议使用“四个空格”缩进
代码段使用花括号{}包裹
语句结束使用分号;
变量和函数在使用前进行声明
以大写字母开头命名构造函数，全大写命名常量
规范定义JSON对象，补全双引号
用{}和[]声明对象和数组
# 110 如何编写高性能的JavaScript
遵循严格模式："use strict";
将js脚本放在页面底部，加快渲染页面
将js脚本将脚本成组打包，减少请求
使用非阻塞方式下载js脚本
尽量使用局部变量来保存全局变量
尽量减少使用闭包
使用 window 对象属性方法时，省略 window
尽量减少对象成员嵌套
缓存 DOM 节点的访问
通过避免使用 eval() 和 Function() 构造器
给 setTimeout() 和 setInterval() 传递函数而不是字符串作为参数
尽量使用直接量创建对象和数组
最小化重绘(repaint)和回流(reflow)
# 111 描述浏览器的渲染过程，DOM树和渲染树的区别
浏览器的渲染过程：

解析HTML构建 DOM(DOM树)，并行请求 css/image/js
CSS 文件下载完成，开始构建 CSSOM(CSS树)
CSSOM 构建结束后，和 DOM 一起生成 Render Tree(渲染树)
布局(Layout)：计算出每个节点在屏幕中的位置
显示(Painting)：通过显卡把页面画到屏幕上
DOM树 和 渲染树 的区别：

DOM树与HTML标签一一对应，包括head和隐藏元素
渲染树不包括head和隐藏元素，大段文本的每一个行都是独立节点，每一个节点都有对应的css属性
# 112 script 的位置是否会影响首屏显示时间
在解析 HTML 生成 DOM 过程中，js 文件的下载是并行的，不需要 DOM 处理到 script 节点。因此，script的位置不影响首屏显示的开始时间。
浏览器解析 HTML 是自上而下的线性过程，script作为 HTML 的一部分同样遵循这个原则
因此，script 会延迟 DomContentLoad，只显示其上部分首屏内容，从而影响首屏显示的完成时间
# 113 解释JavaScript中的作用域与变量声明提升
JavaScript作用域：

在Java、C等语言中，作用域为for语句、if语句或{}内的一块区域，称为作用域；
而在 JavaScript 中，作用域为function(){}内的区域，称为函数作用域。
JavaScript变量声明提升：

在JavaScript中，函数声明与变量声明经常被JavaScript引擎隐式地提升到当前作用域的顶部。
声明语句中的赋值部分并不会被提升，只有名称被提升
函数声明的优先级高于变量，如果变量名跟函数名相同且未赋值，则函数声明会覆盖变量声明
如果函数有多个同名参数，那么最后一个参数（即使没有定义）会覆盖前面的同名参数
# 114 JavaScript有几种类型的值？，你能画一下他们的内存图吗
原始数据类型（Undefined，Null，Boolean，Number、String）-- 栈
引用数据类型（对象、数组和函数）-- 堆
两种类型的区别是：存储位置不同：
原始数据类型是直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据；
引用数据类型存储在堆(heap)中的对象，占据空间大、大小不固定，如果存储在栈中，将会影响程序运行的性能；
引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。
当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。
# 115 JavaScript如何实现一个类，怎么实例化这个类
构造函数法（this + prototype） -- 用 new 关键字 生成实例对象
缺点：用到了 this 和 prototype，编写复杂，可读性差
function Mobile(name, price){
  this.name = name;
  this.price = price;
}
Mobile.prototype.sell = function(){
  alert(this.name + "，售价 $" + this.price);
}
var iPhone7 = new Mobile("iPhone7", 1000);
iPhone7.sell();
Object.create 法 -- 用 Object.create() 生成实例对象
缺点：不能实现私有属性和私有方法，实例对象之间也不能共享数据
 var Person = {
     firstname: "Mark",
     lastname: "Yun",
     age: 25,
     introduce: function(){
         alert('I am ' + Person.firstname + ' ' + Person.lastname);
     }
 };

 var person = Object.create(Person);
 person.introduce();

 // Object.create 要求 IE9+，低版本浏览器可以自行部署：
 if (!Object.create) {
　   Object.create = function (o) {
　　　 function F() {}
　　　 F.prototype = o;
　　　 return new F();
　　};
　}
极简主义法（消除 this 和 prototype） -- 调用 createNew() 得到实例对象
优点：容易理解，结构清晰优雅，符合传统的"面向对象编程"的构造
 var Cat = {
   age: 3, // 共享数据 -- 定义在类对象内，createNew() 外
   createNew: function () {
     var cat = {};
     // var cat = Animal.createNew(); // 继承 Animal 类
     cat.name = "小咪";
     var sound = "喵喵喵"; // 私有属性--定义在 createNew() 内，输出对象外
     cat.makeSound = function () {
       alert(sound);  // 暴露私有属性
     };
     cat.changeAge = function(num){
       Cat.age = num; // 修改共享数据
     };
     return cat; // 输出对象
   }
 };

 var cat = Cat.createNew();
 cat.makeSound();
ES6 语法糖 class -- 用 new 关键字 生成实例对象

class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

var point = new Point(2, 3);
# 116 Javascript如何实现继承
构造函数绑定：使用 call 或 apply 方法，将父对象的构造函数绑定在子对象上

function Cat(name,color){
 　Animal.apply(this, arguments);
 　this.name = name;
 　this.color = color;
}
实例继承：将子对象的 prototype 指向父对象的一个实例
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
拷贝继承：如果把父对象的所有属性和方法，拷贝进子对象

function extend(Child, Parent) {
　　　var p = Parent.prototype;
　　　var c = Child.prototype;
　　　for (var i in p) {
　　　   c[i] = p[i];
　　　}
　　　c.uber = p;
　 }
原型继承：将子对象的 prototype 指向父对象的 prototype

function extend(Child, Parent) {
    var F = function(){};
  　F.prototype = Parent.prototype;
  　Child.prototype = new F();
  　Child.prototype.constructor = Child;
  　Child.uber = Parent.prototype;
}
ES6 语法糖 extends：class ColorPoint extends Point {}

class ColorPoint extends Point {
    constructor(x, y, color) {
      super(x, y); // 调用父类的constructor(x, y)
      this.color = color;
    }
    toString() {
      return this.color + ' ' + super.toString(); // 调用父类的toString()
    }
}
# 117 Javascript作用链域
全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节
如果当前作用域没有找到属性或方法，会向上层作用域查找，直至全局函数，这种形式就是作用域链
# 118 介绍 DOM 的发展
DOM：文档对象模型（Document Object Model），定义了访问HTML和XML文档的标准，与编程语言及平台无关
DOM0：提供了查询和操作Web文档的内容API。未形成标准，实现混乱。如：document.forms['login']
DOM1：W3C提出标准化的DOM，简化了对文档中任意部分的访问和操作。如：JavaScript中的Document对象
DOM2：原来DOM基础上扩充了鼠标事件等细分模块，增加了对CSS的支持。如：getComputedStyle(elem, pseudo)
DOM3：增加了XPath模块和加载与保存（Load and Save）模块。如：XPathEvaluator
# 119 介绍DOM0，DOM2，DOM3事件处理方式区别
DOM0级事件处理方式：
btn.onclick = func;
btn.onclick = null;
DOM2级事件处理方式：
btn.addEventListener('click', func, false);
btn.removeEventListener('click', func, false);
btn.attachEvent("onclick", func);
btn.detachEvent("onclick", func);
DOM3级事件处理方式：
eventUtil.addListener(input, "textInput", func);
eventUtil 是自定义对象，textInput 是DOM3级事件
事件的三个阶段

捕获、目标、冒泡
# 120 介绍事件“捕获”和“冒泡”执行顺序和事件的执行次数
按照W3C标准的事件：首是进入捕获阶段，直到达到目标元素，再进入冒泡阶段
事件执行次数（DOM2-addEventListener）：元素上绑定事件的个数
注意1：前提是事件被确实触发
注意2：事件绑定几次就算几个事件，即使类型和功能完全一样也不会“覆盖”
事件执行顺序：判断的关键是否目标元素
非目标元素：根据W3C的标准执行：捕获->目标元素->冒泡（不依据事件绑定顺序）
目标元素：依据事件绑定顺序：先绑定的事件先执行（不依据捕获冒泡标准）
最终顺序：父元素捕获->目标元素事件1->目标元素事件2->子元素捕获->子元素冒泡->父元素冒泡
注意：子元素事件执行前提 事件确实“落”到子元素布局区域上，而不是简单的具有嵌套关系
在一个DOM上同时绑定两个点击事件：一个用捕获，一个用冒泡。事件会执行几次，先执行冒泡还是捕获？

该DOM上的事件如果被触发，会执行两次（执行次数等于绑定次数）
如果该DOM是目标元素，则按事件绑定顺序执行，不区分冒泡/捕获
如果该DOM是处于事件流中的非目标元素，则先执行捕获，后执行冒泡
事件的代理/委托

事件委托是指将事件绑定目标元素的到父元素上，利用冒泡机制触发该事件
优点：
可以减少事件注册，节省大量内存占用
可以将事件应用于动态添加的子元素上
缺点： 使用不当会造成事件在不应该触发时触发
示例：
ulEl.addEventListener('click', function(e){
    var target = event.target || event.srcElement;
    if(!!target && target.nodeName.toUpperCase() === "LI"){
        console.log(target.innerHTML);
    }
}, false);
W3C事件的 target 与 currentTarget 的区别？

target 只会出现在事件流的目标阶段
currentTarget 可能出现在事件流的任何阶段
当事件流处在目标阶段时，二者的指向相同
当事件流处于捕获或冒泡阶段时：currentTarget 指向当前事件活动的对象(一般为父级)
如何派发事件(dispatchEvent)？（如何进行事件广播？）

W3C: 使用 dispatchEvent 方法
IE: 使用 fireEvent 方法
var fireEvent = function(element, event){
    if (document.createEventObject){
        var mockEvent = document.createEventObject();
        return element.fireEvent('on' + event, mockEvent)
    }else{
        var mockEvent = document.createEvent('HTMLEvents');
        mockEvent.initEvent(event, true, true);
        return !element.dispatchEvent(mockEvent);
    }
}
# 121 什么是函数节流？介绍一下应用场景和原理？
函数节流(throttle)是指阻止一个函数在很短时间间隔内连续调用。 只有当上一次函数执行后达到规定的时间间隔，才能进行下一次调用。 但要保证一个累计最小调用间隔（否则拖拽类的节流都将无连续效果）
函数节流用于 onresize, onscroll 等短时间内会多次触发的事件
函数节流的原理：使用定时器做时间节流。 当触发一个事件时，先用 setTimout 让这个事件延迟一小段时间再执行。 如果在这个时间间隔内又触发了事件，就 clearTimeout 原来的定时器， 再 setTimeout 一个新的定时器重复以上流程。
函数节流简单实现：

function throttle(method, context) {
     clearTimeout(methor.tId);
     method.tId = setTimeout(function(){
         method.call(context);
     }， 100); // 两次调用至少间隔 100ms
}
// 调用
window.onresize = function(){
  throttle(myFunc, window);
}
# 122 区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”
客户区坐标：鼠标指针在可视区中的水平坐标(clientX)和垂直坐标(clientY)
页面坐标：鼠标指针在页面布局中的水平坐标(pageX)和垂直坐标(pageY)
屏幕坐标：设备物理屏幕的水平坐标(screenX)和垂直坐标(screenY)
如何获得一个DOM元素的绝对位置？

elem.offsetLeft：返回元素相对于其定位父级左侧的距离
elem.offsetTop：返回元素相对于其定位父级顶部的距离
elem.getBoundingClientRect()：返回一个DOMRect对象，包含一组描述边框的只读属性，单位像素
# 123 解释一下这段代码的意思
  [].forEach.call($$("*"), function(el){
      el.style.outline = "1px solid # " + (~~(Math.random()*(1<<24))).toString(16);
  })
解释：获取页面所有的元素，遍历这些元素，为它们添加1像素随机颜色的轮廓(outline)
$$(sel) // $$函数被许多现代浏览器命令行支持，等价于 document.querySelectorAll(sel)
[].forEach.call(NodeLists) // 使用 call 函数将数组遍历函数 forEach 应到节点元素列表
el.style.outline = "1px solid # 333" // 样式 outline 位于盒模型之外，不影响元素布局位置
(1<<24) // parseInt("ffffff", 16) == 16777215 == 2^24 - 1 // 1<<24 == 2^24 == 16777216
Math.random()*(1<<24) // 表示一个位于 0 到 16777216 之间的随机浮点数
~~Math.random()*(1<<24) // ~~ 作用相当于 parseInt 取整
(~~(Math.random()*(1<<24))).toString(16) // 转换为一个十六进制-
# 124 Javascript垃圾回收方法
标记清除（mark and sweep）
这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”
垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了
引用计数(reference counting)

在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个 变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间

# 125 请解释一下 JavaScript 的同源策略
概念:同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。这里的同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议
指一段脚本只能读取来自同一来源的窗口和文档的属性
为什么要有同源限制？

我们举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。
缺点
现在网站的JS都会进行压缩，一些文件用了严格模式，而另一些没有。这时这些本来是严格模式的文件，被 merge后，这个串就到了文件的中间，不仅没有指示严格模式，反而在压缩后浪费了字节
# 126 如何删除一个cookie
将时间设为当前时间往前一点
var date = new Date();

date.setDate(date.getDate() - 1);//真正的删除
setDate()方法用于设置一个月的某一天

expires的设置
document.cookie = 'user='+ encodeURIComponent('name')  + ';expires = ' + new Date(0)
# 127 页面编码和被请求的资源编码如果不一致如何处理
后端响应头设置 charset
前端页面<meta>设置 charset
# 128 把<script>放在</body>之前和之后有什么区别？浏览器会如何解析它们？
按照HTML标准，在</body>结束后出现<script>或任何元素的开始标签，都是解析错误
虽然不符合HTML标准，但浏览器会自动容错，使实际效果与写在</body>之前没有区别
浏览器的容错机制会忽略<script>之前的</body>，视作<script>仍在 body 体内。省略</body>和</html>闭合标签符合HTML标准，服务器可以利用这一标准尽可能少输出内容
# 129 JavaScript 中，调用函数有哪几种方式
方法调用模式 Foo.foo(arg1, arg2);
函数调用模式 foo(arg1, arg2);
构造器调用模式 (new Foo())(arg1, arg2);
call/applay调用模式 Foo.foo.call(that, arg1, arg2);
bind调用模式 Foo.foo.bind(that)(arg1, arg2)();
# 130 简单实现 Function.bind 函数
  if (!Function.prototype.bind) {
    Function.prototype.bind = function(that) {
      var func = this, args = arguments;
      return function() {
        return func.apply(that, Array.prototype.slice.call(args, 1));
      }
    }
  }
  // 只支持 bind 阶段的默认参数：
  func.bind(that, arg1, arg2)();

  // 不支持以下调用阶段传入的参数：
  func.bind(that)(arg1, arg2);
# 131 列举一下JavaScript数组和对象有哪些原生方法？
数组：

arr.concat(arr1, arr2, arrn);
arr.join(",");
arr.sort(func);
arr.pop();
arr.push(e1, e2, en);
arr.shift();
unshift(e1, e2, en);
arr.reverse();
arr.slice(start, end);
arr.splice(index, count, e1, e2, en);
arr.indexOf(el);
arr.includes(el); // ES6
对象：

object.hasOwnProperty(prop);
object.propertyIsEnumerable(prop);
object.valueOf();
object.toString();
object.toLocaleString();
Class.prototype.isPropertyOf(object);
# 132 Array.splice() 与 Array.splice() 的区别？
slice

“读取”数组指定的元素，不会对原数组进行修改

语法：arr.slice(start, end)
start 指定选取开始位置（含）
end 指定选取结束位置（不含）
splice

“操作”数组指定的元素，会修改原数组，返回被删除的元素
语法：arr.splice(index, count, [insert Elements])
index 是操作的起始位置
count = 0 插入元素，count > 0 删除元素
[insert Elements] 向数组新插入的元素
# 133 MVVM
MVVM 由以下三个内容组成

View：界面
Model：数据模型
ViewModel：作为桥梁负责沟通 View 和 Model
在 JQuery 时期，如果需要刷新 UI 时，需要先取到对应的 DOM 再更新 UI，这样数据和业务的逻辑就和页面有强耦合
在 MVVM 中，UI 是通过数据驱动的，数据一旦改变就会相应的刷新对应的 UI，UI 如果改变，也会改变对应的数据。这种方式就可以在业务处理中只关心数据的流转，而无需直接和页面打交道。ViewModel 只关心数据和业务的处理，不关心 View 如何处理数据，在这种情况下，View 和 Model 都可以独立出来，任何一方改变了也不一定需要改变另一方，并且可以将一些可复用的逻辑放在一个 ViewModel 中，让多个 View 复用这个 ViewModel
在 MVVM 中，最核心的也就是数据双向绑定，例如 Angluar 的脏数据检测，Vue 中的数据劫持
脏数据检测

当触发了指定事件后会进入脏数据检测，这时会调用 $digest 循环遍历所有的数据观察者，判断当前值是否和先前的值有区别，如果检测到变化的话，会调用 $watch 函数，然后再次调用 $digest 循环直到发现没有变化。循环至少为二次 ，至多为十次
脏数据检测虽然存在低效的问题，但是不关心数据是通过什么方式改变的，都可以完成任务，但是这在 Vue 中的双向绑定是存在问题的。并且脏数据检测可以实现批量检测出更新的值，再去统一更新 UI，大大减少了操作 DOM 的次数
数据劫持

Vue 内部使用了 Obeject.defineProperty() 来实现双向绑定，通过这个函数可以监听到 set 和 get的事件
var data = { name: 'yck' }
observe(data)
let name = data.name // -> get value
data.name = 'yyy' // -> change value

function observe(obj) {
  // 判断类型
  if (!obj || typeof obj !== 'object') {
    return
  }
  Object.keys(data).forEach(key => {
    defineReactive(data, key, data[key])
  })
}

function defineReactive(obj, key, val) {
  // 递归子属性
  observe(val)
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      console.log('get value')
      return val
    },
    set: function reactiveSetter(newVal) {
      console.log('change value')
      val = newVal
    }
  })
}
以上代码简单的实现了如何监听数据的 set 和 get 的事件，但是仅仅如此是不够的，还需要在适当的时候给属性添加发布订阅

<div>
    {{name}}
</div>
在解析如上模板代码时，遇到 {name} 就会给属性 name 添加发布订阅

// 通过 Dep 解耦
class Dep {
  constructor() {
    this.subs = []
  }
  addSub(sub) {
    // sub 是 Watcher 实例
    this.subs.push(sub)
  }
  notify() {
    this.subs.forEach(sub => {
      sub.update()
    })
  }
}
// 全局属性，通过该属性配置 Watcher
Dep.target = null

function update(value) {
  document.querySelector('div').innerText = value
}

class Watcher {
  constructor(obj, key, cb) {
    // 将 Dep.target 指向自己
    // 然后触发属性的 getter 添加监听
    // 最后将 Dep.target 置空
    Dep.target = this
    this.cb = cb
    this.obj = obj
    this.key = key
    this.value = obj[key]
    Dep.target = null
  }
  update() {
    // 获得新值
    this.value = this.obj[this.key]
    // 调用 update 方法更新 Dom
    this.cb(this.value)
  }
}
var data = { name: 'yck' }
observe(data)
// 模拟解析到 `{{name}}` 触发的操作
new Watcher(data, 'name', update)
// update Dom innerText
data.name = 'yyy' 
接下来,对 defineReactive 函数进行改造

function defineReactive(obj, key, val) {
  // 递归子属性
  observe(val)
  let dp = new Dep()
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      console.log('get value')
      // 将 Watcher 添加到订阅
      if (Dep.target) {
        dp.addSub(Dep.target)
      }
      return val
    },
    set: function reactiveSetter(newVal) {
      console.log('change value')
      val = newVal
      // 执行 watcher 的 update 方法
      dp.notify()
    }
  })
}
以上实现了一个简易的双向绑定，核心思路就是手动触发一次属性的 getter 来实现发布订阅的添加

Proxy 与 Obeject.defineProperty 对比

Obeject.defineProperty 虽然已经能够实现双向绑定了，但是他还是有缺陷的。
只能对属性进行数据劫持，所以需要深度遍历整个对象
对于数组不能监听到数据的变化
虽然 Vue 中确实能检测到数组数据的变化，但是其实是使用了 hack 的办法，并且也是有缺陷的

# 134 WEB应用从服务器主动推送Data到客户端有那些方式
AJAX 轮询
html5 服务器推送事件 (new EventSource(SERVER_URL)).addEventListener("message", func);
html5 Websocket
(new WebSocket(SERVER_URL)).addEventListener("message", func);
# 135 继承
原型链继承，将父类的实例作为子类的原型，他的特点是实例是子类的实例也是父类的实例，父类新增的原型方法/属性，子类都能够访问，并且原型链继承简单易于实现，缺点是来自原型对象的所有属性被所有实例共享，无法实现多继承，无法向父类构造函数传参。
构造继承，使用父类的构造函数来增强子类实例，即复制父类的实例属性给子类，构造继承可以向父类传递参数，可以实现多继承，通过call多个父类对象。但是构造继承只能继承父类的实例属性和方法，不能继承原型属性和方法，无法实现函数服用，每个子类都有父类实例函数的副本，影响性能
实例继承，为父类实例添加新特性，作为子类实例返回，实例继承的特点是不限制调用方法，不管是new 子类（）还是子类（）返回的对象具有相同的效果，缺点是实例是父类的实例，不是子类的实例，不支持多继承
拷贝继承：特点：支持多继承，缺点：效率较低，内存占用高（因为要拷贝父类的属性）无法获取父类不可枚举的方法（不可枚举方法，不能使用for in访问到）
组合继承：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
寄生组合继承：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
# 136 this指向
1. this 指向有哪几种

默认绑定：全局环境中，this默认绑定到window
隐式绑定：一般地，被直接对象所包含的函数调用时，也称为方法调用，this隐式绑定到该直接对象
隐式丢失：隐式丢失是指被隐式绑定的函数丢失绑定对象，从而默认绑定到window。显式绑定：通过call()、apply()、bind()方法把对象绑定到this上，叫做显式绑定
new绑定：如果函数或者方法调用之前带有关键字new，它就构成构造函数调用。对于this绑定来说，称为new绑定
构造函数通常不使用return关键字，它们通常初始化新对象，当构造函数的函数体执行完毕时，它会显式返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值
如果构造函数使用return语句但没有指定返回值，或者返回一个原始值，那么这时将忽略返回值，同时使用这个新对象作为调用结果
如果构造函数显式地使用return语句返回一个对象，那么调用表达式的值就是这个对象
2. 改变函数内部 this 指针的指向函数（bind，apply，call的区别）

apply：调用一个对象的一个方法，用另一个对象替换当前对象。例如：B.apply(A, arguments);即A对象应用B对象的方法
call：调用一个对象的一个方法，用另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法
bind除了返回是函数以外，它的参数和call一样
3. 箭头函数

箭头函数没有this，所以需要通过查找作用域链来确定this的值，这就意味着如果箭头函数被非箭头函数包含，this绑定的就是最近一层非箭头函数的this，
箭头函数没有自己的arguments对象，但是可以访问外围函数的arguments对象
不能通过new关键字调用，同样也没有new.target值和原型
# 137 判断是否是数组
Array.isArray(arr
Object.prototype.toString.call(arr) === '[Object Array]'
arr instanceof Array
array.constructor === Array
# 138 加载
1. 异步加载js的方法

defer：只支持IE如果您的脚本不会改变文档的内容，可将 defer 属性加入到<script>标签中，以便加快处理文档的速度。因为浏览器知道它将能够安全地读取文档的剩余部分而不用执行脚本，它将推迟对脚本的解释，直到文档已经显示给用户为止
async：HTML5 属性，仅适用于外部脚本；并且如果在IE中，同时存在defer和async，那么defer的优先级比较高；脚本将在页面完成时执行
2. 图片的懒加载和预加载

预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。
懒加载：懒加载的主要目的是作为服务器前端的优化，减少请求数或延迟请求数
两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。

# 139 垃圾回收
找出那些不再继续使用的变 量，然后释放其占用的内存。为此，垃圾收集器会按照固定的时间间隔(或代码执行中预定的收集时间)， 周期性地执行这一操作。

标记清除

先所有都加上标记，再把环境中引用到的变量去除标记。剩下的就是没用的了

引用计数

跟踪记录每 个值被引用的次数。清除引用次数为0的变量 ⚠️会有循环引用问题 。循环引用如果大量存在就会导致内存泄露。

# 140 有四个操作会忽略enumerable为false的属性
for...in循环：只遍历对象自身的和继承的可枚举的属性。
Object.keys()：返回对象自身的所有可枚举的属性的键名。
JSON.stringify()：只串行化对象自身的可枚举的属性。
Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。
# 141 属性的遍历
ES6 一共有 5 种方法可以遍历对象的属性。

（1）for...in

for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

（2）Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

（3）Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

（4）Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。

（5）Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

首先遍历所有数值键，按照数值升序排列。
其次遍历所有字符串键，按照加入时间升序排列。
最后遍历所有 Symbol 键，按照加入时间升序排列。
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
上面代码中，Reflect.ownKeys方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性2和10，其次是字符串属性b和a，最后是 Symbol 属性。

# 142 为什么通常在发送数据埋点请求的时候使用的是 1x1 像素的透明 gif 图片
能够完成整个 HTTP 请求+响应（尽管不需要响应内容）
触发 GET 请求之后不需要获取和处理数据、服务器也不需要发送数据
跨域友好
执行过程无阻塞
相比 XMLHttpRequest 对象发送 GET 请求，性能上更好
GIF的最低合法体积最小（最小的BMP文件需要74个字节，PNG需要67个字节，而合法的GIF，只需要43个字节）
# 
143 在输入框中如何判断输入的是一个正确的网址
function isUrl(url) {
       try {
           new URL(url);
           return true;
       }catch(err){
     return false;
}}
# 
四、jQuery
# 1 你觉得jQuery或zepto源码有哪些写的好的地方
jquery源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当jquery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找undefined时的作用域链
(function( window, undefined ) {

    //用一个函数域包起来，就是所谓的沙箱

    //在这里边var定义的变量，属于这个函数域内的局部变量，避免污染全局

    //把当前沙箱需要的外部变量通过函数参数引入进来

    //只要保证参数对内提供的接口的一致性，你还可以随意替换传进来的这个参数

  window.jQuery = window.$ = jQuery;

})( window );
jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法
有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度
jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率
# 2 jQuery 的实现原理
(function(window, undefined) {})(window);

jQuery 利用 JS 函数作用域的特性，采用立即调用表达式包裹了自身，解决命名空间和变量污染问题

window.jQuery = window.$ = jQuery;

在闭包当中将 jQuery 和 $ 绑定到 window 上，从而将 jQuery 和 $ 暴露为全局变量

# 3 jQuery.fn 的 init 方法返回的 this 指的是什么对象
jQuery.fn 的 init 方法 返回的 this 就是 jQuery 对象
用户使用 jQuery() 或 $() 即可初始化 jQuery 对象，不需要动态的去调用 init 方法
# 4 jQuery.extend 与 jQuery.fn.extend 的区别
$.fn.extend() 和 $.extend() 是 jQuery 为扩展插件提拱了两个方法
$.extend(object); // 为jQuery添加“静态方法”（工具方法）
$.extend({
　　min: function(a, b) { return a < b ? a : b; },
　　max: function(a, b) { return a > b ? a : b; }
});
$.min(2,3); //  2
$.max(4,5); //  5
$.extend([true,] targetObject, object1[, object2]); // 对targt对象进行扩展
var settings = {validate:false, limit:5};
var options = {validate:true, name:"bar"};
$.extend(settings, options);  // 注意：不支持第一个参数传 false
// settings == {validate:true, limit:5, name:"bar"}
$.fn.extend(json); // 为jQuery添加“成员函数”（实例方法）

$.fn.extend({
   alertValue: function() {
      $(this).click(function(){
        alert($(this).val());
      });
   }
});

$("# email").alertValue();
# 5 jQuery 的属性拷贝(extend)的实现原理是什么，如何实现深拷贝
浅拷贝（只复制一份原始对象的引用） var newObject = $.extend({}, oldObject);

深拷贝（对原始对象属性所引用的对象进行进行递归拷贝） var newObject = $.extend(true, {}, oldObject);

# 6 jQuery 的队列是如何实现的
jQuery 核心中有一组队列控制方法，由 queue()/dequeue()/clearQueue() 三个方法组成。
主要应用于 animate()，ajax，其他要按时间顺序执行的事件中
var func1 = function(){alert('事件1');}
var func2 = function(){alert('事件2');}
var func3 = function(){alert('事件3');}
var func4 = function(){alert('事件4');}

// 入栈队列事件
$('# box').queue("queue1", func1);  // push func1 to queue1
$('# box').queue("queue1", func2);  // push func2 to queue1

// 替换队列事件
$('# box').queue("queue1", []);  // delete queue1 with empty array
$('# box').queue("queue1", [func3, func4]);  // replace queue1

// 获取队列事件（返回一个函数数组）
$('# box').queue("queue1");  // [func3(), func4()]

// 出栈队列事件并执行
$('# box').dequeue("queue1"); // return func3 and do func3
$('# box').dequeue("queue1"); // return func4 and do func4

// 清空整个队列
$('# box').clearQueue("queue1"); // delete queue1 with clearQueue
# 7 jQuery 中的 bind(), live(), delegate(), on()的区别
bind() 直接绑定在目标元素上
live() 通过冒泡传播事件，默认document上，支持动态数据
delegate() 更精确的小范围使用事件代理，性能优于 live
on() 是最新的1.9版本整合了之前的三种方式的新事件绑定机制
# 8 是否知道自定义事件
事件即“发布/订阅”模式，自定义事件即“消息发布”，事件的监听即“订阅订阅”
JS 原生支持自定义事件，示例：
  document.createEvent(type); // 创建事件
  event.initEvent(eventType, canBubble, prevent); // 初始化事件
  target.addEventListener('dataavailable', handler, false); // 监听事件
  target.dispatchEvent(e);  // 触发事件
jQuery 里的fire 函数用于调用jQuery自定义事件列表中的事件
# 9 jQuery 通过哪个方法和 Sizzle 选择器结合的
Sizzle 选择器采取 Right To Left 的匹配模式，先搜寻所有匹配标签，再判断它的父节点
jQuery 通过 $(selecter).find(selecter); 和 Sizzle 选择器结合
# 10 jQuery 中如何将数组转化为 JSON 字符串，然后再转化回来
// 通过原生 JSON.stringify/JSON.parse 扩展 jQuery 实现
 $.array2json = function(array) {
    return JSON.stringify(array);
 }

 $.json2array = function(array) {
    // $.parseJSON(array); // 3.0 开始，已过时
    return JSON.parse(array);
 }

 // 调用
 var json = $.array2json(['a', 'b', 'c']);
 var array = $.json2array(json);
# 11 jQuery 一个对象可以同时绑定多个事件，这是如何实现的
  $("# btn").on("mouseover mouseout", func);

  $("# btn").on({
      mouseover: func1,
      mouseout: func2,
      click: func3
  });
# 12 针对 jQuery 的优化方法
缓存频繁操作DOM对象
尽量使用id选择器代替class选择器
总是从# id选择器来继承
尽量使用链式操作
使用时间委托 on绑定事件
采用jQuery的内部函数data()来存储数据
使用最新版本的 jQuery
# 13 jQuery 的 slideUp 动画，当鼠标快速连续触发, 动画会滞后反复执行，该如何处理呢
在触发元素上的事件设置为延迟处理：使用 JS 原生 setTimeout 方法
在触发元素的事件时预先停止所有的动画，再执行相应的动画事件：$('.tab').stop().slideUp();
# 14 jQuery UI 如何自定义组件
通过向 $.widget() 传递组件名称和一个原型对象来完成
$.widget("ns.widgetName", [baseWidget], widgetPrototype);
# 15 jQuery 与 jQuery UI、jQuery Mobile 区别
jQuery 是 JS 库，兼容各种PC浏览器，主要用作更方便地处理 DOM、事件、动画、AJAX

jQuery UI 是建立在 jQuery 库上的一组用户界面交互、特效、小部件及主题

jQuery Mobile 以 jQuery 为基础，用于创建“移动Web应用”的框架

# 16 jQuery 和 Zepto 的区别？ 各自的使用场景
jQuery 主要目标是PC的网页中，兼容全部主流浏览器。在移动设备方面，单独推出 `jQuery Mobile
Zepto从一开始就定位移动设备，相对更轻量级。它的API 基本兼容jQuery`，但对PC浏览器兼容不理想
# 17 jQuery对象的特点
只有 JQuery对象才能使用 JQuery 方法
JQuery 对象是一个数组对象
# 
五、Bootstrap
# 1 什么是Bootstrap？以及为什么要使用Bootstrap？
Bootstrap 是一个用于快速开发 Web应用程序和网站的前端框架。Bootstrap是基于 HTML、CSS、JAVASCRIPT 的

Bootstrap具有移动设备优先、浏览器支持良好、容易上手、响应式设计等优点，所以Bootstrap被广泛应用
# 2 使用Bootstrap时，要声明的文档类型是什么？以及为什么要这样声明？
使用Bootstrap时，需要使用 HTML5 文档类型（Doctype）。<!DOCTYPE html>
因为Bootstrap使用了一些 HTML5 元素和 CSS 属性，如果在 Bootstrap创建的网页开头不使用 HTML5 的文档类型（Doctype），可能会面临一些浏览器显示不一致的问题，甚至可能面临一些特定情境下的不一致，以致于代码不能通过 W3C 标准的验证
# 3 什么是Bootstrap网格系统
Bootstrap 包含了一个响应式的、移动设备优先的、不固定的网格系统，可以随着设备或视口大小的增加而适当地扩展到 12 列。它包含了用于简单的布局选项的预定义类，也包含了用于生成更多语义布局的功能强大的混合类

响应式网格系统随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。
# 4 Bootstrap 网格系统（Grid System）的工作原理
（1）行必须放置在 .container class 内，以便获得适当的对齐（alignment）和内边距（padding）。
（2）使用行来创建列的水平组。
（3）内容应该放置在列内，且唯有列可以是行的直接子元素。
（4）预定义的网格类，比如 .row 和 .col-xs-4，可用于快速创建网格布局。LESS 混合类可用于更多语义布局。
（5）列通过内边距（padding）来创建列内容之间的间隙。该内边距是通过 .rows 上的外边距（margin）取负，表示第一列和最后一列的行偏移。
（6）网格系统是通过指定您想要横跨的十二个可用的列来创建的。例如，要创建三个相等的列，则使用三个 .col-xs-4
# 5 对于各类尺寸的设备，Bootstrap设置的class前缀分别是什么
超小设备手机（<768px）：.col-xs-*
小型设备平板电脑（>=768px）：.col-sm-*
中型设备台式电脑（>=992px）：.col-md-*
大型设备台式电脑（>=1200px）：.col-lg-*
# 6 Bootstrap 网格系统列与列之间的间隙宽度是多少
间隙宽度为30px（一个列的每边分别是15px）

# 7 如果需要在一个标题的旁边创建副标题，可以怎样操作
在元素两旁添加<small>，或者添加.small的class

# 8 用Bootstrap，如何设置文字的对齐方式？
class="text-center" 设置居中文本
class="text-right" 设置向右对齐文本
class="text-left" 设置向左对齐文本
# 9 Bootstrap如何设置响应式表格？
增加class="table-responsive"

# 10 使用Bootstrap创建垂直表单的基本步骤？
（1）向父<form>元素添加role="form"；

（2）把标签和控件放在一个带有class="form-group"的<div>中，这是获取最佳间距所必需的；

（3）向所有的文本元素<input>、<textarea>、<select>添加class="form-control"

# 11 使用Bootstrap创建水平表单的基本步骤？
（1）向父<form>元素添加class="form-horizontal"；

（2）把标签和控件放在一个带有class="form-group"的<div>中；

（3）向标签添加class="control-label"。

# 12 使用Bootstrap如何创建表单控件的帮助文本？
增加class="help-block"的span标签或p标签。

# 13 使用Bootstrap激活或禁用按钮要如何操作？
激活按钮：给按钮增加.active的class
禁用按钮：给按钮增加disabled="disabled"的属性
# 14 Bootstrap有哪些关于的class？
（1）.img-rounded 为图片添加圆角
（2）.img-circle 将图片变为圆形
（3）.img-thumbnail 缩略图功能
（4）.img-responsive 图片响应式 (将很好地扩展到父元素)
# 15 Bootstrap中有关元素浮动及清除浮动的class？
（1）class="pull-left" 元素浮动到左边

（2）class="pull-right" 元素浮动到右边

（3）class="clearfix" 清除浮动

# 16 除了屏幕阅读器外，其他设备上隐藏元素的class？
`class="sr-only"``

# 17 Bootstrap如何制作下拉菜单？
（1）将下拉菜单包裹在class="dropdown"的<div>中；
（2）在触发下拉菜单的按钮中添加：class="btn dropdown-toggle" id="dropdownMenu1" data-toggle="dropdown"
（3）在包裹下拉菜单的ul中添加：class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1"
（4）在下拉菜单的列表项中添加：role="presentation"。其中，下拉菜单的标题要添加class="dropdown-header"，选项部分要添加tabindex="-1"。
# 18 Bootstrap如何制作按钮组？以及水平按钮组和垂直按钮组的优先级？
（1）用class="btn-group"的<div>去包裹按钮组；class="btn-group-vertical"可设置垂直按钮组。
（2）btn-group的优先级高于btn-group-vertical的优先级。
# 19 Bootstrap如何设置按钮的下拉菜单？
在一个 .btn-group 中放置按钮和下拉菜单即可。

# 20 Bootstrap中的输入框组如何制作？
（1）把前缀或者后缀元素放在一个带有class="input-group"中的<div>中
（2）在该<div>内，在class="input-group-addon"的<span>里面放置额外的内容；
（3）把<span>放在<input>元素的前面或后面。
# 21 Bootstrap中的导航都有哪些？
（1）导航元素：有class="nav nav-tabs"的标签页导航，还有class="nav nav-pills"的胶囊式标签页导航；
（2）导航栏：class="navbar navbar-default" role="navigation"；
（3）面包屑导航：class="breadcrumb"
# 22 Bootstrap中设置分页的class？
默认的分页：class="pagination"
默认的翻页：class="pager"
# 23 Bootstrap中显示标签的class？
class="label"

# 24 Bootstrap中如何制作徽章？
<span class="badge">26</span>

# 25 Bootstrap中超大屏幕的作用是什么？
设置class="jumbotron"可以制作超大屏幕，该组件可以增加标题的大小并增加更多的外边距

# 
六、微信小程序
# 1 微信小程序有几个文件
WXSS (WeiXin Style Sheets)是一套样式语言，用于描述 WXML 的组件样式， js 逻辑处理，网络请求json小程序设置，如页面注册，页面标题及 tabBar。
app.json 必须要有这个文件，如果没有这个文件，项目无法运行，因为微信框架把这个作为配置文件入口，整个小程序的全局配置。包括页面注册，网络设置，以及小程序的window 背景色，配置导航条样式，配置默认标题。
app.js 必须要有这个文件，没有也是会报错！但是这个文件创建一下就行 什么都不需要写以后我们可以在这个文件中监听并处理小程序的生命周期函数、声明全局变量。
app.wxss 配置全局 css
# 2 微信小程序怎样跟事件传值
给 HTML 元素添加 data-*属性来传递我们需要的值，然后通过 e.currentTarget.dataset 或onload的param参数获取。但 data - 名称不能有大写字母和不可以存放对象

# 3 小程序的 wxss 和 css 有哪些不一样的地方？
wxss的图片引入需使用外链地址
没有 Body；样式可直接使用 import 导入
# 4 小程序关联微信公众号如何确定用户的唯一性
使用 wx.getUserInfo 方法 withCredentials 为 true 时 可获取 encryptedData，里面有 union_id。后端需要进行对称解密

# 5 微信小程序与vue区别
生命周期不一样，微信小程序生命周期比较简单
数据绑定也不同，微信小程序数据绑定需要使用{{}}，vue 直接:就可以
显示与隐藏元素，vue中，使用 v-if 和 v-show 控制元素的显示和隐藏，小程序中，使用wx-if 和hidden 控制元素的显示和隐藏
事件处理不同，小程序中，全用 bindtap(bind+event)，或者 catchtap(catch+event) 绑定事件,vue：使用 v-on:event 绑定事件，或者使用@event 绑定事件
数据双向绑定也不也不一样在 vue中,只需要再表单元素上加上 v-model,然后再绑定 data中对应的一个值，当表单元素内容发生变化时，data中对应的值也会相应改变，这是 vue非常 nice 的一点。微信小程序必须获取到表单元素，改变的值，然后再把值赋给一个 data中声明的变量。
# 
七、webpack相关
# 1 打包体积 优化思路
提取第三方库或通过引用外部文件的方式引入第三方库
代码压缩插件UglifyJsPlugin
服务器启用gzip压缩
按需加载资源文件 require.ensure
优化devtool中的source-map
剥离css文件，单独打包
去除不必要插件，通常就是开发环境与生产环境用同一套配置文件导致
# 2 打包效率
开发环境采用增量构建，启用热更新
开发环境不做无意义的工作如提取css计算文件hash等
配置devtool
选择合适的loader
个别loader开启cache 如babel-loader
第三方库采用引入方式
提取公共代码
优化构建时的搜索路径 指明需要构建目录及不需要构建目录
模块化引入需要的部分
# 3 Loader
编写一个loader

loader就是一个node模块，它输出了一个函数。当某种资源需要用这个loader转换时，这个函数会被调用。并且，这个函数可以通过提供给它的this上下文访问Loader API。 reverse-txt-loader

// 定义
module.exports = function(src) {
  //src是原文件内容（abcde），下面对内容进行处理，这里是反转
  var result = src.split('').reverse().join('');
  //返回JavaScript源码，必须是String或者Buffer
  return `module.exports = '${result}'`;
}
//使用
{
	test: /\.txt$/,
	use: [
		{
			'./path/reverse-txt-loader'
		}
	]
},
# 4 说一下webpack的一些plugin，怎么使用webpack对项目进行优化
构建优化

减少编译体积 ContextReplacementPugin、IgnorePlugin、babel-plugin-import、babel-plugin-transform-runtime
并行编译 happypack、thread-loader、uglifyjsWebpackPlugin开启并行
缓存 cache-loader、hard-source-webpack-plugin、uglifyjsWebpackPlugin开启缓存、babel-loader开启缓存
预编译 dllWebpackPlugin && DllReferencePlugin、auto-dll-webapck-plugin
性能优化

减少编译体积 Tree-shaking、Scope Hositing
hash缓存 webpack-md5-plugin
拆包 splitChunksPlugin、import()、require.ensure
# 
八、编程题
# 1 写一个通用的事件侦听器函数
 // event(事件)工具集，来源：github.com/markyun
    markyun.Event = {

        // 视能力分别使用dom0||dom2||IE方式 来绑定事件
        // 参数： 操作的元素,事件名称 ,事件处理程序
        addEvent : function(element, type, handler) {
            if (element.addEventListener) {
                //事件类型、需要执行的函数、是否捕捉
                element.addEventListener(type, handler, false);
            } else if (element.attachEvent) {
                element.attachEvent('on' + type, function() {
                    handler.call(element);
                });
            } else {
                element['on' + type] = handler;
            }
        },
        // 移除事件
        removeEvent : function(element, type, handler) {
            if (element.removeEventListener) {
                element.removeEventListener(type, handler, false);
            } else if (element.datachEvent) {
                element.detachEvent('on' + type, handler);
            } else {
                element['on' + type] = null;
            }
        },
        // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
        stopPropagation : function(ev) {
            if (ev.stopPropagation) {
                ev.stopPropagation();
            } else {
                ev.cancelBubble = true;
            }
        },
        // 取消事件的默认行为
        preventDefault : function(event) {
            if (event.preventDefault) {
                event.preventDefault();
            } else {
                event.returnValue = false;
            }
        },
        // 获取事件目标
        getTarget : function(event) {
            return event.target || event.srcElement;
        }
# 2 如何判断一个对象是否为数组
function isArray(arg) {
    if (typeof arg === 'object') {
        return Object.prototype.toString.call(arg) === '[object Array]';
    }
    return false;
}
# 3 冒泡排序
每次比较相邻的两个数，如果后一个比前一个小，换位置
var arr = [3, 1, 4, 6, 5, 7, 2];

function bubbleSort(arr) {
for (var i = 0; i < arr.length - 1; i++) {
    for(var j = 0; j < arr.length - i - 1; j++) {
        if(arr[j + 1] < arr[j]) {
            var temp;
            temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
        }
    }
}
return arr;
}

console.log(bubbleSort(arr));
# 4 快速排序
采用二分法，取出中间数，数组每次和中间数比较，小的放到左边，大的放到右边

快速排序的思想很简单，整个排序过程只需要三步：

（1）在数据集之中，找一个基准点
（2）建立两个数组，分别存储左边和右边的数组
（3）利用递归进行下次比较
var arr = [3, 1, 4, 6, 5, 7, 2];

function quickSort(arr) {
    if(arr.length == 0) {
        return [];    // 返回空数组
    }

    var cIndex = Math.floor(arr.length / 2);
    var c = arr.splice(cIndex, 1);
    var l = [];
    var r = [];

    for (var i = 0; i < arr.length; i++) {
        if(arr[i] < c) {
            l.push(arr[i]);
        } else {
            r.push(arr[i]);
        }
    }

    return quickSort(l).concat(c, quickSort(r));
}

console.log(quickSort(arr));
# 5 编写一个方法 求一个字符串的字节长度
假设：一个英文字符占用一个字节，一个中文字符占用两个字节
function GetBytes(str){

        var len = str.length;

        var bytes = len;

        for(var i=0; i<len; i++){

            if (str.charCodeAt(i) > 255) bytes++;

        }

        return bytes;

    }

alert(GetBytes("你好,as"));

# 6 bind的用法，以及如何实现bind的函数和需要注意的点
bind的作用与call和apply相同，区别是call和apply是立即调用函数，而bind是返回了一个函数，需要调用的时候再执行。 一个简单的bind函数实现如下
Function.prototype.bind = function(ctx) {
    var fn = this;
    return function() {
        fn.apply(ctx, arguments);
    };
};
# 7 实现一个函数clone
可以对JavaScript中的5种主要的数据类型,包括Number、String、Object、Array、Boolean）进行值复

考察点1：对于基本数据类型和引用数据类型在内存中存放的是值还是指针这一区别是否清楚
考察点2：是否知道如何判断一个变量是什么类型的
考察点3：递归算法的设计
// 方法一：
  Object.prototype.clone = function(){
          var o = this.constructor === Array ? [] : {};
          for(var e in this){
                  o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
          }
          return o;
  }

 //方法二：
   /**
      * 克隆一个对象
      * @param Obj
      * @returns
      */
     function clone(Obj) {   
         var buf;   
         if (Obj instanceof Array) {   
             buf = [];                    //创建一个空的数组
             var i = Obj.length;   
             while (i--) {   
                 buf[i] = clone(Obj[i]);   
             }   
             return buf;    
         }else if (Obj instanceof Object){   
             buf = {};                   //创建一个空对象
             for (var k in Obj) {           //为这个对象添加新的属性
                 buf[k] = clone(Obj[k]);   
             }   
             return buf;   
         }else{                         //普通变量直接赋值
             return Obj;   
         }   
     }
# 8 下面这个ul，如何点击每一列的时候alert其index
考察闭包

 <ul id=”test”>
     <li>这是第一条</li>
     <li>这是第二条</li>
     <li>这是第三条</li>
 </ul>
  // 方法一：
  var lis=document.getElementById('2223').getElementsByTagName('li');
  for(var i=0;i<3;i++)
  {
      lis[i].index=i;
      lis[i].onclick=function(){
          alert(this.index);
  }

 //方法二：
 var lis=document.getElementById('2223').getElementsByTagName('li');
 for(var i=0;i<3;i++)
 {
     lis[i].index=i;
     lis[i].onclick=(function(a){
         return function() {
             alert(a);
         }
     })(i);
 }
# 9 定义一个log方法，让它可以代理console.log的方法
可行的方法一：

 function log(msg)　{
     console.log(msg);
 }

 log("hello world!") // hello world!
如果要传入多个参数呢？显然上面的方法不能满足要求，所以更好的方法是：

 function log(){
     console.log.apply(console, arguments);
 };
# 10 输出今天的日期
以YYYY-MM-DD的方式，比如今天是2014年9月26日，则输出2014-09-26

var d = new Date();
  // 获取年，getFullYear()返回4位的数字
  var year = d.getFullYear();
  // 获取月，月份比较特殊，0是1月，11是12月
  var month = d.getMonth() + 1;
  // 变成两位
  month = month < 10 ? '0' + month : month;
  // 获取日
  var day = d.getDate();
 day = day < 10 ? '0' + day : day;
 alert(year + '-' + month + '-' + day);
# 11 用js实现随机选取10–100之间的10个数字，存入一个数组，并排序
var iArray = [];
 funtion getRandom(istart, iend){
         var iChoice = istart - iend +1;
         return Math.floor(Math.random() * iChoice + istart;
 }
 for(var i=0; i<10; i++){
         iArray.push(getRandom(10,100));
 }
 iArray.sort();
# 12 写一段JS程序提取URL中的各个GET参数
有这样一个URL：http://item.taobao.com/item.htm?a=1&b=2&c=&d=xxx&e，请写一段JS程序提取URL中的各个GET参数(参数名和参数个数不确定)，将其按key-value形式返回到一个json结构中，如{a:'1', b:'2', c:'', d:'xxx', e:undefined}

function serilizeUrl(url) {
     var result = {};
     url = url.split("?")[1];
     var map = url.split("&");
     for(var i = 0, len = map.length; i < len; i++) {
         result[map[i].split("=")[0]] = map[i].split("=")[1];
     }
     return result;
 }
# 13 写一个function，清除字符串前后的空格
使用自带接口trim()，考虑兼容性：

if (!String.prototype.trim) {
    String.prototype.trim = function() {
        return this.replace(/^\s+/, "").replace(/\s+$/,"");
    }
}

 // test the function
 var str = " \t\n test string ".trim();
 alert(str == "test string"); // alerts "true"
# 14 实现每隔一秒钟输出1,2,3...数字
for(var i=0;i<10;i++){
  (function(j){
     setTimeout(function(){
       console.log(j+1)
     },j*1000)
   })(i)
}
# 15 实现一个函数，判断输入是不是回文字符串
function run(input) {
  if (typeof input !== 'string') return false;
  return input.split('').reverse().join('') === input;
}
# 16、数组扁平化处理
实现一个flatten方法，使得输入一个数组，该数组里面的元素也可以是数组，该方法会输出一个扁平化的数组

function flatten(arr){
    return arr.reduce(function(prev,item){
        return prev.concat(Array.isArray(item)?flatten(item):item);
    },[]);
}
# 17、实现一个函数clone，可以对JavaScript中的5种主要的数据类型（包括Number、String、Object、Array、Boolean）进行值复制
Object.prototype.clone = function(){
    var o = this.constructor === Array ? [] : {};
    for(var e in this){
      o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
    }
    return o;
  }
# 
九、其他
# 1 负载均衡
多台服务器共同协作，不让其中某一台或几台超额工作，发挥服务器的最大作用

http重定向负载均衡：调度者根据策略选择服务器以302响应请求，缺点只有第一次有效果，后续操作维持在该服务器 dns负载均衡：解析域名时，访问多个ip服务器中的一个（可监控性较弱）
反向代理负载均衡：访问统一的服务器，由服务器进行调度访问实际的某个服务器，对统一的服务器要求大，性能受到 服务器群的数量
# 2 CDN
内容分发网络，基本思路是尽可能避开互联网上有可能影响数据传输速度和稳定性的瓶颈和环节，使内容传输的更快、更稳定。

# 3 内存泄漏
定义：程序中己动态分配的堆内存由于某种原因程序未释放或无法释放引发的各种问题。

js中可能出现的内存泄漏情况

结果：变慢，崩溃，延迟大等，原因：

全局变量
dom清空时，还存在引用
ie中使用闭包
定时器未清除
子元素存在引起的内存泄露
避免策略

减少不必要的全局变量，或者生命周期较长的对象，及时对无用的数据进行垃圾回收；
注意程序逻辑，避免“死循环”之类的 ；
避免创建过多的对象 原则：不用了的东西要及时归还。
减少层级过多的引用
# 4 babel原理
ES6、7代码输入 -> babylon进行解析 -> 得到AST（抽象语法树）-> plugin用babel-traverse对AST树进行遍历转译 ->得到新的AST树->用babel-generator通过AST树生成ES5代码

# 5 js自定义事件
三要素： document.createEvent() event.initEvent() element.dispatchEvent()

// (en:自定义事件名称，fn:事件处理函数，addEvent:为DOM元素添加自定义事件，triggerEvent:触发自定义事件)
window.onload = function(){
    var demo = document.getElementById("demo");
    demo.addEvent("test",function(){console.log("handler1")});
    demo.addEvent("test",function(){console.log("handler2")});
    demo.onclick = function(){
        this.triggerEvent("test");
    }
}
Element.prototype.addEvent = function(en,fn){
    this.pools = this.pools || {};
    if(en in this.pools){
        this.pools[en].push(fn);
    }else{
        this.pools[en] = [];
        this.pools[en].push(fn);
    }
}
Element.prototype.triggerEvent  = function(en){
    if(en in this.pools){
        var fns = this.pools[en];
        for(var i=0,il=fns.length;i<il;i++){
            fns[i]();
        }
    }else{
        return;
    }
}
# 6 前后端路由差别
后端每次路由请求都是重新访问服务器
前端路由实际上只是JS根据URL来操作DOM元素，根据每个页面需要的去服务端请求数据，返回数据后和模板进行组合
# 
十、综合
# 1 谈谈你对重构的理解
网站重构：在不改变外部行为的前提下，简化结构、添加可读性，而在网站前端保持一致的行为。也就是说是在不改变UI的情况下，对网站进行优化， 在扩展的同时保持一致的UI
对于传统的网站来说重构通常是：
表格(table)布局改为DIV+CSS
使网站前端兼容于现代浏览器(针对于不合规范的CSS、如对IE6有效的)
对于移动平台的优化
针对于SEO进行优化
# 2 什么样的前端代码是好的
高复用低耦合，这样文件小，好维护，而且好扩展。
具有可用性、健壮性、可靠性、宽容性等特点
遵循设计模式的六大原则
# 3 对前端工程师这个职位是怎么样理解的？它的前景会怎么样
前端是最贴近用户的程序员，比后端、数据库、产品经理、运营、安全都近
实现界面交互
提升用户体验
基于NodeJS，可跨平台开发
前端是最贴近用户的程序员，前端的能力就是能让产品从 90分进化到 100 分，甚至更好，
与团队成员，UI设计，产品经理的沟通；
做好的页面结构，页面重构和用户体验；
# 4 你觉得前端工程的价值体现在哪
为简化用户使用提供技术支持（交互部分）
为多个浏览器兼容性提供支持
为提高用户浏览速度（浏览器性能）提供支持
为跨平台或者其他基于webkit或其他渲染引擎的应用提供支持
为展示数据提供支持（数据接口）
# 5 平时如何管理你的项目
先期团队必须确定好全局样式（globe.css），编码模式(utf-8) 等；
编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
页面进行标注（例如 页面 模块 开始和结束）；
CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）；
JS 分文件夹存放 命名以该JS功能为准的英文翻译。
图片采用整合的 images.png png8 格式文件使用 - 尽量整合在一起使用方便将来的管理
规定全局样式、公共脚本
严格要求代码注释(html/js/css)
严格要求静态资源存放路径
Git 提交必须填写说明
# 6 组件封装
目的：为了重用，提高开发效率和代码质量 注意：低耦合，单一职责，可复用性，可维护性 常用操作

分析布局
初步开发
化繁为简
组件抽象
# 7 Web 前端开发的注意事项
特别设置 meta 标签 viewport
百分比布局宽度，结合 box-sizing: border-box;
使用 rem 作为计算单位。rem 只参照跟节点 html 的字体大小计算
使用 css3 新特性。弹性盒模型、多列布局、媒体查询等
多机型、多尺寸、多系统覆盖测试
# 8 在设计 Web APP 时，应当遵循以下几点
简化不重要的动画/动效/图形文字样式
少用手势，避免与浏览器手势冲突
减少页面内容，页面跳转次数，尽量在当前页面显示
增强 Loading 趣味性，增强页面主次关系
# 9 你怎么看待 Web App/hybrid App/Native App？（移动端前端 和 Web 前端区别？）
Web App(HTML5)：采用HTML5生存在浏览器中的应用，不需要下载安装

优点：开发成本低，迭代更新容易，不需用户升级，跨多个平台和终端
缺点：消息推送不够及时，支持图形和动画效果较差，功能使用限制（相机、GPS等）
Hybrid App(混合开发)：UI WebView，需要下载安装

优点：接近 Native App 的体验，部分支持离线功能
缺点：性能速度较慢，未知的部署时间，受限于技术尚不成熟
Native App(原生开发)：依托于操作系统，有很强的交互，需要用户下载安装使用

优点：用户体验完美，支持离线工作，可访问本地资源（通讯录，相册）
缺点：开发成本高（多系统），开发成本高（版本更新），需要应用商店的审核
# 10 页面重构怎么操作
网站重构：不改变UI的情况下，对网站进行优化，在扩展的同时保持一致的UI。

页面重构可以考虑的方面：
升级第三方依赖
使用HTML5、CSS3、ES6 新特性
加入响应式布局
统一代码风格规范
减少代码间的耦合
压缩/合并静态资源
程序的性能优化
采用CDN来加速资源加载
对于JS DOM的优化
HTTP服务器的文件缓存
# 
十一、一些常见问题
自我介绍
面试完你还有什么问题要问的吗
你有什么爱好?
你最大的优点和缺点是什么?
你为什么会选择这个行业，职位?
你觉得你适合从事这个岗位吗?
你有什么职业规划?
你对工资有什么要求?
如何看待前端开发？
未来三到五年的规划是怎样的？
你的项目中技术难点是什么？遇到了什么问题？你是怎么解决的？
你们部门的开发流程是怎样的
你认为哪个项目做得最好？
说下工作中你做过的一些性能优化处理
最近在看哪些前端方面的书？
平时是如何学习前端开发的？
你最有成就感的一件事
你为什么要离开前一家公司？
你对加班的看法
你希望通过这份工作获得什么？
# 
十二、HR面
# 你还有其他公司的Offer吗?
表明自己有三四个已经确认过的offer了(没有offer也要吹,但是不要透露具体公司)
但是第一意向还是本公司,如果薪资差距不大,会优先考虑本公司
再透露出,有一两个offer催得比较急,希望这边快点出结果
# 为什么从上一家公司离职?
因为公司搬家，通勤时间三个小时
因为离家远花费在路途上的时间过多,不如用来充电,因为加班多导致没有时间充电,无法提高等等
# 如何看待加班(996)?
把加班分为紧急加班和长期加班
对于紧急加班,表示这是每个公司都会遇到的情况,自己愿意牺牲时间帮助公司和团队
对于长期加班,如果是自己长期加班那么会磨练自己的技能,提高自己的效率,如果是团队长期加班,自己会帮助团队找到问题,利用自动化工具或者更高效的协作流程来提高整个团队的效率,帮助大家摆脱加班
# 你对未来3-5年的职业规划
首先表示考虑过这个问题(有规划),如何谈一谈自己的现状(结合实际).
接着从工作本身出发,谈谈自己会如何出色完成本职工作,如何对团队贡献、如何帮助带领团队其他成员创造更多的价值、如何帮助团队扩大影响力.
最后从学习出发,谈谈自己会如何精进领域知识、如何通过提升自己专业能力,如何反哺团队.
# 如何与HR谈薪资
正确的回答可以这样，并且还能够反套路一下HR

HR: 您期望的薪资是多少？
你: 就我的面试表现，贵公司最高可以给多少薪水？
如果经验不够老道的HR可能就真会说出一个报价（如25K）来，然后，你就可以很开心地顺着这个价慢慢地往上谈了。所以这种情况下，你最终的薪资肯定是大于25K的。当然，经验老道的HR会给你一句很官方的套话：

HR: 您期望的薪资是多少？
你: 就我的面试表现，贵公司最高可以给多少薪水？
HR: 这个暂且没法确定，要结合您几轮面试结果和用人部门的意见来综合评定。
虽然薪资很重要，但是我个人觉得这不是最重要的。我有以下建议：

如果你觉得你技术面试效果很好，可以报一个高一点的薪资，这样如果HR想要你，会找你商量的。
如果你觉得技术面试效果一般，但是你比较想进这家公司，可以报一个折中的薪资。
如果你觉得面试效果很好，但是你不想进这家公司，你可以适当“漫天要价”一下。
如果你觉得面试效果不好，但是你想进这家公司，你可以开一个稍微低一点的工资