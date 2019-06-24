# Cookie 和 Session #
　Cookie 和 Session 都为了用来保存状态信息，都是保存客户端状态的机制，它们都是
为了解决 HTTP 无状态的问题而所做的努力。
　Session 可以用 Cookie 来实现，也可以用 URL 回写的机制来实现。用 Cookie 来实现的
Session 可以认为是对 Cookie 更高级的应用。


- 两者比较
Cookie 和 Session 有以下明显的不同点：
		1） Cookie 将状态保存在客户端， Session 将状态保存在服务器端；
		2） Cookies 是服务器在本地机器上存储的小段文本并随每一个请求发送至同一个服务器。
		Cookie 最早在 RFC2109 中实现，后续 RFC2965 做了增强。网络服务器用 HTTP 头向客户端发
		送 cookies，在客户终端，浏览器解析这些 cookies 并将它们保存为一个本地文件，它会自
		动将同一服务器的任何请求缚上这些 cookies。 Session 并没有在 HTTP 的协议中定义；
		3） Session 是针对每一个用户的，变量的值保存在服务器上，用一个 sessionID 来区分是
		哪个用户 session 变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁
		用 cookie 时，这个值也可能设置为由 get 来返回给服务器；
		4）就安全性来说：当你访问一个使用 session 的站点，同时在自己机子上建立一个 cookie，
		建议在服务器端的 SESSION 机制更安全些.因为它不会任意读取客户存储的信息。


- Session 机制
			Session 机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就
		是使用散列表）来保存信息。
			当程序需要为某个客户端的请求创建一个 session 的时候，服务器首先检查这个客户端
		的请求里是否已包含了一个 session 标识 - 称为 session id，如果已包含一个 session id
		则说明以前已经为此客户端创建过 session，服务器就按照 session id 把这个 session 检
		索出来使用（如果检索不到，可能会新建一个），如果客户端请求不包含 session id，则
		为此客户端创建一个 session 并且生成一个与此 session 相关联的 session id，session id
		的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个 session id 将被
		在本次响应中返回给客户端保存。

-  Session 的实现方式
	   1. 使用 Cookie 来实现
		服务器给每个 Session 分配一个唯一的 JSESSIONID，并通过 Cookie 发送给客户端。
		当客户端发起新的请求的时候，将在 Cookie 头中携带这个 JSESSIONID。这样服务器能
		够找到这个客户端对应的 Session。
		流程如下图所示：
![](https://i.imgur.com/6WgqTBB.png)
	   2. 使用 URL 回显来实现
		URL 回写是指服务器在发送给浏览器页面的所有链接中都携带 JSESSIONID 的参数，这
		样客户端点击任何一个链接都会把 JSESSIONID 带会服务器。
		如果直接在浏览器输入服务端资源的 url 来请求该资源，那么 Session 是匹配不到的。
		Tomcat 对 Session 的实现，是一开始同时使用 Cookie 和 URL 回写机制，如果发现客户
		端支持 Cookie，就继续使用 Cookie，停止使用 URL 回写。如果发现 Cookie 被禁用，就一直
		使用 URL 回写。 jsp 开发处理到 Session 的时候，对页面中的链接记得使用
		response.encodeURL() 。
 
- 在 J2EE 项目中 Session 失效的几种情况
		1）Session 超时： Session 在指定时间内失效，例如 30 分钟，若在 30 分钟内没有操作，
		则 Session 会失效，例如在 web.xml 中进行了如下设置：
		<session-config>
			<session-timeout>30</session-timeout> //单位：分钟
		</session-config>
		2）使用 session.invalidate()明确的去掉 Session。

-  与 Cookie 相关的 HTTP 扩展头
		1） Cookie：客户端将服务器设置的 Cookie 返回到服务器；
		2） Set-Cookie：服务器向客户端设置 Cookie；
		3） Cookie2 (RFC2965)）：客户端指示服务器支持 Cookie 的版本；
		4） Set-Cookie2 (RFC2965)：服务器向客户端设置 Cookie。


- Cookie 的流程
服务器在响应消息中用 Set-Cookie 头将 Cookie 的内容回送给客户端，客户端在新的请
求中将相同的内容携带在 Cookie 头中发送给服务器。从而实现会话的保持。
流程如下图所示：
![](https://i.imgur.com/05OeCzo.png)