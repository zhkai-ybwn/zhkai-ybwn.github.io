---
title: Vue学习二
date: 2021-12-08 20:32:35
tags:
  - el与data
  - MVVM模型

categories: [前端, Vue]
---

## Vue 学习二

### el 与 data 的两种写法

el 有两种写法
(1)new Vue 时配置 el 属性
(2)先创建 Vue 实例，随后通过 vm.$mount 指定 el 的值。
data 有两种写法
(1)对象式

```js
data: {
  name: "尚硅谷";
}
```

(2)函数式

```js
data() { // 这里写成箭头函数时，this是window
       console.log(this) // 此处的this是Vue实例对象
       return {
         name: '尚硅谷'
       }
     }
```

### MVVM 模型

M：模型(Model)，对应 data 中的数据
V：视图(View)，模板
VM：视图模型(ViewModel)，Vue 实例对象

![avatar](https://pic.imgdb.cn/item/61b0a95c2ab3f51d914b1836.jpg)
观察发现：
1.data 中所有的属性，最后都出现在了 vm 上。
2.vm 上所有的属性以及 Vue 原型上的所有属性，在 Vue 模板中都可以直接使用。

### 数据代理

#### 回顾 Object.defineProperty 方法

```js
let number = 18;
let person = {
  name: "张三",
  sex: "男",
};
Object.defineProperty(person, "age", {
  // value: 18,
  // enumerable: true, // 控制属性是否可以枚举，默认值是false
  // writable: true, // 控制属性是否可以被修改，默认值是false
  // configurable: true // 控制属性是否可以被删除，默认值是false
  // 当person的age的属性被调用时，get函数就会被调用
  get() {
    return number;
  },
  set(value) {
    number = value;
  },
});
console.log(person);
console.log(Object.keys(person));
```
