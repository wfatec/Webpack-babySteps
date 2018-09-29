# css模块化

一直以来前端的模块化一直限定在JS上，而另一位重要成员css则由于天然的弱编程能力，难以实现模块化，但是webpack强大的扩展能力让css模块化成为了可能，需要的仅仅只是为css文件添加相应的loader

## 安装css-loader

```
yarn add css-loader -D
```

css-loader作用是让我们的css文件能够用`import`的方式实现模块化传递。

## 安装style-loader

```
yarn add style-loader -D
```

`style-loader`能够让`css-loader`载入的css文件以`<style></style>`的形式插入到`<header></header>`中。
配置module.rules

```js
{
    //匹配css文件
    test:/\.css$/,
    use:[
        //生成一个内容为最终解析完的css代码的style标签，放到head标签里
        'style-loader',
        //解析css模块引入
        'css-loader',
    ]
}
```

接下来我们src下新建index.css

```css
body{
    background:red;
}
```

在index.js中加入

```js
import './index.css'
```

启动后我们打开浏览器控制台，发现css文件中的内容已经通过`<style></style>`插入到`<header></header>`中。

## 安装postcss-loader

[PostCSS](https://www.ibm.com/developerworks/cn/web/1604-postcss-css/index.html)本身是一个功能比较单一的工具。它提供了一种方式用 JavaScript 代码来处理 CSS。它负责把 CSS 代码解析成抽象语法树结构（Abstract Syntax Tree，AST），再交由插件来进行处理。通过PostCSS能够极大的提高CSS的开发效率。

webpack通过`postcss-loader`来对`.css`文件进行处理，并添加在`style-loader`和`css-loader`之后。首先进行安装：

```
yarn add -D postcss-loader
```

postcss有一些非常好用的插件：

1. Autoprefixer
其作用是为 CSS 中的属性添加浏览器特定的前缀。为了兼容不同浏览器的不同版本，在编写 CSS 样式规则声明时通常需要添加额外的带前缀的属性。这是一项繁琐而无趣的工作。Autoprefixer 可以自动的完成这项工作。

2. cssnext
cssnext 插件允许开发人员在当前的项目中使用 CSS 将来版本中可能会加入的新特性。cssnext 负责把这些新特性转译成当前浏览器中可以使用的语法。从实现角度来说，cssnext 是一系列与 CSS 将来版本相关的 PostCSS 插件的组合。比如，***cssnext 中已经包含了对 Autoprefixer 的使用***，因此使用了 cssnext 就不再需要使用 Autoprefixer。

我们一步到位直接安装postcss-cssnext

```
yarn add -D postcss-cssnext
```

在根目录下新建`postcss.config.js`作为postcss的配置文件，postcss-loader会首先优先使用webpack配置文件中的loader选项，然后再检查`postcss.config.js`，将postcss的配置文件单独分离出来有助于构建意图更为清晰(.babelrc也是一样)。`postcss.config.js`内容为：

```js
module.exports = {
    plugins: {
        //兼容css4语法
        'postcss-cssnext': {}
    }
}
```