# 如何在 antd form 或 高阶组件 中使用 ref

> **问题**：使用 ```antd 3``` 版本 ```form``` 组件，使用 ```ref``` 获取不到子组件的值，而是 ```form``` 的属性及方法 

有个老项目使用的是 ```antd 3``` 版本，```form``` 组件还是高阶组件的形式。再做一个需求时是要父组件获取子组件的 ```state```，这个很简单，在这篇文章 [父组件获取子组件值的四种情况](https://younglele.cn/four-cases-where-a-parent-component-gets-a-child-component) 中已经介绍过了，但是问题在于，子组件被 form 套了一层，用文章中的方法获取的是 form 的属性，而非子组件的值。

**解决**：使用 ```wrappedComponentRef```


```JavaScript
// 父组件
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
        // ref 获取的是 form 组件的属性及方法
        {/* <ChildFun ref={this.child} />  */}
        // wrappedComponentRef 返回的是子组件 useImperativeHandle 方法返回的 value 和 handleChange
        <ChildFun wrappedComponentRef={this.child} />
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
// 子组件
import React, { useState, forwardRef, useImperativeHandle } from 'react';
import { Form } from 'antd';

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

export default forwardRef(Form(ChildFun));
```