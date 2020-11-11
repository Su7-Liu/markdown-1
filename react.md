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