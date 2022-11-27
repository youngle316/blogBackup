# EditableProTable 组件的 onSave 方法如何保持编辑状态

问题：最近项目中使用了 [EditableProTable](https://procomponents.ant.design/components/editable-table) 这个组件，不论新建还是编辑时，点击保存后无论接口返回成功还是失败，编辑状态会直接消失。现在需要的是失败后保持编辑状态，可以修改再次保存。

解决：

我们先看文档中 onSave 属性的使用方法

![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/A1BEFB33-8146-4A06-9601-2CE75D072E63/BEE91F24-AFF2-473E-91BF-DD03E285163E_2/xv2VMpD7QDgUcnDz8zem7WxACBq9jQHUqYsC2Girycsz/Image.png)

该方法返回一个 `Promise`，所以就可以在接口返回错误时，返回 `Promise.reject()` 即可。

参考：[🧐[问题]EditableProTable的onSave方法，发送请求得到失败结果时，希望能保持当前行编辑](https://github.com/ant-design/pro-components/issues/4101)