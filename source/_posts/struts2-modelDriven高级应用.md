title: struts2-modelDriven高级应用
date: 2015-08-26 21:33:59
tags:
---

struts2 modelDriven 高级应用

[![Build Status](https://travis-ci.org/izerui/struts2-advanced-modelDriven.svg?branch=master)](https://travis-ci.org/izerui/struts2-advanced-modelDriven)

#封装的第四种Action传参方式

##传统的struts传参有三种
- 1. 通过属性传参
- 2. 通过域模型传参
- 3. 通过模型驱动传参数

##以上这三种各有利弊,总之一句话概括,无法完全的实现业务和参数分离.总得在自己的Action中定义相关的get set 方法.

希望能实现参数传递跟业务action完全分离.故封装了模型驱动参数获取方式.

#如此一来,action 还可以这样写:

-Action

```java
public class YourAction extends BaseActionSupport<YourParam> {

    //TODO your code ....
    //在需要获取request参数的地方只需要通过 formParam对象get你需要的参数即可

}

```

-param class 就是一个简单的po类.随你写喽!

```java
public class yourParam{
    // 各种 param fields
    // 各种 get set
}
```

#OK 所有的action请求,都会自动的封装到你定义的param类中. 前提这个类要有不带参数的构造方法,要有你希望获取的参数的get set 方法.

其实就是把action中的各种 get set 放到了一个统一的param类中.

一个action  一个 param类. 是不是很清晰.业务跟参数代码分离了. struts action 是不是更易读了.

# 附: 顺带将 BaseActionSupport 放入了一个applicationContext
## 这样所有的action 都可以直接使用applicationContext 对象,如果您不喜欢可以自行去掉.
##步骤如下:
- 1. 删除 protected ApplicationContext applicationContext;
- 2. 删除 applicationContext 的 set方法
- 3. 去掉实现的接口 ApplicationContextAware

#不过建议留着吧.
- 当你用上 spring 的event 的时候,你会需要它.
- 当你需要动态的根据className 或者 classType 获取bean 的时候,你会需要它.
- 

![图](http://dl.iteye.com/upload/picture/pic/130175/017e87c4-e4a6-3da8-92c6-7502a0e84486.png)



