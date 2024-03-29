# Linux 文件类型含义 #
- 目录文件含义
		　/bin（binary file）： 存放用户最常使用的命令的可执行文件。　例如：ls、chmod
		　/sbin（system binary）：存放基本的系统和系统维护的命令的可执行文件。　例如：shutdown、init
		　/etc : 存放系统需要使用的系统配置文件。　例如： passwd、 group、 inittab
		　/boot: 存放系统启动需要的文件，包括系统引导程序和系统核心部分。
		　/root： 这个是系统管理员（超级用户 root）的主目录。
		　/home:：这个目录是所有普通用户的家目录，在这个目录下，会以各个用户名命
		名的相对应的子目录，用户的文件一般存放在这个目录之下。
		　/usr：这个目录是“unix system resource”的简称，其中存放用户使用的命令、
		程序库、文档和其他文件。　例如：C 语言所使用的头文件等。
		　/proc：这个目录是一个虚拟目录，其中存放以进程为单位的内存的映射，其中
		主要是以进程号命名的各个子目录。
		　/var 放置系统执行过程中经常变化的文件，比方说各种服务的日志文件
		　/mnt 系统提供这个目录是让用户临时挂载其他的文件系统
		　/dev 设备特殊文件
		　/lib 标准程序设计库，又叫动态链接共享库，作用类似windows里的.dll文件
		　/tmp 一般用户或正在执行的程序临时存放文件的目录,任何人都可以访问,重要数据不可放置在此目录下
		　/lost+found这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里

- 系统文件类型

1.  普通文件（ 文档文件 ）
　狭义的文件，普通的文档文件。属性的第一个字符是 -

2. 目录文件
　Linux 下的目录文件就是 windows 下的文件夹，目录是一种特殊的文件，用来维护文件系统的层次结构，目录记录它所包含的文件、子目录以及与它相关的信息。
　一个目录文件是由一个索引节点描述的，在这个索引节点数据块中存放用来描述该目录下的所有目录项，在 Linux 中，/ 是系统的根目录。属性的第一个字符是 d 

3. 管道文件
　管道可以将一个进程的数据连接使其流入另一个进程。即将一个进程的输出作
为另一个的输入。管道分命名管道和无名管道。属性的第一个字符是 p 

4. 链接文件
　在 linux 系统中，内核为每一个新创建的文件分配一个 inode 号（索引节点），
文件属性保存在索引节点里，在访问文件时，索引节点被复制到内存里，从而实现文
件得快速访问。
　链接文件分为两种：软链接和硬链接
　软链接：类似于 windows 上的快捷方式，软链接文件和源文件有不同的 inode号，所以软链接用一个新的 inode 存储源文件的 block，这个信息比较小，所以软链接文件大小很小。如果删除源文件，则磁盘上的文件会被删除，所以软链接不能再访问文件。
　硬链接：硬链接有两个限制：一、不能对目录文件创建硬链接。二、只有在同
一文件系统中的文件之间才可以创建链接。
	　**软链接和硬链接的不同点是：**
	　　1.软链接文件可以链接不存在的文件，可以对目录文件创建软链接，甚至可以循环链接自己。类似于编程语言中的递归；硬链接文件不能对目录文件创建硬链接，更不能链接不存在的文件。
	　　2.软链接文件能够跨越文件系统；硬链接文件只有在同一文件系统中的文件之间才可以创建链接。
	　　3.软链接文件只是其源文件的一个标记，当删除了源文件后，链接文件不能独立存在，虽然仍保留文件名，但却不能查看软链接文件的内容了；硬链接与源文件有相同的 inode 号，但是创建一个硬链接，其 inode节点中的链接数会随之增加，删除的时候，链接数随之减小。所以删除源文件后，硬链接还可以继续使用。
	　**软链接和硬链接的相同点是**：无论修改链接文件还是源文件的内容，链接文件
	和源文件会同步更改。
	　创建软连接命令： ln -s 源文件 链接文件
	　创建硬链接命令： ln 源文件 链接文件
	　属性的第一个字符是 l 

5. 设备文件
		linux 中的设备分为：
		　(1)块设备,它是可寻址，寻址以块为单位，块大小随设备不同而不同，并且支持
		对数据的随机访问，如硬盘，蓝光光碟，光驱等设备。属性的第一个字符是 b
		　(2)字符设备，不可寻址，仅提供数据的流式访问，即一个个字符或者一个个字
		节。字符设备如：键盘、鼠标、打印机等。属性的第一个字符是 c
		　(3)网络设备，提供对网络的访问，这是通过一个物理适配器和和特定的协议（如
		IP协议）进行的
		　这个种类的文件，是用mknode来创建，用rm来删除。目前在最新的Linux发行版本中，我们一般不用自己来创建设备文件。因为这些文件是和内核相关联的。

6. 套接口文件
　当我们启动MySQL服务器时，会产生一个mysql.sock的文件。属性的第一个字符是 s
