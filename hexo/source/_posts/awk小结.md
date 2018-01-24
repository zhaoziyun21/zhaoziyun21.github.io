---
title: awk小结
date: 2018-01-22 22:40:53
tags:
  - linux
  - awk
  - 运维
category: [linux]
---
# awk用法
- ***只显示最近登录的5个帐号***
    
        last -n 5 | awk  '{print $1}'
- ***F指定分割符分割***

        cat /etc/passwd|awk -F ':' '{print $1}'
- ***以tab键分割***

        cat /etc/passwd |awk  -F ':'  '{print $1"\t"$7}'    
- ***第一行添加两列，最后一行添加两列***

        cat /etc/passwd|awk -F ':' 'BEGIN {print "name,zhaoziyun"} {print $1","$7} END {print "yeah,byebye"}'
- ***搜索含root的行，显示第七个域***
        
        awk -F: '/root/{print $7}' /etc/passwd
- ***awk统计某个文件夹下的文件占用的字节数***

        ls -l |awk 'BEGIN {size=0;}{size=size+$5} END{print "[end]size is ",size/1024/0124,"M"}'
- ***循环***

        awk -F ':' 'BEGIN {count =0;} {name[count] =$1;count++;}; END{for(i=0;i<NR;i++) print i,name[i]}' /etc/password
        