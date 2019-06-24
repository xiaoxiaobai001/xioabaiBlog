# 进程的相关知识 #

1. 定义
进程：一个正在运行的程序。

2. 相关函数
- 创建子进程 
	  fork（）
		（1）函数原型：pid_t fork()
		*pid_t是一个宏，其实质是一个整形，且是一个16位的整形（-32768-----32768），因此linux中可以创建的最大进程数为32768

		（2）fork的复制过程
			I：先申请一个pid（如果当前进程数已经达到了版本规定的上限，那么fork时将会出错）；
			II：先进行进程描述符（PCB）的复制，在接下来进行进程实体的复制
			III：然后进行写实拷贝
			*写实拷贝：内核并不需要复制整个进程地址空间，只有在写入的时候才进行资源的复制。而且它的操作是对页进行操作的

		（3）fork函数的简单应用
		  Eg：A.有如下代码段：
			int main(){
			pid_t pid1;
			pid_t pid2;
			pid1 = fork();
			pid2 = fork();
			} 
			a.执行这个程序后一共会产生几个进程？
			b.如果其中一个进程的输出结果是pid1：1001；pid2：1002其他进程的输出结果为?
				解：设由shell启动的进程为p0;
				I：当程序执行到pid1=fork()时，p0启动一个新进程p1,且p1的pid为1001；
				II：p0中的fork将1001返回给pid1,继续执行pid2=fork()，此时启动了一个新的进程为p2，p2的pid2为1002；
				III：p0中的第二个fork将1002返回给pid2。因此，pid1:1001,pid2:1002;
				IV：因为p2生成时p0中的pid1为1001，所以p2继承了p0中的pid1：1001，但是p2作为一个子进程它的返回值pid2:0,p2是从第二个fork后运行的，所以，pid1：1001，pid2：0；
				V：p1作为p0的一个子进程，在第一个fork之后将0返回给pid1,在进行第二个fork时，p1产生了一个新进程p3；p1中的第二个fork将p3的pid返回给pid2,由题可知pid2为1003，因此，p1进程 pid1:0  pid2:1003
				IV：p3作为p1的子进程继承p1中的pid1:0,而它本身也为一个子进程，所以 pid2 :0
				综上所述：执行这个程序后一共会产生4个进程，分别为：
					p0；
					p1：pid1:0   pid2:1003
					p2:  pid1:1001   pid2:1002
					P3:  pid1:0      pid2:0
		B.fork() && fork()会产生几个进程？
			解：当第一个fork为0，不执行第二个fork，则只有一个fork() && fork()；当第一个fork>0,执行第二个fork，则会出现两个fork() && fork()；所以总共会有3个进程产生；

		（4）vfork()
		特点：父子进程共享数据段，并且保证子进程先于父进程运行，在它调用exec或者_exit时，父进程才会被运行

- 子进程替换
	  exec()
		（1）函数的功能：调用它并没有产生新的进程，一个进程一旦调用exec()函数，它本身就死亡了。就好比鬼上身了一样，身体还是你的，但是灵魂和思想已经被替换了-----系统把代码段替换成新的程序的代码，在这其中唯一保留的就是进程的ID，对于系统而言，还是同一个进程，只不过是执行另一个程序罢了。
		***I.只有fork()和vfork()才能创建一个新的进程
		II.在使用exec()之前，首先要使用fork(),创建一个子进程，子进程调用exec()函数
		（2）exec()函数族
			I.说明：
	 	   int execl(const char *path, const char *arg, ...);
	        int execlp(const char *file, const char *arg, ...);
	        int execle(const char *path, const char *arg, ..., char * const envp[]);
	        int execv(const char *path, char *const argv[]);
	        int execvp(const char *file, char *const argv[]);
			int execve(const char *path, const char *arg[], char * const envp[]);
			int execl（const char *path,const char *arg,...）;
			
			tips：1.与execv函数的用法类似，只是在传递argv参数的时候每个命令行参数都声明为一个单独的参数（“.....”说明参数个数不确定），这些参数要以一个空指针结尾。
				  2.通过路径名方式调用可执行文件作为新的进程映像，参数pathname是将要执行的程序的路径名，参数file指文件名
				
			II.区别
				a.后面带p则取文件名做参数，其他不带则取路径名做参数；
				b.后面带l则分别传指针参数，带v则传指针数组（l表示list，v表示矢量vector）。函数execl、execlp和execle要求将新程序的每个命令行参数都说明为一个单独的参数，这中参数表以空指针结尾。而execv、execve和 execvp则要先构造一个指向各参数的指针数组，然后将该数组的地址作为这三个函数的地址。
				c.后面有e 则为带环境变量，没有则不带

3. 特殊的进程
	   A.僵死进程
		I.描述：父进程未结束，子进程结束并且父进程没有调用wait获取子进程的退出码（进程主体结束，PCB还存在)
		II.处理方法：
			a.程序调用signal(SIGCHLD,SIG_IGN)，来忽略SIGCHLD信号，这样子进程结束后会由内核释放资源
			b.对子进程的退出捕获他们的退出信号SIGCHLD,父退出信号时，在信号处理函数中调用waitpid()操作来释放他们的资源。
		
	   B.孤儿进程
		I.描述：父进程已经结束，而子进程还在继续。
		II.处理：孤儿进程会由进程号为1的init所收养，并且会为他们完成状态收集工作
	  
	   C.守护进程
		 I.描述:守护进程有称精灵进程，常常在系统启动时自启，仅在系统关闭时才终止，生存期比较长,一般都是在后台运行。
		 可通过 ps -axj 命令查看常用系统守护进程，其中最为常见的 init 进程，负责各运行层次间的系统服务。
		 II.守护进程编程规则 
			（1）首先调用umask(mode_t umask())函数将文件模式创建屏蔽字设置为0；
			（2）调用fork(),然后使父进程退出（exit）;
			（3）调用setsid（）创建一个新会话；
			（4）将当前目录更改为 根目录；
			（5）关闭不再需要的文件描述符；
			（6）某些守护进程打开/dev/null 使其具有文件描述符0，1，2，这样任何一个进程就不会产生其他不好的效果；
4. 信号
		1.概念：信号是系统响应某些条件而产生的一个事件，然后进程会相应的采
		取一些行动，信号可以产生，可以被接收。信号是系统预先定义好
		的某些特定的事件。信号的产生和接收的主体是进程。

		2.存储位置 系统信号定义：在文件/usr/include/bits/signum.h 中定义。
	
		3.处理方式
			1、默认处理，分以下五种方式
				term  终止进程
				Ign   忽略（层级不同）
				stop  暂停
				core  先终止进程然后生成core文件,死后验尸用于GDB调试
				cont  继续运行
			2、忽略
			3、自定义：用户自定义捕捉函数处理

		4.相关函数原型： 
		<1 int kill（pid_t pid, int sig）; 发送信号
			第一个参数Pid 设定给谁发送信号
			第二个参数sig 设定发送的信号类型

		<2 void (*signal(int sig, void ( *func )( int ) ) ) ( int );接受信号（修改信号的响应方式）
		
		注： Typedef void ( *sighandler_t )( int );
		Sighandler_t signal( int signum, sighandler_t handler);
		函数解析： Sig 指定信号类型， func 为函数指针，指定信号处理函数。
			设置默认响应方式：signal 函数中的第二个参数设置为 SIG_DFL 或者 0；
			设置忽略响应方式：signal 函数中的第二个参数设置为 SIG_IGN 或者 1；
		信号响应的主体是调用 signal 函数的进程，发送信号的进程仅仅是给了一个信号
		而已。
		Signal 使用时，第二个参数是函数指针，所以使用时只需要函数名，但在此不
		会调用此函数。
		
Signal 函数使用代码示例：
		#include <stdio.h>
		#include <stdlib.h>
		#include <unistd.h>
		#include <signal.h>
		#include <string.h>
		#include <assert.h>
		void fun(int sign)
		{
			printf("hello sig=%d\n", sig);
		}
		void main()
		{
			signal(SIGINT, fun);//注册信号处理函数
			while(1)
			{
				printf("hello\n");
				sleep(1);
			}
		}
		
Kill 函数使用示例：向指定进程发送信号
		#include <stdio.h>
		#include <stdlib.h>
		#include <unistd.h>
		#include <signal.h>
		#include <string.h>
		#include <assert.h>
		int main(int argc, char *argv[])
		{
			if(argc != 2)
			{
				printf("arg error\n");
				exit(0);
			}
			int id = 0;
			sscanf(argv[1], "%d", &id);//使 argv[1]中的内容以%d 的形式输入到 id 中
			if(kill(id, SIGINT) == -1)
			{
				perror("kill error\n");
			}
		}
		
5.进程状态转移图
　三态图：
![](https://i.imgur.com/aEt9aoT.png)

　五态图
![](https://i.imgur.com/wbMP4jm.png)