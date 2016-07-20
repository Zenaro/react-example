---
title: React入门笔记
date: 2016-07-20 13:12:55
categories: "前端框架"
tags: 
    - React
    - react教程
    - react文档
    - 前端
    - JavaScript
    - 框架
    - DOM
---
## React简介
React 是 Facebook 和 Instagram 用来创建用户界面的一个 JavaScript 框架。

很多人认为 React 是 MVC 中的 V（视图）。React是个很年轻的框架，它正引起了越来越多的人关注和使用，在github上的热门程度为4w+颗星。一提起它，很多人会立即联想到虚拟DOM。是的，以往JavaScript的性能瓶颈问题便是DOM，而在React框架里，它着眼解决DOM渲染的性能问题，其涉及到的许多新思想如虚拟DOM，DOM渲染算法，JSX语法都可谓是技术上的革新，而其实现的代码逻辑却简明易懂，故而受到越来越多的人支持和使用，并认为它很有可能成为主流的前端开发框架。

接下来我们会尝试了解和使用React框架，体验一下在React开发里会有哪些新奇的做法。

以往普通的html结构，小到一个按钮，一段文本，一个输入框，大到一个表单，一个container，在React看来都是一个个组件，React倾向于对界面进行组件化开发，最后拼合起来形成一个完整的html。React把html拆分成一个个小组件，以此来提高代码复用和逻辑清晰度，更重要的，方便进行虚拟DOM树的构建。组件可以看成是一个div框，可以看成是一个函数，还可以看成是一个状态机。概念说得有点多了，别着急，我们会在接下来的实例中一一感受到这些。

## 制作第一个组件
让我们来动手制作第一个组件，结果是输出Hello, world。我们先到React官网上下载React相关文件，[示例代码]也包含了所需的JS库，接着将其引入到html。html结构如下：
``` bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="../Lib/react/react.js"></script>
    <script type="text/javascript" src="../Lib/react/react-dom.js"></script>
    <script type="text/javascript" src="../Lib/react/browser.min.js"></script>
</head>
<body>
    <div id="container"></div>
    <script type="text/babel">
        /* 这里写JSX代码，需要指定text/babel */
    </script>
</body>
</html>
```
这里的JS语法和以往已经有所不同，现在它有一个新的名字叫__JSX__，是React框架里独有的语法。这里我们需要用text/babel来声明，少了这个指定将会报错。另外我们需要引入三个JS库，react.js，react-dom.js，browser.js。react为核心库，react-dom则是一些DOM方法，browser负责将JSX转化成JS语法。好了，配置完成，我们开始编写JSX来制作组件。
``` bash
ReactDOM.render(
    <p>Hello, wolrd</p>,
    document.getElementById('container')
);
```
上述JSX语句中我们使用ReactDOM.render()方法，来创建一个__"p"__标签，并将其插入到id为container的div中。JSX语法允许我们在JS中直接使用原生的html标签。使用原生的标签有不少好处，它有固定的标签开启和闭合。这能让复杂的树更易于阅读，优于方法调用和对象字面量的形式。在React框架中使用JSX语法有很大的优势，当然如果你不喜欢，还是可以用原声的JS来实现，比如像这样：
``` bash
ReactDOM.render(
    React.createElement('p', null, 'Hello, world'), // 替换原先的<p>标签</p>
    document.getElementById('container')
);
```
上面的例子中，__< p >__便可以理解为一个小组件。

## 组件也是类
React开发中，我们更倾向于把组件拿到 render() 函数外去创建，然后在 render() 函数中引入它.像这样：
``` bash
<script type="text/babel">
    var HelloWorld = React.createClass({
        render: function() {
            return <p>Hello, world</p>;
        }
    });
    ReactDOM.render(
        <HelloWorld/>,
        document.getElementById('container')
    );
</script>
````
上述的例子中，我们使用createClass创建一个外部组件（你也可以理解为一个类），然后再ReactDOM.render()函数中直接使用HelloWorld标签即可。有一点需要注意，组件的变量命名首字母必须大写，如上的HelloWorld。

## 多重组件
接下来稍微尝试复杂点的，我们看看组件之间是怎么拼合的。
``` bash
<script type="text/babel">
    var MyButton = React.createClass({
        render: function() {
            return <button>我是按钮</button>
        }
    });
    var MyInput = React.createClass({
        render: function() {
            return <input type="text" placeholder="我觉得我应该说点什么" />
        }
    });
    var HelloWorld = React.createClass({
        render: function() {
            return (
                <div>
                    <p>Hello, world</p>
                    <MyInput/>
                    <MyButton/>
                </div>
            );
        }
    });
    ReactDOM.render(
        <HelloWorld/>,
        document.getElementById('container')
    );
</script>
```
上述代码中我们先创建了两个小组件：MyButton和MyInput，随后在HelloWorld中引用它们，便可以实现这些组件之间的父子节点关系，兄弟关系。不过有一点需要注意
**每一个组件的return结果中，都应该有且只有一个父节点。**
比如上面的HelloWorld组件，它原本最后返回的应该是
``` bash
<p>Hello, world</p>
<MyInput/>
<MyButton/>
```
但这样无法正确渲染，所以我们给这三个标签加上一层 < div > 

## 组件也是函数
我们之所以这么说，是因为很多情况下需要向组件传递参数，这时候把组件理解为一个接受param的函数会好接受很多。看下第4个例子：
``` bash
<script type="text/babel">
    var HelloWorld = React.createClass({
        render: function() {
            return (
                <div>
                    <p>Hello, {this.props.name}!</p>
                    <span>Your age is {this.props.age}</span>
                </div>
            );
        }
    });
    ReactDOM.render(
        <HelloWorld name="Johnny" age="12"/>,
        document.getElementById('container')
    );
</script>
```


## 组件也是状态机
