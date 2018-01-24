---
title: linux常用命令集锦
date: 2018-01-21 21:37:15
tags: [linux,运维]
category: [linux]
---
# linux命令
- 查看当前磁盘
>df  -h
- 查看端口
>netstat -tln
- 查看当前目录：
> pwd
- 查看编码：
> echo $LANG
- 修改编码：
>LANG=zh_CN.UTF-8
- 解压文件：
>tar -zxvf **.tar.gz 
 unzip filename.zip 
查看当前tomcat 进程
ps -ef|grep tomcat
查看文件
cat view.sh
查看日志
tail -f -n 500 catalina.out 
拷贝目录
cp -r  a文件夹  b文件夹
修改文件
vi test.xml
------------------------------------------------------------ 
# 软件安装
***1、jdk安装配置***
tar zxvf jdk-7u79-linux-x64.tar.gz   -C  /usr/local/
 
JAVA_HOME=/usr/local/jdk1.7.0_79
JAVA_BIN=/usr/local/jdk1.7.0_79/bin
PATH=$PATH:$JAVA_BIN
CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JAVA_BIN PATH CLASSPATH

sudo update-alternatives --install /usr/lib/java java /usr/local/jdk1.7.0_79/bin/java 300  
sudo update-alternatives --install /usr/lib/javac javac /usr/local/jdk1.7.0_79/bin/javac 300  
***2、tomcat 安装配置***
tar zxvf apache-tomcat-7.0.73.tar.gz   -C  /usr/local/

#set tomcat environment
CATALINA_HOME=/usr/local/apache-tomcat-7.0.73
export CATALINA_HOME


CATALINA_HOME=/usr/local/apache-tomcat-7.0.73
JAVA_HOME=/usr/local/jdk1.7.0_79

***3、mysql 方面***
启动MySQL服务： sudo start mysql
停止MySQL服务： sudo stop mysql
授权
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
FLUSH  PRIVILEGES;
 
-------------------------------------------------------------------------------------
# 其他
***1、tomcat 在project 放项目配置***
/usr/local/apache-tomcat-7.0.73/project/HiFive
<Context path="" docBase="/usr/local/apache-tomcat-7.0.73/project/HiFive" /> 
      </Host>     
***2、js时间   谷歌浏览器 date的横杠 '-'***
***3、ubuntu 关闭防火墙***
http://www.cnblogs.com/kluan/p/5993767.html 
sudo ufw status

***4、内存溢出***
set JAVA_OPTS=-Xms128m -Xmx350m
或者set CATALINA_OPTS=-Xmx300M -Xms256M
***5、远程复制***
scp -r /home/shangdong/apache-tomcat-7.0.42/project/HiFive kssj@101.200.40.179:/home/kssj/tomcat/apache-tomcat-7.0.42/webapps
  