# 父组件获取子组件值的四种情况

> 子组件获取父组件的值都知道，最基本的就是通过props传值即可，同样也可以使用context

由于是单向数据流，那么父组件是怎么获取子组件的值呢。

对**类组件**，**函数组件**都进行梳理

## 均为类组件

**使用 ```ref```**


```JavaScript
import React, { Component } from 'react';
import ChildClass from './ChildClass';

class FatherClass extends Component {
  constructor(props) {
    super(props);
    this.state = {};
    this.child = React.createRef();
  }

  getValue = () => {
    console.log('this.child', this.child);
  };

  render() {
    return (
      <div>
        <ChildClass ref={this.child} />
        <button onClick={this.getValue}>获取输入框内容</button>
      </div>
    );
  }
}

export default FatherClass;
``` 

```JavaScript
import React, { Component } from 'react';

class ChildClass extends Component {
  constructor(props) {
    super(props);
    this.state = {
      value: '',
    };
  }

  handleChange = (e) => this.setState({ value: e.target.value });

  render() {
    const { value } = this.state;
    return (
      <div>
        <input value={value} onChange={this.handleChange} />
      </div>
    );
  }
}

export default ChildClass;
``` 

打印的日志如下

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666232470263/_BM5geaxd.png align="left")

这样就可以获取子组件的所有值

## 均为函数组件


```JavaScript
import React, { useRef } from 'react';
import ChildFun from './ChildFun';

function FatherFun() {
  const childRef = useRef();

  const getValue = () => {
    console.log('childRef', childRef);
  };

  return (
    <div>
      <ChildFun ref={childRef} />
      <button onClick={getValue}>获取输入框内容</button>
      <button onClick={() => childRef.current?.handleChange('666')}>
        父组件修改输入框的值为666
      </button>
    </div>
  );
}

export default FatherFun;
``` 


```JavaScript
import React, { useState, forwardRef, useImperativeHandle } from 'react';

function ChildFun(props, ref) {
  const [value, setValue] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  useImperativeHandle(ref, () => ({ value, handleChange: (res) => setValue(res) }));

  return (
    <div>
      <input value={value} onChange={handleChange} />
    </div>
  );
}

export default forwardRef(ChildFun);
``` 

打印的日志如下

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666232588307/x6ekPhX_c.png align="left")

与类组件不同的是

1. 父组件使用 ```useRef``` 创建 ```ref```
2. 子组件需要使用 ```useImperativeHandle```，```forwardRef```
3. ```useImperativeHandle```可以在使用 ```ref``` 时自定义暴露给父组件的实例值 （也就是说只有```value```，```handleChange``` 会暴露给父组件，组件其他的内容不会暴露给父组件）

## 父组件是类组件，子组件是函数组件
这种方式很常见，只需将上面代码中 ```FatherClass``` 与 ```ChildFun``` 这两个组件组合即可


```JavaScript
import React, { Component } from 'react';
import ChildFun from './ChildFun';

class FatherClass extends Component {
  constructor(props) {
    super(props);
    this.state = {};
    this.child = React.createRef();
  }

  getValue = () => {
    console.log('this.child', this.child);
  };

  render() {
    return (
      <div>
        <ChildFun ref={this.child} />
        <button onClick={this.getValue}>获取输入框内容</button>
        <button onClick={() => this.child.current?.handleChange('666')}>
          父组件修改输入框的值为666
        </button>
      </div>
    );
  }
}

export default FatherClass;
``` 

```JavaScript
import React, { useState, forwardRef, useImperativeHandle } from 'react';

function ChildFun(props, ref) {
  const [value, setValue] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  useImperativeHandle(ref, () => ({ value, handleChange: (res) => setValue(res) }));

  return (
    <div>
      <input value={value} onChange={handleChange} />
    </div>
  );
}

export default forwardRef(ChildFun);
``` 

打印的日志如下


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666232829738/T9fRj4w8j.png align="left")

## 父组件是函数组件，子组件是类组件


```JavaScript
import React, { useRef } from 'react';
import ChildClass from './ChildClass';

function FatherFun() {
  const childRef = useRef();

  const getValue = () => {
    console.log('childRef', childRef);
  };

  return (
    <div>
      <ChildClass ref={childRef} />
      <button onClick={getValue}>获取输入框内容</button>
      <button onClick={() => childRef.current?.customChange('666')}>
        父组件修改输入框的值为666
      </button>
    </div>
  );
}

export default FatherFun;
``` 


```JavaScript
import React, { Component } from 'react';

class ChildClass extends Component {
  constructor(props) {
    super(props);
    this.state = {
      value: '',
    };
  }

  customChange = (res) => {
    this.setState({
      value: res,
    });
  };

  handleChange = (e) => this.setState({ value: e.target.value });

  render() {
    const { value } = this.state;
    return (
      <div>
        <input value={value} onChange={this.handleChange} />
      </div>
    );
  }
}

export default ChildClass;
``` 











