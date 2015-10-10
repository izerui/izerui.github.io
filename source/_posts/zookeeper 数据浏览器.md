title: zookeeper 数据浏览器
date: 2015-04-07 22:20:57
tags: [zookeeper]
---
又是一个清明小长假，一个人远在深圳也没有回家，闲来无事写了一个zookeeper的数据浏览器。
上图：

 <img src="https://raw.githubusercontent.com/izerui/zookeeper-explorer/master/src/main/resources/public/assets/img/sample.png">
 
使用上的技术：
 
1. spring boot 
2. flex
3. zookeeper curator client
4. shiro security
5. fastxml jackson 
6. spring mvc restful
 
 
基于spring boot微服务做的一个应用，以 独立jar包发布，内嵌了tomcat容器。因为一直也没有找到合适的可以方便管理和查看 zookeeper 数据的一个东东。就自己做了一个，方便公司的zk管理。
 
有兴趣的。或者也有同样需求的可以down下来玩玩。 东西虽小五张俱全。操作权限该有的都有。
 
 
项目地址： https://github.com/izerui/zookeeper-explorer