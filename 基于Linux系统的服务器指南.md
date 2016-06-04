# 基于Linux系统的服务器指南
[@FoolishBoys](https://github.com/FoolishBoys)

-----
## 1、目录
[TOC]

-----
## 2、购买配置信息
所在区：华东
操作系统：CentOS 7.0
CPU：1核
内存：1G
弹性公网IP：121.196.212.151
带宽：2Mbps（0.08元/小时）

------
## 3、登陆方式
###  --》阿里云网页版终端连接
* 点击基本信息右边的“更多”，进入网页版终端
![基本信息](http://ww4.sinaimg.cn/large/87bbc54cgw1f4iwbiarnxj21050gb3zy.jpg)
* 第一次进入会弹出6个字符的网页版登陆密码（只出现一次，谨记）
* 登陆后，进入系统登陆界面
* 初始情况下，系统默认有且只有一个管理员用户（root），类似于Windows的Administrator。可以后期添加新的非管理员用户。
	* 所以第一次进入：
		* login：root
		* password：CoolCoding！（该处的密码为购买云服务时设置的密码，也可以在管理控制台实例中修改） 

### --》[Xshell](http://www.netsarang.com/download/down_xsh.html)工具（免费的终端模拟软件）

* 安装完成后打开，点击新建
![Xshell新建](http://ww3.sinaimg.cn/large/87bbc54cgw1f4iwsvd4rxj20g00dwtal.jpg)
* 主机号填公网IP地址，点击确定后出现用户名和密码窗口
![Col1](http://ww1.sinaimg.cn/large/87bbc54cgw1f4iwxrvfpsj209l05gmxo.jpg)    
![Col2](http://ww3.sinaimg.cn/large/87bbc54cgw1f4ix19spuaj20c50b0dh3.jpg) 
* 输入信息完成后点击确定进入

------
## 4、[Xftp](http://www.netsarang.com/download/down_xfp.html)工具（免费的文件传输软件）
* 安装好Xftp后，打开Xshell，连接远程服务器
![终端](http://ww3.sinaimg.cn/large/87bbc54cgw1f4ixsiducjj211h0f9tey.jpg)
* 按ctrl+alt+f，可直接打开Xftp，并直接连接到服务器
* 现在就可以将自己想要传输的文件，拖动到对应的目录中去，传输协议是FTP协议，也可改为较为安全的SFTP ，但是速度较慢。
![Xftp窗口](http://ww3.sinaimg.cn/large/87bbc54cgw1f4ixxkiwwpj20sg0fzwjg.jpg)

------
## 5、Linux自定义垃圾箱
和Windows相比，在Linux系统下，用命令rm删除文件是不会存在一个垃圾箱来存放垃圾文件的。所以，想要有一个垃圾回收站防止误删，必须自己设置。
### --》原理
 就是将rm命令修改为mv。然后设置路径，文件就会被移动到相应的文件夹中
### --》步骤
``` dos
//tmp目录下创建一个回收站文件夹Rabbish
mkdir /tmp/Rabbish
//编辑一个trash文件
vi /bin/trash
	内容为
	mv $@ /tmp/Rabbish
	:wq保存退出
//添加别名
alias rm=/bin/trash
//编辑环境变量
vi /etc/bashrc
	在最后一行添加
	alias rm=/bin/trash
	:wq保存退出
//提升权限
chmod 755 /bin/trash
chmod 777 /tmp/Rabbish
//启动环境变量
source /etc/bashrc	
```
### --》注意&命令还原
* 如果真的要删除某个文件的时候就用

	``` dos
	/bin/rm filename
	```
* 如果要还原，则将别名设置成/bin/rm，环境变量bashrc中的别名也设置成/bin/trash，再用source命令启动环境变量。

-------
## 6、Linux下的Java安装
### --》.rpm文件的安装方法
* a.查看Linux系统的系统位数

``` dos
//第一种方法
uname -a

//第二种方法
lsb_release -a
```

* b.从[Java官网](http://www.oracle.com)上下载当前版本的JDK，文件类型是.rpm
	* 在Linux下下载，可以使用wget  [OPTION]... [URL]...命令
``` dos
//例如(区分大小写)
//其中 -P 表示指定目录
wget -P /home http://119.29.187.100:8080/aaa.png
```
* c.下载完成后，进入文件所在目录
``` dos
//755：给.rpm文件root权限，但用户组和其他用户无法执行
chmod 755 filename
//安装程序
/**
 *-i：install
 *-v：显示详情
 *-h：显示执行进度
 *--prefix=<dir>:指定安装目录
 */
rpm -ivh --prefix=/uer/Java filename
```

### --》JDK压缩包安装方法


### --》Java环境变量配置
* a.用编辑器打开环境变量文件

	``` dos
	vi /etc/profile
	```
* b.先按insert键，变成编辑模式，然后在最后面加入

	``` dos
	#Set Java Environment
	export JAVA_HOME=/usr/Java/JDK 1.8.xxx
	export PATH=$PATH:$JAVA_HOME/bin
	```
* c.按esc退出编辑模式，然后输入“：”，底部弹出“：”，再输入wq，即保存退出

* d.使用echo命令检查环境变量

	``` dos
	echo $JAVA_HOME
	echo $PATH
	```
* 检查JDK是否安装成功

	``` dos
	//显示java版本号
	java -version
	```

-------

