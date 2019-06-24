# 多线程 #


-  **线程库函数 **

1. 线程的创建：pthread_create()
		int pthread_create(pthread_t *thread,const pthread_attr_t *attr,void *(*start_routine)(void *),void *arg0);
	      第一个参数是为类型为pthread_t类型的线程id
		  第二个参数为属性，一般设置为NULL
		  第三个参数就是函数指针
		  最后一个参数是要传递给参数3的函数参数。
		  返回值：如果成功，返回0，如果失败，返回-1
2. 退出当前线程：pthread_exit()
		void pthread_exit(void *retval)；
3. int pthread_join(pthread_t thread, void **retval);
		合并线程，等待某个线程结束，并接受它返回给主线程的信息
  	 	 第一个参数是要等待的线程的id
			第二个参数用来接收等待的线程结束时传来的退出信息的。
			返回值：如果成功，函数返回0，否则返回一个错误的值。
tips:   exit()退出进程，当前所有的线程都将结束
             
                 

- **线程同步**
1.信号量
   	 信号量只能在线程间使用，是进程内部局部信号量，与进程间通信的信号量不同。头文件为semaphore.h
      a.方法：
         1) sem_init():初始化信号量，赋初值
             int sem_init(sem_t *sem,int pshared,unsigned int value)
               第一个参数是信号量的指针，是我们定义的信号量
			   第二个参数指明信号量是由进程内线程共享，还是由进程之间共享。如果 pshared 的值为 0，那么信号量将被进程内的线程共享，并且应该放置在这个进程的所有线程都可见的地址上(如全局变量，或者堆上动态分配的变量)。如果 pshared 是非零值，那么信号量将在进程之间共享，并且应该定位共享内存区域。因为通过 fork 创建的孩子继承其父亲的内存映射，因此它也可以见到这个信号量。所有可以访问共享内存区域的进程都可以用 sem_post、sem_wait等等操作信号量。
			   第三个参数就是初始化信号量的值。
         2) sem_wait():等待信号量变得可用，也就是p操作，减1
             int sem_wait(sem_t *sem)
             参数是等待要获得信号量，获得信号量表示拿到资源，其他线程不能再进入临界区，如果成功返回0，失败返回-1。
         3) sem_post():v操作，加1
             int sem_post(sem_t *sem)
             参数是释放信号量，结束对临界区的访问，即v操作，临界资源加1，如果成功，返回0，失败返回-1。
         4) sem_destroy():销毁
             int sem_destroy(sem_t *sem)
             参数是销毁信号量的指针，如果成功返回0，失败返回-1。
2.互斥量
	  互斥量的声明类型为pthread_mutex_t
        a. 方法：
            1) int pthread_mutex_init(pthread_mutex_t *mutex,const pthread_mutexattr_t *attr)
                 第一个参数是由我们定义的互斥量的指针（地址）
				 第二个参数为互斥量的属性，一般设为0
				 返回值:如果成功，返回0，否则，返回一个错误的值。
            2) int pthread_mutex_lock(pthread_mutex_t *mutex)
                  参数是初始化时的互斥量的指针（地址），如果成功，返回0，失败返回错误的值。
            3) int pthread_mutex_unlock(pthread_mutex_t *mutex)
                  参数是初始化时的互斥量的指针
            4) int pthread_mutex_destroy(pthread_mutex_t *mutex)
                  参数是初始化时的互斥量的指针
3.消息队列
      消息队列也是线程间同步的一种方式，头文件为#include<sys/msg.h>
       a. 方法：
         1) int msgget(key_t key,int msgflg)
                第一个参数是一个类型为key_t 的一个值，这里可以用整型数字强制转换类型作为实参传递
				第二个参数是权限标志，一般为IPC_CREAT | 0666，如果给出的是一个已有的消息队列，并不会产生错误，而是忽略掉IPC_CREAT标志，而使用已有的消息队列。
				返回值：成功返回一个整型值，是队列标志符，失败返回-1。
         2) int msgsnd(int msqid,const void *msg_ptr,size_t msg_sz,int msgflg)
            用来将消息添加到队列中，消息的结构需要自己定义一个结构体：
              struct my_message{
                  long int message_type;
                   ......        //自己的数据结构
                       }
             由于在消息的接受中要用到message_type，所以不能忽略，在声明结构体时必须包含它，之后还要将它初始化。
              第一个参数是由msgget函数返回的消息队列标识符
			  第二个参数是一个指向准备发送消息的指针，消息必须以上述结构体中的长整型成员变量开始，即要传入结构体指针。
			  第三个参数为指向消息的长度，这个长度不包含结构体中长整型成员变量的长度，也就是要传入自己定义的数据类型的长度。
			  第四个参数为当前消息队列满或队列消息到达系统的限制范围时将要发生的事，如果设为IPC_NOWAIT，函数将立刻返回，不发送消息并且返回-1，如果该标志被清除，那么发送进程将挂起以等待队列中腾出空间。
			  返回值：成功函数返回0，失败返回-1，如果调用成功，消息数据的一份副本将被放到消息队列中。
          3) int msgrcv(int msqid,void *msg_ptr,siz_t msg_sz,long int msgtype,int msgflg)
               该函数从一个消息队列中获取消息。前3个参数意义同上。
			   第四个参数是一个长整型，是一个简单的接收优先级，如果值为0，就代表获取队列中的第一个可用消息，如果它的值大于0，将获取具有相同消息类型的第一个消息，如果它的值小于0，将获取消息类型等于或小于msgtype的绝对值的第一个消息。
			   第五个参数意义与上面第四个参数意义相同。
          4) int msgctl(int msqid,int command,struct msgqid_ds *buf)
                msqid_ds结构至少包含以下成员：
                  struct msqid_ds{
                       uid_t msg_perm.uid;
                       uid_t msg_perm.gid;
                       mode_t msg_perm.mode;
                      }
              第一个参数是由msgget返回的消息队列标识符。
			  第二个参数是将要采取的动作。
              返回值：成功时它返回0，失败时返回-1，如果删除消息队列时，某个进程正在msgsnd或msgrcv函数中等待，这两个函数将失败。
        

- 多线程中执行fork()
	  1.多线程执行fork一个进程时，只执行fork所在线程的执行路径上的事情。
	  2. 如果父进程中用了锁或信号量时，而fork出来的子线程会复制时父进程的锁的状态，即父子进程各有一把锁，且子进程中的锁的状态时复制那一刻的父进程的状态，但我们无法得到该锁的状态，所以需要用一个函数atfork()来等待父进程放开锁时进行fork复制，此时复制出的子进程的锁的状态就是放开锁的状态。
       