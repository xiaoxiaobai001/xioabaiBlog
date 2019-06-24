# libevent库 #

1. 简介
	   libevent框架:高性能IO框架库，开源的，跨平台的，轻量级的网络IO库
	   libevent实例应包含的功能：注册，注销以及事件循环

   　　我们将文件描述符，读写事件以及回调函数注册到libevent实例中，当有事件发生时，IO复用函数负责通知该实例去管理调用注册的回调函数，而这些都是由这个框架完成的，我们不需要关心，只需要将所需要的信息注册到libevent实例中就可以了。

2. 包含组件
		1.句柄：需要文件描述符
		2.事件多路分发器：相当于IO复用函数，用于收集有事件发生的文件描述符，并通过关注信号事件队列，定时器事件队列以及IO事件队列，一旦哪个队列有事件发生，则调用相应回调函数
		3.事件处理器和具体的事件处理器：事件处理器执行对应的业务逻辑，包含一个或多个handle_event 回掉函数。事件描述符是所有各类事件发生的处理，而具体事件发生器是这各类事件中一个具体事件的处理流程。
		4.Reactor核心库：是I/O 框架库的核心，它提供的几个主要方法：
			<1 handle_events 事件循环 重复执行以下过程：等待事件，然后依次处理所有就绪事件对应的事件处理器
			<2 register_handler  调用事件多路分发器的 register_event 方法来往事件多路分发器中注册一个事件
			<3 remove_handler 调用事件多路分发器的 remove_event 来删除事件多路分发器中的一个事件

3. 使用安装libevent库的步骤

	    1.下载红帽系统中：库：/usr/lib    头文件：/usr/include
	    2.解压：tar zxf
	    3.make ->makefile编译：
	       生成Makefile文件： ./configure --prefix=/usr   
	       查看是否已有Makefile文件：ls | grep makefile
	       编译文件：make          
	    4.然后切换到管理，执行make install
	    5.验证 是否安装成功：ls /usr/lib | grep libevent
	    6.写代码测试

	tips:如何使虚拟的操作系统与物理机的操作系统通信安装VMvare Tools
		1.我的电脑里选择操作系统右键安装VMvare Tools
		2.桌面出现VM_TOOL的安装包，解压缩
		3.管理员用户运行可执行程序

4. 主要逻辑及相关函数
	   1.struct event_base * base=event_init();//调用 event_init 函数创建 event_base 对象，一个 event_base 相当于一个Reactor实例。
	   2.struct event * sig_ev=event_new(base,SIGINT,EV_SIGNAL | EV_PERSIST,sig_cb,NULL);//创建具体的事件处理，并设置它们从属的Reactor实例
	    函数原型如下：
		struct event * event_new(struct event_base,evutil_socket_t fd, short events,void(*cb)(evutil_soccket_t,short ,void *),void *arg) 
		 第一个参数是libevent实例地址;
		 第二个参数关心的事件队列（相当于文件描述符）；
		 第三个参数为指定事件类型;
		 第四个参数为要注册的回调函数;
		 第五个参数为第四个参数中回调函数的参数；
		 返回值：成功时返回一个event 类型的对象，也就是事件处理器
	   3.event_add(sig_ev,timeout);//将第一个参数中注册关注的信息添加注册到libevent库中
	   4.event_base_dispatch(base)；//启动事件循环，io复用方法就封装在这个函数中
	   5.event_free(sig_ev);//释放我们注册事件的那段空间
	   6.event_base_free（base);//释放libevent实例
tips: libevent支持事件类型如下图:
![](https://i.imgur.com/tXHFUua.png)
EV_PERSIST作用：事件被触发后，循环调用event_add函数

5. 退出事件循环的方法
	    1.调用主动退出的函数event_base_loop_exit或者用户主动退出。
	    2.预先定义的事件均被触发，没有事件可以做时退出。
		　　　　　　　　tips:面试问题：事件循环是怎么做的（需要看源代码）

6. 代码示例

<pre>
#include < stdio.h>
#include < stdlib.h>
#include < event.h>
#include < assert.h>	
#include < signal.h>
#include < string.h>
#include < unistd.h>
#include < time.h>
void sig_cb(int fd,short ev,void * arg)
{
   printf("recv sig:%d\n",fd);
}
void time_cb(int fd,short ev,void *arg)
{
  printf("time out\n");
}
int main()
{
   struct event_base * base=event_init(); //调用 event_init 函数创建 event_base 对象，一个  event_base 相当于一个Reactor实例。
   assert(base!=NULL);
   struct event * sig_ev=evsignal_new(base,SIGINT,sig_cb,NULL); //创建信号事件处理器
   assert(sig_ev!=NULL);
   event_add(sig_ev,NULL);
   struct event * time_ev=evtimer_new(base,time_cb,NULL); //创建定时时间处理器
   assert(time_ev!=NULL);
   struct timeval tv={5,0};
   event_add(time_ev,&tv);
   event_base_dispatch(base);
   event_free(sig_ev);
   event_free(time_ev);
   event_base_free(base);
}</pre>
tips: evsignal_new 和 evtimer_new 的统一入口函数 event_new()
<pre>
#include < stdio.h>
#include < stdlib.h>
#include < unistd.h>
#include < string.h>
#include < assert.h>
#include < event.h>
#include < signal.h>
#include < time.h>
#include < sys/socket.h>
#include < netinet/in.h>
#include < arpa/inet.h>

int create_socket();
void io_cb(int fd, short ev, void * arg)
{
    if ( ev & EV_READ )
    {
        char buff[128] = {0};
        if ( recv(fd,buff,127,0) <= 0 )
        {
            //event_free();
            //close(fd);
            printf("one client close\n");
            return ;
        }
        else
        {
            printf("buff=%s\n",buff);
            send(fd,"ok",2,0);
        }
    }
}
void accept_cb(int fd, short ev, void * arg)
{
    struct event_base * base = (struct event_base*)arg;
    if ( EV_READ & ev )
    {
        struct sockaddr_in caddr;
        int len = sizeof(caddr);

        int c = accept(fd,(struct sockaddr*)&caddr,&len);
        if ( c < 0 )
        {
            return ;
        }

        printf("accept c=%d\n",c);
        struct event * io_ev = event_new(base,c,EV_READ|EV_PERSIST,io_cb,NULL);
        event_add(io_ev,NULL);

    }
}
int main()
{
    int sockfd = create_socket();
    assert( sockfd != -1 );

    struct event_base * base = event_init();
    assert( base != NULL );

    struct event * sock_ev = event_new(base,sockfd,EV_READ | EV_PERSIST,accept_cb,base);
    event_add(sock_ev,NULL);

    event_base_dispatch(base);

    event_free(sock_ev);
    event_base_free(base);
}
int create_socket()
{
    int sockfd = socket(AF_INET,SOCK_STREAM,0);
    if ( sockfd == -1 )
    {
        return -1;
    }
    
    struct sockaddr_in saddr;
    memset(&saddr,0,sizeof(saddr));
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(6000);
    saddr.sin_addr.s_addr = inet_addr("192.168.31.120");

    int res = bind(sockfd,(struct sockaddr*)&saddr,sizeof(saddr));
    if ( res == -1 )
    {
        return -1;
    }

    listen(sockfd,5);

    return sockfd;
}</pre>