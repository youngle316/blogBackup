# Reduce的用法与使用场景

[Reduce](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 是 ```Array``` 中内置的函数。相比于其他提供的函数，```Reduce``` 的参数较多，是功能强大的一个方法，同时也在面试中常见的。

```Reduce```的功能很强大，可以模拟和替代 ```Array``` 别的函数，但是也不是必须使用 ```Reduce``` 还是得看具体情况。

## 概念
MDN官方给出的解释为

> **reduce()** 方法对数组中的每个元素按序执行一个由您提供的 **reducer** 函数，每一次运行 **reducer** 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值。

> 第一次执行回调函数时，不存在“上一次的计算结果”。如果需要回调函数从数组索引为 0 的元素开始执行，则需要传递初始值。否则，数组索引为 0 的元素将被作为初始值 *initialValue*
，迭代器将从第二个元素开始执行（索引为 1 而不是 0）。

简单的举个例子：如计算数组元素的总和


```JavaScript
const arr = [1, 2, 3, 4];
const result = arr.reduce((preValue, curValue) => preValue + curValue);
console.log('result', result);
``` 

### 语法

```JavaScript
reduce(callbackFn, initialValue)
``` 

**参数**
reduce 接收两个参数

`callbackFn` 回调函数包含四个参数

- `previousValue`：上一次调用 `callbackFn`时的返回值。在第一次调用时，若指定了初始值`initialValue`，则其值为 `initialValue`，否则为数组索引为 0 的元素 `array[0]`
- `currentValue`：数组中正在处理的元素。在第一次调用时，若指定了初始值 `initialValue`，其值则为数组索引为 0 的元素 `array[0]`，否则为 `array[1]`
- `currentIndex`：数组中正在处理的元素的索引。若指定了初始值 `initialValue`，则起始索引号为 0，否则从索引 1 起始
- `array`：用于遍历的数组

`initialValue`：(可选)

作为第一次调用 `callback`函数时参数 `previousValue` **的值。若指定了初始值 `initialValue`
，则 `currentValue`则将使用数组第一个元素；否则 `previousValue`将使用数组第一个元素，而 `currentValue`将使用数组第二个元素

**返回值**

使用回调函数遍历整个数组后的结果

## 基本使用

`reduce` 方法的使用分为有无 `initialValue`

下面使用一个求数组和的例子演示一下

### 无 initialValue


```JavaScript
const arr = [1, 2, 3, 4, 5]
const result = arr.reduce((preValue, curValue, curIndex, array) => {
  console.log('value', preValue, curValue, curIndex, array)
  return preValue + curValue
})
console.log('result', result)
``` 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667632961676/07D1d7O82.png align="left")

### 有 initialValue


```JavaScript
const arr = [1, 2, 3, 4, 5]
const result = arr.reduce((preValue, curValue, curIndex, array) => {
  return preValue + curValue
}, 100)
console.log('result', result)
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667633072044/Yt6Nunf9E.png align="left")

## 使用场景
reduce 的使用场景很多，功能强大

### 数组求和

```JavaScript
const arr = [1, 2, 3, 4, 5]
const result = arr.reduce((preValue, curValue, curIndex, array) => {
  return preValue + curValue
}, 0)
``` 

### 数组对象求和

```JavaScript
const arr = [{ x: 1 }, { x: 2 }, { x: 3 }, { x: 4 }, { x: 5 }]
const result = arr.reduce((preValue, curValue, curIndex, array) => {
  console.log('value', preValue, curValue, curIndex, array)
  return preValue + curValue.x
}, 100)
``` 

### 数组去重

```JavaScript
const arr = [1, 2, 1, 3, 4, 5, 3, 8]
let map = {};
const result = arr.reduce((preValue, curValue) => {
  if (!map[curValue]) {
    map[curValue] = true;
    preValue.push(curValue)
  }
  return preValue
}, [])
``` 

### 二维数组转为一维数组

```JavaScript
const arr = [[1, 2], [3, 4], [5, 6], [7, 8]]
const result = arr.reduce((preValue, curValue) => {
  return preValue.concat(curValue)
})
``` 