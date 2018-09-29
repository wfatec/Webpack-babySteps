# webpack配置文件

你当然可以直接通过命令行来运行webpack，并完全使用默认配置，但在大多数场景我们仍需要进行一些个性化配置，这时将这些设置直接卸载命令行中就显得不合时宜了，这时我们可以通过编写webpack.config.js，来规范化我们的webpack配置，这也是目前最主流的做法。

在根目录下创建config文件夹，之后新建webpack.dev.js和webpack.prod.js分别作为开发环境和生产环境下的配置文件。我们主要以开发环境的配置为主，生产环境的配置其实大同小异。将webpack.dev.js修改为

```js
const path = require("path")
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
    }
}
```

build命令修改为:

```
"build": "webpack --config=config/webpack.config.js"
```

我们可以看到dist文件下出现了我们打包后的main.js文件。