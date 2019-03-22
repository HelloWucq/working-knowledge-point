
#一、基础命令：
	【1】补齐键：Table 
	【2】显示当前工作目录的绝对路径：pwd 
	【3】清屏：clear 
	【4】如果父目录不存在，创建所有的父目录：mkdir -p 
	【5】修改一个目录的权限，包括其子目录及文件：chmod 777 -R test
	【6】在不注销的情况切换用户身份：su 
	【7】以另一个用户的身份执行某个命令：sudo 
	【8】将文件source更名为target ：mv source target
	【9】文件上传：rz - bye
	【10】下载文件：sz 文件名
	【11】自动下载文件： wget www.lignag.com/test abc.zip
#二、查看：
	【1】查看文件内容：cat /etc/services
	【2】查看文件前n行的内容：head -n /etc/services
	【3】查看文件后n行内容：tail -n /etc/services
	【4】统计文件内容的行数：wc -l /etc/services
	【5】返回文件开头的快捷键：gg
	【6】返回文件末尾的快捷键：shift+g(即G)
	【7】查看文件内容，并在每行前面加上行号：cat -n test.txt
	【8】查看文件内容，在不是空行的前面加上行号：cat -b test.txt
 	 PS: 从最后一行开始显示：tac
	【9】显示所有文件，包括以.开头的隐含文件：ls -a
	【10】显示文件的详细信息：ls -l
	【11】显示当前目录及所有子目录信息：ls -Rl
	【12】以时间排序显示目录,这在找最新文件有用：ls -tl
	【13】以文件大小排序：ls -Sl
	【14】显示文件大小,并按大小排序：ls -s -l -S
	【15】ls heatmap*
	 ? 表示该位置可以是一个任意的单个字符
	 * 表示该位置可以是若干个任意字符
	 [a-z] 表示该位置中可以出现任意单个a到z的字符
	【16】more +100 file.txt
    	 more +/usertime file.txt
		 +n 从笫n行开始显示
		+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示   
		Enter   向下n行，需要定义。默认为1行
		Ctrl+F  向下滚动一屏
		空格键  向下滚动一屏
		Ctrl+B  返回上一屏
		=       输出当前行的行号
		q       退出more
#三、压缩&解压缩：
	【1】创建一个zip格式的压缩包：zip file1.zip file1  
	【2】将几个文件和目录同时压缩成一个zip格式的压缩包：zip -r file1.zip file1 file2 dir1
	【3】解压一个zip格式压缩包：unzip file1.zip    
	【4】创建一个叫做'file1.rar'的包：rar a file1.rar test_file 
	【5】同时压缩多个文件及目录：rar a file1.rar file1 file2 dir1
	【6】创建一个包含了多个文件的压缩包：tar -cvf archive.tar file1 file2 dir1  
	【7】显示一个包中的内容：tar -tf archive.tar  
	【8】释放一个包：tar -xvf archive.tar 
#四、复制：
	【1】cp source target 将文件source 复制为 target 
	【2】scp [可选参数] local_file remote_username@remote_ip:remote_file
	【3】scp -P port user@serverip:/home/user/filename /home/user/
           -P 选择端口
   		   port 端口 
	【5】scp -r /home/user/本地 user@serverip:/home/user/远程
   			user 为ssh user名
   			serverip 为远程服务器ip或者域
     	实例：scp -r /data/test ligang@192.168.1.1:/data/test
#五、工具命令：
	【1】显示年历、月历：cal 
	【2】显示当前服务器日期：date
	【3】简单好用的计算器：bc
#六、文件夹操作：
	【1】比较目录1与目录2的文件列表是否相同，但不比较文件的实际内容，不同则列出：diff dir1 dir2 
	【2】比较文件，显示两个文件不相同的内容：comm file1 file2
#七、文件搜索：
	【1】递归搜索：find /data -name other.js
	【2】搜索属于用户 'ligang' 的文件和目录：find / -user ligang 
	【3】在目录 '/data' 中搜索带有'.js' 结尾的文件：find /data -name \*.js  
	【4】搜索在过去100天内未被使用过的执行文件：find /data -type f -atime +100 
	【5】搜索在10天内被创建或者修改过的文件：find /data -type f -mtime -10 
#八、其他命令：
	【1】Ctrl+r 然后输入若干字符，开始向上搜索包含该字符的命令，继续按Ctrl+r,搜索上一条匹配的命令
	【2】Ctrl+h 用于改正输入的错误
	【3】创建一个空白文件或改变文件的时间戳：touch 
		不加任何参数创建一个空白文件
    	-a 改变文件访问时间为当期时间
    	-m 改变文件修改时间为当前时间
	【4】创建软连接、硬链接：ln 
	【5】过滤、查找文件中的内容：grep 
	【6】显示内存使用情况：free 
	【7】显示当前系统进程：ps 
			ps -ef|grep tomcat
	【8】杀死指定进程：kill 
	【9】显示一串字符：echo [-n] message
			n表示输出文字后不换行
	【10】计算/root目录的容量并以M为单位：du -sm /root
#九、tomcat相关：
	【1】首先查看Tomcat进程号(8082为tomcat-http端口号)：
   		losf -i:8082
	【2】查看是否配置生效(4424为tomcat进程号)：
   		sudo jmap  – heap 4424   
	【3】查看内存使用情况：free

	【4】查看系统位数：
		uname -a

#十、linux下各种安装包安装命令：
	【1】rpm包安装方式步骤： 
		1、找到相应的软件包，比如soft.version.rpm，下载到本机某个目录； 
		2、打开一个终端，su -成root用户； 
		3、cd soft.version.rpm所在的目录； 
		4、输入rpm -ivh soft.version.rpm 
	【2】deb包安装方式步骤： 
		1、找到相应的软件包，比如soft.version.deb，下载到本机某个目录； 
		2、打开一个终端，su -成root用户； 
		3、cd soft.version.deb所在的目录； 
		4、输入dpkg -i soft.version.deb 
	【3】tar.gz源代码包安装方式： 
		1、找到相应的软件包，比如soft.tar.gz，下载到本机某个目录； 
		2、打开一个终端，su -成root用户； 
		3、cd soft.tar.gz所在的目录； 
		4、tar -xzvf soft.tar.gz //一般会生成一个soft目录 
		5、cd soft 
		6、./configure 
		7、make 
		8、make install 
	【4】tar.bz2源代码包安装方式： 
		1、找到相应的软件包，比如soft.tar.bz2，下载到本机某个目录； 
		2、打开一个终端，su -成root用户；  
		3、cd soft.tar.bz2所在的目录； 
		4、tar -xjvf soft.tar.bz2 //一般会生成一个soft目录 
		5、cd soft 
		6、./configure 
		7、make 
		8、make install 
	【5】apt方式安装： 
		1、打开一个终端，su -成root用户； 
		2、apt-cache search soft 注：soft是你要找的软件的名称或相关信息 
		3、如果2中找到了软件soft.version，则用apt-get install soft.version命令安 装软件 注：只要你可以上网，只需要用apt-cache search查找软件，用apt-get install软件 
	【6】bin文件安装：
		如果你下载到的软件名是soft.bin，一般情况下是个可执行文件，安装方法如下： 
		1、打开一个终端，su -成root用户； 
		2、chmod +x soft.bin 

		3、./soft.bin //运行这个命令就可以安装软件了

#十一.注意点
###双引号内的特殊字符如 $ 等，可以保有原本的特性
###单引号内的特殊字符则仅为一般字符 (纯文本)
###在一串指令中，在 `之内的指定将会被先执行，而其执行出来的结果将做为外部的输入信息

#十二.Shell编程
##12.1.基本知识
###12.1.1.变量替换在单引号中无效
###12.1.2.环境变量，变量（$var,${var}）
###12.1.3.使用单引号时，变量不会被扩展，将依照原样显示
###12.1.4.获取变量的长度（${#var}）
###12.1.5.数学计算
###12.1.6.文件描述符以及重定向（0-stdin;1-stdout;2-stderr）:注意“>”与“>>”的区别
###12.1.7./dev/null是一个特殊的设备文件，它接收到的任何数据都会被丢弃。 null设备通常也被称为黑洞
###12.1.8.关联数组
###12.1.9.选项-echo禁止将输出发送到终端，而选项echo则允许发送输出
###12.1.10.时间和延时
###12.1.11.反引用（有些人们也称它为反标记）的方法也可以用于存储命令输出
###12.1.12.字段分隔符和迭代器

##12.2.命令
###12.2.1.cat命令不仅可以读取文件、拼接数据，还能够从标准输入中进行读取
###12.2.2.文件查找与文件列表（find）
###12.2.3.xargs:将标准输入数据转换成命令行参数
###12.2.4.用tr进行转换
###12.2.5.校验和核实
###12.2.6.加密与散列
###12.2.7.排序，唯一与重复（sort/uniq）
###12.2.8.存储临时数据的唯一一般放置于/tmp(该目录中的内容在系统重启后会被清空)
###12.2.9.分割文件与数据（split）

##12.3.操作文件
###12.3.1.目录、普通文件、块设备、字符设备、符号链接、套接字和命名管道等
###12.3.2.comm命令可用于两个文件之间的比较。它有很多不错的选项可用来调整输出，以便我们执行交集、求差（difference）以及差集操作
###12.3.3.文件权限、所有权和粘滞符（用户，用户组，其他用户）
###12.3.4.chattr能够将文件设置为不可修改
###12.3.5.文件类型统计信息（find,file）
###12.3.6.环回文件：环回文件系统是指那些在文件中而非物理设备中创建的文件系统(dd)
###12.3.7.文件差异并修补（diff/patch）
###12.3.8.打印目录树：tree
###12.3.9.文件类型
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B.png)
###12.3.10.文件时间戳
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E6%97%B6%E9%97%B4%E6%88%B3.png)

##12.4.操作文本
###12.4.1.正则表达式
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F.png)
###12.4.2.grep在文件中搜索文本
###12.4.3.cut按列切分文件（每列成为一个字段）
###12.4.4.sed进行文本替换
###12.4.5.awk进行高级文本处理
###12.4.6.统计特定文件中的词频（wc）

##12.5.网络管理
###12.5.1.curl:
###12.5.2.网络配置（ifconfig,route,nslooup,host）
###12.5.3.ping命令使用互联网控制消息协议（ICMP）中的echo分组
###12.5.4.网络间的连通（ping/traceroute）
###12.5.5.SSH在远程主机上运行命令
###12.5.6.网络传输文件（FTP,SFTP，RSYNC和SCP）
###12.5.7.连接无线网络（iwconfig/iwlist）
###12.5.8.网络流量与端口分析（lsof/netstat）
###12.5.9.互联网连接共享：使用iptables设置网络地址转换，使得多个联网的设备可以共享互联网连接，需要使用iwconfig命令来获得无线接口的名称
###12.5.10.iptables架设防火墙

##12.6.版本控制及备份
###12.6.1.压缩和打包（gzip与tar）
###12.6.2.系统备份（rsync,crontab）
###12.6.3.版本控制（git）
###12.6.4.用 fsarchiver 创建全盘镜像

##12.7.系统监控
###12.7.1.df和du是Linux中用于统计磁盘使用情况的两个重要命令。df是disk free的缩写，du是diskusage的缩写


###12.7.2.time:计算命令执行时间（real时间：命令从开始到结束的时间；user时间：进程花费在用户模式中的CPU时间，唯一真正用于执行进程所花费的时间；sys时间：进程花费在内核中的CPU时间，代表在内核中执行系统调用所使用的时间）


###12.7.3.登录用户，启动日志及启动故障等相关信息（who,w users，uptime,last,lastb）

###12.7.4.进程监控与日志记录（ps,watch,top,pgrep(获取特定命令的进程ID列表)）

###12.7.5.记录文件及目录访问（inotifywait,logrotate,syslog）

###12.7.6.监视用户登录

###12.7.7.监视远程磁盘的健康情况

###12.7.8.监视磁盘活动iotop
###12.7.9.检查磁盘及文件系统fsck

###12.7.10.查找及负载（which,whereis,file,whatis,uptime）

###12.7.11.杀死进程与信号相关

###12.7.12.采集系统信息（/proc是一个在内存中的为文件系统，提供了一个可以从用户空间读取系统参数的接口）


###12.7.13.命令调度cron/crontab

###12.7.14.用户管理（-adduser,-deluser,-shell,-disable,-enable,expiry,-passwd,-newgroup,-delgroup,-addgroup,-details,-usage）

###12.7.15.图像管理及图片相关（ImageMagick）

###12.7.16.管理多个终端（screen）

#十三.Linux内核相关
##13.1.sendfile系统调用，可以高效的把硬盘的数据发送到网络上，不需要先把硬盘数据复制到用户态内存中再发送，极大的减少了内核态与用户态数据间的复制动作

#十四.网络相关配置文件
###14.1./etc/networks：机器所连接的网络中那些可以访问的网络名和网络地址
###14.2./etc/protocols：列举了当前可用的协议名称
###14.3./etc/resolv.conf：DNS服务器信息
###14.4./etc/services：列举服务器名称对应的端口号和协议
###14.5./etc/xinetd.conf：xinetd的配置文件，其中包含网络服务的相关信息
###14.6./etc/hostname：包含了系统的主机名，包括完整的域名
###14.7./etc/host.conf：指定如何解析主机名
###14.8./etc/sysconfig/network：用来指定服务器上网络配置信息
###14.9./etc/hosts：主机与IP对应关系