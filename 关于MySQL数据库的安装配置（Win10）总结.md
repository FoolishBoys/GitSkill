# 关于MySQL数据库的安装配置（Win10）总结

## 前言
 刚接触Android没多长时间，就因为项目的需要，要创建一个MySQL的数据库。虽然Google在Android SDK中有专门为Developer提供轻量级的SQLite，但由于要涉及到与数据库交互，所以个人认为MySQL比SQLite更好一点，虐心历程开始…

（p.s. 如果有任何问题，请给楼楼私信 :-)）
   
诉我直言，MySQL的安装配置说来简单，但是小楼硬是捣鼓了两天才弄好，其中有一台电脑安装失败了，到现在还没办法重新安装。所以，数据库服务和数据库的配置还是要谨慎，不然还是弯路颇多。废话不多说上成功后的图片：

 ![成品图](http://ww1.sinaimg.cn/large/87bbc54cgw1f48qjw4uptj210a0klqbi.jpg)
## 安装配置流程
* 解压官网下载的5.6版本压缩包后，会有如下文件。针对最新版本5.7.12版本里没有data，mysql-test…等文件，小楼确实无法给出一个明确的解释，但后文中会针对5.7版本无法成功配置给出一些分析和看起来并不完美的解决方案。
![文件信息](http://ww4.sinaimg.cn/large/87bbc54cgw1f48qnj0dtkj20hw09mabw.jpg)

* 首先解压后第一步，配置环境变量。右键“我的电脑”=》属性=》高级系统设置=》环境变量
    * 找到环境变量中的“path”，双击点开
    * 将自己电脑中MySQL文件夹中的bin文件夹路径添加进去，如图最后一条。楼楼的MySQL就放在D盘中。
![环境变量配置](http://ww4.sinaimg.cn/large/87bbc54cgw1f48qp6276fj20el0fmgon.jpg)

* 第二步，配置MySQL文件夹中的“my-default.ini”文件
    * 将basedir、datadir分别替换成如图，即MySQL文件夹路径和MySQL文件夹中data文件夹路径
    * port端口为3306（固定）
![文件信息配置](http://ww3.sinaimg.cn/large/87bbc54cgw1f48qq9kt5zj20s40faae2.jpg)

* 第三步，开启管理员命令窗口，对MySQL进行安装、开启服务、关闭服务等操作
    * 安装
        * 找到bin文件夹，使用mysqld.exe进行安装，安装成功后会显示一行安装成功的提示，如果很多行warning，那就说明安装失败。
![安装](http://ww2.sinaimg.cn/large/87bbc54cgw1f48qrcmeuoj20h50aggn0.jpg)
    * 安装成功后就可以操作MySQL数据库了，如图开启和停止数据库服务
![开启/停止服务](http://ww3.sinaimg.cn/large/87bbc54cgw1f48qt0hhhaj20ef0bagnp.jpg)
## 设置密码
为MySQL的默认密码设置新密码
* 最初始的MySQL配置好后，用mysql -uroot -p进入monitor，虽然会显示“Enter Password:”但直接回车就能进入。
* 使用命令行mysqladmin -u root password "newpass"即可设置新密码
>==例如==：mysqladmin -u root password xxxxx
* 然后重启mysql服务，再次登入，则需要正确密码才能进入
![密码修改](http://ww2.sinaimg.cn/large/87bbc54cgw1f48qug6xbyj20ip081q5x.jpg)
## 故障解决方案
* 1、对于删除MySQL后，重新安装时，在cmd中开启install出现“服务已存在”的信息
    * 说明之前删除MySQL时未能彻底删除,还有注册表信息和mysql服务名
        * 打开运行,输入regedit进入注册表,删除:
            * HKEY_LOCAL_MACHINE\SYSTEM\ ==ControlSet001==\Services\Eventlog\Application\MySQL
            * HKEY_LOCAL_MACHINE\SYSTEM\ ==ControlSet002==\Services\Eventlog\Application\MySQL
            * HKEY_LOCAL_MACHINE\SYSTEM\ ==CurrentControlSet==\Services\Eventlog\Application\MySQL
![注册表](http://ww4.sinaimg.cn/large/87bbc54cgw1f48rx61vo2j20s50eg41c.jpg)
        * 打开运行,输入cmd进入命令窗口,输入sc delete MySQL,即可删除服务名
        * 最后重启
* 2、对于官方最新版本5.7，解压后缺少部分文件夹，导致无法正常运行程序的问题
    * 楼楼到网上谷歌了一下，找到了一个粗暴但是实用的方法，就是将新版中缺少的文件夹，从旧版本中提取出来加进去。试了一下发现确实能用，但是经过填补的版本到底还是不是原来的5.7版本，那就不得而知了:-(