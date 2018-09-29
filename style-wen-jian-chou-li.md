# style文件抽离

在实际开发过程中，将style直接插入到JS或Html中是非常不经济的做法，对于缓存，CDN等技术均无法很好的支持，所以我们还需要想办法把style文件单独打包出来，以link的方式引入，这就需要用到[extract-text-webpack-plugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)或者[mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin),这里我们使用前者，需要注意的是该插件目前仅支持到webpack3，要想在webpavk4中使用需要安装next版本：

```
yarn add -D extract-text-webpack-plugin@next
```

在webpack.config.js中做如下更改

1. 引入extract-text-webpack-plugin

```js
//将css抽离成单独的文件
const ExtractTextPlugin = require('extract-text-webpack-plugin');
```

2. 更改module.rules

```js
{
    //匹配css文件
    test:/\.css$/,
    use:ExtractTextPlugin.extract({
        /**
         * 一般情况下，由于ExtractTextPlugin已经将css文件单独打包并引入，不再需要style-loader,
         * 但是在以下三种情况下会执行fallback：
         * 1. css不被抽离/ExtractTextPlugin失效
         * 2. HMR
         * 3. 异步chuncks/bundles
         * 
         * style-loader：生成一个内容为最终解析完的css代码的style标签，放到head标签里
         */
        fallback:"style-loader",
        use:[
            //解析css模块引入
            'css-loader',
            //可以对css进行样式补全等操作
            'postcss-loader',
        ]
    })
}
```

3. 增加plugin

```js
//将css单独打包
new ExtractTextPlugin({
    /**
     * 生成文件的文件名。可能包含 [name], [id] and [contenthash]
     * [contenthash]在webpack4.3以上需要改成[hash],原因是与在webpack4存在命名冲突
     */
    filename: 'css/[name].css',
    /**
     * filename:  (getPath) => {
     *      return getPath('css/[name].css').replace('css/js', 'css');
     *  },
     *  从所有额外的 chunk提取,当使用 CommonsChunkPlugin 
     *  并且在公共 chunk 中有提取的 chunk（来自ExtractTextPlugin.extract）时，
     *  allChunks **必须设置为 true
     */
    allChunks:true
})
```

我们发现dist/css下出现了main.css文件：

```css
body {
  background: red;
}
div {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
}
```

box-sizing自动添加了前缀，打开dist/index.html，head部分插入了:

```html
<link href="css/main.css" rel="stylesheet">
```

完全符合我们的预期~
