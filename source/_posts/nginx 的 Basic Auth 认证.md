layout: nginx
title: 使用nginx 实现Basic Auth认证
date: 2015-10-10 10:13:57
tags: [nginx,ssl,auth]
---
### 以centos 系统为例:

添加httpd-tools工具
```
yum -y install httpd-tools
```

修改nginx 配置文件
```
vi /etc/nginx/conf.d/default.conf
```

在server内增加
```
location /basic {
    auth_basic "Basic Auth"; 
    auth_basic_user_file "/etc/nginx/.htpasswd"; 
 }
```

创建一个用户 fedora 替换成你自己的账号，回车输入密码并确认
```
htpasswd -c /etc/nginx/.htpasswd fedora
```

重启nginx服务
```
/etc/rc.d/init.d/nginx restart
```
或者
```
service nginx reload
```
```
service nginx restart
```
