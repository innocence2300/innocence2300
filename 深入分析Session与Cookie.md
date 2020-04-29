Time: April 25, 2020 11:44 PM
Writer: DC
##为什么需要cookie和session
==在Web发展史中，我们知道浏览器与服务器间采用的是 http协议，而这种协议是无状态的，所以这就导致了服务器无法知道是谁在浏览网页，但很明显，一些网页需要知道用户的状态，例如登陆，购物车等。==

所以为了解决这一问题，先后出现了四种技术，分别是隐藏表单域，URL重写，cookie，session，而用的最多也是比较重要的就是cookie和session了。
##### **Cookie是什么**
cookie是浏览器保存在用户电脑上的一小段文本，通俗的来讲就是当一个用户通过 http访问到服务器时，服务器会将一些 Key/Value键值对返回给客户端浏览器，并给这些数据加上一些限制条件，在条件符合时这个用户下次访问这个服务器时，数据通过请求头又被完整地给带回服务器，服务器根据这些信息来判断不同的用户。
也就是说， cookie是服务器传给客户端并保存在客户端的一段信息，这个 Cookie是有大小，数量限制的！！

##### **Cookie的创建**

当前 Cookie有两个版本，分别对应两种设置响应头：“Set-Cookie”和 “Set-Cookie2”。在Servlet中并不支持Set-Cookie2，所以我们来看看Set-Cookie的属性项：
![](https://mmbiz.qpic.cn/mmbiz_png/xq9PqibkVAzoe2DcbwVo14WDRZbrY5JHwnVswabW1AYuK30z7LKWrv41NC1w46ovic6pPePM3eID4nvYcjZ9XAIg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这些属性项，其他的都说的很清楚了，我们来看看Domain有什么用：

现在，我们假设这里有两个域名：
`域名A：a.b.f.com.cn 域名B：c.d.f.com.cn`
显然，域名A和域名B都是 f.com.cn的子域名
- 如果我们在域名A中的Cookie的domain设置为f.com.cn，那么f.com.cn及其子域名都可以获取这个Cookie，即域名A和域名B都可以获取这个Cookie

- 如果域名A和域名B同时设置Cookie的doamin为f.com.cn，那么将出现覆盖的现象

- 如果域名A没有显式设置Cookie的domain方法，那么domain就为a.b.f.com.cn，不一样的是，这时，域名A的子域名将无法获取这个Cookie

好的，现在了解完了Set-Cookie的属性项，开始创建Cookie

Web服务器通过发送一个称为Set-Cookie的http消息来创建一个Cookie：

Set-Cookie: value[; expires=date][; domain=domain][; path=path][; secure]
**这里我们思考一个问题，当我们在服务器创建多个Cookie时，这些Cookie最终是在一个Header项中还是以独立的Header存在的呢？**
![](https://mmbiz.qpic.cn/mmbiz_png/xq9PqibkVAzoe2DcbwVo14WDRZbrY5JHwygryk2Tc9XkJbRiceoso0nia1iaXU4bxrAYEFPATPHRoBO3Ealf2pAhnA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
我们可以看到，构建http返回字节流时是将Header中所有的项顺序写出，而没有进行任何修改。所以可以想象在浏览器在接收http返回的数据时是分别解析每一个Header项。

接着，在客户端进行保存，如何保存呢？这里又要对Cookie进行进一步的了解
####Cookie的分类

- 会话级别Cookie：所谓会话级别Cookie，就是在浏览器关闭之后Cookie就会失效。

- 持久级别Cookie：保存在硬盘的Cookie，只要设置了过期时间就是硬盘级别Cookie。
好的，现在cookie保存在了客户端，当我们去请求一个URL时，浏览器会根据这个URL路径将符合条件的Cookie放在请求头中传给服务器。

_ _ _

**Cookie是有大小限制和数量限制的**，并且越来越多的Cookie代表客户端和服务器的传输量增加，可不可以每次传的时候不传所有cookie值，而只传一个唯一ID，通过这个ID直接在服务器查找用户信息呢？答案是有的，这就是我们的session.
**
Session是基于Cookie来工作的**，同一个客户端每次访问服务器时，只要当浏览器在第一次访问服务器时，服务器设置一个id并保存一些信息(例如登陆就保存用户信息，视具体情况)，并把这个id通过Cookie存到客户端，客户端每次和服务器交互时只传这个id，就可以实现维持浏览器和服务器的状态，而这个ID通常是NAME为JSESSIONID的一个Cookie
#####实际上，有四种方式让Session正常工作：
- 通过URL传递SessionID

- 通过Cookie传递SessionID

- 通过SSL传递SessionID

- 通过隐藏表单传递SessionID

##### 第一种情况
`当浏览器不支持Cookie功能时，浏览器会将用户的SessionCookieName(默认为JSESSIONID)重写到用户请求的URL参数中。格式：/path/Servlet;name=value;name2=value2?Name3=value3`
##### 第三种情况：
`会根据javax.servlet.request.ssl_session属性值设置SessionID。`
`注：如果客户端支持Cookie，又通过URL重写，Tomcat仍然会解析Cookie中的SessionID并覆盖URL中的SessionID`

**先看session工作的时序图**
![](https://mmbiz.qpic.cn/mmbiz_png/xq9PqibkVAzoe2DcbwVo14WDRZbrY5JHwBSbucaby79P35I2Gt6B4jOkicJCryrSDoQzJGRI4pD6xIxqlE8KIFjg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

####**一、创建session**
当客户端访问到服务器，服务器会为这个客户端通过request.getSession()方法创建一个Session，如果当前SessionID还没有对应的HttpSession对象，就创建一个新的，并添加到org.apache.catalina.Manager的sessions容器中保存，这就做到了对状态的保持。当然，这个SessionID是唯一的
####**二、session保存**
由图可知，session对象已经保存在了Manager类中，StandardManager作为实现类，通过requestedSessionId从StandardManager的sessions集合中取出StandardSession对象。

我们来看看StandardManager时如何对所有StandardSession对象进行生命周期管理

当Servlet容器关闭：
`StandardManager将持久化没过期的StandardSession对象(必须调用Servlet容器中的stop和start命令，不能直接kill)`
当Servlet容器重启时：
`StandardManager初始化会重读这个文件，解析出所有session对象。`
#### **三、session的销毁**
这里有一个误区，也是我之前的错误理解，就是我将session的生命周期理解成一次会话，浏览器打开就创建，浏览器关闭就销毁，这样理解是错的！！

session的声明周期是从创建到超时过期

也就是说，当session创建后，浏览器关闭，会话级别的Cookie被销毁，如果没有超过设定时间，该SessionID对应的session是没有被销毁的，
#### **检查session失效**
检查每个Session是否失效是在Tomcat的一个后台线程完成的(backgroundProcess()方法中)；除了后台进程检验session是否失效外，调用request.getSession()也会检查该session是否过期，当然，调用这种方法如果过期的话又会重新创建一个新的session。
#### **二者的异同**
相同点(有关系的地方)：
- Session和Cookie都是为了让http协议又状态而存在

- Session通过Cookie工作，Cookie传输的SessionID让Session知道这个客户端到底是谁
不同点：
- Session将信息保存到服务器，Cookie将信息保存在客户端

#### **工作流程**

`当浏览器第一次访问服务器时，服务器创建Session并将SessionID通过Cookie带给浏览器保存在客户端，同时服务器根据业务逻辑保存相应的客户端信息保存在session中；客户端再访问时上传Cookie，服务器得到Cookie后获取里面的SessionID，来维持状态。`
