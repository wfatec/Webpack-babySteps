# 兼容ES6语法

ES6语法的推出是前端领域一件振奋人心的大事件，极大的提高了JS的工程化性能，让我们能够更好的组织我们的代码并让团队协作变得更加舒适。但由于目前浏览器的支持度不一，所以很难进行大规模应用。 webpack本身只是一个构建工具，并不会对我们代码本身的语法层面进行处理，但是其提供的强大的可扩展性却让一切变得可能，通过各种各样的loader工具，我们可以方便的对各类文件进行转译，其中babel-loader可以将我们的JS或类JS语法进行转译，ES6转ES5自然不在话下。在babel-loader我们就可以毫无顾虑的体验ES6带来的各种便利，而无须担心兼容性问题。

## 安装babel-loader

```
yarn add babel-loader @babel/core -D
```

在module.rules新增一条：

```js
{
    //匹配js或jsx类型文件
    test:/\.js$/,
    //使用babel-loader进行转义
    use:['babel-loader'],
    //设置目标文件
    include:path.resolve(__dirname,'../src'),
    //设置排除文件
    exclude:path.resolve(__dirname,'../node_modules')
}
```

上述配置告诉webpack，当文件为js时，则使用babel-loader进行转译，目标文件夹为`src`,且排除`node_modules`文件夹下的内容，提高编译效率。

## 安装@babel/preset-env

用于解析ES6语法

```
yarn add @babel/preset-env -D
```

## 建立`.babelrc`文件

```js
{
    "presets": [
        // es6运行环境
         "@babel/preset-env"
    ]
}
```

## 安装[@babel/polyfill](http://babeljs.io/docs/en/babel-polyfill)

此时已经可以运行ES6语法了，但是这里有一个问题，babel只负责语法转换，比如将ES6的语法转换成ES5。但如果有些对象、方法，浏览器本身不支持，比如：

1. 全局对象：Promise、WeakMap 等。
2. 全局静态函数：Array.from、Object.assign 等。
3. 实例方法：比如 Array.prototype.includes 等。

此时，就需要引入babel-polyfill来模拟实现这些对象、方法。

```
yarn add @babel/polyfill -D
```

entry项修改为

```js
// 入口文件地址
entry:{
    //为项目添加垫片
    polyfill:'@babel/polyfill',
    //将src/index.js设置为入口文件，main可任意设置，这里设为文件名相同
    main:'./src/index.js'
}
```

## 安装[@babel/plugin-transform-runtime](http://babeljs.io/docs/en/babel-plugin-transform-runtime)

到了这里似乎已经很完美，但是我们来思考一个问题，babel-polyfill实际上是在全局添加变量来作为那些浏览器未实现API的一个模拟，但是这样必然导致污染全局命名空间，这个时候就轮到babel-plugin-transform-runtime登场了。它能够实现局部加载垫片，避免污染全局空间，且能避免babel编译的工具函数在所有包中重复出现，减少包的体积。但是同样也有一些缺点，那就是不能使用类似`"foobar".includes("foo")`的实例方法。

```
yarn add @babel/plugin-transform-runtime -D
```

## 安装[@babel/runtime](http://babeljs.io/docs/en/babel-runtime)

@babel/plugin-transform-runtime一般仅用于开发环境，但是runtime本身会被代码所依赖，因此需要将@babel/runtime安装到生产环境的依赖项中

```
yarn add @babel/runtime
```

注意这里没有-D。
接下来让我们实验一下ES6语法，在src下新建Rect_class.js文件，内容为：

```js
class Rect {
    constructor(x, y) {
      this.width = x;
      this.height = y;
    }
    area() {
      return this.width * this.height;
    }
    perimeter(){
        return (this.width + this.height)*2
    }

}
export default Rect;
```

逻辑很简单，就是用ES6新增的class方法实现了一个矩形的类，然后再index.js中引入：

```js
    import Rect from './Rect_class'

    const rectObject = new Rect(3,4)
    console.log('周长： ',rectObject.perimeter())
    console.log('面积： ',rectObject.area())
```

执行`npm start`我们可以在控制台打印出：

```
周长：  14
面积：  12
```

下面再运行build，看一下打包后的结果，核心部分变成了：

```js
var Rect =
/*#__PURE__*/
function () {
  function Rect(x, y) {
    _babel_runtime_helpers_classCallCheck__WEBPACK_IMPORTED_MODULE_0___default()(this, Rect);

    this.width = x;
    this.height = y;
  }

  _babel_runtime_helpers_createClass__WEBPACK_IMPORTED_MODULE_1___default()(Rect, [{
    key: "area",
    value: function area() {
      return this.width * this.height;
    }
  }, {
    key: "perimeter",
    value: function perimeter() {
      return (this.width + this.height) * 2;
    }
  }]);

  return Rect;
}();
```

可见我们的ES6代码已经成功转成ES6。