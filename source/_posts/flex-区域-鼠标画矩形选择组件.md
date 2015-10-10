title: 'flex 区域,鼠标画矩形选择组件 '
date: 2013-11-24 21:49:17
tags: [flex]
---

网上类似的代码也不少.但是都只是自己工程中单独贴出来的代码片段. 并不适合通用场景,存在很多bug
举个例子:
1. 如果画框所属的父组件存在滚动的条. 获取的矩形区域大小和坐标就会错乱
2. 如果画矩形过程中,鼠标移出目标组件, 该矩形事件无法继续传播,等等.一些bug
 
今天我把自己工程画框单独剥离出来成一个组件调用.
 
如下图:

<img src="http://dl2.iteye.com/upload/attachment/0091/5073/17b5bc9d-e217-330e-9d31-9cefa400c02b.png"/>
 
 
下面是一个demo工程代码,及整个源码提供大家下载

<a href="/files/RectangleDraw.rar">RectangleDraw.rar</a> 