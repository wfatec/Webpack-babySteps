# 配置react环境

## react和react-dom

这两个包在React 0.14版本以前是合并在一起的，那么为什么会分开呢？其实React本质是会生成一个抽象语法树\(AST\),而这个AST其实是一个相对独立的存在，包含了所有需要渲染的信息，其实AST本身并不关心具体的渲染过程，而react-dom本质上就是实现一个AST到DOM的映射，它与react本身的逻辑没有直接关系，将其单独抽离出来就为web和native端的组件共享提供了更好的支持，试想一下我们再来一个react-native，是不是就可以将AST映射到移动端呢？这也很好的体现了react的口号：_**write once run anywhere**_。

接下来是具体的安装：

```text
yarn add react react-dom
```

新建src/App.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
const App = () => {
  return (
    <div>
      <p>React here!</p>
    </div>
  );
};
export default App;
ReactDOM.render(<App />, document.getElementById("root"));
```

具体功能很简单，显示'React here!'，

在src/index.js引入

```javascript
import App from './App'
```

运行`npm run build`,发现控制台会显示如下错误信息：

```text
ERROR in ./src/App.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
SyntaxError: D:\workspace_github\webpack-dev-env\src\App.js: Unexpected token (5:4)

  3 | const App = () => {
  4 |   return (
> 5 |     <div>
    |     ^
  6 |       <p>React here!</p>
  7 |     </div>
  8 |   );
```

原因是这里我们使用了React配套的JSX语法，而webpack根本不认识它，但没关系，我们有强大的loader体系

## @babel/preset-react

`@babel/preset-react`能够解释并转译JSX语法。

1. 安装`@babel/preset-react`:

```text
yarn add -D  @babel/preset-react
```

1. 配置`.banelrc`

增加@babel/preset-react项

```javascript
{
    "presets": [
        //es6运行环境
        "@babel/preset-env",
        //react运行环境
        "@babel/preset-react"
    ],
    "plugins":[
        //运行时编译es6，入口文件引用作为辅助和内建，
        //1.自动添加垫片到你的当前代码模块而非全局，以避免编译输出的重复问题
        //2.为你的代码创建一个沙盒环境，将内置插件起了别名 core-js，这样就可以无缝的使用它们，并且无需使用 polyfill
        "@babel/plugin-transform-runtime"
    ]
}
```

现在start或build都能运行无误了，大家可以尝试一下.

