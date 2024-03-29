# HTTP状态码和请求方法 #


![](https://i.imgur.com/wswKUI9.png)




1**：请求收到，继续处理

	100——客户必须继续发出请求
	101——客户要求服务器根据请求转换 HTTP 协议版本


2**：操作成功收到，分析、接受

	200——交易成功
	201——提示知道新文件的 URL
	202——接受和处理、但处理未完成
	203——返回信息不确定或不完整
	204——请求收到，但返回信息为空
	205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
	206——服务器已经完成了部分用户的 GET 请求

3**：完成此请求必须进一步处理

	300——请求的资源可在多处得到
	301——删除请求数据
	302——在其他地址发现了请求数据
	303——建议客户访问其他 URL 或访问方式
	304——客户端已经执行了 GET，但文件未变化
	305——请求的资源必须从服务器指定的地址得到
	306——前一版本 HTTP 中使用的代码，现行版本中不再使用
	307——申明请求的资源临时性删除

4**：请求包含一个错误语法或不能完成

	400——错误请求，如语法错误
	401——未授权
	HTTP 401.1 - 未授权：登录失败
	HTTP 401.2 - 未授权：服务器配置问题导致登录失败
	HTTP 401.3 - ACL 禁止访问资源
	HTTP 401.4 - 未授权：授权被筛选器拒绝
	HTTP 401.5 - 未授权： ISAPI 或 CGI 授权失败
	402——保留有效 ChargeTo 头响应
	403——禁止访问
	HTTP 403.1 禁止访问：禁止可执行访问
	HTTP 403.2 - 禁止访问：禁止读访问
	HTTP 403.3 - 禁止访问：禁止写访问
	HTTP 403.4 - 禁止访问：要求 SSL
	HTTP 403.5 - 禁止访问：要求 SSL 128
	HTTP 403.6 - 禁止访问： IP 地址被拒绝
	HTTP 403.7 - 禁止访问：要求客户证书
	HTTP 403.8 - 禁止访问：禁止站点访问
	HTTP 403.9 - 禁止访问：连接的用户过多
	HTTP 403.10 - 禁止访问：配置无效
	HTTP 403.11 - 禁止访问：密码更改
	HTTP 403.12 - 禁止访问：映射器拒绝访问
	HTTP 403.13 - 禁止访问：客户证书已被吊销
	HTTP 403.15 - 禁止访问：客户访问许可过多
	HTTP 403.16 - 禁止访问：客户证书不可信或者无效
	HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
	404——没有发现文件、查询或 URl
	405——用户在 Request-Line 字段定义的方法不允许
	406——根据用户发送的 Accept 拖，请求资源不可访问
	407——类似 401，用户必须首先在代理服务器上得到授权
	408——客户端没有在用户指定的饿时间内完成请求
	409——对当前资源状态，请求不能完成
	410——服务器上不再有此资源且无进一步的参考地址
	411——服务器拒绝用户定义的 Content-Length 属性请求
	412——一个或多个请求头字段在当前请求中错误
	413——请求的资源大于服务器允许的大小
	414——请求的资源 URL 长于服务器允许的长度
	415——请求资源不支持请求项目格式
	416——请求中包含 Range 请求头字段，在当前请求资源范围内没有 range 指示值，
	求也不包含 If-Range 请求头字段
	417——服务器不满足请求 Expect 头字段指定的期望值，如果是代理服务器，可能是
	一级服务器不能满足请求长。

5**：服务器执行一个完全有效请求失败

	HTTP 500 - 内部服务器错误
	HTTP 500.100 - 内部服务器错误 - ASP 错误
	HTTP 500-11 服务器关闭
	HTTP 500-12 应用程序重新启动
	HTTP 500-13 - 服务器太忙
	HTTP 500-14 - 应用程序无效
	HTTP 500-15 - 不允许请求 global.asa
	Error 501 - 未实现
	HTTP 502 - 网关错误



![](https://i.imgur.com/2kzaZEJ.png)