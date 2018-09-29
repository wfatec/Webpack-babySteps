# 解析less文件

由于个人比较习惯使用less来编写style文件，所以这里在做一下配置

1. 安装[less](http://lesscss.org/)和[less-loader](https://github.com/webpack-contrib/less-loader)

```
yarn add -D less less-loader
```

2. 配置module.rules

```js
{
    //匹配less文件
    test:/\.less$/,
    // #extract,从一个已存在的 loader 中，创建一个提取(extract) loader
    use:ExtractTextPlugin.extract({
        fallback:"style-loader",
        use:[
            //生成一个内容为最终解析完的css代码的style标签，放到head标签里
            // 'style-loader',
            //解析css模块引入
            'css-loader',
            //可以对css进行样式补全等操作
            'postcss-loader',
            //将less解析为css
            'less-loader'
        ]
    })
}
```