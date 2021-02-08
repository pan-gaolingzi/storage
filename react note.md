# 一、安装

### 1. 安装nodejs

www.nodejs.org 下载

查看版本

```
npm -v
node -v
```

### 2. 安装脚手架工具

常用的脚手架工具有webpack, grunt culp等，官方脚手架工具是create-react-app

```
npm install -g create-react-app
```

### react特点

![1606918427919](react note.assets/1606918427919.png)

1. 声明式开发：通过改变数据来改变dom，而不是直接操作dom（命令式）
2. 可以与其他框架并存：angularjs等
3. 组件化
4. 单项数据流：父组件可以向子组件传值，子组件可以使用父组件的值，但最好不要改变父组件的值
5. 视图层框架：树形结构，一旦项目组件层级多了，末端组件之间的交流成本很高（单继承？）
6. 函数式编程 方便自动化测试

# 二、项目搭建

跳转到目标路径，创建项目

```
create-react-app project_name
```

项目文件的组成

1. yarn.lock: 依赖说明

2. readme

3. package.json: node依赖包的版本信息，启动指令等

4. node_modules: node library

5. public:

   - favixon.ico: 显示在标题栏的缩略图，可以将其他图片改成这个名字来替换
   - index.html
   - manifest.json：定义快捷方式（图标，url等）PWA的组成部分，不需要可删除

6. src: 源代码文件夹

   - index.js: 整个程序执行入口 -- 引入其他js文件，渲染
   - App.test.js: 自动化测试文件

   

App.js

```
import React from 'react';

class App extends React.Component {

}



// 写法二
import React, {Component} from 'react';

class App extends Component {

}

// 写法三
import React from 'react';
const Component = React.Component

class App extends React.Component {}
```

### ReactDOM

第三方组件，其中render方法可以将某个组件挂载到某个dom节点上。

```
import React from 'react';
ReactDOM.render(<App />, document.getElementById('root'));   // <App /> jsx语法，需要引入react才能编译
```

<b>组件名大小写敏感，开头大写</b>

### 构造函数

constructor(){}优先被调用



immutable原则：不要直接改变state里的变量

要通过setState方法给变量赋值

### 注释

```
{/* */}
单行注释 //和}不能在同一行，不然会被注释掉
{
//
}
```

### className

style的class要改为className，不然容易和类class混淆

### 转义

dangerouslySetInnerHTML={{__html: xxx}}

外层括号代表js表达式，内层对象为js对象

```jsx
<li key={index} onClick={this.handleItemDelete.bind(this, index)
dangerouslySetInnerHTML={{__html: item}}}></li>
```

label的for（绑定）要改为htmlFor，不然容易和for循环关键字混淆。



### 组件拆分 组件传值

父组件通过属性值给子组件传值

子组件通过{this.props.attribute_name}接收父组件的值

### ES6 setState写法

```react
  handleBtnAdd() {
    this.setState({
      list: [...this.state.list, this.state.inputValue],   // ...this.state.list put the origin content into the new list here
      inputValue: ''
    })
  }
  // es6 返回函数
  handleBtnAdd() {
    this.setState((prevState) => ({
              list: [...prevState.list, prevState.inputValue],   // ...this.state.list put the origin content into the new list here
      inputValue: ''
    }))
  }
```

<b>key值要放在循环的最外层</b>



### 参数定义

#### PropTypes -- 参数验证

```
NoteItem.propTypes = {
  content: PropTypes.string,
  deleteItem: PropTypes.func,
  index: PropTypes.number
}
```

如果传递的值类型不符合，会生成warning，但不会阻止程序运行。

仅仅定义传递的类型，如果父组件没有传递该变量，是不会报错的。

如果想规定参数必须被传递，验证时加上isRequired

```
NoteItem.propTypes = {
  test: PropTypes.string.isRequired,
  content: PropTypes.string,
  deleteItem: PropTypes.func,
  index: PropTypes.number
}
```

此时，不传test参数会报undefined warning

arrayOf允许接收多种类型，这种写法要求对象是一个数组：

```
NoteItem.propTypes = {
  test: PropTypes.string.isRequired,
  content: PropTypes.arrayOf(PropTypes.number, PropTypes.string),
  deleteItem: PropTypes.func,
  index: PropTypes.number
}
```

想要对象为列举类型中的一种，应该用oneOfType：

```
NoteItem.propTypes = {
  test: PropTypes.string.isRequired,
  content: PropTypes.oneOfType([PropTypes.number, PropTypes.string]),
}
```



#### DefaultProps -- 定义默认值



### ref

```
  <textarea
    id="insertArea"
    className="textarea"
    value={this.state.inputValue}
    onChange={this.handleInputChange}
  />
          
  handleInputChange(e) {
    const value = e.target.value;
    this.setState(() => ({
      inputValue: value
    }));
  }
  
  等价于：
    <textarea
    id="insertArea"
    className="textarea"
    value={this.state.inputValue}
    onChange={this.handleInputChange}
    ref= {(input) => {this.input = input} }    // 把input指向绑定到该对象(textarea)
  />
          
  handleInputChange() {
    this.setState(() => ({
      inputValue: this.input.value
    }));
  }
```

#### 注意setState的异步问题

通过ref方法获取dom节点，可能不会得到最新信息，因为它可能在setState之前执行。如果要确保获取到更新完成的dom，需要写入setState的回调函数。

```
  handleBtnAdd() {
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],   // ...this.state.list put the origin content into the new list here
      inputValue: ''
    }), () => {
      console.log(this.ul.querySelectorAll('div').length);
    });
  }
```

### 发送ajax请求

#### 添加依赖

在项目根目录下执行：yarn add axios(某个比较靠谱的库)

```
import axios from 'axios';

componentDidMount() {
  axios.get('/api/todolist')
    .then(() => {alert('success')})
    .catch(() => {alert('error')})
}
```

### Charlse mock

安装Charlse模拟前端发送请求。

Tools --> map local -->  add

![1607698848643](react note.assets/1607698848643.png)

### 使用react-transition-group

包含3个组件：Transition,CSSTransition, TransitionGroup

#### CSSTransition

只能包裹一个元素

1. js中引入

   ```
   import CSSTransition from 'react-transition-group';
   
   ```

2. 将动画对象组件用<CSSTransiton></CSSTransition>包裹起来，并设置属性

```
<CSSTransition
  in={this.state.show}        // 状态改变flag
  timeout={2000}              // 动画时长2s
  classNames='fade'           // 动画类群前缀为fade
  unmountOnExit               // 出场动画结束后移除组件
  onEntered={(el) => {el.style.color='blue'}}  // 钩子函数，在某个生命周期自动执行，传入元素，添加属性等
>
  <div>hello</div>
</CSSTransition>
```

3. css编辑状态

```
// 进入前状态
.fade-enter {
	opacity: 0;
}

// 进入中状态
.fade-enter-active {
	opacity: 1;
	transition: opacity 2s ease-in;
}

// 进入后状态
.fade-enter-done {
	opacity: 1;
}
```

4. 写控制状态的方法

```
  constructor(props) {
    super(props);
    this.state = {
      show: true
    }
    this.handleToggle = this.handleToggle.bind(this);
  }
  
  <button onClick={this.handleToggle}>toggle</button>
  
  handleToggle() {
    this.setState({
      show: this.state.show ? false : true
    })
  }
```



### build

```
npm install -g serve
npm run-script build
npm clean-install

serve -s build
```

### CSS注意事项

react中，一个css文件被某个js文件引入，对所有js文件都生效。所以，这种方式很容易造成冲突。

#### styled components

可解决css管理问题

将css代码放到js文件中

直接在style.js中创建带css样式的组件

```
import styled from 'styled-components';
import logoPic from '../../statics/logo.jpeg';

export const HeaderWrapper = styled.div`
  position: relative;
  height: 56px;
  border-bottom: 1px solid #f0f0f0;
`;

export const Logo = styled.a`
  position: absolute;
  height: 54px;
  width: 54px;
  display: block;      // inline element can't set width
  top: 0;
  left: 0;
  background: url(${logoPic});   // url('../../statics/logo.jpeg') is wrong because it treats '../' as a string
  background-size: contain; 
`;
```

##### 引入图片

不能写<span style='color:grey'>url('../../statics/logo.jpeg')</span>，打包时会被当成字符串

需要import

```
import logoPic from '../../statics/logo.jpeg';

...{
	background: url(${logoPic});
}

```

##### 添加属性

- 方法1，在容器组件中定义：

```
  render() {
    return (
      <HeaderWrapper><Logo href='/'/></HeaderWrapper>
    )
  }
```

- 方法2，在style.js中定义，通过.attrs

```
export const Logo = styled.a.attrs({href: '/'})`
  position: absolute;
  height: 52px;
`;
```

##### 使用新版 createGlobal() 方法

1. 在src目录下新建**common.js**文件作为样式文件，对**common.js**做修改。
    引入新的API **createGlobalStyle** ，在下面的创建一个 **GlobalStyle** 变量，用 **createGlobalStyle**  方法把全局样式包裹在其中（包裹css样式使用反引号字符串）：



```jsx
import { createGlobalStyle } from 'styled-components';
export const GlobalStyle = createGlobalStyle `
    body {
        line-height: 1;
    }
`
```

1. 修改项目入口文件App.js（一般默认指 src 目录下的App.js），引入
    `import { GlobalStyle } from './common.js` 和`<GlobalStyle/>`
    如下



```jsx
import React from 'react';
import { GlobalStyle } from './common.js'

function App() {
  return (
    <div className="App">
        <GlobalStyle/>
    </div>
  );
}

export default App;
```

#### brower variations

reset.css

### 前端素材

#### iconfont

https://www.iconfont.cn/

资源管理 > 我的项目

![1610802945851](react note.assets/1610802945851.png)

添加项目

![1610803039118](react note.assets/1610803039118.png)

挑选图标，放入购物车

![1610803517214](react note.assets/1610803517214.png)

点击右上角购物车

![1610803605219](react note.assets/1610803605219.png)

添加至项目

![1610803643028](react note.assets/1610803643028.png)

图标库就创建好了，点击下载至本地

![1610803735292](react note.assets/1610803735292.png)

解压。下面几个文件是有用的：

iconfont.eot -- 图标     iconfont.svg    iconfont.ttf       iconfont.woff   iconfont.css

将文件放入statics文件夹里，最好新建一个文件夹。

修改iconfont.css文件，添加相对路径

```
@font-face {font-family: "iconfont";
  src: url('./iconfont.eot?t=1610803647615'); /* IE9 */
  src: url('./iconfont.eot?t=1610803647615#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAboAAsAAAAADFQAAAaaAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVg...AAA==') format('woff2'),
  url('./iconfont.woff?t=1610803647615') format('woff'),
  url('./iconfont.ttf?t=1610803647615') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('./iconfont.svg?t=1610803647615#iconfont') format('svg'); /* iOS 4.1- */
}
```

如果要用styled component进行全局管理，还需要做下述修改：

- 把iconfont.css重命名为iconfont.js
- 引入injectGlobal （4.0以后的版本引入createGlobalStyle）

```
import { injectGlobal } from 'styled-components';

injectGlobal`将代码包进来`;

// 4.0+版本
import { createGlobalStyle } from 'styled-components';
```

- 在入口index.js中引入该文件

  - 打开demo.html文件，复制下列代码

    ![1610869725174](react note.assets/1610869725174.png)

  - 找到图标对应的unicode，替换span里的内容，并将class改为className

    ![1610869817924](react note.assets/1610869817924.png)

```
      <NavItem className='right'><span className="iconfont">&#xe636;</span></NavItem>
```

如果引入方式用symbol

​              <use xlink:href="#icon-xxx"></use>

要改为

```
<use xlinkHref="#icon-xxx"></use>
```



## 三、底层原理

### 1. 通过改变数据更新页面显示

当组将的state或者props发生改变的时候，render函数就会重新执行。

当父组件的render函数被运行时，相关子组件的render函数也将被运行。

### 2. 虚拟dom

页面渲染方式一：

- state 数据
- jsx 模板
- 数据 + 模板 --> 生成真实的dom
- state 发生改变
- 数据 + 模板 --> 生成真实的dom，替换原来的dom

缺陷：每次都生成一个完整的dom，非常耗性能

页面渲染方式二：

- state 数据
- jsx 模板
- 数据 + 模板 --> 生成真实的dom
- state 发生改变
- 数据 + 模板 --> 生成真实的dom
- 新旧dom对比，找差异
- 只用变化的元素替换原来dom的元素

缺陷：比较dom也消耗性能，提升并不明显

react页面渲染方式：

- state 数据

- jsx 模板

- 生成虚拟dom（一个js对象，用来描述真实dom）-- 损耗增加

  ['div', {id: 'abc'}, ['span', {}, 'hello world']]

- 数据 + 模板 --> 生成真实的dom

- state发生变化

- 生成新的虚拟dom -- 极大地提升性能（生成js对象时间<<生成dom时间）

  ['div', {id: 'abc'}, ['span', {}, 'new world']]

- 比较新旧虚拟dom，找差异 -- 极大地提升性能（比较js对象时间<<比较dom时间）

- 操作dom，改变差异部分

```
render() {
// jsx --> 调用CreateElement方法 --> 虚拟dom --> 真实dom
	return <div>item</div>
}

render() {
// 和上面相比少了jsx转化为createElement语句的步骤
	return React.CreateElement('div', {}, 'item');
}

当元素嵌套时，直接用createElement方法写会比较麻烦
render() {
	return <div><span>item</span></div>
}

render() {
	return React.CreateElement('div', {}, React.CreateElement('span', {}, 'item'))
}
```

#### 虚拟dom优点

- 性能提升

- 使跨端应用得以实现--react native（用react开发原生应用）：android之类的应用没有dom概念，dom代码只能在浏览器里跑

  在原生应用里，虚拟dom转换成原生应用组件

#### dom的diff算法

![1607322286883](react note.assets/1607322286883.png)

异步设计

当在短时间内连续更新state数据，react会把3次setState合并成一次setState，最终只比较一次（旧虚拟dom，合并后的新虚拟dom）

![1607322621694](react note.assets/1607322621694.png)

比较过程：

1. 从最顶层开始比较，直到出现差异

2. 出现差异后，将原始dom的该节点下的节点全部删除，重新生成dom，用重新生成的dom替换原始dom

   重新渲染时可能会造成浪费，但是同层比较算法简单，比较时节约性能

![1607329066740](react note.assets/1607329066740.png)

##### 用index作key的弊端

随着增删改查，同一个元素的key可能会发生变化，虚拟dom比较的时候无法与原始元素准确对应，导致本不存在的差异产生，降低性能。

### 3. react的生命周期

挂载：组件第一次被加载到页面上的过程。

![1607433503478](react note.assets/1607433503478.png)

##### 生命周期函数

在某一时刻，组件会自动执行的函数。

#### 1. initialization

初始化，由constructor来实现。



#### 2. mounting

挂载。

- componentWillMount: 在组件即将被挂载到页面的时刻自动执行
- render
- componentDidMount：在页面渲染后自动执行

当页面发生变化（不是整个页面重新加载时），只执行render函数，不会执行willMount和didMount。

#### 3. updation

组件更新 -- 当props或者state发生改变时

- componentWillReceiveProps：在接收props前执行，只在props发生改变的时候执行

  具体来说，条件是子组件从父组件接收参数，且不是子组件首次挂载（即子组件更新时），执行时刻是父组件render完成时

- shouldComponentUpdate：在组件更新之前执行，要求返回布尔值--判断组件是否应该被更新。返回false，更新就不会被执行

- componentWillUpdate：在组件更新之前执行，shouldComponentUpdate返回true后被执行。

- render

- componentDidUpdate：在组件更新完成后执行

#### 4. unmounting

- componentWillUnmount：在组件从页面上移除之前执行

<b>React.Component内置了除了render函数以外的所有生命周期函数</b>

##### 生命周期函数应用

```
  // 在子组件中添加下述代码，使每次父组件更新的时候，不去更新没有变化的子组件
  shouldComponentUpdate(nextProps, nextState) {
    if(nextProps.content !== this.props.content) {
      return true;
    }
    return false;
  }

```

## 四. Redux 数据层框架

![1608132100946](react note.assets/1608132100946.png)

![1608213506014](react note.assets/1608213506014.png)

安装

```
npm add redux               / yarn add redux
```

在src目录下创建folder，命名为store

store文件夹中创建index.js文件，创建store。

```
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);         // createStore is a method

export default store;
```

store文件夹下创建reducer.js

```
const defaultState = {}

export default (state=defaultState, action) => {
  return state;
}
```

#### 调试工具redux devtools

redux-devtools-extension

安装后，往createStore中传入参数：

```
 const store = createStore(
   reducer, /* preloadedState, */
+  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
 );
```

### 1. 页面数据更新流程

##### 1）在相关组件中绑定事件，添加action，并将action传给store

![1608560693069](react note.assets/1608560693069.png)

```
<Input 
    onChange={this.handleInputChange}
></Input>

  handleInputChange(e) {
  	// 创建action
    const action = {
      type: 'change_input_value',
      value: e.target.value
    }
    // 将action传给store
    store.dispatch(action);
  }
```

##### 2）store自动将prevState, action转发给reducer

这一步不需要额外编写

##### 3）reducer将新的state传给store

在reducer.js中

```
// always try not to modify state
export default (state=defaultState, action) => {
  // 修改state并返回	
  if (action.type === 'change_input_value') {
    const newState = JSON.parse(JSON.stringify(state));  // deep copy state
    newState.inputValue = action.value;
    return newState;
  }
  console.log(state, action);
  return state;
}
```

此时，store中数据已更新

##### 4）拿到store中的数据，更新页面

```
  constructor(props) {
    super(props);
    // 用store中的数据进行初始化
    this.state = store.getState();
    this.handleInputChange = this.handleInputChange.bind(this);
    // store发生变化自动执行的方法
    this.handleStoreChange = this.handleStoreChange.bind(this);
    // 订阅store，监听store变化，一旦store发生变化，执行传入的方法
    store.subscribe(this.handleStoreChange);
  }
  
  handleStoreChange() {
    // 重新从store中取数据，并用setState方法实现state的改变
    this.setState(store.getState());
  }
```

### 2. ActionTypes的拆分

在store下新建actionTypes.js文件

```
export const CHANGE_INPUT_VALUE = 'change_input_value';
export const ADD_ITEM = 'add_item';
export const DELETE_ITEM = 'delete_item';
```

好处：定义常量，如果有拼写错误会报错，便于debug

### 3. actionCreators

在store文件夹下创建actionCreators.js文件

actionCreators.js中定义action

```
import { CHANGE_INPUT_VALUE, ADD_ITEM} from './actionTypes';

export const getInputChangeAction = (value) => ({
  type: CHANGE_INPUT_VALUE,
  value
});

export const getAddItemAction = () => ({
  type: ADD_ITEM
});
```

在业务js文件中引用

```
import { getInputChangeAction, getAddItemAction, } from './store/actionCreators';
  handleInputChange(e) {
    // const action = {
    //   type: CHANGE_INPUT_VALUE,
    //   value: e.target.value
    // }
    const action = getInputChangeAction(e.target.value);
    store.dispatch(action);
  }

  handleItemAdd() {
    // const action = {
    //   type: ADD_ITEM
    // }
    const action = getAddItemAction();
    store.dispatch(action);
  }
```

### 4. UI组件和容器组件的拆分

UI组件是负责渲染的部分，容器组件负责逻辑处理。

​              {/* <Button style={{textAlign: 'right'}} onClick={this.props.handleItemDelete.bind(this, index)}> Delete item </Button> */}

onClick={()=>{this.props.handleItemDelete(index)}}

src下定义TodoListUI.js

```
import React, { Component, Fragment } from 'react';
import { Input, Button, List } from 'antd';

class TodoListUI extends Component {
  render(){
    return (
      <div style={{marginTop: '10px', marginLeft: '10px'}}>
        <Input 
          value={this.props.inputValue}
          placeholder="write your item here" 
          type="dashed" 
          style={{width: '300px', marginRight: '10px'}}
          onChange={this.props.handleInputChange}
        ></Input>
        <Button
         onClick={this.props.handleItemAdd}
        >
          Add item
        </Button>
        <List
          style={{marginTop: '10px', width: '400px'}}
          bordered
          dataSource={this.props.list}
          renderItem={(item, index) => (
            <Fragment>
              <List.Item style={{display: 'inline'}} >{item}</List.Item>
              <Button style={{textAlign: 'right'}} onClick={this.props.handleItemDelete.bind(this, index)}> Delete item </Button><br/>
            </Fragment> 
          )}
        />
      </div>
    )

  }
}

export default TodoListUI;
```

在TodoList.js中引入

```
import TodoListUI from './TodoListUI';

  constructor(props) {
    super(props);
    this.state = store.getState();
    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleStoreChange = this.handleStoreChange.bind(this);
    this.handleItemAdd = this.handleItemAdd.bind(this);
    this.handleItemDelete = this.handleItemDelete.bind(this);
    store.subscribe(this.handleStoreChange);
  }

  render() {
    return (<TodoListUI 
              inputValue={this.state.inputValue}
              handleInputChange={this.handleInputChange}
              handleItemAdd={this.handleItemAdd}
              handleItemDelete={this.handleItemDelete}
              list={this.state.list}
           />);
  }
```

### 5. 无状态组件

只包含render函数的组件可以定义为无状态组件。

- 无状态组件性能较高，因为它是一个函数；而普通UI组件是一个类，除了render函数外还需要执行其他生命周期函数

```
// 注意，由于直接传入props，不需要通过this访问
const TodoListUI = (props) => {
    return (
      <div style={{marginTop: '10px', marginLeft: '10px'}}>
        <Input 
          value={props.inputValue}
          placeholder="write your item here" 
          type="dashed" 
          style={{width: '300px', marginRight: '10px'}}
          onChange={props.handleInputChange}
        ></Input>
        <Button
         onClick={props.handleItemAdd}
        >
          Add item
        </Button>
        <List
          style={{marginTop: '10px', width: '400px'}}
          bordered
          dataSource={props.list}
          renderItem={(item, index) => (
            <Fragment>
              <List.Item style={{display: 'inline'}} >{item}</List.Item>
              <Button style={{textAlign: 'right'}} onClick={props.handleItemDelete.bind(this, index)}> Delete item </Button><br/>
            </Fragment> 
          )}
        />
      </div>
    )	
}
```

### 6. redux-thunk中间件

- 支持action返回一个函数

![1609304434185](react note.assets/1609304434185.png)



安装

```
npm install redux-thunk
```

使用

store下的index.js中(create store)

```
import { createStore, applyMiddleware, compose } from 'redux';

import thunk from 'redux-thunk';

// 跟其他组件兼容，需要创建enhancer
const composeEnhancers =
  typeof window === 'object' &&
  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
      // Specify extension’s options like name, actionsBlacklist, actionsCreators, serialize...
    }) : compose;

const enhancer = composeEnhancers(
  applyMiddleware(thunk)
  // other store enhancers if any
);
const store = createStore(reducer, enhancer);

export default store;
```

actionCreators.js中，可以创建带异步请求的action

```
export const getDataInitAction = (data) => ({
  type: INIT_DATA,
  data
});

export const getInitList = () => {
  // 接收store的dispatch方法
  return (dispatch) => {
    axios.get('/test.json').then((res) => {
      const data = res.data;
      const action = getDataInitAcion(data);
      dispatch(action);
    })
  }
}
```

容器组件中引用

```
  componentDidMount(){
    const action = getInitList();
    store.dispatch(action);
    // axios.get('/test.json')
    //   .then((res) => {
    //     const data = res.data;
    //     const action = getDataInitAction(data);
    //     store.dispatch(action);
    //     })
    //   .catch(() => {alert('error')})
  }
```

### 7. redux中间件 redux-saga

redux-saga 处理异步问题

https://github.com/redux-saga/redux-saga

安装

```
$ npm install redux-saga
```

使用

在store文件夹下创建一个文件，管理异步逻辑，如sagas.js

```
import { takeEvery, put } from 'redux-saga/effects';
import { GET_INIT_LIST } from './actionTypes';
import { initListAction } from './actionCreators';

function* getInitList() {
  try {
    const res = yield axios.get('/list.json');
    const action = initListAction(res.data);
    // saga里没有dispatch方法，用put取代
    yield put(action);
  }catch(e) {
   console.log('file not found');
  }

}

// generator
function* mySaga() {
  // takeEvery会捕获每一个派发出的action
  // 只要接收到GET_INIT_LIST类型的action，就会执行fetchUser方法
  yield takeEvery(GET_INIT_LIST,getInitList);
  yield takeLatest("USER_FETCH_REQUESTED", fetchUser);
}

export default mySaga;
```

store下的index.js（createStore所在文件）

```
import createSagaMiddleware from 'redux-saga';
import mySaga from './sagas';

// create the saga middleware
const sagaMiddleware = createSagaMiddleware();

// mount it on the Store
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)

// then run the saga
sagaMiddleware.run(mySaga);
```

容器组件下

```
  componentDidMount(){
    const action = getInitList();
    store.dispatch(action);
    // axios.get('/test.json')
    //   .then((res) => {
    //     const data = res.data;
    //     const action = getDataInitAction(data);
    //     store.dispatch(action);
    //     })
    //   .catch(() => {alert('error')})
  }
```

actionCreators.js中，可以创建带异步请求的action

```
export const getDataInitAction = (data) => ({
  type: INIT_DATA,
  data
});

export const getInitList = () => {
  type: GET_INIT_LIST
}
```

actionTypes.js文件中创建type，略。



#### 其他中间件 redux-logger

- redux-logger 打印action

### 8. react-redux

安装，略。

使用步骤：

1. 在需要用到store的最外层组件中引入Provider，并用Provider组件包围需要使用store的组件

```
import React, { Component } from 'react';
import { Provider } from 'react-redux';
import Header from './common/header';
import store from './store';

class App extends Component {
  render() {
    return (
      <Provider store={store}>     // allow components inside to use store
        <Header />
      </Provider>
    )
  }
}

export default App;
```

2. 在子组件中引入connect方法，连接store。

```
import { connect } from 'react-redux';

const mapStateToProps = (state) => {
  return {
    focused: state.focused       // reflect data in state to props of current component
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    handleInputFocus() {
      const action = {
        type: 'search_focus'
      };
      dispatch(action);
    },
    handleInputBlur() {
      const action = {
        type: 'search_blur'
      };
      dispatch(action);
    }
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Header);
```

### 9. store拆分

在app下新建store文件夹，在store文件夹下新建reducer.js

将app需要用的数据和处理转移到这个文件。

```
const defaultState = {
  focused: false
};

export default (state = defaultState, action) => {
  if (action.type === 'search_focus') {
    return {
      focused: true
    }
  }
  if (action.type === 'search_blur') {
    return {
      focused: false
    }
  }
  return state;
}
```

主store用combineReducers方法，将所有reducer整合

```
import { combineReducers } from 'redux';

import headerReducer from '../common/header/store/reducer';


export default combineReducers({
  header: headerReducer
})
```

修改容器组件中state的引用路径

```
const mapStateToProps = (state) => {
  return {
    focused: state.header.focused       // reflect data in state to props of current component
  }
}
```



### 总结

- store是唯一的
- 只有store能改变自己的数据
- reducer必须是纯函数：给定固定的输入，就会有固定的输出，且不会有任何副作用（对传入的参数进行修改等）
- 核心API:
  - createStore：创建store
  - store.dispatch：派发action
  - store.getState：获取store里的数据
  - store.subscribe：监听store的数据变化，只要数据有变化，自动执行传入的函数

## 五、前端UI框架

### ant Design



## 六、优化方法总结

### 1. 在constructor中绑定指向

当多个方法需要做同一个绑定时，只需要在初始化时执行一次

### 2. 将多次数据改变合并成一次

setState异步，使得将多次数据改变合并成一次再比对得以实现，降低比对虚拟dom的频率。

### 3. 生成虚拟dom



### 4. 同层比对，key值比对



### 5. shouldComponentUpdate

数据不改变时不更新。

### 6. 用第三方模块immutable.js防止传入的state被修改

```
npm install immutable
```



## 七、避坑指南

### 1. 推荐将ajax请求的发送放到componentDidMount方法中

如果写进render里，每当数据发生变化就会发请求。

如果写进componentWillMount，会和某些高级方法冲突。

写在constructor里理论上也是可以的。

