Preview: [https://luweij.github.io/react-adapter/demo/dist/](https://luweij.github.io/react-adapter/demo/dist/)
# ListView
## 安装、调试
```js
$ npm i
$ npm run start
// 访问 http://localhost:3000/
```

## 单元测试
```js
$ npm i
$ npm run test
```

## What? Why?

主要解决react渲染超长（如1万条）listview的性能问题

原理：通过计算适配，每次只渲染可见的部分

## How to use?
```javascript
import ListView from '../ListView';
const { List, Adapter } = ListView;
// 继承并实现适配器
class MyAdapter extends Adapter {
	getDataArr() {
    // 该部分是返回内容数组
		return [1,2,3,4,5];
	}

	getItem(data, index) {
    // 返回每条 item 对应内容。data 既 getDataArr 的单例。index 对应 getDataArr 数组下标。
		return <div>{data}</div>;
	}

	getItemHeight(i) {
    // 高度可以根据 item 类型自定义分配，如果每条高度一致可返回固定值。
		return 50;
	}
}
const adapter = new MyAdapter();
// 渲染 200高度的 list,如果每行是50，这样最多同时渲染5条 item。尤其在复杂item情况下极大节约出DOM性能。
<div style={{ height: '200px' }}><List adapter={adapter}/></div>
```

<a name="API"></a>
### API
List 组件所涉及的 API 通常集中在 adapter 部分。其中主要的对外函数为 updateList、checkItem、onScroll
<a name="82ba9fd3"></a>
#### updateList():void
该函数主要用于数据 getDataArr 数据发生变化时调用，以更新渲染。
<a name="c7da9ced"></a>
#### checkItem(index):void
该函数用于快速指定需要选中的部分。主要用于处理数据过长时候的item展示。
<a name="49ace86d"></a>
#### onScroll(index):void
该函数主要用于解决集合 antd Slider 类似组件展示滚动条搭配。可以理解为每次 item change 都会触发该条。
