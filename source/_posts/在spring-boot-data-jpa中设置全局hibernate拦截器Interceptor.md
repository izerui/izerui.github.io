title: 在spring-boot-data-jpa中设置全局hibernate拦截器Interceptor
date: 2018-03-23 11:30:32
tags: [spring,boot,jpa,hibernate]
---
在spring boot工程的 application.properties 文件中加入：
```
spring.jpa.properties.hibernate.ejb.interceptor=全类名例如:[com.ierp.jpa.interceptors.TraceInterceptor]
```
对应的 TraceInterceptor 继承 一个hibernate的 空拦截器[org.hibernate.EmptyInterceptor] 即可

