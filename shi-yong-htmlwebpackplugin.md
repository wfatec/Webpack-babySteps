# 使用HtmlWebpackPlugin

我们在dist下新建一个index.html文件，内容为

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="root"></div>
    <script src="./main.js" type="text/javascript" />
</body>
</html>
```

将html在浏览器中打开即可。

我们之前通过手写html文件，并将打包后的main.js引入，但是如果出现类似打包文件名称发生变更的情况，就需要重新更改引入项，有没有什么方法能自动生成html文件，并自动引入依赖文件呢？答案就是html-webpack-plugin。
安装html-webpack-plugin：

```
yarn add html-webpack-plugin -D
```

在public文件夹下新建index.html作为模板文件，内容为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

放入favicon.ico文件，webpack.config.js加入html-webpack-plugin插件

```js
const path = require("path")
//自动生成html文件并注入script标签引用
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development',
    // 入口文件地址
    entry:{
        //将src/index.js设置为入口文件，main可任意设置，这里设为文件名相同
        main:'./src/index.js'
    },
    // 出口文件配置
    output:{
        // 最终打包路径
        path:path.resolve(__dirname,'../dist'),
        // 打包文件的名称,name为入口文件的名称
        filename:'[name].js'
    },
    //插件，类似于中间件，可在打包过程中进行功能扩展
    plugins:[
        new HtmlWebpackPlugin({
            //生成html文件的标题
            title:"webpack",
            //html文件的文件名，默认是index.html
            filename:"index.html",
            //生成文件依赖的模板，支持加载器(如handlebars、ejs、undersore、html等)
            template: './public/index.html',
            // script标签插入位置
            // true 默认值，script标签位于html文件的 body 底部
            // body script标签位于html文件的 body 底部
            // head script标签位于html文件的 head中
            // false 不插入生成的js文件，这个几乎不会用到的
            inject:true,
            //将给定的图标加入到输出的html文件
            favicon:'./public/favicon.ico'
        })
    ],
}
```

打包后的dist/index.html为

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
<link rel="shortcut icon" href="favicon.ico"></head>
<body>
    <div id="root"></div>
<script type="text/javascript" src="main.js"></script></body>
</html>
```

我们可以看到JS和icon文件自动被引入进来