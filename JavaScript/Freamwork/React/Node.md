# React特点
+ 声明式
+ 组件化
+ web,mobile,vr

# React安装
`npm i react react-dom`

# react使用
1. 引入
```
<script src="./node_modules/react/umd/react.development.js"></script>
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
```
2. 创建元素
```
// 名称，属性，之后子节点
const title = React.createElement('h1', null, 'hello react')
```
3. 渲染
```
// 挂载元素，挂载位置
ReactDOM.render(title, document.getElementById('root'))
```

# React脚手架
`npx create-react-app my-app`
`npm start`

# React脚手架使用
1. ES6导入
2. React.createElement()
3. ReactDOM.render()

# JSX
直接使用声明式的HTML的标签
该语法为ECMA扩展语法，使用babel/preset-react编译后使用

### 语法规则
驼峰命名
+ class -> className
+ for -> htmlFor
+ tabindex -> tabIndex

无子节点标签可以 />结尾

推荐小括号包裹()

### js嵌入js

```
// 使用 {}
const name = 'dog';
const = (<div>hello,{name}</div>)
```

### {} 注意点
1. {} 中可以输入js表达式
2. {} 中可以调用函数
3. jsx自身也可以放入{}
4. 不能直接使用object,一般只出现在style中
5. 不能出现语句

### 条件渲染、列表渲染
使用map进行列表渲染时需要加上key,渲染谁,key就加谁身上（避免使用索引作为key）

### jsx样式处理
+ 行内：style= { { color:'red'} }
+ 类名：className

### 附
React完全使用js语言自身的能力来编写ui,而不是造轮子增强html功能

# 组件
### 组件创建
1. 函数(无状态组件)

约定
  + 首字母大写
  + 返回值&&表示组件结构
  ```
  function Hello(){
    return <h1>第一个函数组件</h1>
  }
  ReactDOM.render(<Hello/>, document.getElementById('root'))
  ```
2. 类(有状态组件)

约定
  + 首字母大写
  + 继承React.Component
  + 必须有render()方法且返回值为组件结构

### 抽离组件
1. 创建组件.js
2. `import React from 'react'`
3. 创建类
4. export default 'className'
5. 在使用的文件中import改组件

### 事件处理
on+event={事件处理程序}

function类型组件不需要this

### state

|组件类型|名称|功能|
|--|--|--|
|function|无状态|只负责展示|
|class|有状态|更新UI|

#### state使用

```
// 1
constructor(){
  super()
  this.state={
    count:0
  }
}
// 2
state={
  count:0
}
```

#### 修改state

不能直接修改

this.setState()

### 将事件处理抽离出render

此时this为undefined

this应该指向组件实例

#### 事件绑定this指向

1. 箭头函数
```
//函数要加()
<button onClick={()=>this.handleClick()}>点我</button>
```

2. Function.prototype.bind()
```
//在constructor内部将this绑定到方法上
this.myFunction = this.myFunction.bind(this);
```

3. class实例方法(推荐)
```
handleClick=()=>{
  this.setState(
    {count:this.state.count+1}
  )
}
```

### 表单处理

#### 受控组件

将表单元素的value交给state去控制
```
<input type="text" value = {this.state.txt} onChange={this.handleChange}></input>

handleChange=(e)=>{
    this.setState(
      {txt:e.target.value}
    )
  }
```
```
//处理多表单
//为表单元素添加name属性，name的值与state绑定的value一样
handleChange=(e)=>{
    const value = e.target.type === 'checkbox' ? e.target.checked : e.target.value;
    const name = e.target.name
    this.setState(
      //属性名表达式？？
      {[name]:value}
    )
  }
```

#### 非受控组件
1. React.createRef()方法创建ref对象

    + `this.txtRef = React.createRef();`

2. 添加ref属性

    + `<button onClick={this.getText}>点我</button>`

3. ref获取文本框的值

    + `getText =()=> this.txtRef.current.value`

### 表单联系出错事项
1. 凡是使用this.state的地方必须使用{}包裹
2. 凡是使用html标签的地方要使用()包裹
3. 渲染数据需要加key属性到要遍历的节点上
4. 直接调用function要加(),事件处理程序不需要
5. 添加state数组方法 
   + `[{新数据},...原数据]`

# 组件通讯

### props

props
  + 接收传递给组件的数据
  + 可以传递任何类型的值，包括function和jsx
  + 只读
  + class组件中，构造函数中拿props需要super(props)

传递数据：组件标签添加属性
接收数据：
  + function(参数接收)
  + class通过this.props

### 组件传值
#### 父传子

#### 子传父
1. 父组件提供一个回调函数
2. 将函数作为属性传给子组件
3. 在子组件中将数据作为函数参数传递且调用
4. 在父组件中触发改回调函数

#### 兄弟组件

状态提升，将state给最近的父组件
1. 在父组件提供共享数据
2. 在父组件提供操作方法

### Context
跨组件传递数据
1. React.createContext();
```
// ??
// 创建context得到两个组件
const { Provider, Consumer } = React.createContext()
```
2. 在两个传值组件中使用
```
// 传递
<Provider value="pink">
  <div>
    <span>我是1级</span>
    <Child></Child>
  </div>
</Provider>

// 接收
<Consumer>
  {
    data=><span>{data}</span>
  }
</Consumer>
```
### props.children

jsx,function,components都可以作为子节点

`<Component>子节点</Component>`

### props校验
#### props使用
创建组件时，指定props的类型
1. package: prop-types

  + `npm i prop-types `

```
import PropTypes from 'prop-types'
//注意大小写
ComponentName.propTypes={
  color: PropTypes.array
}
  ```

#### props约束规则
  + 类型
  + element
  + isRequired
  + 特定结构对象：shape({})
#### props默认值
```
ComponentName.defaultProps={
  pageSize: 10
}
```

# 组件生命周期
### 创建（挂载阶段）
+ Constructor()
  - this指向
  - state初始化
+ render()
  - 组件渲染
  - render中不能调用setState
+ componentDidMount()
  - DOM渲染后
  - 网络请求
  - 此时可以获取、操作DOM

### 更新（更新阶段）
+ rener()
  - New props
  - setState
  - forceUpdate()
+ componentDidUpdate(preProps)
  - DOM重新渲染后
  - 在if语句中调用setState

### 卸载（卸载阶段）
+ componentWillUnmount()
  - 清理(定时器)

# 组件复用
### render props

#### render模式
1. 使用组件时，添加一个值为函数的prop,通过参数来获取state
```
state={
  x:0,
  y:0
}
render(){
  return this.props.render(this.state)
}
componentDidMount(){
  window.addEventListener('mousemove',this.handleMousePos)
}
handleMousePos=(e)=>{
  this.setState({
    x:e.clientX,
    y:e.clientY
  })
}
```
2. 使用函数的返回值作为渲染的UI内容
```
root.render(<MousePos render={(data)=>(
<span>当前位置：{data.x}:{data.y}</span>
)}/>)
```
#### children模式
Context的用法和children一致
1.
```
<MousePos>
  {mouse=>{return (<span>当前位置:{mouse.x}:{mouse.y}</span>)}}
</MousePos>
```
2.
```
this.props.children(this.state)
```
#### 添加render-props优化
+ 校验
```
ComponentName.propTypes={
  children:PropTypes.func.isRequired
}
```
+ 组件卸载时事件绑定
```
componentWillUnmount(){
  window.removeEventListener('mousemove')
}
```

### HOC(高阶组件-函数)
//??
包装模式
#### 使用
1. 创建函数，约定以<font color="yellow">with</font>开头
2. 指定参数，参数为组件因此<font color="yellow">大写字母</font>开头
3. 在内部创建类组件，提供状态逻辑代码
4. 通过prop传递给参数组件
```
import React from 'react'

function withMouse(WrappedComponent){
  class MouseMove extends React.Component{
    state={
      x:0,
      y:0
    }
    componentDidMount(){
      window.addEventListener('mousemove',this.handleMouseMove)
    }
    handleMouseMove=(e)=>{
      this.setState({
        x:e.clientX,
        y:e.clientY
      })
    }
    componentWillUnmount(){
      window.removeEventListener('mousemove')
    }

    render(){
      return <WrappedComponent {...this.state}></WrappedComponent>
    }
  }
  return MouseMove
}

const Position = (props) =>(<p>鼠标当前位置：{props.x}:{props.y}</p>)

const Don = withMouse(Position);

class MyCom extends React.Component{
  render(){
    return (
      <div>
        123456
        <Don></Don>
      </div>
    )
  }
}

export default MyCom
```

#### displayName
区分不同高阶组件名字
```
MouseMove.displayName = `WithMouse${getDisplayName(WrappedComponent)}`

function getDisplayName(componentName){
  return componentName.displayName|| componentName.name || 'Component'
}
```

#### props丢失
```
return <WrappedComponent {...this.state} {...this.props}></WrappedComponent>
```

### 附
子组件多次使用需要在外边包裹div
1. 解构，将对象中的某些元素给暴露出来，直接使用
2. 获取0-2的随机数，Math.floor(Math.random()*3)

# 原理

### setState
+ 异步更新数据
+ 避免多次调用setState
+ 只触发一次render()

#### 推荐语法
```
// state为最新state
// props为最新props
// 回调会在DOM重新更新后触发
this.setState((state,props)=>{
  return {
    num:state.num +1
  }
},()=>{})
```

### JSX语法转化过程
JSX->preset-react->React.createElement()->React元素 Obj

### 组件更新机制
setState
+ 修改state
+ 更新UI

只会更新当前组件以及子组件

# 组件性能优化

### 减轻state
+ 之储存跟组件渲染有关的数据
+ 多个方法之间共享但和组件渲染无关的数据放入this中

### 避免不必要的重新渲染
更新阶段
shouldComponentUpdate->render
```
//false为不渲染该组件
// nextProps 最新props this.props当前props
// nextState 最新状态 this.state当前状态
shouldComponentUpdate(nextProps,nextState){
  return false
}
```

### 纯组件
React.PureComponent与React.Component类似，只是自动调用shouldComponentUpdate
分别比较前后两次的props和state来觉得是否需要重新渲染组件

通过shallow compare来进行判断
obj仅仅比较地址有没有变，因此不能直接copy该object

#### 附
```
//创建新对象
const newObj = {...state.obj,number:2}
//数组
//不要使用push,unpush
//使用concat,slice
[...this.state.list,{newList}]
```

### 虚拟DOM和Diff算法
同组件相同，组件内不变的DOM是否需要更新
1. state(Model)->Virtual DOM->DOM
2. setState->new Virtual DOM->DOM
3. diff:compare Virtual(Virtual DOM,new Virtual DOM)
4. render the changed part to the DOM

render之后 
V-DOM -> JSX + State
性能，跨平台

# 路由
1. 安装：`yarn add react-router-dom`
2. import { BrowserRouter as Router,Route,Link,Routes} from 'react-router-dom'
// 6版本以上
```
<Router>
  <div>
    路由配置规则
    <Link to="/first">页面去把</Link>
    <Routes>
      <Route path="/first" element={<First/>}></Route>
    </Routes>
  </div>
</Router>
```
// 以下
```
//不需要Routes
<Route path="/first" component={First}></Route>
```

### 注意
+ 包裹整个应用

#### BrowserRouter/HashRouter
HashRouter：使用URL的哈希实现
BrowserRouter（推荐）：使用H5的history API实现

#### Link
路由入口
`<Link to="/first">页面去把</Link>`
等同于
`<a href="first"></a>` + (location.pathname)

#### Route
路由入口
+ path 路由规则
+ component 指定的组件
+ Route 指定渲染位置

### 执行过程
1. 点击Link标签，修改浏览器地址栏中的URL
2. React监听地址栏URL变化
3. React路由内部遍历所有的Route组件，使用路由规则（path）和pathname匹配
4. 匹配成功展示该组件

### 编程式导航
JS代码实现页面跳转
```
// 跳转到指定路径
this.props.history.push('/path')
// 前进后退到第几个
this.props.history.go(n)
```

### 默认路由
`<Route path="/" element={<Home/>}></Route>`

### 模糊匹配
path匹配
+ path:`/` 可以匹配所有路由
+ path:`/first` 可以匹配 pathname/to:`/first/`或`/first/a`或`/first/a/b/..`

### 精确匹配
path和pathname完全匹配才可以
给Route加上exact属性
