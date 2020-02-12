# Redux

Redux 是一个用来管理数据状态和 UI 状态的 JavaScript 应用工具

```js
npm install --save redux
```

> 官网附加包

```js
npm install --save react-redux
npm install --save-dev redux-devtools
```

![img](http://ww3.sinaimg.cn/large/006y8mN6jw1f9g7n28043j30hq0dbdgo.jpg)

---

### create-react-app 快速生成开发环境

```js
npm install -g create-react-app
```

### 安装 Ant Design

```js
npm install antd --save
```

##### 使用 Ant Design

```js
import "antd/dist/antd.css";
import { Input } from "antd";
```

### 使用 redux

```js
npm install redux --save
```

### redux todolist 实战

```js
// store index.js
// import { createStore } from 'redux'  // 引入createStore方法
// import reducer from './reducer'
// const store = createStore(reducer)          // 创建数据存储仓库
// // window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
// window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
// export default store                 //暴露出去

import { createStore } from "redux";
import reducer from "./reducer";

export default createStore(reducer);
```

```js
// store reducer.js
// const defaultState = {
//   inputValue: 'Write Something',
//   list: [
//     '早上4点起床，锻炼身体',
//     '中午下班游泳一小时'
//   ]
// }  //默认数据
// export default (state = defaultState, action) => {  //就是一个方法函数
//   return state
// }

const defaultState = {
  inputValue: "Write Something",
  list: ["早上4点起床，锻炼身体", "中午下班游泳一小时222"]
};

export default (state = defaultState, action) => {
  // console.log(state, action);
  if (action.type === "changeInput") {
    // Object.assgin()
    let newState = JSON.parse(JSON.stringify(state));
    newState.inputValue = action.value;
    return newState; // 程序退出并返回
  }
  if (action.type === "addItem") {
    let newState = JSON.parse(JSON.stringify(state));
    if (newState.inputValue === "") return;
    newState.list.push(newState.inputValue);
    newState.inputValue = "";
    return newState;
  }
  if (action.type === "removeItem") {
    let newState = JSON.parse(JSON.stringify(state));
    newState.list.splice(action.index, 1);
    return newState;
  }
  return state;
};
```

```js
// import React, { Component, Fragment } from 'react'

// export default class App extends Component {
//   constructor(props) {
//     super(props);
//     this.state = {}
//   }
//   render() {
//     return (
//       <Fragment>

//       </Fragment>
//     )
//   }
// }

import React, { Component, Fragment } from "react";
import "antd/dist/antd.css";
import { Input, Button, List } from "antd";
import store from "./store/index";

export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = store.getState();
    this.changeInputValue = this.changeInputValue.bind(this);
    this.storeChange = this.storeChange.bind(this);
    this.addValue = this.addValue.bind(this);
    // this.removeItem = this.removeItem.bind(this);
    store.subscribe(this.storeChange);
  }
  changeInputValue(e) {
    // console.log(e.target.value);
    let action = {
      type: "changeInput",
      value: e.target.value
    };
    store.dispatch(action); // dispatch 一个action;
  }
  storeChange() {
    this.setState(store.getState);
  }
  addValue() {
    let action = {
      type: "addItem"
    };
    store.dispatch(action);
  }
  removeItem(index) {
    console.log(index);
    let action = {
      type: "removeItem",
      index
    };
    store.dispatch(action);
  }

  render() {
    return (
      <Fragment>
        <Input
          placeholder="请输入"
          style={{ width: "240px", margin: "10px" }}
          onChange={this.changeInputValue}
          value={this.state.inputValue}
        ></Input>
        <Button type="primary" onClick={this.addValue}>
          Add
        </Button>
        <div style={{ margin: "10px", width: "300px" }}>
          <List
            bordered
            dataSource={this.state.list}
            renderItem={(item, index) => (
              <List.Item onClick={this.removeItem.bind(this, index)}>
                {item}
              </List.Item>
            )}
          />
        </div>
      </Fragment>
    );
  }
}
```

### 分离 Action Type

> 在 src/store 文件夹下面，新建立一个 actionTypes.js 文件

```js
export const CHANGE_INPUT = "changeInput";
export const ADD_ITEM = "addItem";
export const DELETE_ITEM = "deleteItem";
```

### 分离 actionCreatores.js

```js
import { CHANGE_INPUT, ADD_ITEM, DELETE_ITEM } from "./actionTypes";

export const changeInputAction = value => ({
  type: CHANGE_INPUT,
  value
});

export const addItemAction = () => ({
  type: ADD_ITEM
});

export const deleteItemAction = index => ({
  type: DELETE_ITEM,
  index
});
```

### 组件拆分

### 异步请求 axios

### Redux-thunk

```js
npm install --save redux-thunk
```
