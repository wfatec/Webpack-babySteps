# 使用webpack-dev-server

之前的操作可以让我们顺利的用webpack打包项目文件，但是在开发过程中每次打包后才能看到最终效果无疑会造成严重的效率损失，那么有什么方法能让我们实时的看到代码更改所产生的效果呢？答案就是[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)

首先进行安装，执行

```text
yarn add webpack-dev-server -D
```

在webpack.config.js中加入

```javascript
// webpack开发服务器
    devServer:{
        //设置开发服务起的目标地址
        contentBase:path.resolve(__dirname,'../dist'),
        //服务器访问地址
        host:'localhost',
        //服务器端口
        port:8088,
        //是否启用服务器压缩
        compress:true
    }
```

在package.json的script下增加

```text
"start": "webpack-dev-server --config=config/webpack.config.js",
```

执行

```text
npm start
```

这时我们就能在8088端口下实时调试我们的代码了

