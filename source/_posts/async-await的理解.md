---
title: async/await的理解
date: 2022-03-27 15:22:54
tags:
    - 异步
    - async/await
categories: [前端]
---
## async/await的理解

遇到个五连回调的代码，真的是地狱级拷打，用`async await`进行了一波改写，顿时神清气爽，总结一下。
顾名思义，`async`是异步的简写，`await`是 `async await`的简写。所以async就是用于声明一个`function`是异步的，而`await`就是用来等待这个异步方法执行完成的。另外，规定`await`只能在`async`中使用。

### async作用及工作原理

先看下`async`是怎么处理返回值的

```js
    async function testAsync() {
    return "hello world";
    }

    const result = testAsync();
    console.log(result);// Promise {'hello world'}
```

可见，`async`返回一个`Promise`对象。如果在`async`函数中直接`return`一个值，那么`async`会把这个值通过`Promise.resolve()`封装成`Promise`对象。`Promise`的特点——无等待，所以在没有使用`await`的情况下，`async`会立即执行，不会阻塞后面的代码。

### await在等谁呢？

原以为，`await`在等待`async`的函数完成，等`async`的讯息。看了文档后，`await`等待的其实是一个表达式，这个表达式的计算结果是`Promise`对象或者其它值。
`await`不仅仅用于等`Promise`对象，它可以等任意表达式的结果，所以`await`是可以接普通函数的。

```js
    function getValue() {
        return 123;
    }

    async function testAsync() {
        return Promise.resolve("hello world");
    }

    async function test() {
        const v1 = await getValue();
        const v2 = await testAsync();
        console.log(v1, v2);// 123 hello world
    }

    test();
```

### await等到结果之后

返回`Promise`对象的处理结果，如果等待的不是`Promise`对象，则返回值本身。
`await`会在暂停`async`函数，等待`Promise`处理完成。
如果`Promise`正常处理，则回调的`resolve`函数参数作为`await`的值，继续执行`async`函数。
如果`Promise`处理异常，`await`会把Promise的异常原因抛出。

### 为啥要使用async/await

反正都是处理Promise对象，为啥不直接用.then()呢？用setTimeout模拟耗时的异步操作。

```js
    function mockApi() {
        return new Promise((resolve) => {
            setTimeout(() => resolve("hello world"), 1000);
        });
    }

    // then写法
    mockApi().then(v => {
        console.log("then", v);
    })

    // async/await写法
    async function test() {
        const v = await mockApi();
        console.log(v);
    }

test();
```

`async/await`反而要多写一点代码，繁重的工作雪上加霜了。😂😂😂😂😂
不要着急，脱裤子肯定不是为了...
单一的`Promise`链并不能发现`async/await`的妙用😏，当遇到多个`Promise`组成的`then`链时，你会发现`async/await`就是救世主。
试试看，一个业务处理分成多个步骤，每一步都是异步 的，而且每一步都依赖前一步的结果。
`setTimeout`受累一下😙😙😙

```js
/**
 * 传入参数value，表示这个函数执行的时间
 * 执行结果增加1000，用于下一步
 * @param {*} value 时间
 * @returns 时间+1000ms
 */
    function mockApi(value) {
    return new Promise((resolve) => {
        setTimeout(() => resolve(value + 1000), value);
    });
    }

    function step1(value) {
    console.log(`step1 with ${value}`);
    return mockApi(value);
    }

    function step2(value) {
    console.log(`step2 with ${value}`);
    return mockApi(value);
    }

    function step3(value) {
    console.log(`step3 with ${value}`);
    return mockApi(value);
    }

    // then写法

    function testThen() {
    console.time("testThen");
    const time1 = 300;
    step1(time1).then((time2) => {
        step2(time2).then((time3) => {
        step3(time3).then((result) => {
            console.log(`result is ${result}`); // 我已经晕了😵😵😵😵
            console.timeEnd("testThen");
        });
        });
    });
    }

    testThen();
    // step1 with 300
    // step2 with 1300
    // step3 with 2300
    // result is 3300
    // testThen: 3921.948ms

    // async/await写法
    async function testAsync() {
        console.time("testAsync");
        const time1 = 300;
        const time2 = await step1(time1);
        const time2 = await step3(time1);
        const result = await step3(time1);
        console.log(`result is ${result}`); // YYDS
        console.timeEnd("testAsync");
    }

    testAsync();
```

`async/await`有多清晰，不用多说了吧。

## 附庸风雅😳😳😳

**<font size=4>梅花引</font>** <font size=1>荆溪阻雪</font> <font size=2>蒋捷</font>

**白鸥问我泊孤舟**

*是身留？是心留？*

**心若留时，何事锁眉头？**

*风拍小帘灯晕舞*

**对闲影，冷清清，忆旧游。**

*旧游旧游今在不？*

**花外楼，柳下舟。**

*梦也梦也，梦不到，寒水空流。*

**漠漠黄云，湿透木棉裘。**

*都道无人愁似我*

**今夜雪，有梅花，似我愁。**
