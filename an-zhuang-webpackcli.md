# 安装webpack-cli

[webpack-cli](https://webpack.docschina.org/api/cli/)可以通过CLI或者配置文件（默认值：webpack.config.js）获取配置并传递给Webpack进行打包。

```text
yarn add webpack-cli -D
```

然后再package.json中添加

```javascript
"scripts": {
  "build": "webpack"
}
```

执行

```javascript
npm run build
```

此时我们发现控制台出现以下错误

```text
ERROR in Entry module not found: Error: Can't resolve './src' in '~\webpack-dev-env'
```

也就是说webpack默认打包入口在'./src'文件夹下的'index.js'，这是webpack4的一个新特性。此外还有一个warning

```text
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or
 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
```

大意为'mode'未设置，webpack默认会使用'production'模式，即生产模式。现在我们将build命令修改为

```text
"build": "webpack --mode=production"
```

且新建src文件夹，并在文件夹内创建'index.js',内容为

```javascript
console.log('hello webpack!')
```

此时执行'npm run build'，我们发现打包成功，切打包文件自动被放到'dist/main.js'，由于使用了生产模式，所以webpack会开启一系列额外的优化，包括minification, scope hoisting, tree-shaking等。

