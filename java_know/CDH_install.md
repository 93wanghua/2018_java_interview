## 基本详细
[虚拟机安装centOS7](https://www.linuxidc.com/Linux/2017-12/149910.htm)

[参考安装教程](https://blog.csdn.net/Tx_Smile/article/details/78338110)
> 使用虚拟机安装CDH机器集群
```
ip分配:

    192.168.2.100 master
    192.168.2.101 slaver1
    192.168.2.102 slaver2
    
```
## 配置ip
```bash
#打开配置脚本
vim /etc/sysconfig/network-scripts/ifcfg-ens33

#按照如下修改,主要配置写注释
TYPE=Ethernet
BOOTPROTO=static    #开机协议,有dhcp和static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=df29bcbb-eabb-4da0-86b0-48af13f0fa63
DEVICE=ens33
ONBOOT=yes  #开机自启
DNS1=114.114.114.114    #国内DNS固定值
IPADDR=192.168.2.100    #自己设置的IP
NETMASK=255.255.255.0   #子网掩码不需要修改
GATEWAY=192.168.2.2 #虚拟机软件的网管

#重启服务
service network restart

#查看ip
ip addr show

```
## 修改每个节点的hostname
```bash
#  不修改hostname,后面安装无法识别IP
vi /etc/hostname
master

vi /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=master
```
## 关闭防火墙/开启端口
```bash
---直接关闭
systemctl stop firewalld.service  #禁止firewall
systemctl disable firewalld.service #禁止firewall开机启动
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
---按需求开启
#开启80端口
firewall-cmd --zone=public --add-port=80/tcp --permanent  
#开启3306端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent  
#重启防火墙：
firewall-cmd --reload  
```
## 配置host
```
vi /etc/hosts

master 192.168.2.100
slaver1 192.168.2.101
slaver2 192.168.2.102
```
## 配置免密登录
```bash
#进入到我的home目录
cd ~/ .ssh
ssh-keygen -t rsa （四个回车）

#执行完这个命令后，会生成两个文件id_rsa（私钥）、id_rsa.pub（公钥）,将公钥拷贝到要免密登陆的目标机器上
ssh-copy-id host
#---
	ssh免登陆：
		生成key:
		ssh-keygen
		复制从A复制到B上:
		ssh-copy-id B
		验证：
		ssh localhost/exit，ps -e|grep ssh
		ssh A  #在B中执行
#---

```
## JAVA环境安装
```bash
#将jdk.tar.gz解压到制定文件夹下
tar -zxvf /usr/myjob/jdk-8u131-linux-x64.tar.gz -C /opt/
#改目录名(去除版本号,便于以后升级不需要从新修改配置文件)
cp -r /opt/jdk-8u131 /opt/jdk
#配置jdk这里采用全局设置方法，就是修改etc/profile，它是是所有用户的共用的环境变量
vi /etc/profile
#打开之后在末尾添加:
JAVA_HOME=/opt/jdk
JRE_HOME=/opt/jdk/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export PATH JAVA_HOME CLASSPATH
#使环境变量生效
source /etc/profile
#验证
echo $JAVA_HOME
```
## 分发文件
```bash
scp -r /bigdata/app/ slaver1:/bigdata/
```
## 安装cdh
### mysql 安装
```bash
# 1.上传rpm包
	mysql-community-common-5.7.20-1.el7.x86_64.rpm
	mysql-community-libs-5.7.20-1.el7.x86_64.rpm
	libaio-0.3.109-13.el7.x86_64.rpm
	mysql-community-client-5.7.20-1.el7.x86_64.rpm
	mysql-community-server-5.7.20-1.el7.x86_64.rpm
# 2.安装mysql数据库（切换到root用户）（装在哪里没有限制，只有能联通hadoop集群的节点）
# 3.先查看是否有:mariadb
rpm -qa | grep mariadb
> mariadb-libs-5.5.52-1.el7.x86_64
# 4.卸载mariadb
rpm -e --nodeps mariadb-libs-5.5.52-1.el7.x86_64

# 5.安装mysql
rpm -ivh mysql-community-common-5.7.20-1.el7.x86_64.rpm
    
rpm -ivh mysql-community-libs-5.7.20-1.el7.x86_64.rpm
    
#(min centOS版本才需要) 
rpm -ivh libaio-0.3.109-13.el7.x86_64.rpm
    
rpm -ivh mysql-community-client-5.7.20-1.el7.x86_64.rpm

# 安装server可能会报缺少依赖错误
# yum install perl
# yum install net-tools
rpm -ivh mysql-community-server-5.7.20-1.el7.x86_64.rpm


# 6.安装完成后启动mysql服务
systemctl start mysqld
    
# 7.查看mysql的状态
netstat -plntu | grep mysqld
    
# 8.查看超级用户的密码: >5QyDtFyfqOG
grep 'temporary password' /var/log/mysqld.log

> 2017-12-03T18:29:05.629055Z 1 [Note] A temporary password is generated for root@localhost: >5QyDtFyfqOG
    
# 9.登录更改默认root密码
mysql -u root -p
```
```sql    
-- # 更改密码验证方式
set global validate_password_policy=0;
-- # 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '12345678';

-- # 配置远程连接
-- # 允许任何ip以root用户登录
grant select,insert,update,delete on *.* to root@"%" identified by "12345678";
-- # 允许任何ip以root用户登录
-- # 将所有数据库的所有表（*.*）的所有权限（all privileges）
-- # 授予通过任何ip（%）访问的root用户，密码为123456，如果要限制只有某台机器可以访问，将其换成相应的IP即可
use mysql;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '12345678' WITH GRANT OPTION; 
flush privileges;

-- # 10.建立数据库
create database hive DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database oozie DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database hue DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```
### 关闭所有节点SELinux
```bash
vim /etc/selinux/config
SELINUX=disabled
```
### 配置所有节点的NTP时间同步
```bash

yum install ntp
# 开机自启
chkconfig ntpd on
# 启动服务
service ntpd start
```
### 安装Cloudera Manager Server和Agent
```bash
# 解压CM到opt
tar -zxvf cloudera-manager-centos7-cm5.13.3_x86_64.tar.gz -C /opt/
# opt目录下
drwxr-xr-x. 4 1106 4001  36 Mar 28 16:16 cloudera
drwxr-xr-x. 9 1106 4001  88 Mar 28 16:16 cm-5.13.3

# 创建用户
useradd --system --home=/opt/cm-5.13.3/run/cloudera-scm-server/ --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
# Cloudera Manager建立数据库，此处需要前边下载好的mysql-connector-java-5.1.46.jar放到
/opt/cm-5.13.3/share/cmf/lib/
/usr/share/java
# 建库
/opt/cm-5.13.3/share/cmf/schema/scm_prepare_database.sh mysql -uroot -p12345678 scm scm
# 输入新的SCM库密码

# 配置Agent
vi /opt/cm-5.13.3/etc/cloudera-scm-agent/config.ini
server_host=master
# 分发
scp -r /opt/cm-5.13.3/ root@slaver1:/opt/
```
### 安装CDH5
```bash
# 将文件拷贝到/opt/cloudera/parcel-repo/
cp CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel /opt/cloudera/parcel-repo/
cp CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel.sha1 /opt/cloudera/parcel-repo/
cp manifest.json /opt/cloudera/parcel-repo/

# xxx.sha1改为xxx.sha
CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel
CDH-5.13.3-1.cdh5.13.3.p0.2-el7.parcel.sha
manifest.json

# 启动aggent
#主节点
/opt/cm-5.13.3/etc/init.d/cloudera-scm-server start
#主节点及其他从节点
/opt/cm-5.13.3/etc/init.d/cloudera-scm-agent start
```
### 在webui安装
```
```
### 安装失败
```bash
# 中途安装失败，重新安装

# 关闭所有服务
#主节点
/opt/cm-5.13.3/etc/init.d/cloudera-scm-server stop
#主节点及其他节点
/opt/cm-5.13.3/etc/init.d/cloudera-scm-agent stop
# 删除agent uuid
rm -rf /opt/cm-5.13.0/lib/cloudera-scm-agent/*
# 删除数据库scm
# 重新配置数据库
/opt/cm-5.13.3/share/cmf/schema/scm_prepare_database.sh mysql -hlocalhost -uroot -p12345678 scm scm
# 启动Cloudera server和agent，按照之前步骤安装即可
```

