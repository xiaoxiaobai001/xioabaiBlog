# linux基本命令 #
- 基本命令： ls   cd  cp  rm  mv  more  cat  head  vi   touch  mkdir
      ls 命令：ls  [option]   [pathname] 显示给定目录中的文件
       后面两个参数可选，单独的ls命令显示出当前目录下的文件
         ls  -a   显示当前目录下所有的文件，包括隐藏文件
         ls  -l    显示当前目录下的文件的详细信息。等同于ll命令
			具体为：文件总共大小（以k为单位）- 文件类型（-为普通文件 d为目录文件 p为管道文件 l为链接文件）
			和文件权限- 链接数- 创建者用户名- 组用户名- 文件大小（以字节单位）- 最后修改时间- 文件名
         ls  -al   综合上面的两个参数的功能
         ls  -i    显示当前目录下文件所对应的inode节点号 
      上述命令都可以加上第二个参数，显示给定目录下对应的内容

      cd 命令：cd  [pathname]  切换工作目录到指定的路径下
             pathname可以是相对路径也可以是绝对路径
             绝对路径：  从根目录开始指定目录所在的全路径， 以/开始的都是绝对路
                         径，例如：/home/username/Desktop
             相对路径：  相对于当前工作目录的路径 例如：../dvd
             pathname 也可以是特殊字符：~   -  ..
             ~ 进入当前登录用户的家目录
             - 在上一次工作路径与当前工作目录间切换
             ..  进入当前目录的上一级目录，即当前目录的父目录  

      cp 命令：cp  [filename]  [pathname] 将filename文件拷贝到pathname指定的目
                 录下， pathname后可以加文件名，实现拷贝并重命名文件

      mv 命令：mv  [filename]  [pathname]  将filename文件移动到pathname指定的
                 目录下pathname后可以加文件名，实现移动并重命名文件，如果
                 pathname也仅仅是个文件名，则实现重命名文件。

      touch 命令： touch  [newfile]  创建普通文件，创建文件时必须指定文件的扩展
                 名，例如： touch main.c   touch log.txt

      mkdir 命令： mkdir  [newdir]   创建目录文件。例如： mkdir test 和 mkdir dvd
                 mkdir 创建的目录都是空目录，里面只包含了 “. ” 和 “..” 两个
                 目录，其中“.”代表当前目录   “..”代表上一级目录（父目录）

      rm 命令： rm  filename  删除普通文件。
      rmdir 命令： rm  dirname  删除空目录文件
      rm  -r  ： 删除非空目录 

	  clear 命令： 清屏

      vi或vim命令： vim  filename  编辑文件内容

- 文件操作
	  Vi的三种模式：命令模式 插入模式 末行模式
	  三种模式的切换:1.命令模式 Esc 2.插入模式 i o a  3.末行模式 :  /
	  命令模式转到插入模式的操作：(在执行以上操作时要注意事先应处于命令模式)
		i 在当前光标所在位置前面插入
		a 在当前光标后面插入
		o(小写)在当前光标的下一行插入新的一行。
		O(大写)在当前光标的上一行插入新的一行。


	1. 末行操作：
		在命令模式下输入  ‘/’ ‘:’ ‘?’可切换到末行模式
		显示行号：set number /set nonumber 简写 ：set nu / set nonu
		替换字符：n,$s/oldchar/newchar/ g全文替换(从第n行开始替换，不加g每行只替换第一次遇到的，加g所有的都替换)
		:n光标停在第几行
		J 将光标所在下一行和该行组成一行.
		/搜索从当前往下搜索
		?从当前往上搜索
		：w 存盘  :w newfile 另存为 :wq保存退出  :q!强制退出  :q退出
	2.命令模式下的操作
	  shift+4 将光标移动到行尾   shift+6 将光标移动到行头  
		输入n,再输入shift+g 将光标移动到第n行
		shift+g将光标移动到文件末尾 shift+l将光标移动到屏幕底 
		shift+h 将光标移动到屏幕头 shift+m 将光标移动到屏幕中间
		ctrl+f向下翻页 ctrl+b向上翻页 
	  <1. 复制粘体删除操作:
		dd删除一行  ndd删除n行,使用 dw 命令是删除一个单词 单词是用空格区辨的。  
		yy拷贝行，nyy拷贝n行。 p粘贴 
	  <2. 撤销操作:u每按一次撤销一次最近的操作	
- 查看文件内容  　head 　 more 　tail　less 
  	  more 和 less命令其后直接跟文件名，可查看文件内容，more 和less 都默认显示一屏，more不可以上下滚动，less可以
		head -num filename 和 tail -num filename 查看文件前 num 或者后 num 行内容
		cat命令： cat  a.c  b.c  >  c.c  将文件 a.c b.c 中的内容合并到文件 c.c 中
          cat  filename  将文件filename 中的内容完全显示到终端上 
		q 退出

- 查找： find　grep
        find 命令 :  find pathname  罗列出给定目录下所有文件的路径以及名称
        grep 命令： grep string   根据string过滤出相应的内容  参数：-i 不区分大小写

- 进程： ps　kill　pkill　bg　fg　jobs　& Ctrl+z Ctrl+c
      前台执行：占据终端的控制权，进程没执行完，提示符不输出，无法解析此进程执行期间输入的命令
      后台执行：不占据终端的控制权，进程执行过程不影响终端的工作，输出提示符并且正常解析接下来输入的命令。
        & 用法：exe &  运行进程exe，并将其放到后台执行
      ps命令： 显示当前终端上运行的进程信息
               ps -f  显示当前终端上运行的进程的详细信息
               ps -ef  显示系统上运行的所有进程的详细信息
      kill命令：kill  pid  关闭进程号为pid的进程
                 kill  -stop pid  挂起进程号为pid的进程 
      pkill命令： pkill  cmd(进程名字)  批处理关闭所有cmd执行起来的进程
      jobs命令：显示当前终端的任务信息
      bg命令：  bg  jobid   将挂起的任务号为jobid的进程放到后台执行
      fg命令：  fg  jobid   将挂起的或者后台运行的任务号为jobid的进程放到前台执行
	  Ctrl+z 命令 功能：当前进程转为后台，并使之挂起（暂停）
	  Ctrl+c 命令 功能：强制终止当前进程

- 状态　top　ipcs 
		top 命令：动态显示系统上所有进程的执行情况以及资源的使用情况     
		ipcs  查看共享内存和信号量    ipcrm -s uid   -s 删除信号量   -m 删除共享内存

- 用户管理　useradd　passwd　userdel　usermod
		1)useradd命令
		（也可以使用adduser）用来创建新的用户帐号，其命令格式如下：
		〔root@localhost root〕# useradd user1
		 useradd命令常用选项
			-d 设置新用户的登陆目录
			-e 设置新用户的停止日期，日期格式为MM/DD/YY
			-f 帐户过期几日后永久停权。当值为0时帐号则立刻被停权。而当值为-1时则关闭此功能。预设值为-1
			-g 指定新用户所属的主组
			-G 指定新用户所属的副组。每个组之间使用逗号“，”隔开，不可以夹杂空白字
			-s 指定新用户的登陆Shell
			-u 设定新用户的ID值
		
		成功创建一个新用户以后，在/etc/passwd文件中就会增加一行该用户的信息，其格式如下：
		（用户名〕：〔密码〕：〔UID〕：〔GID〕：〔身份描述〕：〔主目录〕：〔登陆Shell〕
		
		新创建的用户没有密码，所以不能够登陆系统，必须使用passwd命令设定新用户的密码
		
		2）设置和修改用户口令　passwd
		      〔root@localhost root〕# passwd user1
		因为只有root用户可以修改密码，所以修改密码时不需要知道原来的密码，只需要输入两次相同的新密码即可，并且密码的输入是不回显的。
		
		3)删除用户 userdel
		〔root@localhost root〕# userdel   user1

		4）修改用户信息  usermod
		〔root@localhost root〕# usermode  -g group1 user1
		修改用户信息，其参数和useradd相似



- 文件压缩解压
		文件打包： tar cvf 文件名.tar + 压缩的文档   (c:创建文件 f:制定目标为文件而不是设备 v:列出打包详细过程)
		    压缩： gzip 文件名.tar
		文件解压：gzip -d 文件名.tar.gz
		    解包：tar xf 文件名.tar     x 释放文档   t 显示包里的文件而不真正释放
		文件一步压缩  tar zcvf 文件名.tgz + 压缩的文档
		    一步解压  tar zxvf 文件名.tgz  


- gcc 使用
	  两步完成编译链接工作： gcc - c *.c 　gcc -o main *.o
	  一步完成编译链接工作： gcc -o main *.c
	  -L 指定路径 -l 指定库名   -lpthread编译线程库函数

- 源代码到可执行程序的过程
		->预编译 gcc - E main.c -o main.i  头文件引入  删除注释  宏定义替换  处理所有预编译命令 增加行号和文件名标志
		->编译   gcc - S main.i -o main.s 词法分析 语法分析 语义分析  中间语言生成 目标代码生成及优化 （生成汇编代码文件）
		->汇编   gcc -c main.s -o main.o or as main.s -o 将汇编文件生成二进制文件  main.o
		->链接   gcc main.o -o main  地址和空间分配、符号决议（符号绑定）、重定位
		->运行  ./main
-  gdb 使用
　1. debug版本：可调式版本 （文件较大） 编译阶段决定是否生成debug版本   
		eg:gcc -o main*.o -g --->release   gcc -o *.o--->debug     gcc -o exe *.c -g  --->debug
　2. release：发行版本 （文件较小）   
		gdb  main 调试    
		list 显示主函数所在.c文件的代码
		list filename:num   显示指定.c文件的代码
		b +行号（N）     将断点添加到最近一次显示的文件的第N行上
	    b +funtion_name  将断点添加到指定函数执行的第一行代码处
		b +代码行号  if（条件）  添加条件断点
		bt 查看函数调用堆栈关系
		info b  查看断点信息
		disable +断点号    暂停,断点不起作用   enable+断点号   断点继续起作用
		d + 断点号   删除断点  (delete 1)
		r  运行程序  (run)
		p +变量名  查看变量值
		p +数组名 显示数组内容   p+ &变量名  显示数组的地址   p *指针名@len 通过指针显示数组元素或者堆区空间的值   p+变量名.成员变量名 显示结构体变量的成员变量值
		n  下一行执行（next）
		c  执行到下一个段点处（continue）
		s 进入将要被调用的函数里执行
		finish 退出函数
	    display +变量名  以展台显示变量的值
		info +display 查看display信息   
		undisplay num  退出显示
	    x /n  f（四个字节显示） b（一个字节显示）  查看内存中的值
		ptype +变量名   查看变量类型
		q  退出gdb调试（quit）

		进程
			set follow-fork-mode mode (mode 的值可选项为parent和child)  跟踪父进程或子进程	
		线程	
			info threads 显示当前线程
			thread n 切换到第n个线程（注：n是线程编号）
			set scheduler-locking [off/on/step] off 表示不锁定任何线程 on 表示只当前被调试的线程会继续执行 step 表示在单步执行的时候，只有这个线程会继续执行

- 更改文件的属性
		1.chmod 746 wang.txt
		2.chmod u(g/o) +(-) x wang.txt
		3.chmod a=rwx wang.txt
- 	关机命令:
		1. shutdown
				-h 0(时间s)　0s后立即关机   -c 取消之前的关机命令  -k 并不关机，给出警告信息
		2. halt 关机
		3. reboot 重启		

- 下载命令
	
1. curl命令　　curlURL地址　eg:curl http://man.linuxde.net/text.iso --silent -O  
		curl命令可以用来执行下载、发送各种HTTP请求，指定HTTP头部等操作。curl是将下载文件输出到stdout，将进度信息输出到stderr
		-silent 选项不显示进度信息使用-。	
		-O 将下载的数据写入到文件，必须使用文件的绝对地址
		-C 断点续传 curl能够从特定的文件偏移处继续下载，它可以通过指定一个便宜量来下载部分文件： curl URL/File -C 偏移量 #偏移量是以字节为单位的整数
		-C -URL  curl自动推断出正确的续传位置

2. wget命令　wget URL地址　eg:wget http://www.linuxde.net/testfile.zip
		-O 下载并以不同的文件名保存 wget -O wordpress.zip http://www.linuxde.net/download.aspx?id=1080
		-c 断点续传 重新启动下载中断的文件，对于我们下载大文件时突然由于网络等原因中断非常有帮助，我们可以继续接着下载而不是重新下载一个文件。

- 软件安装方式
　rpm 包和deb 包是两种Linux系统下最常见的安装包格式，rpm包主要应用在RedHat系列包括 Fedora 等发行版的Linux系统上，deb包主要应用于Debian系列包括现在比较流行的Ubuntu等发行版上。 
　yum可以用于运作rpm包，例如在Fedora系统上对某个软件的管理：

	1. yum常用命令
			1.列出所有可更新的软件清单命令：yum check-update
			2.更新所有软件命令：yum update
			3.仅安装指定的软件命令：yum install <package_name>
			4.仅更新指定的软件命令：yum update <package_name>
			5.列出所有可安裝的软件清单命令：yum list
			6.删除软件包命令：yum remove <package_name>
			7.查找软件包 命令：yum search <keyword>
			8.查看 yum 仓库命令： yum repolist
			9.清除缓存命令:
				yum clean packages: 清除缓存目录下的软件包
				yum clean headers: 清除缓存目录下的 headers
				yum clean oldheaders: 清除缓存目录下旧的 headers
				yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的headers

	apt-get可以用于运作deb包，例如在Ubuntu系统上对某个软件的管理：
	
	2. 常用的APT命令参数
			apt-cache search package  搜索包 
			apt-cache show package    获取包的相关信息，如说明、大小、版本等 
			sudo apt-get install package  安装包 
			sudo apt-get install package -- reinstall 重新安装包 
			sudo apt-get -f install     修复安装"-f = --fix-missing" 
			sudo apt-get remove package 删除包 
			sudo apt-get remove package -- purge 删除包，包括删除配置文件等 
			sudo apt-get update  更新源 
			sudo apt-get upgrade 更新已安装的包 
			sudo apt-get dist-upgrade 升级系统 
			sudo apt-get dselect-upgrade 使用 dselect 升级 
			apt-cache depends package 了解使用依赖 
			apt-cache rdepends package 是查看该包被哪些包依赖 
			sudo apt-get build-dep package 安装相关的编译环境 
			apt-get source package 下载该包的源代码 
			sudo apt-get clean && sudo apt-get autoclean 清理无用的包 
			sudo apt-get check 检查是否有损坏的依赖

- 	linux模式：
			0 关机  1 单用户模式   2 无网络的命令多用户模式  3 命令多用户模式  
			4 未使用的  5 图形界面  6重启    eg：init 5  进入5模式
- 网络相关命令
		  关闭防火墙
			iptables -F
			setenforce 0
		  更改网络IP地址
			vi /etc/sysconfig/network-scripts/ifcfg-eth0 
			system-config-network  （图形化界面）
		  网络重新启动
			service network restart
		  查看网络IP地址等
			ifconfig
		  检测是否可以通信
			ping xxx.xxx.xxx.xxx  检测两个主机是否可以通信
		  查看网络状态
			net -stat
	端口号
			0- 1024  知名端口  1025 - 4096  保留  4096以上 随机端口


- 相关命令杂项
			passwd 更改使用者的密码
			pwd　查看所在路径
			su 进去超级用户 exit 退出管理员用户 sudo 这条命令给用户管理员的权限  
			man 命令 功能：显示具有一定格式的在线命令帮助手册，也可以显示某个命令的格式。
			date 命令 功能：显示及修改日期和时间。 
			cal 命令 功能： 查看日历
			& 后台命令 功能：将&符放在一条命令后，使该条命令在后台执行。
			wc 命令 功能：统计一个文件中的字节数 -c、单词数 -w 、行数 -l。
			mail 命令 功能：用户之间进行通信的命令。
			grep 命令 功能：过滤  条件   根据给定的条件显示输出内容
			| 命令 功能：管道命令，将前一个命令的输出传递给后一个命令，作为后一个命令的输入,通常与其它命令联合使用。如：$ls –l |more
			who 功能：查看系统中其它登录全部的用户。
			who am i功能：查看当前用户信息
			jobs 命令 功能：查看当前系统中各作业的状态。
			wait 命令 功能:等待后台进程结束。
			sleep 命令 功能:该命令使进程暂停执行一段时间。
			kill 命令 功能:终止一个进程的运行。
			find 命令 功能：搜寻文件与目录  格式：find [目录名] <选项>
			echo 命令 功能：1.在屏幕上输出信息 eg:a=10 echo $a 输出10  echo a 输出a 2.查看环境变量值
			df -v 命令 功能：查看磁盘空间的大小
			ip ad 命令 功能：查看自己的 ip
			in 命令 功能：文件的链接 eg： /user/wang/wb . 把 wb 连接到当前目录下 （与 cp 不同，只会占一个空间）
			> 命令 功能： 合并文件内容 eg:cat a.c b.c ...>newfile.c
			nm main 列出全局变量和入口地址
			umask 命令 功能:利用掩码设置创建文件默认权限   如：0002  第一个0为粘住位，第二个0为系统所属用户所要去掉的权限，第三个0为所属组所要去掉的权限，第四个0为其他用户所要去掉的权限
			mkfifo FIFO 创建管道文件
