# OpenStack 基本组件 #
OpenStack 三大核心组件（网络，计算，存储）





- 1.Horizon (UI模块)  控制台,又名Dashboard

	web展示界面操作平台，方便用户交互的



- 2.keystone(身份服务模块)

	1）用户身份认证（Idemity）
	
	user：用户（租户下有很多用户，验证方式用户名密码，API keys等）
	
	kenant：租户（可以访问资源的集合）
	
	role：角色 (一组用户可以访问资源的权限)
	
	2）访问请求控制（Token）
	
	Service(nova,glance,swift等服务需要在keystone上注册)
	
	Endpoint(service暴露出来的访问地址)
	
	Token（访问资源的令牌，具有时效性）
	
	3)注册表服务(Catalog)
	
	openstack服务需要注册到keystone注册表中
	
	4)身份验证引擎（Policy）
	
	决定用户有哪些访问控制权限




- 3.Nova(计算服务组件)

![](https://i.imgur.com/LLYQVqj.png)


		openstack核心组件，核心服务包括：实例生命周期的管理（虚拟机），计算资源的管理，对外提供Restful API。
		
		Nova组件主要有三个模块构成（nova-api,nova-scheduler,nova-compute）,
		
		nova-api在表示层主要负责处理外部请求，nova-scheduler在逻辑控制层，主要负责选择那个主机创建VM，nova-compute虚拟机创建和资源分配，不提供虚拟化功能，但是支持kvm,LXC,xen等。
		
		三个组件通过rabbit MQ进行消息传递。



- 4.Glance(镜像服务组件)

![](https://i.imgur.com/3GYtfvY.png)

		主要功能：提供虚拟机镜像的存储，查询和检索功能，为nova进行服务，依赖于存储服务（存储镜像本身）和数据库服务（存储镜像相关的数据）。

- 5.Swift(对象存储服务模块)

![](https://i.imgur.com/yegWvDy.png)

		openstack核心组件，主要功能：高可用分布式对象存储服务，特点是无限和扩展没有单点故障。
		account-->container-->Object 某个账户下的某个容器的某个对象，可以通过HTTP(S),Object API,S3进行存取。



- 6.Cinder(块存储服务模块)

![](https://i.imgur.com/TEIlxpT.png)
		
	主要功能：管理所有块存储设备，为VM服务。
		cinder-api处理发送过来的请求，处理结果发送到rabbit MQ,通过消息中间件把所有请求发送到cinder-scheduler,通过调度器决定存储到哪里，并且创建VM，cinder-volume管理存储模块的生命周期。

- 7.Neutorn(网络服务组件)
		
		主要功能：为云计算提供虚拟的网络功能，为每个不同的租户建立独立的网路环境。
		三种不同的网络模式（Flat模式 Flat DHCP模式，Vlan模式）



- 8.Ceilometer(监控服务组件)

		Ceilometer 的目标是 计量 Metering 方面，为上层的计费、结算或者监控应用提供统一的资源使用数据收集功能。

详情见博客： https://www.cnblogs.com/jingtyu/p/6379490.html