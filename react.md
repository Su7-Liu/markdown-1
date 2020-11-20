# react 基本概念

```
react 不适用模板
不是一个mvc框架
响应式
轻量级js库
```
# react开发环境搭建
```
引用：
1. react.js 核心文件
2. react-dom.js 渲染页面中的DOM，当前文件以来react核心文件
3. babel.js ES6转换成ES5，jsx语法转换成JavaScript（方便浏览器代码的兼容）

使用：
下载：npm i react --save|npm i react-dom --save|npm i babel-standalone --save
或
yarn add react react-dom --save

react 开发环境cdn
https://unpkg.com/react@16/umd/react.development.js
https://unpkg.com/react-dom@16/umd/react-dom.development.js
https://unpkg.com/babel-standalone@6/babel.min.js
```
# 基本语法
```
ReactDom.render()
React.createElement()
React.Component
```
## 组件间通信
redux,react-redux,redux-thunk
```
redux是状态管理模块，把所有状态都统一进行管理，方便组件之间传递数据.
场景：
1、某个组件的状态，需要共享
2、某个状态需要在任何地方都可以拿到
3、一个组件需要改变全局状态
4、一个组件需要改变另一个组件的状态
二原理：
客户端通过dispatch发送action，action的变化会触发redures，redures返回新的state，store接收到新的state，subscribejian监听store变化，渲染根组件。
```
react-redux解析
```
主要是分离视图和数据，简化、方便redux。提供了Provider组件，方便跨组件传输数据。还提供connect组件，绑定当前组件和state、dispatch的关系，当前组件通过this.props就可以访问state和dispatch。
```
redux-thunk解析-用来处理异步任务
```
redux-thunk是redux的中间件，使action可以接收一个函数，函数接收两个参数dispatch和state，可用来处理异步任务。
```

# jsx
## jsx 注释 
```
    <div id='demo-react'></div>
    <script  type='text/babel'>
    let mydom=<h1>
    {/*注释内容*/}
    hello
             </h1>;
    ReactDOM.render(mydom,document.getElementById("demo-react"));
    </script>
```
## jsx 语法-变量
```
变量用{}引起来
<body>
    <div id='demo-react'></div>
    <script  type='text/babel'>
    let num =123;
    let text='你好';
    let message='haha';
    function funs(obj){
        // jsx
        // return 'name is:'+obj.name+",age is:"+obj.age
        // es6
        return `name is:${obj.name},age is:${obj.age}`
    }
    let users={
        name:"xiaoming",
        age:'28'
    }
    let mydom=(
        <div>
        {/*变量引用*/}
        <div>{text}</div>
        {/*计算*/}
        <div>{num+123}</div>
        <div>{message}</div>
          {/*函数的使用*/}
        <div>{funs(users)}</div>
        </div>
    )
    ReactDOM.render(mydom,document.getElementById("demo-react"));
    </script>
</body>
```

## React内三种函数的写法
```
//写法一：让函数内部的this指向这个类的实例，它是用bind实现的，bind的第一个参数表示context，就是this。 
class ManageAddress extends React.Component {

    constructor(props) {
        super(props);
        this.handleChangeAddressType = this.handleChangeAddressType.bind(this)        ...
    }

    /**
     * 切换地址类型，重新获取地址列表
     * @param key
     */
    handleChangeAddressType(key) {
     ...
    };

  render() {
        return (
            ...
            <button onClick={this.handleChangeAddressType}>测试
            </button >
           ...
        )
    }}
```
```
写法二：相当于让handleChangeAddressType的值为一个箭头函数，所以调用处直接传入这个值就可以，注意不能加括号会报错.它不会自己创建this，它会从自己的作用域链上层继承this，这里this就会指向这个类的实例。这不是js标准写法，但是babel已经支持了。

class ManageAddress extends React.Component {

    constructor(props) {
        super(props);
        ...
    }

    /**
     * 切换地址类型，重新获取地址列表
     * @param key
     */
    handleChangeAddressType = (key) =>{
     ...
    };

  render() {
        return (
            ...
            <button onClick={this.handleChangeAddressType}>测试
            </button >
           ...
        )
    }
}
```
```
写法三：在调用处使用箭头函数，与第二种方法类似
//写法三
class ManageAddress extends React.Component {

    constructor(props) {
        super(props);
        ...
    }

    /**
     * 切换地址类型，重新获取地址列表
     * @param key
     */
    handleChangeAddressType(key) {
     ...
    };

  render() {
        return (
            ...
            <button onClick={(key)=>this.handleChangeAddressType(key)}>测试
            </button >
           ...
        )
    }
}
```

## react 跨域
1、正向代理 - 开发环境

2、反向代理 - 生产环境

api：http://www.tianqiapi.com/api?version=v6&appid=23035354&appsecret=8YvlPNrz&city=北京'

## state和props
```
state可变，props对于当前页面组件来说，是只读的，如果想要修改props的数据，那么我们修改传递给当前组件的父组件。
如果想要使用状态，那么不能使用无状态组件。
```
```
定义state 步骤
初始化状态
更新状态
读取状态
```
  

## refs 转发

React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render() 输出的任何组件上。
这个特殊的属性允许你引用 render() 返回的相应的支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。
```
字符串的方式
回调函数(推荐)
React.createRef()
```
## 条件渲染
根据状态的变化，只渲染部分



## create-react-app
facebook推出的一款react脚手架工具
npm install -g create-react-app 全局安装
create-react-app --version  查看版本
创建项目
```
cd 项目目录下
创建：create-react-app 项目名
启动：cd 项目名 & npm start
```
## react项目目录详解
```
public 静态资源文件夹
.git 是 git文件夹
dist 是打包后文件放置地方
src 是源码文件，一般做开发就在这个文件夹。
.babelrc是babel配置文件
webpack.config.js是webpack配置文件
server.js 是 express服务器文件
.editorconfig 是编辑器配置文件，用来同意团队开发的编辑器配置
node_modules是各个插件存放位置
```
## 组件传值
```
正向传值
逆向传值
同胞传值
```
##  json-server/axios
```
安装
json-server 模拟数据 npm install json-server -g
axios		数据请求 npm install axios -g

启动json-server
json-server json文件数据名字 --port 4000
访问：http://localhost:4000/arr

```

## react-router
```
1 总体的步骤分为三步:
2 配置路由的容器Router;
3 配置路由的连接LInk;
4 配置路由填充的位置以及路径和组件的映射关系;

路由模式：
hash HashRouter  hash模式，带#号，刷新的时候，页面不会丢失
brower：BrowerRouter  历史记录模式，没有#号，通过历史记录api进行路由切换，刷新会丢失，本地模式不会丢失
index.js 引用路由模块

路由模块包裹组件

withRouter：高阶组件，监控路由变化。参数是一个组件，返回也是一个组件。
编程式导航：props.history.push('./xxx')

路由传参：params 方式进行传参
```


## hock
让无状态组件，可以使用状态

## redux 
```
安装：npm install redux --save
js提供的一个可预测行的状态组件
集中管理react中的多组件状态
redux是专门的状态管理库（在vue中也可以使用）

需求场景：
多个组件状态需要共享的时候
一个组件需要改变另一个组件状态的时候
组件状态需要在任何地方可以拿到

三大原则：
1、单一数据源
2、redux中的state是只读的
3、使用函数进行修改redux中的state
```

# npm 命令
```
npm install moduleName # 安装模块到项目目录
npm install -g moduleName # -g 意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
npm install --save moduleName # --save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
npm install --save-dev moduleName # --save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。

自定义start端口：
linux：PORT=8081 npm start /export PORT=8081  npm start
window:
set PORT=8081
npm start#关闭命令窗口后，端口配置也会失效

```


# 注意事项
## import
```
import 'components' 引用全局module
import './components' 引用当前目录下的module



```

## vscode插件
1、VS Code ES7 React/Redux/React-Native/JS snippets













































>>>>>>> 6ee41d80ddd42e5e158f9e510163e436181064aa
