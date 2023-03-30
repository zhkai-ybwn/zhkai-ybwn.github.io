---
title: Vue3中Mitt的使用
date: 2023-03-30 13:11:44
tags: 
	- Vue
	- Mitt
categories: 
	[前端,Vue]
---

## Vue中使用Mitt

Mitt是一个在Vue.js应用程序中使用的小型事件总线库。该库允许组件进行通信，而不必过度依赖父级或子级组件之间的props。

### Mitt的特性和功能介绍

- 轻量级: Mitt只有200字节大小，不会增加你的应用程序的负担
- 方便易用：只需要引入mitt并配置即可使用
- 支持任何Javascript环境：Mitt支持在任何Javascript环境下使用，不仅限于Vue
- 应用场景：组件通信

### 引入Mitt

#### 安装依赖

执行安装命令

```Shell
npm install --save mitt
```

在src目录下面，新建`/libs/bus.js`，内容如下：

```Javascript
// 事件总线第三方库：
    import mitt from 'mitt'

    const bus = mitt()
    export default bus
```

#### 使用

```Javascript
    import bus from '@/libs/bus.js'

    //in component A 触发
    bus.emit('event-name', eventData)

    //in component B 监听
    bus.on('event-name', eventData => { /* do something with eventData */ })
```
