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


## state和props
```
state可变，props对于当前页面组件来说，是只读的，如果想要修改props的数据，那么我们修改传递给当前组件的父组件。
如果想要使用状态，那么不能使用无状态组件。
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