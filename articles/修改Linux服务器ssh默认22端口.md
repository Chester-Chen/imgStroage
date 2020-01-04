2019/7/16 14:03:59 

# 修改Linux服务器ssh默认22端口

## vim /etc/ssh/sshd_config

按 **i** 进行编辑，

将Port 22改为 Port xxx

## 如果想多端口同时开放，就将端口都写上

Port 22

Port xxx


修改完成后，按esc ，:q保存并退出(或者按两次shift+z)。

## 重启ssh服务

service ssh restart

(本人用的是Ubuntu，Centos好像是service sshd restart)

## 还有一点要注意

在修改完端口后，要在云控制台，配置安全组，开放你添加的端口。