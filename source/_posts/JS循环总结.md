---
title: JS循环总结
date: 2022-03-26 11:07:01
tags:
    - 循环
categories: [前端]
---

## JS循环总结

工作发现，项目中的对于对象、数组的循环，使用的方法各不相同，有`forEach`、`for in`、`for of`、`map`以及`for`，故对这些循环做些总结。

### forEach（数组方法）

#### 特性

- 遍历的时候更加简洁，效率和for循环相同，不用关系集合下标的问题，减少了出错的概率。
- 没有返回值。
- 不能使用break中断循环，不能使用return返回到外层函数。

#### 实例

```js
    const array = [1, 2, 3];
    let newArray = array.forEach(item => {
        item+=1;
        console.log(item);// 2 3 4
    })
    console.log(array); // [1, 2, 3]
    console.log(newArray) // undefined
```

### for in

#### 特性

更适合遍历对象，可以遍历数组，但是会有一些局限性。

#### 实例

`for in`的索引为字符串型数字，不能直接进行几何运算

```js
    const array = [1, 2, 3];
    for(const i in array) {
        const res = i + 1;
        console.log(res);
    }
    // 01 11 21
```

遍历顺序有可能不是按照实际数组的内部顺序，使用`for in`会遍历数组所有的可枚举属性，包括原型，如果不想遍历原型方法和属性的话，可以在循环内部判断一下，使用`hasOwnProperty()`方法可以判断某属性是不是该对象的实例属性

```js
    const array = [1, 2, 3];
    Array.prototype.a = 123;
    for (const index in array) {
        const res = array[index];
        console.log(res);
    }
    // 1 2 3 123

    for (const index in array) {
        if (array.hasOwnProperty(index)) {
            const res = array[index];
            console.log(res);
        }
    }
    // 1 2 3
```

### for of

#### 特性

- 可遍历map/objet/array/set/string等
- 避免了`for in`的所有缺点，可以使用break、continue和return，不支持数组的遍历，还可以遍历类似数组的对象。

#### 实例

`for of`是ES6的新语法，为了弥补`for in`的局限性。
`for of`遍历的数组元素值，而且`for of`遍历的只是数组内的元素，不包括原型属性和索引

```js
    const array = [1, 2, 3];
    array.a = 123;
    Array.prototype.a = 123;
    for(const value of array) {
        console.log(value);
    }
    // 1 2 3
```

`for of`适用遍历拥有迭代器对象的集合，但是不能遍历对象，因为没有迭代器对象，但如果想遍历对象，可以用内建的`Object.keys()`方法

```js
    const myObject = {
        a: 1,
        b: 2,
        c: 3
    };
    for (const key of Object.keys(myObject)) {
        console.log(key + "：" + myObject[key]);
    }
    //> "a：1" "b：2" "c：3
```

### map （数组方法）

#### 特性

- map不改变原数组但是会返回新数组
- 可以使用break中断循环，可以使用return返回到外层函数

#### 实例

```js
    const array = [1, 2, 3];
    const newArray = array.map(index => {
        return index+= 1;
    })
    console.log(array);// [1, 2 , 3]
    console.log(newArray);//  [2, 3 , 4]
```

**在大地上我们只过一生。 ----叶赛宁**
