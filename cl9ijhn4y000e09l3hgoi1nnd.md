# 闭包及闭包在React中的问题

> 🤺 闭包就是可以读取其他函数内部变量的函数 — [阮一峰博客](https://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

## 闭包常见的考题

### 输出的结果  


```JavaScript
for (var i = 0; i < 10; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
// 一秒后，输出了 10 个 10
``` 
出现这种原因就是用 ```var``` 声明的变量，没有块级作用域，所以限定不了 ```var``` 声明变量的访问范围

问：如果要输出 0 - 9 那该怎么改呢

1.简单方法：将 ```var``` 改成 ```let```

```JavaScript
// 简单。常用改法。将 var 改成 let
for (let i = 0; i < 10; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

2.就是使用 [IIFE](https://developer.mozilla.org/zh-CN/docs/Glossary/IIFE)

```JavaScript
// 使用 iife 实现块级作用域
// _i 保存着 iife 对 i 的引用
// 其实也就是闭包
for (var i = 0; i < 10; i++) {
  ((_i) => {
    setTimeout(() => {
      console.log(_i);
    }, 1000);
  })(i);
}
``` 

3.当然也可以使用 ```setTimeout``` 本身解决。接受第三个参数


```JavaScript
for (var i = 0; i < 10; i++) {
  setTimeout((_i) => {
      console.log(_i);
    }, 1000, i);
}
``` 

## ```React``` 与 闭包的问题

### ```useState``` 的闭包问题


```JavaScript
import React, { useState } from 'react';

function FunComponent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    setTimeout(() => {
      console.log('count', count);
    }, 1000);
  };

  return (
    <div>
      <p>你点击了 {count} 次</p>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

export default FunComponent;
``` 

问：快速点击三次后，这段代码会输入什么？

正常看好像应该是输出 三个 3，但是实际上是 0，1，2，这是什么原因导致的呢？

其实这也是和闭包有关系的，```setTimeout``` 回调函数就是一个闭包，里面的 ```count``` 的值来自于函数组件 ```FunComponent``` ```state``` 的 ```count``` ；而且这个 ```count ```在闭包中是不会被销毁的

所以具体的调用过程如下：

- 第一次点击，第一个定时器回调函数获取的 ```count``` 等于 0，一秒之后就会打印 0
- 组件重新渲染，```count``` 变为 1
- 第二次点击，第二个定时器回调函数获取的 ```count``` 此时等于 1，一秒之后就会打印 1
- 之后的以此类推

那么为了让打印出 3，3，3，该怎么修改呢？

### 使用 ```useRef``` 来解决

```JavaScript
+ import React, { useState, useRef } from 'react';

function FunComponent() {
  const [count, setCount] = useState(0);
+  const countRef = useRef(count);

  const handleClick = () => {
    setCount(count + 1);
+    countRef.current = count + 1;
    setTimeout(() => {
      console.log('count', count);
+      console.log('countRef.current', countRef.current);
    }, 1000);
  };

  return (
    <div>
      <p>你点击了 {count} 次</p>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

export default FunComponent;
```


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666358607350/LECb0gGCQ.png align="left")

通过添加上面几行代码就可以实现打印3，3，3的功能。

但是这样每次都要写 ```countRef.current = count + 1``` 很不优雅，可以通过下面的方式来进行优化

### 使用 ```useEffect``` 来优化

```JavaScript
import React, { useState, useRef, useEffect } from 'react';

function FunComponent() {
  const [count, setCount] = useState(0);
  const countRef = useRef(count);

  useEffect(() => {
    countRef.current = count;
  });

  const handleClick = () => {
    setCount(count + 1);
    setTimeout(() => {
      console.log('count', count);
      console.log('countRef.current', countRef.current);
    }, 1000);
  };

  return (
    <div>
      <p>你点击了 {count} 次</p>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

export default FunComponent;
```

组件重新渲染后就会触发 ```useEffect``` 方法，将最新的值赋给 ```current``` 即可，无需再将 ```count + 1```

但是还是可以接着优化，使用```自定义 hook``` 来实现获取最新的 ```state```

### 使用 ```自定义hook``` 获取最新的 ```state```

```JavaScript
import { useEffect, useRef } from 'react';

/**
 * 自定义Hook -- 获取最新的 state 的值
 * @param value
 */
export const useGetCurrentValue = (value: any) => {
  const ref = useRef(value);
  useEffect(() => {
    ref.current = value;
  }, [value]);
  return ref;
};
```

```JavaScript
import React, { useState } from 'react';
import { useGetCurrentValue } from '../../hook/useGetCurrentValue';

function FunComponent() {
  const [count, setCount] = useState(0);
  const countRef = useGetCurrentValue(count);

  const handleClick = () => {
    setCount(count + 1);
    setTimeout(() => {
      console.log('count', count);
      console.log('countRef.current', countRef.current);
    }, 1000);
  };

  return (
    <div>
      <p>你点击了 {count} 次</p>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

export default FunComponent;
```

问：如果自定义 hook return的是ref.current，为什么会有问题 ?

## ```useEffect``` 的闭包问题

同样，在 ```useEffect``` 中也有这样的问题

```JavaScript
import React, { useEffect, useState } from 'react';

function FunComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCount(count + 1);
    }, 1000);
  }, []);

  return (
    <div>
      <p>{count}</p>
    </div>
  );
}

export default FunComponent;
```

会发现，页面上的 ```count``` 到 1 之后就不变了，还以为是卡了。出现这种原因还是因为闭包。```setInterval``` 的回调函数还是一个闭包，```count ``` 就是初始值 0，并且不会被销毁且一直存在，所以就相当于一直是 0 + 1，所以 ```count``` 一直是 1。

解决方法如下：


```JavaScript
import React, { useEffect, useState } from 'react';

function FunComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCount((c) => c + 1);
    }, 1000);
  }, []);

  return (
    <div>
      <p>{count}</p>
    </div>
  );
}

export default FunComponent;
``` 

这样就可以获取到最新的 ```state``` 了

闭包的内容就到这里了。后面再讲讲 ```useRef``` 的原理   👋🏻







