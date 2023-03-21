---
title: Vue3中使用pinia
date: 2023-03-20 13:39:39
tags: 
	- Vue
	- Pinia
categories: 
	[前端,Vue]
---

## Vue3中使用pinia

Pinia是一个轻量级的、基于Vue 3的状态管理库，它的设计目标是提供简单易用的API，使得开发者能够更加便捷地管理Vue 3应用程序中的状态。与Vuex相比，Pinia更加轻量级和易于理解，适用于中小型应用程序。
Pinia的核心是store实例，每个store实例都包含一个状态对象和一组用于更新和读取状态的方法。Pinia还提供了用于定义和创建store的API，以及一组插件，用于扩展store的功能。在使用Pinia时，开发者可以利用Vue 3的响应式数据机制，实现状态的实时更新和自动渲染。
总体来说，Pinia提供了一个简单、灵活和高效的方式来管理Vue 3应用程序中的状态。它具有易于理解的API、轻量级的设计和出色的性能，可以帮助开发者更快速地构建高质量的Vue 3应用程序。

### 安装依赖

执行安装命令

```Shell
npm install pinia
```

### 创建pinia

创建一个pinia（根存储）并将其传递给应用程序，即在`main.js`中添加如下代码

```JavaScript
import { createPinia } from 'pinia'
app.use(createPinia())
```

### 定义一个store

Store类似于Vuex中的Store，它存储了整个应用程序的状态。在Pinia中，您可以使用`defineStore`函数来定义一个Store。
例如，下面是一个简单的Store：

```JavaScript
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
state: () => ({
    count: 0
}),
actions: {
    increment() {
    this.count++
    }
}
})
```

在上面的代码中，我们使用defineStore函数来定义一个名为“counter”的Store。state函数返回一个包含count属性的对象，actions对象包含了一个名为increment的方法，用于增加计数器的值，其中`counter`是唯一的名称。

### 在组件中使用Pinia Store

在Vue 3中，您可以使用useStore函数来访问Store。例如，下面是一个简单的组件：

```JavaScript
<template>
<div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
</div>
</template>

<script>
import { useCounterStore } from './store'

export default {
setup() {
    const store = useCounterStore()
    const count = computed(() => store.count)
    const increment = () => {
        store.increment()
    }
    return { count, increment }
}
}
</script>
```

在上面的代码中，我们使用useCounterStore函数来获取名为“counter”的Store实例。然后，我们使用computed函数来计算count属性，该属性返回Store中的count属性的值。最后，我们使用increment方法来增加计数器的值。

## 总结

这就是在Vue 3中使用Pinia的基础知识。Pinia提供了一种简单而直观的方法来管理应用程序的状态。您可以使用defineStore函数来定义一个Store，使用useStore函数来访问Store，并在组件中使用Store的状态和方法。Pinia还提供了一些高级功能，如插件和Devtools支持，这些功能可以帮助您更好地管理和调试应用程序的状态。
