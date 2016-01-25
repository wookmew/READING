# 目录  
- [第一章 搭建服务器的准备工作](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8A%E9%B8%9F%E5%93%A5%E7%9A%84Linux%E7%A7%81%E6%88%BF%E8%8F%9C--%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E8%AE%BE%E7%AF%87%E3%80%8B.md#第一章-搭建服务器的准备工作)  
- [第二章 网络的基本概念](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8A%E9%B8%9F%E5%93%A5%E7%9A%84Linux%E7%A7%81%E6%88%BF%E8%8F%9C--%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E8%AE%BE%E7%AF%87%E3%80%8B.md#第二章-网络的基本概念)
- [第四章 连接Internet](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8A%E9%B8%9F%E5%93%A5%E7%9A%84Linux%E7%A7%81%E6%88%BF%E8%8F%9C--%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E8%AE%BE%E7%AF%87%E3%80%8B.md#第四章-连接internet)  
- [第五章 Linux中常用的网络命令](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8A%E9%B8%9F%E5%93%A5%E7%9A%84Linux%E7%A7%81%E6%88%BF%E8%8F%9C--%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E8%AE%BE%E7%AF%87%E3%80%8B.md#第五章-linux中常用的网络命令)  
- [第七章 网络安全与主机基本防护:限制端口、网络升级与SELinux]()



# 第一章 搭建服务器的准备工作  

关于搭建服务器的基本：
    - 网络的基本概念，以方便进行联网与配置及排错  
    - 熟悉操作系统的基本操作：包括登陆控制，账号管理，文本编辑器的使用等技巧  
    - 信息安全方面：包括防火墙与软件更新方面的相关知识  
    - 该服务器协议所需软件的基本安装，配置，排错等<br/>
<font size=5>**服务器的基本配置与相关配置**</font>  
        先观察服务使用的通信协议是什么，然后了解该如何设置，接下来编辑主配置文件，根据主配置文件的数据去执行相对应的命令来取得正确的环境配置。<br/>
<font size=5>**搭建一台主机需要知道**</font>  <br/>
    - 各个process 与signal的观念<br/>
    - 账号与组的概念与相关性<br/>
    - 文件与目录的权限，包含与账号相关的特性<br/>
    - 软件管理的学习<br/>
    - Bash与Shell Scripts的语法，及vim<br/>
    - 开机的流程分析，以及日志文件的设置与分析<br/>
    - 知道类似Quota以及文件系统连接等概念<br/>  


# 第二章 网络的基本概念  
OSI七层协议与TCP/IP  


# 第四章 连接Internet

- 在Linux中，网卡以网卡内核模块对应的设备名称来表示，默认网卡名称为eth0,第二章为eth1,以此类推。
- dmesg查看网卡信息，lspci查询芯片数据。
- /etc/sysconfig/network-scripts/ifcfg-eth0:IP,Netmask,DHCP,Gateway等
- /etc/sysconfig/network:主机名
- /etc/resolv.conf:DNS:IP
- /etc/hosts:私有IP对应的主机名  


# 第五章 Linux中常用的网络命令  
- ipconfig:可以手动启动，查看与修改网络接口的相关参数，可以修改的参数很多，包括IP参数以及MTU等。
- ifup,ifdown:实时地动手修改一些网络接口参数
- route:对路由进行修改、查询、添加或删除。  
- ip 功能最为强大:
    1. ip link:关于接口设备的相关设置
    2. ip address:关于额外IP的相关设定
    3. ip route:关于路由的相关设定
- iwlist:利用无线网卡进行无线AP的检测与取得相关的数据
- iwconfig:设置无线网卡的相关参数
- traceroute:两主机之间的节点分析
- netstat:查询网络接口所监听的端口
- host:查出某主机的IP
- nslookup:与host类似，可由IP找出主机名
- talent:远程连接
- ftp,lftp:进行FTP的连接，lftp可进行匿名登录。  
- 文字浏览器：links。好处在于，查阅Linux本机上面以HTML语法写成的文件数据。  
- 文字接口下载器：wegt，用于下载WWW的资料。  
- 捕捉包tcpdump，可分析3次我手，以及明文传输的数据。  
- nc可用来取代telnet进行某些服务器端口的监测工作。  


# 第七章 网络安全与主机基本防护:限制端口、网络升级与SELinux  
- Linux有两层防火墙：第一层是数据包过滤式的Net Filter防火墙，第二层是通过软件管理的TCP Wrappers。  
